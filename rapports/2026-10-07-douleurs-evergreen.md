# Produits classés par douleur humaine evergreen (40–100 €)

*Date : 7 octobre 2026 · Source : BrandSearch*

## Méthode

Les recherches précédentes partaient des niches ou des boutiques, puis cherchaient la France en dernier.
Cette fois, l'entonnoir est inversé :

1. **France d'abord** : `search_products` avec `market_country=FR` (produits dont les pubs sont
   diffusées en France), prix 43–108 $, au moins 80 pubs actives, encore annoncés depuis le
   30/09/2026, tri par nouvelles pubs sur 30 jours. Cela donne 535 produits ; j'ai lu les 75 premiers.
   La demande française est donc prouvée dès le départ.
2. **Tri par douleur** : chaque produit est classé dans une douleur. Sont écartés :
   - les compléments ;
   - les produits à taille ;
   - les produits déjà refusés ;
   - les produits qui ne résolvent aucune douleur (parfum, déco, bijou…).
3. **Preuve** : pour les survivants, je vérifie :
   - la courbe de pubs lancées par période (`get_brand_ads_aggregates`, OBSERVED) ;
   - les autres vendeurs (`get_market`) ;
   - les concurrents français nommés.

Une douleur vaut d'autant plus qu'elle est fréquente, récurrente, émotionnelle **et** qu'elle touche
plusieurs besoins à la fois. La colonne « Besoins touchés » le montre.

## Résultat

