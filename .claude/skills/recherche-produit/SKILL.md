---
name: recherche-produit
description: Trouver et valider des produits evergreen à haut potentiel pour lancer une marque en France, à partir de preuves commerciales observées dans le Big Four (USA, UK, Canada, Australie). À utiliser quand l'utilisateur demande des idées de produits, des produits gagnants, une niche à lancer, ou veut valider un produit précis avant d'ouvrir une boutique.
---

# Recherche et validation de produits evergreen

**Objectif** : identifier des produits evergreen à haut potentiel pour créer une **marque long
terme en France**.

- **Marchés sources** : USA, UK, Canada, Australie (le « Big Four »).
- **Marché cible** : France.
- **Principe** : la data tranche. Aucun produit n'est validé sur une intuition.

On ne cherche pas « un produit qui a l'air gagnant ». On cherche des **anomalies positives
dans les données**, et le meilleur candidat est celui qui **cumule le plus de signaux
indépendants**. **Aucun signal isolé ne suffit.**

Références :
- [signaux.md](signaux.md) : comment lire chaque signal, seuils, calcul du momentum, et quel
  outil fournit quelle donnée.
- [grille-scoring.md](grille-scoring.md) : le Product Score /100.

## Sources de données

| Source | Usage |
|---|---|
| **BrandSearch** (`mcp__BRANDSEARCH__*`) | Source principale : produits, marchés de vendeurs, pubs Meta/TikTok, boutiques, trafic, revenus estimés |
| **Shopify** (`mcp__Shopify__*`) | Noms de marque, domaines, aperçus de boutique, une fois un produit validé |
| **WebSearch / WebFetch** | Prix fournisseur, ancienneté d'un domaine, Bibliothèque publicitaire Meta, concurrents FR hors BrandSearch |

**N'utilise jamais One Radar** (`mcp__ONE_RADAR__*`) ni TrendTrack (`mcp__TRENDTRACK__*`), même s'ils sont connectés.

BrandSearch facture des crédits (1 par produit, vendeur ou pub renvoyé). Garde `page_size` bas
(10 par défaut), vérifie le solde avec `get_usage` en début de session, et ne lance l'analyse
profonde (étapes 3 à 6) que sur 10 à 15 candidats.

## Confiance data (obligatoire)

Chaque chiffre du rapport porte une étiquette :

| Étiquette | Sens | Exemples |
|---|---|---|
| **OBSERVED** | Donnée directement disponible | pubs actives, date de début d'une pub, prix, nombre de vendeurs |
| **ESTIMATED** | Estimation de l'outil | trafic mensuel, revenu, dépense publicitaire |
| **INFERRED** | Déduite de plusieurs données | pubs actives à J-30, statut ACCELERATING, ancienneté de la boutique |
| **UNKNOWN** | Indisponible | trafic à J-90 sans snapshot antérieur, dépense pub hors UE/UK |

- Ne transforme **jamais** une estimation en certitude : écris « ~28 k visites/mois (ESTIMATED) ».
- Ne pénalise **jamais** automatiquement un UNKNOWN : voir la règle de calcul dans
  [grille-scoring.md](grille-scoring.md).

## Méthode en 7 étapes

### 1. Découvrir dans le Big Four (large)

Lance plusieurs angles en parallèle, puis fusionne les doublons par `market.id` :

- `search_products` trié par `ads_30d` puis par `active_ads`, avec `first_advertised_to` = il y a
  6 mois (produits déjà installés dans le temps).
