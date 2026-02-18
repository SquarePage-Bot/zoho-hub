# Diagramme de Gantt

## Présentation

Le diagramme de Gantt dans Zoho Projects offre une vue chronologique interactive de toutes les tâches, jalons et dépendances d'un projet. C'est l'outil principal de planification visuelle.

## Accéder au Gantt

```
Projet → Menu latéral → Diagramme de Gantt
```

## Interface du Gantt

```
┌─────────────────────────────────────────────────────────────────────┐
│  Barre d'outils : [Zoom] [Chemin critique] [Baseline] [Exporter]  │
├────────────────────┬────────────────────────────────────────────────┤
│ Tâche              │  Mars 2026          Avril 2026                │
│                    │  S1  S2  S3  S4  S1  S2  S3  S4              │
├────────────────────┼────────────────────────────────────────────────┤
│ 🏁 Jalon 1         │  ◆                                            │
│  ├ Analyse         │  ████████                                     │
│  └ Conception      │          ██████                               │
│ 🏁 Jalon 2         │                  ◆                            │
│  ├ Dev Backend     │                  ████████████████             │
│  ├ Dev Frontend    │                      ████████████████         │
│  └ Intégration     │                                  ████████     │
│ 🏁 Jalon 3         │                                          ◆   │
│  └ Tests           │                                      ████████ │
└────────────────────┴────────────────────────────────────────────────┘
```

## Fonctionnalités

### Niveaux de zoom

| Niveau | Affichage | Usage |
|--------|-----------|-------|
| **Jour** | Chaque colonne = 1 jour | Projets courts (< 1 mois) |
| **Semaine** | Chaque colonne = 1 semaine | Projets moyens (1-6 mois) |
| **Mois** | Chaque colonne = 1 mois | Projets longs (> 6 mois) |
| **Trimestre** | Chaque colonne = 3 mois | Vision portfolio |

### Manipulation des tâches dans le Gantt

#### Déplacer une tâche
```
Glisser-déposer la barre horizontalement
→ Modifie les dates de début et de fin
→ Conserve la durée
```

#### Modifier la durée
```
Étirer le bord droit ou gauche de la barre
→ Modifie la date de fin (ou de début)
→ Ajuste la durée
```

#### Créer une dépendance
```
Survoler une tâche → Un point apparaît sur le bord droit
Glisser depuis ce point vers une autre tâche
→ Crée une dépendance Fin-Début (FS) par défaut
```

#### Changer le type de dépendance
```
Clic droit sur la flèche de dépendance → Modifier
Types : FS (Fin-Début) | SS (Début-Début) | FF (Fin-Fin) | SF (Début-Fin)
Lag : Ajouter un décalage en jours
```

### Code couleur

| Couleur | Signification |
|---------|---------------|
| 🟦 Bleu | Tâche normale, dans les temps |
| 🟥 Rouge | Tâche en retard ou sur le chemin critique |
| 🟩 Vert | Tâche terminée |
| 🟨 Jaune | Tâche en cours, à surveiller |
| ◆ Losange | Jalon |
| → Flèche | Dépendance entre tâches |

## Chemin critique

Le chemin critique identifie la chaîne de tâches la plus longue, déterminant la durée minimale du projet.

### Activer l'affichage

```
Diagramme de Gantt → Barre d'outils → ☑ Chemin critique
```

### Interprétation

```
Projet : Refonte application (durée totale : 60 jours)

Chemin critique (en rouge) :
[Spécifications 10j] → [Design UI 15j] → [Dev Frontend 20j] → [Tests intégration 10j] → [Déploiement 5j]
Total : 60 jours ← Aucune marge

Hors chemin critique :
[Documentation 8j] ← Marge de 12 jours
[Formation 5j] ← Marge de 25 jours
```

**Implications :**
- Tout retard sur une tâche du chemin critique retarde le projet
- Les tâches hors chemin critique peuvent avoir du retard (dans leur marge) sans impact

## Baseline (Référence)

La baseline permet de comparer le planning actuel avec le planning initial.

### Créer une baseline

