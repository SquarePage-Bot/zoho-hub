# Zoho Inventory - Gestion des Articles

## Types d'Articles

### Article Simple
Produit individuel avec un seul SKU.

```
Nom : T-shirt Basique
SKU : TSH-BAS-001
Prix de vente : 19.99€
Prix d'achat : 8.50€
Unité : Pièce
Taxe : TVA 20%
```

### Article avec Variantes
Produit décliné en plusieurs options (taille, couleur, etc.).

```
Article parent : T-shirt Basique
Attributs :
  - Taille : S, M, L, XL
  - Couleur : Blanc, Noir, Bleu

Variantes générées :
  TSH-BAS-S-BLC  → T-shirt Basique S Blanc    (Stock: 45)
  TSH-BAS-M-BLC  → T-shirt Basique M Blanc    (Stock: 120)
  TSH-BAS-L-BLC  → T-shirt Basique L Blanc    (Stock: 85)
  TSH-BAS-S-NOR  → T-shirt Basique S Noir     (Stock: 60)
  ...
```

### Article Composite (Kit / Bundle)
Produit composé de plusieurs articles.

```
Article composite : Pack Bureau Complet
Composants :
  - 1x Bureau en bois (BUR-001) → 199€
  - 1x Chaise ergonomique (CHA-001) → 149€
  - 1x Lampe LED (LAM-001) → 39€

Prix du pack : 349€ (au lieu de 387€)
Le stock est calculé selon le composant le plus limitant
```

### Article Groupé
Ensemble d'articles vendus ensemble mais gérés séparément.

```
Groupe : Offre Rentrée
Articles :
  - Cahier A4 (x5)
  - Stylos (x10)
  - Trousse (x1)
Chaque article conserve son propre stock
```

## Informations d'un Article

### Champs Principaux
| Champ | Description | Obligatoire |
|-------|-------------|-------------|
| Nom | Nom de l'article | ✅ |
| SKU | Référence unique | ✅ |
| Unité | Pièce, kg, litre, etc. | ✅ |
| Prix de vente | Prix HT de vente | Non |
| Prix d'achat | Coût d'achat | Non |
| Compte de vente | Compte comptable vente | Non |
| Compte d'achat | Compte comptable achat | Non |
| Taxe | Taux de TVA applicable | Non |
| Description | Description pour devis/factures | Non |

### Suivi des Stocks
```
Stock disponible : 150 unités
Stock engagé : 30 unités (commandes en cours)
Stock réel : 180 unités
Stock en commande : 200 unités (PO en attente)

Point de réapprovisionnement : 50 unités
Quantité de réapprovisionnement : 200 unités
```

### Numéros de Série
Suivi individuel de chaque unité :
```
Article : Laptop Pro 15"
Numéros de série :
  LP15-2026-00001 → Vendu (Commande SO-0042)
  LP15-2026-00002 → En stock (Entrepôt Paris)
  LP15-2026-00003 → En stock (Entrepôt Lyon)
```

### Lots (Batches)
Suivi par lot de production :
```
Article : Crème hydratante 50ml
Lot : LOT-2026-02-A
  Quantité : 500
  Date fabrication : 01/02/2026
  Date expiration : 01/02/2028
  Entrepôt : Central
```

## Gestion des Prix

### Listes de Prix
Créer des tarifs différents par segment client :
```
Liste "Tarif Public" :
  T-shirt Basique → 19.99€

Liste "Tarif Revendeur" :
  T-shirt Basique → 12.00€ (-40%)

Liste "Tarif VIP" :
  T-shirt Basique → 16.99€ (-15%)
```

### Règles de Remise
```
Remise par quantité :
  1-10 unités → Prix standard
  11-50 unités → -10%
  51-100 unités → -15%
  100+ unités → -20%
```

## Images et Fichiers

- Jusqu'à **15 images** par article
- Image principale affichée dans les documents
- Pièces jointes : fiches techniques, certificats

## Import / Export

### Import CSV
```csv
Item Name,SKU,Unit,Selling Price,Purchase Price,Opening Stock,Reorder Level
"T-shirt Basique","TSH-001","pcs",19.99,8.50,150,50
"Jean Slim","JEA-001","pcs",49.99,22.00,80,30
"Casquette","CAS-001","pcs",14.99,5.00,200,75
```

### Export
Formats disponibles : CSV, XLS, PDF
Filtres : par catégorie, statut de stock, entrepôt

## Bonnes Pratiques

1. **Convention SKU cohérente** : CAT-TYPE-NUMERO (ex: TSH-BAS-001)
2. **Définir les seuils de réapprovisionnement** pour chaque article
3. **Utiliser les catégories** pour organiser le catalogue
4. **Mettre à jour les coûts d'achat** régulièrement
5. **Activer le suivi par numéro de série** pour les articles à forte valeur
