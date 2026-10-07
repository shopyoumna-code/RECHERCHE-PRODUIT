# Boutiques monoproduit en plein scaling (40–100 €)

*Date : 7 octobre 2026 · Source : BrandSearch*

## Ce qui change par rapport au rapport précédent

On part des **boutiques** qui montent, comme sur la capture de référence (courbe qui monte, centaines de
pubs, gros trafic, activité depuis fin 2025), au lieu de partir des produits. Les filtres ne sont pas
appliqués en bloc : chaque variante en relâche un et en serre un autre.

| # | Variante de filtres (`search_brands`) | Boutiques trouvées |
|---|---|---|
| 1 | Big Four, ≥ 100 pubs actives, ≥ 30 k visites, ≤ 15 produits, tri croissance trafic 1 mois | 434 |
| 2 | Boutiques françaises, ≥ 80 pubs actives | 27 |
| 3 | Big Four, ≥ 150 pubs, ≥ 50 k visites, ≤ 30 produits, tri croissance des pubs 30 j | 449 |
| 4 | Livre en France, tri croissance trafic 1 mois | 423 |
| 5 | Livre en France, ≤ 12 produits, hors compléments et mode, tri dépense pub 30 j | 174 |
| 6 | Big Four, ≤ 12 produits, hors compléments et mode, tri croissance trafic | 102 |
| 7 | Boutiques françaises, ≥ 150 pubs, ≥ 40 k visites, ≤ 8 produits, tri dépense pub | 10 |
| 8 | Livre en France, ≥ 200 pubs, ≥ 50 k visites, ≤ 6 produits, tri croissance des pubs | 41 |
| 9 | Big Four **et** livre en France, ≥ 300 pubs, ≥ 80 k visites, ≤ 10 produits, tri dépense pub | 27 |

Pour chaque boutique retenue, la **courbe** est construite avec `get_brand_ads_aggregates` :
nombre de nouvelles pubs lancées et dépense pub UE par période (OBSERVED). La dépense n'existe que
pour les pubs diffusées en UE/UK. Pour une boutique qui ne vise que les USA, elle vaut 0 : c'est une
absence de donnée (UNKNOWN), pas une absence de dépense.

## Résumé

