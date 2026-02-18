# Automatisations

## Présentation

Zoho Projects offre plusieurs mécanismes d'automatisation pour réduire le travail manuel, standardiser les processus et accélérer l'exécution des projets.

## Règles de workflow (Business Rules)

### Concept

Les règles de workflow déclenchent automatiquement des actions lorsque certaines conditions sont remplies.

```
Structure d'une règle :
QUAND [événement] + SI [condition] → ALORS [action]
```

### Événements déclencheurs

| Événement | Description |
|-----------|-------------|
| **Tâche créée** | Nouvelle tâche ajoutée |
| **Tâche modifiée** | Changement de champ sur une tâche |
| **Statut changé** | Transition de statut (ex : En cours → Terminé) |
| **Tâche assignée** | Changement d'assignation |
| **Jalon atteint** | Un jalon passe au statut "Terminé" |
| **Tâche en retard** | La date d'échéance est dépassée |
| **Commentaire ajouté** | Nouveau commentaire sur une tâche |

### Actions disponibles

| Action | Description |
|--------|-------------|
| **Envoyer un email** | Notification par email personnalisable |
| **Modifier un champ** | Changer la valeur d'un champ automatiquement |
| **Assigner la tâche** | Réassigner à un autre membre |
| **Créer une tâche** | Générer une nouvelle tâche |
| **Appeler un webhook** | Envoyer des données à une URL externe |
| **Notifier** | Notification in-app à un utilisateur |

### Exemples de règles

#### Règle 1 : Notification de retard
```
QUAND : Tâche en retard (quotidien à 9h)
SI : Statut ≠ Fermé ET Priorité = Haute
ALORS :
  - Envoyer un email au responsable de la tâche
  - Envoyer un email au manager du projet
  - Modifier le champ "Drapeau" → "Urgent"
```

#### Règle 2 : Assignation automatique au QA
```
QUAND : Statut de tâche changé
SI : Nouveau statut = "Développement terminé"
ALORS :
  - Assigner la tâche à l'équipe QA
  - Modifier le statut → "En test"
  - Envoyer notification au testeur
```

#### Règle 3 : Escalade automatique
```
QUAND : Tâche en retard depuis 3 jours
SI : Priorité = Haute ET Statut = En cours
ALORS :
  - Envoyer email au directeur de projet
  - Créer une tâche "Revue d'escalade" assignée au manager
  - Modifier la priorité → Critique
```

#### Règle 4 : Création automatique de tâche de revue
```
QUAND : Tâche créée
SI : Liste de tâches = "Développement"
ALORS :
  - Créer une tâche "Code Review : {nom_tâche}"
  - Assigner au Tech Lead
  - Ajouter dépendance FS avec la tâche source
```

## Blueprints

### Concept

Les Blueprints définissent un processus formel avec des transitions contrôlées entre statuts. Contrairement aux règles simples, les Blueprints imposent un workflow strict.

```
Disponible uniquement sur le plan Enterprise
```

### Créer un Blueprint

```
Paramètres → Blueprints → + Nouveau Blueprint

1. Définir les statuts : Ouvert → En cours → En revue → En test → Terminé
2. Définir les transitions autorisées
3. Configurer les conditions et actions par transition
```

### Exemple : Blueprint de développement

```
┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────┐
│  Ouvert  │────→│ En cours │────→│ En revue │────→│ En test  │────→│ Terminé  │
└──────────┘     └──────────┘     └──────────┘     └──────────┘     └──────────┘
                       │                │                │
                       │ Rejeté         │ Rejeté         │ Bug trouvé
                       ▼                │                │
                  ┌──────────┐          │                │
                  │  Bloqué  │←─────────┘────────────────┘
                  └──────────┘
```

### Transitions avec conditions

```
Transition : "En cours" → "En revue"

Avant la transition (conditions obligatoires) :
☑ Le champ "Lien du commit Git" ne doit pas être vide
☑ Le champ "% avancement" doit être = 100%
☑ Au moins 1 fichier attaché

Pendant la transition (formulaire) :
- Commentaire de revue (obligatoire)
- Checklist : Tests unitaires passés ? (obligatoire)

Après la transition (actions) :
- Assigner au Tech Lead
- Envoyer notification "Code prêt pour revue"
- Créer entrée dans le journal d'activité
```

