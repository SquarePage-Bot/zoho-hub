# Zoho Inventory - Guide Complet

## Introduction

Zoho Inventory est un logiciel de gestion des stocks et des commandes en ligne. Il permet de suivre les articles, gérer les entrepôts, traiter les commandes d'achat et de vente, et s'intègre avec les principales plateformes e-commerce et les transporteurs.

## Fonctionnalités Principales

- **Gestion des articles** : Produits simples, composites, groupés, avec variantes
- **Suivi des stocks** : Niveaux en temps réel, alertes de réapprovisionnement
- **Commandes de vente** : Devis → Commande → Livraison → Facture
- **Commandes d'achat** : Demande → Bon de commande → Réception
- **Multi-entrepôts** : Gestion de plusieurs sites de stockage
- **Numéros de série & lots** : Traçabilité complète
- **Intégrations e-commerce** : Shopify, Amazon, eBay, Etsy, WooCommerce
- **Expédition** : Intégration transporteurs (DHL, FedEx, UPS, etc.)

## Flux de Travail Principal

```
                    ┌──────────────┐
                    │   Articles   │
                    └──────┬───────┘
           ┌───────────────┼───────────────┐
           ▼               ▼               ▼
   ┌──────────────┐ ┌─────────────┐ ┌────────────┐
   │ Commande     │ │  Entrepôts  │ │ Commande   │
   │  d'achat     │ │  & Stocks   │ │  de vente  │
   └──────┬───────┘ └─────────────┘ └─────┬──────┘
          ▼                                ▼
   ┌──────────────┐                ┌──────────────┐
   │  Réception   │                │  Expédition  │
   └──────────────┘                └──────┬───────┘
                                          ▼
                                   ┌──────────────┐
                                   │   Facture    │
                                   │ (Zoho Books) │
                                   └──────────────┘
```

## Tarification

| Plan | Articles | Commandes/mois | Entrepôts | Prix/mois |
|------|----------|---------------|-----------|-----------|
| Gratuit | 50 | 50 | 1 | 0€ |
| Standard | 500 | 1 500 | 2 | 59€ |
| Professionnel | Illimité | Illimité | Illimité | 99€ |
| Premium | Illimité | Illimité | Illimité | 159€ |

## Fichiers de cette section

- [articles.md](articles.md) — Gestion des articles et produits
- [commandes.md](commandes.md) — Commandes d'achat et de vente
- [entrepots.md](entrepots.md) — Entrepôts et gestion des stocks
- [integrations.md](integrations.md) — Intégrations e-commerce et transporteurs
- [api.md](api.md) — API REST Zoho Inventory
