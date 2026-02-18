# Zoho Inventory - Commandes

## Commandes de Vente (Sales Orders)

### Cycle de Vie
```
Devis (Quote) → Commande de Vente (SO) → Bon de Livraison → Facture → Paiement
```

### Créer une Commande de Vente
```
Numéro : SO-0001
Date : 18/02/2026
Client : Entreprise ABC
Référence client : PO-EXT-789

Lignes :
| Article          | SKU       | Qté | Prix HT | Total HT |
|-----------------|-----------|-----|---------|----------|
| T-shirt Basique | TSH-001   | 50  | 12.00€  | 600.00€  |
| Casquette       | CAS-001   | 20  | 10.00€  | 200.00€  |

Sous-total : 800.00€
Remise (5%) : -40.00€
TVA (20%) : 152.00€
Total TTC : 912.00€

Entrepôt source : Paris
Date de livraison prévue : 25/02/2026
```

### Statuts d'une Commande de Vente
| Statut | Description |
|--------|-------------|
| Brouillon | Non confirmée |
| Confirmée | Validée, en attente de traitement |
| Emballée | Articles préparés (packing slip) |
| Expédiée | Colis remis au transporteur |
| Livrée | Réception confirmée |
| Facturée | Facture émise |
| Annulée | Commande annulée |

### Expédition
```
Commande : SO-0001
Colis 1/1 :
  Transporteur : Colissimo
  N° suivi : 8R12345678901
  Poids : 3.5 kg
  Dimensions : 40x30x20 cm
  Date expédition : 20/02/2026
  
Notification automatique au client : ✅
Lien de suivi envoyé : ✅
```

## Commandes d'Achat (Purchase Orders)

### Cycle de Vie
```
Demande d'achat → Bon de Commande (PO) → Réception → Facture Fournisseur
```

### Créer un Bon de Commande
```
Numéro : PO-0015
Date : 18/02/2026
Fournisseur : Textile Import SARL
Entrepôt de réception : Paris

Lignes :
| Article          | SKU     | Qté | Prix achat | Total    |
|-----------------|---------|-----|-----------|----------|
| T-shirt Basique | TSH-001 | 500 | 8.50€     | 4250.00€ |
| Casquette       | CAS-001 | 300 | 5.00€     | 1500.00€ |

Total HT : 5750.00€
Date de livraison attendue : 10/03/2026
```

### Réception Partielle
```
PO-0015 — Réception 1/2 :
  T-shirt Basique : 300/500 reçus ✅
  Casquette : 300/300 reçus ✅

PO-0015 — Réception 2/2 :
  T-shirt Basique : 200/500 reçus ✅ (total complété)
```

## Transferts entre Entrepôts

```
Transfert : TR-0008
De : Entrepôt Paris
Vers : Entrepôt Lyon
Date : 18/02/2026

Articles transférés :
| Article          | SKU     | Qté |
|-----------------|---------|-----|
| T-shirt Basique | TSH-001 | 100 |
| Jean Slim       | JEA-001 | 50  |

Statut : En transit
Transporteur : Chronopost
N° suivi : FR98765432
```

## Ajustements de Stock

### Types d'Ajustements
```
Ajustement positif (entrée) :
  - Inventaire physique supérieur au système
  - Retour produit
  - Production interne

Ajustement négatif (sortie) :
  - Inventaire physique inférieur au système
  - Casse / perte
  - Échantillons / dons
  - Vol / disparition
```

### Exemple d'Ajustement
```
Référence : ADJ-0012
Date : 18/02/2026
Raison : Inventaire annuel
Compte d'ajustement : Écart d'inventaire

| Article          | Stock système | Stock réel | Ajustement |
|-----------------|--------------|-----------|------------|
| T-shirt Basique | 150          | 147       | -3         |
| Casquette       | 200          | 203       | +3         |
```

## Documents Générés

| Document | Description | Envoyable |
|----------|-------------|-----------|
| Devis | Proposition commerciale | ✅ Client |
| Commande de vente | Confirmation de commande | ✅ Client |
| Bon de livraison | Liste des articles expédiés | ✅ Client |
| Packing slip | Liste de colisage (interne) | Non |
| Bon de commande | Commande au fournisseur | ✅ Fournisseur |
| Bon de réception | Confirmation de réception | Non |

## Bonnes Pratiques

1. **Toujours confirmer les commandes** avant traitement
2. **Utiliser les réceptions partielles** pour les livraisons fractionnées
3. **Faire des inventaires réguliers** et ajuster les écarts
4. **Configurer les alertes de stock bas** sur les articles critiques
5. **Automatiser les bons de commande** via les points de réapprovisionnement