| # | Douleur | Produit | Besoins touchés | Boutique qui scale | Concurrents FR nommés | Verdict |
|---|---|---|---|---|---|---|
| 1 | Feu du rasoir, poils incarnés, coût des lames | Rasoir de sûreté métal (59 €) | Apparence, douleur, argent | [Le Lamier](https://lelamier.com/products/lamier) | [Elios](https://elios-shop.com/products/rasoir-5-en-1), [Shavest](https://tryshavest.com/products/rasoir-de-surete-shavest-noir-mat), [Thomyle](https://thomyle.com/products/rasoir-electrique-pour-homme-le-fidele) | **Passe tous les critères** |
| 2 | Se réveiller sans réveiller l'autre ; sommeil trop profond | Réveil vibrant (45 $) | Sommeil, couple, stress | [Fitsleeps](https://fitsleeps.com/products/premium-vibration-alarm) | [Taqavibe](https://taqavibe.fr/products/wake-le-reveil-silencieux), [KOVA](https://my-kova.com/products/reveil-vibrant-intelligent-reveillez-vous-sans-reveiller-les-autres), [Vibraya](https://vibraya.store/products/shadow-matte) | **Passe tous les critères** |
| 3 | Tartre et mauvaise haleine du chien, détartrage véto coûteux | Spray anti-tartre / détartreur à ultrasons | Responsabilité animal, argent, hygiène | [Paw Guardian](https://paw-guardian.com/products/anti-tartar-spray-dogs-cats), [Canivet](https://trycanivet.com/products/canivet-kit-silent-ultrasonic-plaque-tartar-remover-for-dogs) | [Vitamii](https://vitamii.fr/products/poudre-dentaire) (poudre, 24,90 €) | **À valider** : un seul concurrent FR, sur un format moins cher |
| 4 | Dentier ou gouttière sale, odeur, honte | Nettoyeur à ultrasons + UV (69,99 €) | Hygiène, honte sociale, autonomie (seniors) | [Lyra](https://lyra-officiel.fr/products/dentaclean-nettoyeur-ultrason) (FR) | Lyra seulement | **À valider** : 2e concurrent FR et vendeur Big Four non trouvés |

---

## 1. Rasoir de sûreté : feu du rasoir, poils incarnés, budget lames

**Pourquoi c'est une bonne douleur** : quotidienne, récurrente (les lames se rachètent), visible
(boutons, rougeurs) et chiffrable (le prix d'un paquet de cartouches).

**Courbe de [Le Lamier](https://lelamier.com/products/lamier) (OBSERVED)**

| 2026 | Avr | Mai | Juin | Juil | Août | Sept |
|---|---|---|---|---|---|---|
| Nouvelles pubs | 174 | 167 | 211 | **370** | 299 | **361** |
| Dépense pub UE | 99 k€ | 147 k€ | 139 k€ | **176 k€** | **176 k€** | 49 k€* |

\* Septembre est incomplet : la dépense des pubs récentes remonte avec retard.

- Fiche produit : 1 968 pubs actives et 857 nouvelles pubs sur 30 jours (OBSERVED).
- Dépense quotidienne estimée du marché : 44 000 $ (ESTIMATED).

**Concurrents français**

| Boutique | Prix | Pubs actives |
|---|---|---|
| [Elios](https://elios-shop.com/products/rasoir-5-en-1) | 39,90 € | 415 |
| [Shavest](https://tryshavest.com/products/rasoir-de-surete-shavest-noir-mat) | 38,90 € | 331 |
| [Thomyle](https://thomyle.com/products/rasoir-electrique-pour-homme-le-fidele) | 49,90 € | — |

**Big Four** : [Leaf Shave](https://leafshave.com/products/leaf-two-razor) (USA, 86 $) et
[Henson](https://hensonshaving.com) (Canada).

**Angle libre** : les femmes (jambes, maillot, poils incarnés). Tous les concurrents FR parlent aux hommes.

**Point faible** : trafic de Le Lamier −52 % sur 1 mois (ESTIMATED), à vérifier sur 3 et 6 mois.

## 2. Réveil vibrant : se réveiller sans réveiller l'autre

**Pourquoi c'est une bonne douleur** :
- tous les jours ;
- touche le couple : le réveil de l'un réveille l'autre ;
- touche les horaires décalés et les gros dormeurs (TDAH, malentendants).

**Courbe de [Fitsleeps](https://fitsleeps.com/products/premium-vibration-alarm)** : nouvelles pubs par
mois, d'avril à septembre (OBSERVED).

| 2026 | Avr | Mai | Juin | Juil | Août | Sept |
|---|---|---|---|---|---|---|
| Nouvelles pubs | 672 | 427 | 635 | 1 936 | 792 | **3 630** |

8 816 pubs actives sur le produit.

**Big Four** : plusieurs vendeurs. Cela corrige la limite du rapport précédent, qui ne connaissait que Fitsleeps.

| Vendeur | Positionnement | Prix | Pubs actives | Nouvelles pubs sur 30 j |
|---|---|---|---|---|
| [Fitsleeps](https://fitsleeps.com/products/premium-vibration-alarm) | Généraliste | 44,95 $ | 8 816 | — |
| [Rise Band](https://risebands.com/products/deaf-band) | Sourds et malentendants | 45 $ | 1 072 | 539 |
| [Dawn Bands](https://dawnbands.com/products/wake-up-band-adhd) | Dormeurs TDAH | 44,99 $ | 330 | — |

**Concurrents français** :

| Boutique | Prix | Pubs actives |
|---|---|---|
| [Taqavibe](https://taqavibe.fr/products/wake-le-reveil-silencieux) | 29,90 € | 139 |
| [KOVA](https://my-kova.com/products/reveil-vibrant-intelligent-reveillez-vous-sans-reveiller-les-autres) | 79,90 $ | — |
| [Vibraya](https://vibraya.store/products/shadow-matte) | 44,99 € | — |

**Angles libres** : les deux angles qui marchent aux USA ne sont pas pris en France :
- malentendants (Rise Band) ;
- TDAH (Dawn Bands).

Les 3 concurrents FR vendent tous « sans réveiller l'autre ».

## 3. Hygiène dentaire du chien : tartre, haleine, facture véto

**Pourquoi c'est une bonne douleur** :
- **responsabilité** envers l'animal ;
- **argent** : un détartrage chez le vétérinaire coûte cher ;
- **hygiène** et **honte** : mauvaise haleine.

Le problème est récurrent : le tartre revient.

**Courbes Big Four (OBSERVED, nouvelles pubs par tranche de 2 mois)**

| Boutique | Produit | Prix | Avr–Mai | Juin–Juil | Août–Sept |
|---|---|---|---|---|---|
| [Paw Guardian](https://paw-guardian.com/products/anti-tartar-spray-dogs-cats) (USA) | Spray anti-tartre | 55,99 $ | 39 | 219 | **238** |
| [Canivet](https://trycanivet.com/products/canivet-kit-silent-ultrasonic-plaque-tartar-remover-for-dogs) (USA) | Détartreur à ultrasons | 99,90 $ | — | 153 | **547** |

- **Paw Guardian** : 1 742 pubs actives. 41 k€ de dépense UE en avril–mai, diffusée au UK.
- **Canivet** : 1 431 pubs actives, 5 vendeurs sur ce marché.

**France** : seul [Vitamii](https://vitamii.fr/products/poudre-dentaire) (poudre dentaire, 24,90 €,
79 pubs actives) attaque ce problème. Aucun vendeur FR du spray ni du détartreur entre 40 et 100 €
n'a été trouvé.

**Verdict : à valider.** La demande française est prouvée, mais sur un format moins cher. Elle ne
l'est pas encore sur le produit lui-même.

## 4. Nettoyeur de dentier ou gouttière : hygiène et honte

**Pourquoi c'est une bonne douleur** :
- **hygiène quotidienne** ;
- **honte** : odeur, dentier taché ;
- **autonomie** des seniors.

Cible large : porteurs de prothèses, de gouttières d'alignement et de gouttières de bruxisme.

**Courbe de [Lyra](https://lyra-officiel.fr/products/dentaclean-nettoyeur-ultrason)** (boutique
française, 69,99 €), nouvelles pubs par tranche de 2 mois (OBSERVED) :

| Période | Avr–Mai | Juin–Juil | Août–Sept |
|---|---|---|---|
| Nouvelles pubs | 271 | 465 | **739** |

- **Pubs** : 1 212 actives, diffusées en France, en Belgique et au Luxembourg.
- **Panier** : Lyra vend aussi une crème adhésive pour dentier à 29,99 €, ce qui fait monter le
  panier moyen.

**Verdict : à valider.** Lyra est la seule boutique trouvée. Il manque :
- un 2e concurrent français ;
- un vendeur Big Four.

---

## Écartés à cette passe (douleur réelle, mais critère raté)

| Produit | Douleur | Critère raté |
|---|---|---|
| [Cellsius](https://cellsius-shop.com/products/le-coussin-orthopedique) (coussin genoux) | Douleur, sommeil | 39,90 € (sous 40 €) |
| [Inolyo](https://inolyo.com/products/sangle-rotulienne-inolyo) (sangle rotulienne) | Douleur au genou | 39,90 € |
| [Bebysh](https://1uy2y7-t6.myshopify.com/products/bebysh) (dissolvant poils d'animaux) | Corvée | 39,90 € |
| [FisioRest](https://artuvate.co/products/fisiorest) (support cervical) | Nuque | 7 vendeurs sur 8 ont arrêté leurs pubs |
| [Airquit](https://airquit.shop/products/journey-pack) (arrêt du tabac) | Santé, argent | Un seul vendeur, aucun concurrent FR vérifié |
| [Spotminders](https://spotminders.com/products/spotminders-tracking-cards) (carte traceur) | Perte, stress | Un seul vendeur |
| Oreiller cervical, coussin de voyage, fontaine à eau, poêle titane | — | Déjà refusés |
| Gouttes lymphatiques, compléments, leggings, vêtements | — | Compléments ou tailles |

## Prochaine étape proposée

Lancer **un seul** de ces deux produits : **rasoir de sûreté** ou **réveil vibrant**. Ce sont les deux
seuls qui passent tous les critères. Pour le choix, compare :
- le coût fournisseur ;
- les courbes de trafic sur 3 et 6 mois dans l'interface BrandSearch (non accessibles via le connecteur).
