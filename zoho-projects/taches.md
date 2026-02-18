# Tâches, Jalons et Dépendances

## Tâches (Tasks)

### Créer une tâche

Les tâches sont l'unité de travail fondamentale dans Zoho Projects.

**Champs d'une tâche :**

| Champ | Description | Obligatoire |
|-------|-------------|-------------|
| **Nom** | Titre de la tâche | ✅ |
| **Liste de tâches** | Groupe parent | ✅ |
| **Assigné à** | Un ou plusieurs membres | ❌ |
| **Date de début** | Date de démarrage | ❌ |
| **Date d'échéance** | Date limite | ❌ |
| **Priorité** | Aucune, Basse, Moyenne, Haute | ❌ |
| **Statut** | Ouvert, En cours, Fermé | ❌ |
| **% d'avancement** | 0-100% | ❌ |
| **Durée estimée** | Heures de travail prévues | ❌ |
| **Description** | Détails en texte riche | ❌ |
| **Tags** | Étiquettes pour filtrage | ❌ |

### Exemple de création de tâche

```
Projet : Refonte Site Web
└── Liste : Design
    └── Tâche : Créer les maquettes Figma
        ├── Assigné à : Marie Dupont
        ├── Date début : 01/03/2026
        ├── Échéance : 10/03/2026
        ├── Priorité : Haute
        ├── Durée estimée : 24h
        └── Tags : design, UI, figma
```

### Statuts personnalisés

Par défaut, les statuts sont : **Ouvert**, **En cours**, **Fermé**. Vous pouvez en créer de personnalisés :

```
Paramètres → Personnalisation → Statuts des tâches

Exemple de workflow personnalisé :
À faire → En cours → En revue → En test → Terminé
```

### Sous-tâches

Chaque tâche peut avoir des sous-tâches pour décomposer le travail :

```
Tâche : Développer la page d'accueil
├── Sous-tâche : Intégrer le header (4h)
├── Sous-tâche : Section hero (6h)
├── Sous-tâche : Section témoignages (4h)
├── Sous-tâche : Footer (3h)
└── Sous-tâche : Tests responsive (2h)
```

### Tâches récurrentes

Créer des tâches qui se répètent automatiquement :

```
Fréquences disponibles :
- Quotidienne
- Hebdomadaire (choisir les jours)
- Mensuelle (date fixe ou relatif)
- Annuelle

Exemple : "Revue hebdomadaire de sprint"
→ Récurrence : Chaque lundi
→ Assigné à : Chef de projet
→ Se termine après : 12 occurrences
```

### Champs personnalisés

Ajouter des champs spécifiques à vos besoins :

```
Paramètres → Personnalisation → Champs personnalisés

Types disponibles :
- Texte (ligne simple, multi-lignes)
- Nombre (entier, décimal)
- Date
- Liste déroulante (choix unique, multi-choix)
- Case à cocher
- URL
- Utilisateur
- Formule
```

**Exemple : Champ "Complexité"**
```
Type : Liste déroulante
Valeurs : Simple | Moyenne | Complexe | Critique
Obligatoire : Non
Visible par : Tous les membres
```

## Listes de tâches (Task Lists)

Les listes de tâches regroupent les tâches par thème ou phase.

```
Projet : Application Mobile
├── Liste : Backend API
│   ├── Créer endpoints auth (8h)
│   ├── API produits (12h)
│   └── API commandes (16h)
├── Liste : Frontend Mobile
│   ├── Écran connexion (6h)
│   ├── Catalogue produits (10h)
│   └── Panier d'achat (8h)
└── Liste : Tests & QA
    ├── Tests unitaires backend (8h)
    ├── Tests d'intégration (6h)
    └── Tests utilisateur (4h)
```

### Propriétés d'une liste

| Propriété | Description |
|-----------|-------------|
| **Nom** | Nom de la liste |
| **Jalon** | Jalon associé (optionnel) |
| **Modèle de vue** | Classique, Kanban, ou personnalisée |
| **Drapeau** | Interne ou Externe (visible par les clients) |

## Jalons (Milestones)

Les jalons marquent les étapes importantes du projet. Ils ne contiennent pas de travail en soi, mais regroupent les listes de tâches.

### Propriétés d'un jalon

| Propriété | Description |
|-----------|-------------|
| **Nom** | Nom du jalon |
| **Date de début** | Début de la phase |
| **Date d'échéance** | Deadline du jalon |
| **Responsable** | Propriétaire du jalon |
| **Drapeau** | Interne / Externe |
| **Statut** | Non démarré / En cours / Terminé |

### Exemple : Jalons d'un projet web

