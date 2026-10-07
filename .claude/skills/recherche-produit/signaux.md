# Signaux : lecture, seuils et sources

Tous les seuils ci-dessous sont des **heuristiques, pas des preuves isolées**. Un signal ne
compte que s'il est confirmé par d'autres signaux indépendants.

## 1. Signaux boutique

À collecter pour chaque boutique, sur **7 j, 30 j et 90 j** quand c'est possible.

| Signal | Outil BrandSearch | Confiance |
|---|---|---|
| Pubs actives actuelles | `get_brand` → `last_meta_active_count` ; par produit : `ads.active` (`get_market`) | OBSERVED |
| Croissance des pubs actives 3 j / 7 j / 30 j | `get_brand` (avec `fields`, voir plus bas) → `active_growth_percentage_3d`, `active_growth_percentage_7d`, `active_growth_percentage` (30 j) | OBSERVED (calcul BrandSearch) |
| Pubs actives à J-90 | `suivi/snapshots.csv` si présent ; sinon `search_meta_ads` `brand_ids=<domaine>`, `ad_started_to=<J-90>`, `status=active` (borne basse : pubs lancées avant J-90 et encore actives) | OBSERVED (snapshot) / INFERRED |
| Croissance du total de pubs / pubs coupées | `total_growth_percentage`, `inactive_growth_percentage` | OBSERVED (calcul BrandSearch) |
| Nouvelles pubs sur 7 / 30 / 90 j | `get_brand_ads_aggregates` avec `from_date`/`to_date` → `window.ad_count` ; compare avec la période précédente de même durée | OBSERVED |
| Duplications | `search_meta_ads` `brand_ids=<domaine>` `sort_by=duration` → `duplicate_count` des pubs ; `duplicate_count_min` pour filtrer | OBSERVED |
| Dépense pub 3 / 7 / 30 / 60 j | `spend_3d`, `spend_7d`, `spend_30d`, `spend_60d` ; tendance 30 j = `spend_30d` ÷ (`spend_60d` − `spend_30d`) − 1 (UE/UK uniquement) | ESTIMATED |
| Trafic actuel | `get_brand` → `monthly_visits` | ESTIMATED |
| Croissance 30 j BrandSearch | `growth_30d` (en %, définition non documentée, probablement le trafic) ; valeurs > 1 000 % = base quasi nulle, à ne pas interpréter seule | ESTIMATED |
| Évolution du trafic 90 j | `suivi/snapshots.csv` ; sinon UNKNOWN | ESTIMATED / UNKNOWN |
| Revenu estimé | `revenue` des cartes produit, `min_revenue`/`max_revenue` des marques | ESTIMATED |
| Évolution du revenu | `suivi/snapshots.csv` ; sinon UNKNOWN | ESTIMATED / UNKNOWN |
| Dépense pub du marché produit (série quotidienne) | `get_market` `history=true` → `advertisers`, `est_daily_spend_usd` (données UE/UK) | ESTIMATED |
| Ancienneté boutique | `get_brand` → `created_at` (= entrée dans le catalogue, pas création réelle) ; WHOIS / date de la page Meta via WebSearch | INFERRED |
| Ancienneté produit | `first_advertised` de la carte produit | OBSERVED |
| Ancienneté des pubs | `search_meta_ads` `sort_by=duration` → plus vieille pub encore active | OBSERVED |
| Produit toujours vendu | `in_stock`, `last_advertised`, fiche produit accessible (WebFetch) | OBSERVED |

### Récupérer tous les champs de croissance en un appel

`get_brand` ne renvoie pas ces champs par défaut : il faut les demander.

```
get_brand(brand_id="<domaine>", fields="id,created_at,monthly_visits,growth_30d,last_meta_active_count,last_meta_total_count,active_growth_percentage_3d,active_growth_percentage_7d,active_growth_percentage,total_growth_percentage,inactive_growth_percentage,spend_3d,spend_7d,spend_30d,spend_60d,min_revenue,max_revenue")
```

Le même `fields` marche sur `search_brands` / `query_brands`, qui acceptent aussi le tri par
`active_growth_percentage`, `active_growth_percentage_7d`, `growth_30d`, `spend_7d`, `spend_30d`.

### Filtres de période (ancienneté)

| Question | Filtre |
|---|---|
| Boutiques présentes depuis 3 mois / 6 mois / 1 an / 2 ans / 3 ans | `search_brands` ou `query_brands` : `created_to` = aujourd'hui − durée (`created_from` pour « depuis moins de ») |
| Produits annoncés depuis au moins X mois | `search_products` : `first_advertised_to` = aujourd'hui − X |
| Produits apparus récemment | `search_products` : `first_advertised_from` = aujourd'hui − X |
| Produits encore annoncés | `search_products` : `last_advertised_from` = aujourd'hui − 7 j |
| Pubs lancées sur une période | `search_meta_ads` : `ad_started_from` / `ad_started_to` |
| Pubs actives depuis au moins X jours | `search_meta_ads` : `duration_min` = X, `status=active` |
| Pubs lancées par une marque sur une période | `get_brand_ads_aggregates` : `from_date` / `to_date` |

`created_at` est la date d'entrée dans le catalogue BrandSearch : c'est une borne basse de
l'âge réel de la boutique (INFERRED).

