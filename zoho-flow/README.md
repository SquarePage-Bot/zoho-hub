# Zoho Flow - Guide Complet

## Introduction

Zoho Flow est une plateforme d'intégration et d'automatisation qui permet de connecter vos applications cloud entre elles sans écrire de code. Similaire à Zapier ou Make (Integromat), Zoho Flow se distingue par son intégration native avec l'écosystème Zoho.

## Concepts Clés

### Flow (Flux)
Un **flow** est une automatisation composée d'un déclencheur (trigger) et d'une ou plusieurs actions. Chaque flow suit la logique : "Quand X se produit, faire Y".

### Connexions
Les connexions authentifient Zoho Flow auprès de vos applications. Chaque app connectée nécessite une autorisation OAuth ou une clé API.

### Gallery (Galerie)
La galerie propose des flows pré-construits pour les cas d'usage courants, prêts à être activés en quelques clics.

## Architecture d'un Flow

```
┌─────────────┐     ┌──────────────┐     ┌──────────────┐
│  Trigger     │────▶│   Logique    │────▶│   Action(s)  │
│ (Déclencheur)│     │ (Conditions) │     │  (Exécution) │
└─────────────┘     └──────────────┘     └──────────────┘
```

## Tarification

| Plan       | Flows | Tâches/mois | Apps premium |
|------------|-------|-------------|--------------|
| Gratuit    | 5     | 100         | Non          |
| Standard   | 25    | 2 000       | Oui          |
| Professionnel | Illimité | 10 000  | Oui          |

## Cas d'Usage Courants

- **CRM → Email** : Envoyer un email de bienvenue quand un lead est créé
- **Formulaire → CRM** : Créer un contact depuis un formulaire web
- **Desk → Slack** : Notifier l'équipe d'un nouveau ticket critique
- **Facture → Comptabilité** : Synchroniser les factures entre Zoho Books et un ERP
- **Webhook → CRM** : Recevoir des données externes et créer des enregistrements

## Limites et Bonnes Pratiques

- Les tâches sont comptabilisées à chaque exécution d'action
- Utiliser des conditions pour éviter les exécutions inutiles
- Tester chaque flow en mode brouillon avant activation
- Surveiller les journaux d'exécution pour détecter les erreurs
- Nommer clairement chaque flow avec une convention cohérente

## Fichiers de cette section

- [triggers.md](triggers.md) — Déclencheurs disponibles
- [actions.md](actions.md) — Actions et transformations
- [logique.md](logique.md) — Conditions, boucles et logique
- [connecteurs.md](connecteurs.md) — Connecteurs et applications
- [exemples.md](exemples.md) — Exemples concrets de flows
