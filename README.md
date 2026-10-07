# Recherche produit

Agent IA qui identifie des **produits evergreen à haut potentiel pour créer une marque en
France**, à partir de preuves commerciales observées dans le **Big Four** (USA, UK, Canada,
Australie).

**Principe : la data tranche.** On cherche des anomalies positives dans les données et on
retient le produit qui cumule le plus de signaux indépendants. Aucun signal isolé ne suffit.

## Comment ça marche

L'agent tourne dans Claude Code et s'appuie sur :
- **BrandSearch** : produits, vendeurs d'un même produit, pubs Meta/TikTok, trafic, revenus estimés ;
- **Shopify** : noms de marque, domaines, aperçus de boutique ;
- **la recherche web** : fournisseurs, ancienneté des domaines, concurrents français.

Étapes : **découvrir (Big Four) → filtrer → signaux boutique sur plusieurs périodes → momentum →
validation multi-boutiques → longévité et Big Four → France → score /100**.

| Bloc | Poids |
|---|---|
| Evergreen / longévité | 20 |
| Momentum commercial | 25 |
| Validation multi-boutiques | 20 |
| Validation Big Four | 15 |
| Opportunité France | 15 |
| Marge / logistique | 5 |

90–100 : EXCEPTIONNEL · 80–89 : FORT POTENTIEL · 70–79 : À SURVEILLER · < 70 : NON PRIORITAIRE.

Chaque chiffre est étiqueté **OBSERVED / ESTIMATED / INFERRED / UNKNOWN**. Les données UNKNOWN
ne sont pas pénalisées : elles sortent du calcul et le rapport affiche la couverture data.

## Utilisation

Dans Claude Code, à la racine de ce dépôt :

```
Utilise l'agent chasseur-produits : trouve-moi 5 produits evergreen dans la maison et la cuisine, validés dans le Big Four, pour une marque en France.
```

```
Valide ce produit avant que je le lance en France : https://exemple.com/products/xxx
```

Les rapports sont écrits dans `rapports/`. Chaque recherche ajoute des mesures dans
`suivi/snapshots.csv` : plus on relance de recherches, plus les évolutions sur longue période sont
observées plutôt que déduites.

## Structure

```
.claude/
  agents/chasseur-produits.md              # l'agent
  skills/recherche-produit/SKILL.md        # la méthode
  skills/recherche-produit/signaux.md      # lecture des signaux, seuils, momentum, sources
  skills/recherche-produit/grille-scoring.md
rapports/                                  # rapports générés
suivi/snapshots.csv                        # historique des mesures par boutique
```

Pour ajuster les critères (seuils, poids), modifie `signaux.md` et `grille-scoring.md`.