```
Projet : Plateforme E-commerce
│
├── 🏁 Jalon 1 : Spécifications (01/03 → 15/03)
│   └── Liste : Cahier des charges
│       ├── Recueil des besoins (5j)
│       ├── Wireframes (3j)
│       └── Validation client (2j)
│
├── 🏁 Jalon 2 : Développement (16/03 → 30/04)
│   ├── Liste : Backend
│   │   ├── Architecture BDD (5j)
│   │   ├── API REST (15j)
│   │   └── Intégration paiement (5j)
│   └── Liste : Frontend
│       ├── Intégration design (10j)
│       └── Développement React (20j)
│
├── 🏁 Jalon 3 : Tests (01/05 → 15/05)
│   └── Liste : QA
│       ├── Tests fonctionnels (5j)
│       ├── Tests de charge (3j)
│       └── Corrections bugs (5j)
│
└── 🏁 Jalon 4 : Mise en production (16/05 → 20/05)
    └── Liste : Déploiement
        ├── Migration données (2j)
        ├── Déploiement serveur (1j)
        └── Formation utilisateurs (2j)
```

## Dépendances (Dependencies)

Les dépendances définissent les relations entre tâches et l'ordre d'exécution.

### Types de dépendances

| Type | Notation | Description | Exemple |
|------|----------|-------------|---------|
| **Fin-Début (FS)** | Finish-to-Start | B commence quand A finit | Design → Développement |
| **Début-Début (SS)** | Start-to-Start | B commence quand A commence | Tests ↔ Documentation |
| **Fin-Fin (FF)** | Finish-to-Finish | B finit quand A finit | Développement ↔ Code review |
| **Début-Fin (SF)** | Start-to-Finish | B finit quand A commence | Ancien système → Nouveau système |

### Créer une dépendance

```
Méthode 1 : Via le diagramme de Gantt
→ Glisser une flèche de la tâche source vers la tâche cible

Méthode 2 : Via les détails de la tâche
→ Ouvrir la tâche → Onglet "Dépendances"
→ Ajouter un prédécesseur ou un successeur

Méthode 3 : Via l'API
POST /api/v3/portal/{portalId}/projects/{projectId}/tasks/{taskId}/dependencies
```

### Décalages (Lag / Lead)

Ajouter un délai entre les tâches dépendantes :

```
Dépendance avec lag (délai) :
Tâche A (Fin) → +2 jours → Tâche B (Début)
= B commence 2 jours après la fin de A

Dépendance avec lead (avance) :
Tâche A (Fin) → -1 jour → Tâche B (Début)
= B commence 1 jour avant la fin de A
```

### Exemple de chaîne de dépendances

```
[Analyse besoins] ──FS──→ [Conception BDD] ──FS──→ [Développement API]
                                                         │
                                                    FS (+2j)
                                                         │
                                                         ▼
                   [Tests unitaires] ←──SS──  [Développement Frontend]
                         │
                        FS
                         │
                         ▼
                  [Recette client]
```

### Chemin critique

Le **chemin critique** est la séquence de tâches la plus longue déterminant la durée minimale du projet. Visible dans le diagramme de Gantt (en rouge).

```
Activer le chemin critique :
Diagramme de Gantt → Menu → Afficher le chemin critique

Les tâches du chemin critique :
- Sont mises en surbrillance rouge
- Tout retard sur ces tâches retarde le projet entier
- Les tâches hors chemin critique ont de la "marge" (float)
```

## Vues des tâches

### Vue Classique (Liste)
Affichage en liste avec colonnes personnalisables.

### Vue Kanban
Affichage en colonnes par statut, drag & drop entre colonnes.

```
┌──────────┬──────────┬──────────┬──────────┐
│  À faire │ En cours │ En revue │ Terminé  │
├──────────┼──────────┼──────────┼──────────┤
│ Tâche 1  │ Tâche 3  │ Tâche 5  │ Tâche 7  │
│ Tâche 2  │ Tâche 4  │          │ Tâche 8  │
│          │          │          │ Tâche 9  │
└──────────┴──────────┴──────────┴──────────┘
```

### Vue Dépendances
Visualise les relations entre tâches sous forme de graphe.

## Filtres et recherche

### Filtres prédéfinis
- **Mes tâches** : Tâches assignées à moi
- **En retard** : Tâches dépassant l'échéance
- **Non assignées** : Tâches sans responsable
- **Cette semaine** : Échéances de la semaine courante

### Filtres personnalisés

```
Exemple : Tâches critiques en retard
Conditions :
  - Priorité = Haute
  - Date d'échéance < Aujourd'hui
  - Statut ≠ Fermé
Tri : Date d'échéance (croissant)
```

## Notifications

### Types de notifications

| Événement | Notification |
|-----------|-------------|
| Tâche assignée | Email + In-app |
| Commentaire ajouté | Email + In-app |
| Tâche modifiée | In-app |
| Échéance proche (24h) | Email |
| Tâche en retard | Email quotidien |
| Dépendance terminée | Email + In-app |

### Paramétrer les rappels

```
Mon profil → Notifications → Personnaliser

Options :
☑ Tâches assignées à moi
☑ Tâches que je suis
☑ Commentaires sur mes tâches
☐ Toutes les activités du projet
☑ Rappels d'échéance (1 jour avant)
```