```
Diagramme de Gantt → Menu → Définir la baseline

La baseline capture :
- Dates de début et de fin prévues
- Durées planifiées
- Jalons prévus
```

### Comparer avec la baseline

```
Affichage avec baseline :
┌─────────────────┬──────────────────────────────────────┐
│ Tâche           │  Mars          Avril                 │
├─────────────────┼──────────────────────────────────────┤
│ Design UI       │  ░░░░░░░░░░    (baseline : 10j)     │
│                 │  ████████████████  (réel : 16j)      │
│                 │                 ↑ Retard de 6 jours  │
├─────────────────┼──────────────────────────────────────┤
│ Développement   │      ░░░░░░░░░░░░░░  (baseline)     │
│                 │              ████████████████ (réel)  │
└─────────────────┴──────────────────────────────────────┘

░░░ = Baseline (planning initial)
███ = Réel (planning actuel)
```

### Gestion de plusieurs baselines

```
Vous pouvez créer jusqu'à 5 baselines :
- Baseline 1 : Planning initial (approuvé par le client)
- Baseline 2 : Après changement de périmètre
- Baseline 3 : Après réallocation des ressources
```

## Planification des ressources

### Vue charge de travail

```
Diagramme de Gantt → Vue → Charge de travail

┌──────────────┬──────────────────────────────────┐
│ Membre       │  Lundi  Mardi  Mercredi  Jeudi   │
├──────────────┼──────────────────────────────────┤
│ Marie (8h)   │  6h     8h     10h ⚠️    4h     │
│ Paul (8h)    │  8h     8h     8h        8h     │
│ Sophie (8h)  │  2h     0h     4h        6h     │
└──────────────┴──────────────────────────────────┘

⚠️ = Surcharge (> capacité journalière)
```

### Nivellement des ressources

Lorsqu'une ressource est surchargée, le nivellement ajuste automatiquement le planning :

```
Avant nivellement :
Marie : [Tâche A 8h] + [Tâche B 6h] le même jour = 14h ⚠️

Après nivellement :
Marie : [Tâche A 8h] lundi → [Tâche B 6h] mardi ✅
```

## Jours ouvrés et calendrier

### Configurer le calendrier du projet

```
Paramètres du projet → Calendrier de travail

Jours ouvrés : Lundi → Vendredi
Heures par jour : 8h
Jours fériés :
  - 01/01/2026 : Jour de l'An
  - 01/05/2026 : Fête du travail
  - 14/07/2026 : Fête nationale
  - 25/12/2026 : Noël
```

Le Gantt respecte automatiquement le calendrier : une tâche de 5 jours commençant un jeudi se terminera le mercredi suivant (en sautant le week-end).

## Exporter le Gantt

### Formats d'export

| Format | Usage |
|--------|-------|
| **PDF** | Impression, présentation client |
| **PNG / JPEG** | Insertion dans des documents |
| **MS Project (.mpp)** | Interopérabilité avec Microsoft Project |

### Exporter

```
Diagramme de Gantt → Barre d'outils → Exporter
→ Choisir le format
→ Sélectionner la période
→ Options d'affichage (dépendances, chemin critique, baseline)
→ Télécharger
```

## Impression

```
Diagramme de Gantt → Barre d'outils → Imprimer
→ Orientation : Paysage (recommandé)
→ Taille : A3 pour les grands projets
→ Inclure : Légende, en-tête du projet
```

## Bonnes pratiques

1. **Définir les jalons en premier** : Structurer le projet autour des dates clés avant de créer les tâches
2. **Créer les dépendances** : Ne pas laisser de tâches "flottantes" sans relation
3. **Utiliser les baselines** : Toujours créer une baseline avant le démarrage du projet
4. **Surveiller le chemin critique** : Revoir régulièrement les tâches critiques
5. **Mettre à jour quotidiennement** : Le Gantt n'est utile que s'il reflète la réalité
6. **Vérifier la charge de travail** : Éviter la surcharge des ressources
7. **Communiquer** : Partager le Gantt avec le client en export PDF régulier
