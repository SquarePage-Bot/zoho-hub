# Zoho Inventory - Intégrations

## Intégrations E-commerce

### Shopify
```
Configuration :
1. Paramètres → Intégrations → Shopify
2. Connecter votre boutique Shopify
3. Mapper les produits (SKU ↔ SKU)

Synchronisation :
- Produits : Inventory → Shopify (ou bidirectionnel)
- Commandes : Shopify → Inventory (automatique)
- Stock : Inventory → Shopify (temps réel)
- Clients : Shopify → Inventory

Options :
  ☑ Sync automatique des commandes
  ☑ Mise à jour du stock en temps réel
  ☑ Créer les produits manquants
  ☐ Sync des prix (optionnel)
```

### Amazon (FBA & FBM)
```
Marchés supportés :
- Amazon.fr, Amazon.de, Amazon.co.uk, Amazon.com, etc.

FBM (Fulfilled by Merchant) :
- Commandes reçues dans Inventory
- Expédition gérée par vous
- Stock synchronisé

FBA (Fulfilled by Amazon) :
- Stock envoyé à Amazon
- Commandes traitées par Amazon
- Synchronisation automatique des niveaux
```

### Autres Plateformes E-commerce
| Plateforme | Sync Commandes | Sync Stock | Sync Produits |
|------------|---------------|-----------|---------------|
| eBay | ✅ | ✅ | ✅ |
| Etsy | ✅ | ✅ | ✅ |
| WooCommerce | ✅ | ✅ | ✅ |
| Magento | ✅ | ✅ | ✅ |

## Intégrations Transporteurs

### Transporteurs Intégrés
```
France :
- Colissimo / La Poste
- Chronopost
- DPD France
- Mondial Relay

International :
- DHL Express
- FedEx
- UPS
- TNT
- Aramex

Comparateur :
- Afficher les tarifs de tous les transporteurs
- Choisir le moins cher / le plus rapide
- Générer l'étiquette directement
```

### Générer une Étiquette d'Expédition
```
Commande : SO-0001
Transporteur : Colissimo
Service : Colissimo Domicile

Colis :
  Poids : 3.5 kg
  Dimensions : 40 × 30 × 20 cm

→ Étiquette générée (PDF)
→ N° suivi : 8R12345678901
→ Email de suivi envoyé au client
```

## Intégration Zoho Books (Comptabilité)

```
Synchronisation automatique :
- Factures de vente → Zoho Books
- Factures d'achat → Zoho Books
- Paiements ↔ bidirectionnel
- Contacts clients/fournisseurs ↔ bidirectionnel
- Plan comptable partagé

Configuration :
  Paramètres → Intégrations → Zoho Books
  ☑ Sync automatique des factures
  ☑ Sync des paiements
  ☑ Même plan comptable
```

## Intégration Zoho CRM

```
Synchronisation :
- Clients Inventory ↔ Comptes CRM
- Articles Inventory ↔ Produits CRM
- Commandes de vente ↔ Commandes CRM
- Factures ↔ Factures CRM

Avantages :
- Les commerciaux voient le stock disponible dans CRM
- Les commandes CRM créent automatiquement des SO dans Inventory
- Les factures sont synchronisées
```

## Intégration Zoho Desk

```
Le support peut :
- Voir l'historique des commandes d'un client
- Vérifier le statut de livraison
- Consulter le stock disponible
- Créer un retour/échange depuis le ticket
```

## Passerelles de Paiement

| Passerelle | Pays | Devises |
|-----------|------|---------|
| Stripe | International | Multi-devises |
| PayPal | International | Multi-devises |
| GoCardless | Europe | EUR, GBP |
| Razorpay | Inde | INR |

## Bonnes Pratiques

1. **Mapper les SKU** de façon identique sur toutes les plateformes
2. **Activer la sync en temps réel** du stock pour éviter les surventes
3. **Tester avec peu de produits** avant de synchroniser tout le catalogue
4. **Surveiller les erreurs de sync** dans les journaux
5. **Un seul système maître** : Zoho Inventory comme source de vérité pour le stock
