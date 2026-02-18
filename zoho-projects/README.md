# Zoho Projects

## Présentation

Zoho Projects est un outil de gestion de projets en ligne qui permet de planifier, suivre et collaborer sur des projets de toute taille. Il offre des fonctionnalités de gestion des tâches, diagrammes de Gantt, suivi du temps, forums de discussion et bien plus.

## Fonctionnalités principales

- **Gestion des tâches** : Créer, assigner et suivre des tâches avec jalons et dépendances
- **Diagramme de Gantt** : Visualiser la chronologie et les dépendances du projet
- **Feuilles de temps (Timesheets)** : Suivre le temps passé sur chaque tâche
- **Forums** : Espaces de discussion collaboratifs par projet
- **Automatisations** : Règles de workflow, blueprints et webhooks
- **Documents** : Gestion centralisée des fichiers de projet
- **Rapports** : Tableaux de bord et rapports de suivi d'avancement
- **Intégrations** : Connexion avec Zoho CRM, Zoho Books, Zoho Desk, GitHub, etc.

## Concepts clés

### Portail
Un portail est l'espace de travail principal dans Zoho Projects. Chaque organisation dispose d'un portail contenant tous ses projets.

### Projet
Un projet est l'unité de travail principale. Il contient :
- Des **listes de tâches** (task lists) regroupant les tâches
- Des **jalons** (milestones) marquant les étapes importantes
- Des **membres** avec des rôles définis

### Rôles et permissions

| Rôle | Description |
|------|-------------|
| **Administrateur** | Accès complet à tous les projets et paramètres du portail |
| **Manager** | Gère les projets assignés, peut créer des tâches et jalons |
| **Membre** | Travaille sur les tâches assignées |
| **Client** | Accès en lecture avec possibilité de commenter |

## Structure d'un projet

```
Projet
├── Jalons (Milestones)
│   ├── Liste de tâches 1
│   │   ├── Tâche A
│   │   ├── Tâche B (dépend de A)
│   │   └── Sous-tâche B.1
│   └── Liste de tâches 2
│       └── Tâche C
├── Forums
│   ├── Sujet 1
│   └── Sujet 2
├── Documents
├── Feuilles de temps
└── Rapports
```

## Modèles de projet (Templates)

Zoho Projects permet de créer des modèles réutilisables :

```
Paramètres du projet → Modèles → Créer un modèle
```

Un modèle peut inclure :
- Listes de tâches et tâches prédéfinies
- Jalons
- Dépendances
- Assignations par rôle

### Exemple : Modèle de lancement produit

```
Modèle : Lancement Produit
├── Jalon : Phase de recherche
│   └── Liste : Études de marché
│       ├── Analyse concurrentielle (5j)
│       ├── Sondage utilisateurs (10j)
│       └── Rapport synthèse (3j)
├── Jalon : Développement
│   └── Liste : Sprint 1
│       ├── Design UI/UX (10j)
│       ├── Développement frontend (15j)
│       └── Tests QA (5j)
└── Jalon : Lancement
    └── Liste : Go-to-market
        ├── Campagne marketing (7j)
        └── Formation équipe (3j)
```

## Navigation dans la documentation

| Fichier | Contenu |
|---------|---------|
| [taches.md](taches.md) | Tâches, jalons, dépendances et listes de tâches |
| [gantt.md](gantt.md) | Diagramme de Gantt et planification visuelle |
| [forums.md](forums.md) | Forums de discussion et collaboration |
| [timesheet.md](timesheet.md) | Feuilles de temps et suivi des heures |
| [automatisations.md](automatisations.md) | Règles de workflow, blueprints, webhooks |
| [api.md](api.md) | API REST Zoho Projects |

## Tarification

| Plan | Projets | Utilisateurs | Stockage | Fonctionnalités clés |
|------|---------|--------------|----------|----------------------|
| **Gratuit** | 2 | 3 | 10 Mo | Tâches, jalons, forums |
| **Premium** | Illimités | 50+ | 100 Go | Gantt, Timesheets, Rapports avancés |
| **Enterprise** | Illimités | 50+ | 120 Go | Blueprints, Rôles personnalisés, SLA |

## Intégrations natives

- **Zoho CRM** : Lier des projets à des opportunités/clients
- **Zoho Books** : Facturer les heures enregistrées
- **Zoho Desk** : Convertir des tickets en tâches
- **Zoho Sprints** : Gestion agile avancée
- **GitHub / Bitbucket** : Lier les commits aux tâches
- **Google Drive / Dropbox** : Gestion documentaire
- **Zapier / Zoho Flow** : Automatisations cross-applications
