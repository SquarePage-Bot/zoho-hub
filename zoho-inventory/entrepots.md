# Zoho Inventory - Entrepôts et Gestion des Stocks

## Configuration des Entrepôts

### Créer un Entrepôt
```
Nom : Entrepôt Paris
Code : WH-PAR
Adresse : 15 rue de la Logistique, 75012 Paris
Téléphone : +33 1 23 45 67 89
Contact : Jean Dupont (Responsable)
Email : paris@monentreprise.fr
Entrepôt principal : Oui
```

### Types d'Entrepôts
| Type | Description | Exemple |
|------|-------------|---------|
| Principal | Entrepôt par défaut | Siège social |
| Secondaire | Sites additionnels | Succursales |
| Dropship | Stock fournisseur (pas physique) | Fournisseur direct |
| FBA | Amazon Fulfillment | Amazon warehouse |

## Niveaux de Stock

### Vue par Entrepôt
```
=== Entrepôt Paris (WH-PAR) ===
| Article          | Disponible | Engagé | En commande | Total |
|-----------------|-----------|--------|-------------|-------|
| T-shirt Basique | 120       | 30     | 200         | 150   |
| Jean Slim       | 45        | 5      | 100         | 50    |
| Casquette       | 180       | 20     | 0           | 200   |

=== Entrepôt Lyon (WH-LYO) ===
| Article          | Disponible | Engagé | En commande | Total |
|-----------------|-----------|--------|-------------|-------|
| T-shirt Basique | 60        | 10     | 0           | 70    |
| Jean Slim       | 25        | 0      | 50          | 25    |
```

### Alertes de Stock
```
Configuration des alertes :
  Article : T-shirt Basique
  Point de réapprovisionnement : 50 unités
  Quantité de réapprovisionnement : 200 unités
  Entrepôt : Tous
  
  → Quand le stock disponible < 50
  → Notification email au gestionnaire
  → Suggestion de bon de commande automatique
```

## Méthodes de Valorisation

### FIFO (First In, First Out)
Les articles les plus anciens sont vendus en premier.
```
Entrées :
  01/01 : 100 unités à 10€ = 1000€
  15/01 : 100 unités à 12€ = 1200€

Vente de 50 unités :
  Coût = 50 × 10€ = 500€ (lot le plus ancien)
  
Stock restant :
  50 unités à 10€ + 100 unités à 12€ = 1700€
```

### Coût Moyen Pondéré
```
Entrées :
  01/01 : 100 unités à 10€
  15/01 : 100 unités à 12€
  
Coût moyen = (1000 + 1200) / 200 = 11€

Vente de 50 unités :
  Coût = 50 × 11€ = 550€
```

## Inventaire Physique

### Processus
```
1. Geler les mouvements de stock
2. Compter physiquement chaque article
3. Saisir les quantités dans le module d'inventaire
4. Comparer avec le stock système
5. Valider les ajustements
6. Reprendre les mouvements normaux
```

### Rapport d'Inventaire
```
Date : 18/02/2026
Entrepôt : Paris

| Article    | Stock système | Comptage | Écart | Valeur écart |
|-----------|--------------|---------|-------|-------------|
| TSH-001   | 150          | 147     | -3    | -25.50€     |
| JEA-001   | 50           | 50      | 0     | 0€          |
| CAS-001   | 200          | 203     | +3    | +15.00€     |

Écart total : -10.50€
```

## Rapports de Stock

### Rapports Disponibles
| Rapport | Description |
|---------|-------------|
| État des stocks | Niveaux actuels par article/entrepôt |
| Valorisation des stocks | Valeur totale du stock |
| Mouvements de stock | Entrées/sorties sur une période |
| Articles sous le seuil | Produits à réapprovisionner |
| Rotation des stocks | Vitesse d'écoulement |
| Stock vieillissant | Articles non vendus depuis X jours |
| Historique d'un article | Tous les mouvements d'un SKU |

## Bonnes Pratiques

1. **Inventaire cyclique** : compter une catégorie chaque semaine plutôt qu'un inventaire complet annuel
2. **Zones de stockage** : organiser par catégorie ou fréquence de picking
3. **FIFO pour les périssables** : toujours en first-in first-out
4. **Stock de sécurité** : prévoir un tampon au-dessus du point de réapprovisionnement
5. **Traçabilité** : activer les numéros de série pour les articles de valeur
