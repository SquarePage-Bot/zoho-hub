# Widgets Zoho - Guide Complet

## Introduction

Les widgets Zoho sont des composants web personnalisés (HTML/CSS/JS) intégrables dans les applications Zoho (CRM, Desk, Creator, etc.). Ils permettent d'étendre les fonctionnalités natives avec des interfaces sur mesure.

## Cas d'Usage

- **CRM** : Afficher des données externes dans une fiche contact
- **Desk** : Intégrer un outil interne dans le panneau de ticket
- **Creator** : Ajouter des visualisations personnalisées
- **Analytics** : Widgets de tableau de bord interactifs

## Emplacements Disponibles

| Application | Emplacements |
|-------------|-------------|
| Zoho CRM | Boutons, Related Lists, Sidebar, Web Tabs |
| Zoho Desk | Sidebar, Top Bar, Background |
| Zoho Creator | Pages, formulaires |
| Zoho Books | Sidebar |

## Architecture

```
┌──────────────────────────┐
│   Application Zoho       │
│  ┌────────────────────┐  │
│  │  Widget (iframe)   │  │
│  │  ┌──────────────┐  │  │
│  │  │ HTML/CSS/JS  │  │  │
│  │  │              │  │  │
│  │  │  SDK Zoho ◄──┼──┼──── API Zoho
│  │  │              │  │  │
│  │  └──────────────┘  │  │
│  └────────────────────┘  │
└──────────────────────────┘
```

## Fichiers de cette section

- [sdk.md](sdk.md) — SDK et API des widgets
- [deploiement.md](deploiement.md) — Déploiement et publication
- [exemples.md](exemples.md) — Exemples concrets de widgets
