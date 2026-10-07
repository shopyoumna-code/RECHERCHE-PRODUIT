---
name: recherche-produit
description: Trouver et valider des produits e-commerce à fort potentiel (dropshipping, marque DTC, boutique Shopify) avec des preuves de marché chiffrées. À utiliser quand l'utilisateur demande des idées de produits, des produits gagnants, une niche à lancer, ou veut valider un produit précis avant d'ouvrir une boutique.
---

# Recherche et validation de produits

Objectif : sortir une **shortlist de 5 à 10 produits**, chacun noté sur 100 avec des preuves
de marché vérifiables, puis une fiche de lancement pour le meilleur.

## Sources de données

| Source | Usage |
|---|---|
| **BrandSearch** (`mcp__BRANDSEARCH__*`) | Source principale : produits, pubs Meta/TikTok, boutiques concurrentes, revenus estimés |
| **Shopify** (`mcp__Shopify__*`) | Noms de marque, domaines, aperçus de boutique, création de produits une fois validés |
| **WebSearch / WebFetch** | Prix fournisseur (AliExpress, CJ, Alibaba), tendances de recherche, réglementation |

**N'utilise jamais One Radar** (`mcp__ONE_RADAR__*`), même s'il est connecté.

BrandSearch facture des crédits (1 crédit par produit ou pub renvoyé). Garde `page_size`
et `limit` bas (10 par défaut), et vérifie le solde avec `get_usage` en début de session
si la recherche est large.

## Paramètres par défaut

Si l'utilisateur ne précise rien :
- **Marchés** : France (+ francophonie) et États-Unis.
- **Prix de vente** : 20 à 90 USD (zone où l'achat impulsif sur pub fonctionne).
- **Type** : produit physique, léger, non fragile, livrable en moins de 10 jours.

## Méthode en 4 étapes

### 1. Découvrir (large)

Lance plusieurs angles en parallèle, puis fusionne les doublons :

- `search_products` trié par `ads_30d` (pubs lancées ces 30 derniers jours), avec
  `price_min_usd`/`price_max_usd` et éventuellement `niche` ou `q`.
- `search_products` avec `first_advertised_from` = il y a 90 jours et `sort=active_ads` :
  produits **récents** qui scalent déjà.
- `discover_meta_ads` et `discover_tiktok_ads` (filtrés par `niche` si donnée) : repérer
  les produits derrière les pubs virales.
- Pour la France : `market_country=FR` sur `search_products` (données EU), et
  `languages=fr` sur les recherches de pubs.

Les valeurs de `niche` doivent venir de `get_facet("product-niches")`. Ne les invente pas.

**Astuces tirées des recherches précédentes :**
- Pour une cible définie par un **problème** (par exemple « femmes 40+ »), la meilleure entrée est
  `search_meta_ads` avec `q` = le problème dans la langue du marché (« jambes lourdes », « poches
  sous les yeux », « après 40 ans »), `languages=fr`, `status=active`, `sort_by=reach` et
  `max_ads_per_brand=1`. On voit ainsi directement les marques qui dépensent sur ce problème.
- Avec `search_products`, ne combine pas `q` et `sort=active_ads` : le tri écrase la pertinence
  et ramène des produits hors sujet. Garde le tri par défaut (pertinence) quand tu passes `q`.
- `get_brand_ads_aggregates` donne en un appel la dépense publicitaire UE totale, la portée et
  la langue des pubs : c'est la preuve la plus solide pour une marque sans revenu estimé.
- `get_brand_summary` renvoie des réponses très longues : préfère `get_products` et
  `get_brand_ads_aggregates`.
- `get_product` donne l'audience (pays, part de femmes, âge) : vérifie toujours qu'elle
  correspond à la cible avant de retenir un produit.

### 2. Filtrer (éliminatoire)

Écarte immédiatement tout produit qui :
- est une marque déposée ou un produit de marque (Nike, Dyson, Stanley…) : risque de contrefaçon ;
- fait des allégations médicales, ou est un complément alimentaire, un cosmétique actif,
  une arme, un produit pour enfants de moins de 3 ans : réglementation lourde ;
- contient une batterie lithium non amovible, un liquide ou un aérosol, ou est très
  volumineux ou fragile : logistique ;
- a plus de 50 vendeurs (`sellers`) : marché saturé ;
- n'a aucune pub active : pas de preuve de demande.

### 3. Valider (en profondeur, sur 10 à 15 candidats maximum)

Pour chaque candidat :
1. `get_product` → audience, top 5 boutiques, `sourcing_available`, présence TikTok Shop.
2. `get_market` → toutes les boutiques qui le vendent, leurs prix.
3. `get_brand_summary` sur les 2 ou 3 meilleurs vendeurs → revenu estimé, nombre de pubs actives.
4. `search_meta_ads` avec `brand_ids` = ces vendeurs et `sort_by=duration` → la pub la plus
   ancienne encore active (une pub active depuis plus de 30 jours est rentable).
5. WebSearch → prix fournisseur (AliExpress / CJ Dropshipping), délai de livraison.

### 4. Noter sur 100

Applique la grille de [grille-scoring.md](grille-scoring.md). **Chaque point doit être
justifié par un chiffre issu d'un outil.** Si une donnée manque, attribue 0 à ce critère et
écris « donnée manquante » : n'estime jamais un chiffre sans le dire.

## Livrable

Écris le rapport dans `rapports/AAAA-MM-JJ-<sujet>.md` avec :

1. **Résumé** : les 3 meilleurs produits en une ligne chacun.
2. **Tableau** : produit, score /100, prix de vente, coût estimé, marge, vendeurs, pubs actives, verdict (🟢 lancer / 🟡 tester / 🔴 éviter).
3. **Fiche détaillée** par produit retenu : preuves (avec liens vers les pubs et boutiques),
   score par critère, angle marketing suggéré (inspiré des pubs qui marchent, sans les copier),
   risques.
4. **Plan de test** pour le n°1 : budget pub de test (par ex. 30 à 50 €/jour pendant 5 jours),
   KPI d'arrêt (CPA cible = marge brute ÷ 1,5), 3 angles de pub à tester.
5. **Produits écartés** et pourquoi, en une ligne chacun.

Termine en proposant les suites possibles : `generate-business-names` et
`generate-domain-names` (Shopify) pour la marque, puis `get-new-store-previews` pour un
aperçu de boutique. Ne crée rien sur Shopify sans l'accord explicite de l'utilisateur.
