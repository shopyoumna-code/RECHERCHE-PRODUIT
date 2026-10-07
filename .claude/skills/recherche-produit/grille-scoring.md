# Product Score /100

| Bloc | Points |
|---|---|
| Evergreen / longévité | 20 |
| Momentum commercial | 25 |
| Validation multi-boutiques | 20 |
| Validation Big Four | 15 |
| Opportunité France | 15 |
| Marge / logistique | 5 |

Les seuils de lecture des signaux sont dans [signaux.md](signaux.md).

## 1. Evergreen / longévité : 20 points

| Critère | Points |
|---|---|
| Ancienneté de la preuve commerciale encore vivante : 24+ mois → 12 · 12–24 → 10 · 6–12 → 7 · 3–6 → 4 · < 3 → 1 | 12 |
| Survie : part des vendeurs encore actifs parmi ceux ayant annoncé il y a 6+ mois : ≥ 60 % → 5 · 30–60 % → 3 · < 30 % → 0 | 5 |
| Pas de pic court (série `get_market` régulière ou croissante, pas en cloche) → 3 | 3 |

## 2. Momentum commercial : 25 points

| Critère | Points |
|---|---|
| Classe du meilleur vendeur indépendant : ACCELERATING → 8 · GROWING → 6 · STABLE → 3 · DECLINING → 0 | 8 |
| Nouvelles créatives 30 j (meilleurs vendeurs) en hausse vs période précédente → 5 · stables → 2 · en baisse → 0 | 5 |
| Duplications : pubs dupliquées (≥ 3 copies) parmi les plus anciennes actives → 4 · quelques-unes → 2 · aucune → 0 | 4 |
| Trafic du meilleur vendeur : en hausse → 4 · stable → 2 · en baisse → 0 | 4 |
| Niveau de pubs actives sur le produit (tous vendeurs) : 50+ et en croissance → 4 · 30–50 → 3 · 10–30 → 2 · < 10 → 0 | 4 |

## 3. Validation multi-boutiques : 20 points

| Critère | Points |
|---|---|
| Boutiques indépendantes vendant le produit (ou un équivalent fonctionnel) : 10+ → 6 · 5–9 → 5 · 3–4 → 3 · 2 → 1 · 1 → 0 | 6 |
| Boutiques à momentum positif (ACCELERATING ou GROWING) : 4+ → 8 · 3 → 6 · 2 → 4 · 1 → 1 | 8 |
| Taux d'arrêt (arrêtées ÷ total) : < 25 % → 6 · 25–50 % → 3 · > 50 % → 0 | 6 |

## 4. Validation Big Four : 15 points

| Critère | Points |
|---|---|
| Marchés (USA, UK, Canada, Australie) avec plusieurs signaux positifs : 4 → 10 · 3 → 8 · 2 → 5 · 1 → 2 | 10 |
| Au moins un marché avec 3+ vendeurs performants → 3 | 3 |
| Prix cohérents entre marchés (écart < 30 %) → 2 | 2 |

## 5. Opportunité France : 15 points

| Critère | Points |
|---|---|
| Concurrents DTC français actifs : 1–5 → 7 · 6–10 → 4 · 0 → 3 · 11+ → 1 | 7 |
| Preuves commerciales en France (au moins un vendeur FR avec pubs actives depuis 30+ j ou dépense UE réelle) → 5 · indices faibles → 2 · aucune → 0 | 5 |
| Concurrents FR en dessous du Big Four (moins de pubs, angles moins travaillés, prix plus hauts) → 3 | 3 |

« 0 concurrent » ne rapporte que 3 points sur 7 : l'absence n'est pas une preuve.

## 6. Marge / logistique : 5 points

| Critère | Points |
|---|---|
| Ratio prix de vente FR / coût rendu (produit + livraison) ≥ 3 → 3 · 2,5–3 → 2 · 2–2,5 → 1 · < 2 → 0 | 3 |
| Léger (< 1 kg), non fragile, fournisseur avec livraison ≤ 10 j vers la France → 2 | 2 |

## Données UNKNOWN : jamais pénalisées automatiquement

Un critère dont la donnée est UNKNOWN **n'est ni noté 0 ni noté au maximum** : il est retiré du
calcul.

```
Score = (points obtenus ÷ points évaluables) × 100
Couverture = points évaluables ÷ 100
```

- Affiche toujours le score **et** la couverture : « 82/100 (couverture 75 %) ».
- Couverture < 60 % : le score est indicatif ; liste les données à obtenir avant de décider.
- Une donnée ESTIMATED ou INFERRED est notée normalement, mais son étiquette reste visible.

## Verdict

| Score | Verdict |
|---|---|
| 90–100 | **EXCEPTIONNEL** |
| 80–89 | **FORT POTENTIEL** |
| 70–79 | **À SURVEILLER** |
| < 70 | **NON PRIORITAIRE** |

Garde-fous :
- Un produit éliminé à l'étape 2 (filtres éliminatoires) est NON PRIORITAIRE quel que soit son score.
- Un produit vendu par **une seule boutique** ne peut pas dépasser À SURVEILLER.
- Aucun verdict au-dessus de À SURVEILLER sans **au moins 4 signaux positifs indépendants**
  (par exemple : longévité, plusieurs vendeurs, momentum, plusieurs marchés du Big Four).