| Boutique | Produit | Prix | Courbe des pubs lancées | Concurrents FR nommés | Verdict |
|---|---|---|---|---|---|
| [Le Lamier](https://lelamier.com/products/lamier) | Rasoir de sûreté métal | 59 € | 174 → 167 → 211 → **370** → 299 → **361** /mois | [Elios](https://elios-shop.com/products/rasoir-5-en-1), [Shavest](https://tryshavest.com/products/rasoir-de-surete-shavest-noir-mat), [Thomyle](https://thomyle.com/products/rasoir-electrique-pour-homme-le-fidele) | **Candidat n° 1** |
| [Fitsleeps](https://fitsleeps.com/products/premium-vibration-alarm) | Réveil vibrant | 44,95 $ | 672 → 427 → 635 → 1 936 → 792 → **3 630** /mois | [Taqavibe](https://taqavibe.fr/products/wake-le-reveil-silencieux), [KOVA](https://my-kova.com/products/reveil-vibrant-intelligent-reveillez-vous-sans-reveiller-les-autres), [Vibraya](https://vibraya.store/products/shadow-matte) | **À tester** |
| [She's Birdie](https://shesbirdie.com/products/the-complete-safety-bundle) | Alarme de sécurité personnelle | 94–99 $ (pack) | 707 → 1 462 → **3 750** /2 mois | Aucun trouvé | À surveiller |
| [Brick](https://getbrick.com/products/grey-brick) | Boîtier anti-addiction téléphone | 47–64 $ | 768 → 1 836 → **2 235** /2 mois | Aucun trouvé | À surveiller |
| [Nesti](https://nestishop.com/products/nesti-pod) | Cocon sensoriel enfant | 49,99 £ | 486 → 688 → **1 269** /2 mois | Aucun trouvé | À surveiller |

---

## 1. Le Lamier : rasoir de sûreté (candidat n° 1)

- **Lien** : [lelamier.com/products/lamier](https://lelamier.com/products/lamier)
- **Prix** : 59 €. Site en français, boutique enregistrée aux USA.
- **Problème résolu** : feu du rasoir, poils incarnés, coût des lames jetables (récurrent).
- **Pas de taille.** Lames DE standard : consommable à racheter.

### Courbe (OBSERVED)

| Mois 2026 | Avril | Mai | Juin | Juillet | Août | Septembre |
|---|---|---|---|---|---|---|
| Nouvelles pubs lancées | 174 | 167 | 211 | **370** | 299 | **361** |
| Dépense pub UE | 99,5 k€ | 147 k€ | 139 k€ | **176 k€** | **176 k€** | 49 k€* |

\* Septembre est incomplet : la dépense des pubs récentes est encore en cours de remontée.

- D'avril à juin : environ 184 pubs par mois. De juillet à septembre : environ 343, soit **×1,9**.
- La dépense mensuelle UE a été multipliée par 1,8 entre avril et juillet-août.

### Autres signaux

| Indicateur | Valeur | Confiance |
|---|---|---|
| Pubs actives | 549, soit +31 % sur 30 j et +41 % sur 7 j | OBSERVED |
| Dépense pub UE sur 30 j | 114 k€ (333 k€ sur 60 j) | ESTIMATED |
| Trafic | 300 867 visites/mois, −52 % sur 1 mois | ESTIMATED |

### Concurrents français (OBSERVED)

| Boutique | Prix | Pubs actives | Revenu du marché |
|---|---|---|---|
| [Elios](https://elios-shop.com/products/rasoir-5-en-1) | 39,90 € | 415 | 420–560 k$/mois (ESTIMATED) |
| [Shavest](https://tryshavest.com/products/rasoir-de-surete-shavest-noir-mat) | 38,90 € | 331 | 110–140 k$/mois (ESTIMATED) |
| [Thomyle](https://thomyle.com/products/rasoir-electrique-pour-homme-le-fidele) | 49,90 € | — | — |

### Vendeurs Big Four

- [Leaf Shave](https://leafshave.com/products/leaf-two-razor) (USA) : 86 $, 293 pubs actives.
- [Henson Shaving](https://hensonshaving.com) (Canada).

### Points de vigilance

- **Trafic en baisse sur 1 mois** (−52 %, ESTIMATED) alors que les pubs montent. C'est peut-être un
  creux après le pic de l'été. Vérifie la courbe de trafic sur 3 et 6 mois dans l'interface BrandSearch.
- Le Lamier vend à 59 €, Elios et Shavest sous 40 €. Pour tenir 59 €, la promesse doit être plus
  premium (acier, garantie à vie, kit de lames inclus).

### Angles possibles

1. **Femmes** : jambes et maillot, contre les poils incarnés. Les concurrents ne visent que les hommes.
2. **Peau sensible / feu du rasoir** : angle problème pur.
3. **Économie** : coût sur un an comparé aux cartouches Gillette.

---

## 2. Fitsleeps : réveil vibrant (à tester)

La fiche complète est dans [le rapport précédent](2026-10-07-monoproduit-debutant-40-100.md).

- Courbe mensuelle : 672 → 427 → 635 → 1 936 → 792 → **3 630** nouvelles pubs.
- 8 816 pubs actives sur le produit.
- 3 concurrents FR actifs, dont Taqavibe à 29,90 €.

**Limite** : Fitsleeps est le seul gros vendeur dans le Big Four.

---

## 3. Courbes très fortes, sans concurrent français trouvé

Ces boutiques ont exactement le profil de la capture : courbe qui monte fort, des milliers de pubs.
**Mais je n'ai trouvé aucun concurrent français actif.** Ta règle les plafonne donc à À SURVEILLER.
Je les montre pour que tu décides.

### She's Birdie : alarme de sécurité personnelle (USA)

- **Liens** :
  - [pack complet, 98,95 $](https://shesbirdie.com/products/the-complete-safety-bundle) ;
  - [alarme seule, 31,95 $](https://shesbirdie.com/products/birdie-personal-safety-alarm-3-0).
- **Problème résolu** : se sentir en sécurité en marchant seule, en courant, à la fac (Maslow : sécurité).
- **Courbe des nouvelles pubs par tranche de 2 mois** (OBSERVED) :
  - avril–mai : 707 ;
  - juin–juillet : 1 462 ;
  - août–septembre : **3 750**.

  C'est **×5,3** en 6 mois.
- **Autres signaux** :
  - 3 149 pubs actives, soit +272 % sur 30 j (OBSERVED) ;
  - 104 k visites/mois, +28 % sur 1 mois (ESTIMATED).
- **Dépense** : UNKNOWN. La boutique ne diffuse qu'aux USA.
- **Limite** : l'alarme seule coûte 31,95 $. Pour entrer dans 40–100 €, il faut vendre le **pack**
  (2 à 4 alarmes, ou alarme + accessoires), comme le fait déjà She's Birdie.

### Brick : boîtier anti-addiction au téléphone (USA)

- **Lien** : [getbrick.com/products/grey-brick](https://getbrick.com/products/grey-brick)
- **Courbe par tranche de 2 mois** (OBSERVED) :

  | Période | Nouvelles pubs | Dépense UE |
  |---|---|---|
  | 1re tranche | 768 | 57 k€ |
  | 2e tranche | 1 836 | 66 k€ |
  | 3e tranche | **2 235** | **212 k€** |

- **Trafic** : 5,7 M visites/mois (ESTIMATED).
- **Copie européenne** : [Tap Out](https://tapoutclub.com) (Pays-Bas).
  - Courbe : 214 → 384 → **741** pubs.
  - Prix : 36,6 $, sous 40 €.

### Nesti : cocon sensoriel pour enfant (UK)

- **Lien** : [nestishop.com/products/nesti-pod](https://nestishop.com/products/nesti-pod)
- **Prix** : 49,99 £.
- **Courbe des nouvelles pubs par tranche de 2 mois** (OBSERVED) : 486 → 688 → **1 269**.
- **Dépense UE** : 92 k€ → 121 k€ → 73 k€.
- **Pubs actives** : 1 578. Déjà diffusé en France : 593 pubs en août–septembre.

---

## Écartées à cette passe

| Boutique | Données | Raison |
|---|---|---|
| [Coco Seat](https://cocoseat.com) (housse de caddie bébé, 47 $) | 159 → 177 → 108 pubs par tranche de 2 mois | Courbe en baisse |
| [Lunex](https://lunex-officiel.com) (couette rafraîchissante, 89,95 €) | 398 k visites ; pubs par mois : 86 / 313 / 10 / 12 / 12 / 439 | Saisonnier, irrégulier |
| [Lyphéa](https://lyphea.com/products/drainant-lymphatique®), Nuria, Mayaverra (gouttes « drainage lymphatique ») | Lyphéa : 721 pubs, 162 k visites, 55 k€/30 j | Quasi-complément avec allégations santé ; Nuria et Mayaverra sous 40 € |
| [Lovy](https://trylovy.com) (UroControl) | 697 pubs, +202 % | Complément à 29,99 $ |
| [Exeo](https://exeoathletics.com) (ceinture de course) | 30 → 203 → 125 pubs | Pas de scaling |
| [KLIK](https://klikcamera.com) (appareil photo enfant) | 1 855 pubs, +112 % | Aucun concurrent FR (déjà écarté) |
| [Polar Haircare](https://polarhaircare.com) (shampoing colorant) | 651 pubs, −19 % sur 30 j | Déclin |

## Prochaines étapes

1. **Le Lamier** : ouvre dans BrandSearch la courbe de trafic sur 3 et 6 mois de lelamier.com, d'Elios
   et de Shavest, puis commande un échantillon fournisseur.
2. **She's Birdie, Brick, Nesti** : dis-moi si tu acceptes un produit sans concurrent français mais avec
   une courbe aussi forte. Sinon, je les garde en veille dans `suivi/snapshots.csv`.
3. Continuer les pages 2 et 3 des variantes 1, 3 et 4.