**Limite à connaître** : la dépense, la portée et l'audience des pubs ne sont publiées par Meta
que pour l'UE et le UK. Pour les USA, le Canada et l'Australie, on dispose du nombre de pubs,
de leurs dates et duplications, mais pas de leur dépense : marque-la UNKNOWN, ne l'invente pas.

### Signal de scaling fort

```
Pubs actives ↑  +  trafic ↑  +  nouvelles créatives ↑  +  duplications ↑  +  produit toujours vendu
```

Ne jamais utiliser uniquement le nombre de pubs comme preuve de scaling.

## 2. Signaux publicitaires

| Observation | Lecture |
|---|---|
| 5–10 nouvelles pubs | phase de test possible |
| 10–30 pubs actives | signal à investiguer |
| 30–50+ pubs actives | signal commercial important |
| 50–100+ pubs **et** nombre de pubs en croissance | scaling potentiel fort |
| 100+ pubs stables | activité importante, potentiellement mature |

- Une vieille pub seule ≠ produit gagnant.
- Pub active depuis longtemps **+** duplications **+** nouvelles variantes **+** croissance du
  compte = signal beaucoup plus fort.
- Pour une marque multi-produits, rapporte les pubs **au produit étudié** (`ads.active` de la
  carte produit, ou pubs dont le lien pointe vers la fiche), pas seulement au compte entier.

## 3. Momentum

```
AD GROWTH 7D / 30D / 90D      = (pubs actives actuelles − pubs actives J-x) / pubs actives J-x
TRAFFIC GROWTH 7D / 30D / 90D = (trafic actuel − trafic J-x) / trafic J-x
```

Sources :
- AD GROWTH 7D et 30D : directement `active_growth_percentage_7d` et `active_growth_percentage`.
- AD GROWTH 90D : snapshot J-90, sinon reconstitution via `search_meta_ads` (INFERRED).
- TRAFFIC GROWTH 30D : `growth_30d` (ESTIMATED) ; recoupe avec la tendance de dépense
  `spend_30d` vs `spend_60d` quand elle existe.
- TRAFFIC GROWTH 7D et 90D : snapshots, sinon UNKNOWN.

Si la valeur J-x vaut 0 ou est UNKNOWN, n'invente pas de pourcentage : écris « n/a » et appuie-toi
sur les nouvelles pubs par période et la série `get_market`.

### Classement de la boutique

| Classe | Règle (heuristique) |
|---|---|
| **ACCELERATING** | AD GROWTH 30D ≥ +20 % **et** rythme 7 j supérieur au rythme 30 j (AD GROWTH 7D > AD GROWTH 30D ÷ 4) **et** trafic non décroissant (ou UNKNOWN) **et** nouvelles pubs 30 j > période précédente |
| **GROWING** | AD GROWTH 30D entre +5 % et +20 %, ou ≥ +20 % sans accélération ; trafic stable ou en hausse |
| **STABLE** | AD GROWTH 30D entre −5 % et +5 % et pubs toujours actives |
| **DECLINING** | AD GROWTH 30D < −5 %, ou trafic en baisse nette, ou plus aucune pub active |

Si pubs et trafic divergent (pubs ↑, trafic ↓), prends la classe la plus basse des deux et
signale la divergence. Le classement est toujours **INFERRED**.

Priorité : ACCELERATING > GROWING > STABLE > DECLINING.

## 4. Validation produit (multi-boutiques)

À partir de `get_market` (et des produits fonctionnellement identiques) :

| Mesure | Comment |
|---|---|
| Boutiques indépendantes | domaines distincts, en excluant les boutiques d'un même groupe (même page Meta, même fournisseur de marque blanche évident) |
| Pays | `country_code` de chaque marque |
| Boutiques qui scalent | classe ACCELERATING ou GROWING |
| Boutiques stables | classe STABLE |
| Boutiques en déclin | classe DECLINING avec pubs encore actives |
| Boutiques ayant arrêté | `ads.total` > 0 et `ads.active` = 0 |

- **Signal fort** : plusieurs boutiques indépendantes **et** plusieurs à momentum positif.
- **Signal très fort** : même produit performant dans plusieurs marchés du Big Four.
- **Signal négatif** : beaucoup de boutiques ont testé puis arrêté leurs pubs
  (taux d'arrêt = arrêtées ÷ total ; au-delà de 50 %, signal négatif marqué).

## 5. Longévité / evergreen

Mesurée sur la **preuve commerciale la plus ancienne encore vivante** (première pub du produit
si le produit est toujours annoncé, ou plus vieille pub encore active) :

| Durée | Lecture |
|---|---|
| < 3 mois | trop récent, confiance faible |
| 3–6 mois | émergent |
| 6–12 mois | bonne validation |
| 12+ mois | forte validation evergreen |
| 24+ mois | très forte longévité |

Un produit récent reste intéressant si son momentum **multi-boutiques et multi-pays** est
exceptionnel. Écarte les produits dont toute la performance tient dans un pic court (série
`get_market` en cloche, vendeurs qui arrêtent tous au même moment).

## 6. Big Four

| Résultat | Lecture |
|---|---|
| 2+ marchés avec plusieurs signaux positifs | validation internationale intéressante |
| 3–4 marchés avec plusieurs vendeurs performants | validation internationale forte |

## 7. France

Mêmes mesures que pour le Big Four. Opportunité idéale : Big Four fort, France peu
concurrentielle **mais** avec quelques preuves commerciales, produit evergreen. L'absence totale
de concurrent français n'est jamais une preuve positive.