- `search_products` avec `first_advertised_from` = il y a 6 mois et `sort=ads_30d` : produits
  plus récents à **momentum exceptionnel** (gardés seulement s'ils sont multi-boutiques).
- `search_products` avec `sellers_min=3` : produits déjà vendus par plusieurs boutiques.
- `search_meta_ads` avec `country_code` = `US`, `GB`, `CA` ou `AU`, `status=active`,
  `sort_by=duration`, `duplicate_count_min=3`, `max_ads_per_brand=1` : pubs anciennes **et**
  dupliquées, donc rentables.
- `search_brands` avec `country_code` du Big Four, `status=active`, `meta_active_min=10`,
  `created_to` = il y a 6 mois (boutiques installées) et `sort_by=active_growth_percentage`
  (puis `growth_30d`) : boutiques établies dont les pubs accélèrent. Demande les champs de
  croissance avec `fields` (voir [signaux.md](signaux.md)).

Les valeurs de `niche` viennent de `get_facet("product-niches")`. Ne les invente pas.

Astuces :
- Ne combine pas `q` et `sort=active_ads` dans `search_products` : le tri écrase la pertinence.
- `get_brand_summary` est très verbeux : préfère `get_brand` + `get_brand_ads_aggregates`.

### 2. Filtrer (éliminatoire)

Écarte immédiatement tout produit qui :
- est une marque déposée ou une copie de produit de marque (contrefaçon) ;
- relève d'une réglementation lourde en France (allégations médicales, compléments alimentaires,
  cosmétiques actifs, armes, produits pour enfants de moins de 3 ans) ;
- pose un problème logistique majeur (batterie lithium non amovible, liquide, aérosol, très
  volumineux ou fragile) ;
- n'est plus vendu nulle part ;
- est une **mode passagère** évidente (saisonnier, gadget lié à un buzz, licence).

### 3. Signaux boutique (pour chaque vendeur sérieux)

Pour chaque boutique trouvée, appelle `get_brand` avec les champs de croissance (liste dans
[signaux.md](signaux.md)), puis collecte les évolutions sur les **périodes disponibles et pertinentes** (court, moyen et
long terme ; voir [signaux.md](signaux.md)) :
pubs actives et leur évolution, nouvelles pubs, duplications, trafic et son évolution, revenu
estimé et son évolution, ancienneté de la boutique, du produit et des pubs.

**Enregistre chaque mesure** dans `suivi/snapshots.csv` (voir plus bas) : c'est ce qui permet
de calculer de vraies évolutions lors des recherches suivantes.

Signal de scaling fort = **pubs actives ↑ + trafic ↑ + nouvelles créatives ↑ + duplications ↑
+ produit toujours vendu**. Le nombre de pubs seul n'est jamais une preuve de scaling.

### 4. Momentum

Calcule `AD GROWTH` et `TRAFFIC GROWTH` sur les périodes disponibles, puis classe chaque boutique
**ACCELERATING > GROWING > STABLE > DECLINING** selon les règles de [signaux.md](signaux.md).

### 5. Validation produit (multi-boutiques)

Dès qu'un produit est intéressant :
1. `get_product` → top vendeurs, audience, `sourcing_available`.
2. `get_market` avec `history=true` → **tous** les vendeurs du produit, leurs pubs, leurs prix,
   et la série quotidienne du marché (`advertisers`, `est_daily_spend_usd`).
3. Cherche aussi les produits **fonctionnellement identiques** sous d'autres noms
   (`search_products` avec `q` = la fonction, pas le titre).

Mesure : boutiques indépendantes, pays, boutiques qui scalent / stables / en déclin / ayant
arrêté leurs pubs.

- **Signal fort** : plusieurs boutiques indépendantes **et** plusieurs à momentum positif.
- **Signal très fort** : même produit performant dans plusieurs marchés du Big Four.
- **Signal négatif** : beaucoup de boutiques ont testé puis arrêté leurs pubs.

Le succès d'une seule boutique n'est **jamais** une validation suffisante.

### 6. Longévité et validation Big Four

- Longévité : date de première pub du produit (`first_advertised`), plus ancienne pub encore
  active, survie des vendeurs. Barème dans [signaux.md](signaux.md). Évite tout produit dont la
  performance repose sur un pic court.
- Big Four : pour chaque finaliste, remplis le tableau USA / UK / Canada / Australie (présence,
  vendeurs, boutiques en croissance, pubs actives, croissance pubs, trafic, croissance trafic,
  ancienneté, prix). Le pays d'une boutique = `country_code` de la marque ; ses marchés de vente
  = `markets` (noms complets : « United States », « United Kingdom », « Canada », « Australia »).

