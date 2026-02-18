# Zoho Campaigns

## Présentation

Zoho Campaigns est une plateforme d'email marketing qui permet de créer, envoyer et analyser des campagnes email. Elle offre des outils de gestion de listes, de segmentation, d'automatisation et de reporting avancé.

## Fonctionnalités principales

- **Gestion des contacts** : Import, segmentation, scoring et nettoyage des listes
- **Campagnes email** : Création, A/B testing, envoi programmé
- **Templates** : Bibliothèque de modèles et éditeur drag & drop
- **Automatisations** : Workflows, autoresponders, drip campaigns
- **Rapports** : Taux d'ouverture, clics, conversions, heatmaps
- **Conformité** : RGPD, CAN-SPAM, double opt-in
- **Intégrations** : Zoho CRM, Zoho Desk, réseaux sociaux, e-commerce

## Concepts clés

### Contact
Un contact est un individu dans votre base de données avec au minimum une adresse email. Chaque contact peut avoir des champs personnalisés (nom, entreprise, préférences, etc.).

### Liste de diffusion (Mailing List)
Une liste regroupe des contacts partageant des caractéristiques communes ou un même consentement.

### Segment
Un sous-ensemble dynamique d'une liste, filtré par des critères (comportement, données démographiques, etc.).

### Campagne
Un envoi d'email à une ou plusieurs listes/segments. Peut être ponctuel ou récurrent.

## Types de campagnes

| Type | Description | Usage |
|------|-------------|-------|
| **Email classique** | Email envoyé une fois à une liste | Newsletter, promotions |
| **A/B Testing** | Test de variantes pour optimiser | Optimisation objet, contenu |
| **Email RSS** | Généré automatiquement depuis un flux RSS | Blog digest |
| **Email déclenché** | Envoyé suite à une action | Bienvenue, abandon de panier |
| **SMS** | Message texte (avec module SMS) | Rappels, promotions urgentes |

## Conformité et délivrabilité

### RGPD / Protection des données

| Exigence | Implémentation Zoho Campaigns |
|----------|------------------------------|
| **Consentement** | Formulaires avec double opt-in |
| **Droit d'accès** | Export des données du contact |
| **Droit de suppression** | Suppression en un clic |
| **Lien de désinscription** | Obligatoire dans chaque email |
| **Registre de traitement** | Historique des consentements |

### Améliorer la délivrabilité

- **Authentification** : Configurer SPF, DKIM et DMARC
- **Réputation** : IP dédiée disponible (plans avancés)
- **Nettoyage** : Supprimer régulièrement les bounces et inactifs
- **Double opt-in** : Confirmer l'inscription par email
- **Contenu** : Éviter les spam words, ratio texte/image équilibré

## Navigation dans la documentation

| Fichier | Contenu |
|---------|---------|
| [listes.md](listes.md) | Gestion des contacts, listes et segments |
| [campagnes.md](campagnes.md) | Création de campagnes et A/B testing |
| [templates.md](templates.md) | Templates email et éditeur |
| [automatisations.md](automatisations.md) | Workflows et autoresponders |
| [rapports.md](rapports.md) | Statistiques et analyse |
| [api.md](api.md) | API REST Zoho Campaigns |

## Tarification

| Plan | Contacts | Emails/mois | Fonctionnalités clés |
|------|----------|-------------|----------------------|
| **Gratuit** | 2 000 | 6 000 | Campagnes basiques, templates |
| **Standard** | 5 000+ | Illimités | A/B testing, automatisations basiques |
| **Professional** | 5 000+ | Illimités | Workflows avancés, scoring, pop-ups |

## Intégrations natives

- **Zoho CRM** : Sync bidirectionnelle des contacts, campagnes liées aux deals
- **Zoho Desk** : Envoyer des campagnes basées sur les tickets
- **Zoho Commerce** : Emails transactionnels e-commerce
- **Zoho Survey** : Intégrer des sondages dans les emails
- **Zoho Analytics** : Rapports avancés cross-canal
- **Shopify / WooCommerce** : Sync produits et clients
- **Google Ads / Facebook** : Audiences personnalisées
