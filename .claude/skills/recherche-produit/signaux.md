# Signaux : lecture, seuils et sources

Tous les seuils ci-dessous sont des **heuristiques, pas des preuves isolées**. Un signal ne
compte que s'il est confirmé par d'autres signaux indépendants.

## 1. Signaux boutique

À collecter pour chaque boutique, sur **7 j, 30 j et 90 j** quand c'est possible.

| Signal | Outil BrandSearch | Confiance |
|---|---|---|
| Pubs actives actuelles | `get_brand` → `last_meta_active_count` ; par produit : `ads.active` (`get_market`) | OBSERVED |
| Pubs actives à J-7 / J-30 / J-90 | `suivi/snapshots.csv` si présent ; sinon `search_meta_ads` `brand_ids=<domaine>`, `ad_started_to=<J-x>`, `status=active` (borne basse : pubs lancées avant J-x et encore actives) | OBSERVED (snapshot) / INFERRED |
| Nouvelles pubs sur 7 / 30 / 90 j | `get_brand_ads_aggregates` avec `from_date`/`to_date` → `window.ad_count` ; compare avec la période précédente de même durée | OBSERVED |
| Duplications | `search_meta_ads` `brand_ids=<domaine>` `sort_by=duration` → `duplicate_count` des pubs ; `duplicate_count_min` pour filtrer | OBSERVED |
| Trafic actuel | `get_brand` → `monthly_visits` | ESTIMATED |
| Évolution du trafic | `suivi/snapshots.csv` (BrandSearch ne donne que la valeur actuelle) ; sinon UNKNOWN | ESTIMATED / UNKNOWN |
| Revenu estimé | `revenue` des cartes produit, `min_revenue`/`max_revenue` des marques | ESTIMATED |
| Évolution du revenu | `suivi/snapshots.csv` (BrandSearch ne donne que la valeur actuelle) ; sinon UNKNOWN | ESTIMATED / UNKNOWN |
| Dépense pub du marché produit (série quotidienne) | `get_market` `history=true` → `advertisers`, `est_daily_spend_usd` (données UE/UK) | ESTIMATED |
| Ancienneté boutique | `get_brand` → `created_at` (= entrée dans le catalogue, pas création réelle) ; WHOIS / date de la page Meta via WebSearch | INFERRED |
| Ancienneté produit | `first_advertised` de la carte produit | OBSERVED |
| Ancienneté des pubs | `search_meta_ads` `sort_by=duration` → plus vieille pub encore active | OBSERVED |
| Produit toujours vendu | `in_stock`, `last_advertised`, fiche produit accessible (WebFetch) | OBSERVED |

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
