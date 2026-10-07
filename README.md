# Recherche produit

Agent IA qui trouve des produits e-commerce à fort potentiel et **valide leur marché avec des
données réelles** avant de lancer une boutique.

## Comment ça marche

L'agent tourne dans Claude Code et s'appuie sur :
- **BrandSearch** : produits, pubs Meta/TikTok, boutiques concurrentes, revenus estimés ;
- **Shopify** : noms de marque, domaines, aperçus de boutique ;
- **la recherche web** : prix fournisseurs, tendances, réglementation.

Chaque produit passe par 4 étapes : **découvrir → filtrer → valider → noter sur 100**.

| Signal | Poids |
|---|---|
| Pubs qui scalent (pubs actives, nouvelles variantes, ancienneté) | 30 |
| Concurrents rentables (nombre de vendeurs, revenu estimé) | 25 |
| Marge et logistique (ratio prix / coût, poids, délai) | 25 |
| Tendance et demande (nouveauté, TikTok, recherches) | 20 |

≥ 70 : 🟢 lancer un test · 50–69 : 🟡 à tester · < 50 : 🔴 éviter.

## Utilisation

Dans Claude Code, à la racine de ce dépôt :

```
Utilise l'agent chasseur-produits : trouve-moi 5 produits gagnants dans la maison et la cuisine, pour la France, entre 25 et 60 €.
```

```
Valide ce produit avant que je le lance : https://exemple.com/products/xxx
```

Les rapports sont écrits dans `rapports/`.

## Structure

```
.claude/
  agents/chasseur-produits.md            # l'agent
  skills/recherche-produit/SKILL.md      # la méthode
  skills/recherche-produit/grille-scoring.md
rapports/                                # rapports générés
```

Pour ajuster les critères (seuils, poids, marchés par défaut), modifie `grille-scoring.md`
et la section « Paramètres par défaut » de `SKILL.md`.