### 7. France

Une fois le produit validé dans le Big Four, cherche-le en France et mesure **exactement les
mêmes signaux** : concurrents DTC (`country_code=FR`, `market_country=FR`, `languages=fr`),
pubs actives, croissance des pubs, trafic, croissance du trafic, ancienneté, longévité, prix.

**Opportunité idéale** : Big Four = forte validation, France = concurrence réelle mais moins
mature que le Big Four (moins de pubs, angles moins travaillés), France = preuves commerciales
existantes, produit evergreen qui **résout un problème**.

**La concurrence française est une condition, pas un frein.** On veut des produits déjà vendus
en France par plusieurs boutiques actives : c'est la preuve que le marché français achète.
« Aucun concurrent français » n'est **jamais** une preuve positive : le produit est alors
plafonné à À SURVEILLER. L'avantage recherché est d'être **meilleur** que les concurrents
français (angle, offre, marque, créas), pas d'être seul.

### 8. Noter sur 100

Applique [grille-scoring.md](grille-scoring.md). Chaque point est justifié par un chiffre
étiqueté (OBSERVED / ESTIMATED / INFERRED).

## Suivi dans le temps : `suivi/snapshots.csv`

BrandSearch calcule lui-même la croissance des pubs sur 3, 7 et 30 j et une croissance 30 j
(`growth_30d`), mais pas l'historique à 90 j. Pour le construire, ajoute une ligne par boutique analysée à chaque recherche (ne réécris jamais les lignes
existantes) :

```
date,brand_id,pays,produit_market_id,pubs_actives,pubs_total,visites_mensuelles,growth_30d,active_growth_7d,active_growth_30d,spend_30d,revenu_min,revenu_max,source
```

Avant de calculer un momentum, relis ce fichier : une valeur J-30 issue d'un snapshot est
**OBSERVED**, une valeur reconstituée est **INFERRED**.

## Livrable

### Liens cliquables partout (obligatoire)

Chaque boutique, produit ou pub cité (dans le rapport **et** le résumé de chat) a son lien :
- **Boutique** : `[nom](https://domaine.com)`.
- **Produit** : lien direct vers la fiche (champ `url` de BrandSearch).
- **Pub Meta** : `https://www.facebook.com/ads/library/?id=<id de la pub>`.
- **Fournisseur** : lien vers l'annonce.
- Le `dashboard_url` BrandSearch peut s'ajouter en complément, jamais à la place.

Sans URL exacte de fiche produit : lien de la boutique + « fiche produit à retrouver ».
N'invente jamais une URL.

### Contenu du rapport

`rapports/AAAA-MM-JJ-<sujet>.md` :

1. **Résumé** : les 3 meilleurs candidats, chacun avec son score, son verdict et ses signaux
   indépendants en une ligne.
2. **Tableau des finalistes** : produit, score /100, couverture data (%), verdict, momentum
   dominant, nb de boutiques (scalent / stables / déclin / arrêt), marchés Big Four validés,
   ancienneté, concurrents FR.
3. **Fiche par finaliste** :
   - signaux boutique (tableau des évolutions par période et par vendeur, avec étiquettes de confiance) ;
   - momentum et classement ;
   - validation multi-boutiques ;
   - tableau Big Four ;
   - tableau France ;
   - score détaillé par bloc ;
   - **signaux indépendants cumulés** (liste) et **signaux contraires** (liste) ;
   - données UNKNOWN et comment les obtenir.
4. **Plan de test France** pour le n°1 : budget de test, KPI d'arrêt (CPA cible = marge brute
   ÷ 1,5), 3 angles de pub inspirés de ce qui dure dans le Big Four, sans copier.
5. **Produits écartés** et la raison, en une ligne chacun.

Termine en proposant les suites : `generate-business-names` et `generate-domain-names`
(Shopify), puis `get-new-store-previews`. Ne crée rien sur Shopify sans accord explicite.