## Webhooks

### Concept

Les webhooks envoient des données JSON à une URL externe lorsqu'un événement se produit.

### Configurer un webhook

```
Paramètres → Intégrations → Webhooks → + Nouveau

Nom : Notification Slack
URL : https://hooks.slack.com/services/XXXX/YYYY/ZZZZ
Méthode : POST
Format : JSON
Événements :
  ☑ Tâche créée
  ☑ Tâche terminée
  ☑ Jalon atteint
  ☐ Commentaire ajouté
```

### Payload JSON envoyé

```json
{
  "event": "task_completed",
  "project": {
    "id": "123456",
    "name": "Plateforme E-commerce"
  },
  "task": {
    "id": "789",
    "name": "Développer l'API produits",
    "status": "Fermé",
    "completed_by": "Marie Dupont",
    "completed_on": "2026-03-15T14:30:00Z"
  },
  "milestone": {
    "id": "456",
    "name": "Phase Développement"
  }
}
```

### Exemple : Webhook vers Slack

```
Événement : Jalon atteint
URL : https://hooks.slack.com/services/...
Payload personnalisé :
{
  "text": "🏁 Jalon atteint : *{{milestone.name}}* dans le projet *{{project.name}}*\nComplété par : {{milestone.owner}}"
}
```

## Rappels automatiques

### Types de rappels

| Rappel | Déclencheur | Destinataire |
|--------|-------------|-------------|
| **Échéance tâche** | 1 jour/1h avant | Assigné |
| **Échéance jalon** | 3 jours/1 jour avant | Responsable |
| **Tâche en retard** | Quotidien à 9h | Assigné + Manager |
| **Soumission timesheet** | Vendredi 17h | Tous les membres |
| **Approbation en attente** | Quotidien | Approbateur |

### Configurer les rappels

```
Paramètres → Notifications → Rappels automatiques

Rappels d'échéance :
☑ 24h avant l'échéance
☑ Le jour de l'échéance
☑ Chaque jour si en retard

Canal :
☑ Email
☑ Notification in-app
☐ SMS (si configuré)
```

## Modèles d'email

### Personnaliser les notifications

```
Paramètres → Modèles d'email → Modifier

Variables disponibles :
${task.name} - Nom de la tâche
${task.assignee} - Assigné
${task.dueDate} - Date d'échéance
${task.status} - Statut
${task.priority} - Priorité
${project.name} - Nom du projet
${milestone.name} - Jalon associé
${user.name} - Nom de l'utilisateur déclencheur
```

**Exemple de modèle personnalisé :**
```
Objet : ⚠️ Tâche en retard : ${task.name}

Bonjour ${task.assignee},

La tâche "${task.name}" du projet "${project.name}" est en retard.

📅 Échéance prévue : ${task.dueDate}
📊 Avancement : ${task.progress}%
🏁 Jalon : ${milestone.name}

Merci de mettre à jour le statut ou de contacter votre manager.

Cordialement,
L'équipe projet
```

## Intégration Zoho Flow

Pour des automatisations plus complexes impliquant d'autres applications :

```
Exemples :
- Tâche terminée → Créer une facture dans Zoho Books
- Nouveau projet → Créer un canal Slack
- Bug critique → Créer un ticket Zoho Desk
- Jalon atteint → Envoyer un SMS au client
```

Voir [../zoho-flow/README.md](../zoho-flow/README.md) pour plus de détails.

## Bonnes pratiques

1. **Commencer simple** : Implémenter les règles les plus impactantes d'abord
2. **Tester avant de déployer** : Vérifier les règles sur un projet test
3. **Documenter les automatisations** : Maintenir une liste des règles actives et leur objectif
4. **Éviter les boucles** : S'assurer que les actions ne déclenchent pas d'autres règles en boucle
5. **Monitorer** : Vérifier régulièrement les logs d'exécution des webhooks
6. **Utiliser les Blueprints** : Pour les processus critiques nécessitant un contrôle strict
7. **Combiner avec Zoho Flow** : Pour les workflows cross-applications
