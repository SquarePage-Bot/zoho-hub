# Forums de Discussion

## Présentation

Les forums dans Zoho Projects sont des espaces de discussion organisés par projet. Ils permettent aux membres de l'équipe et aux clients de communiquer, poser des questions et partager des idées de manière structurée.

## Accéder aux forums

```
Projet → Menu latéral → Forums
```

## Structure des forums

```
Projet
└── Forums
    ├── Catégorie : Général
    │   ├── Sujet : Bienvenue dans le projet
    │   └── Sujet : Conventions de nommage
    ├── Catégorie : Technique
    │   ├── Sujet : Choix de l'architecture
    │   └── Sujet : Problème de performance API
    └── Catégorie : Client
        ├── Sujet : Retours sur les maquettes
        └── Sujet : Questions fonctionnelles
```

## Catégories

Les catégories organisent les sujets par thème.

### Créer une catégorie

```
Forums → + Nouvelle catégorie
→ Nom : "Décisions techniques"
→ Description : "Discussions et votes sur les choix techniques"
→ Visible par : Tous les membres (ou sélection)
```

### Exemples de catégories courantes

| Catégorie | Usage |
|-----------|-------|
| **Général** | Annonces, présentations, infos générales |
| **Technique** | Questions et discussions techniques |
| **Design** | Retours sur l'UI/UX, maquettes |
| **Client** | Échanges avec le client (visibilité externe) |
| **Rétrospective** | Retours d'expérience après chaque sprint/phase |

## Sujets (Topics)

### Créer un sujet

```
Forums → Catégorie → + Nouveau sujet

Champs :
- Titre : "Choix du framework frontend"
- Contenu : Texte riche (formatage, images, liens, code)
- Catégorie : Technique
- Drapeau : Interne / Externe
- Pièces jointes : Documents, images
- Notifier : Tous les membres / Sélection
```

### Exemple de sujet structuré

```markdown
# Choix du framework frontend

## Contexte
Nous devons choisir un framework JS pour le frontend de l'application.

## Options étudiées

### Option 1 : React
- ✅ Large communauté
- ✅ Écosystème riche
- ❌ Complexité d'apprentissage

### Option 2 : Vue.js
- ✅ Courbe d'apprentissage douce
- ✅ Documentation excellente
- ❌ Communauté plus petite

### Option 3 : Angular
- ✅ Framework complet
- ❌ Plus lourd
- ❌ Complexité

## Recommandation
Je recommande **Vue.js** pour ce projet. Merci de donner votre avis.

📎 Pièce jointe : comparatif-frameworks.pdf
```

## Réponses et commentaires

### Répondre à un sujet

```
Sujet → Zone de réponse en bas
→ Éditeur texte riche
→ @ mentionner un membre
→ Joindre des fichiers
→ Publier
```

### Fonctionnalités des réponses

| Fonctionnalité | Description |
|----------------|-------------|
| **Texte riche** | Gras, italique, listes, code, tableaux |
| **Mentions @** | Notifier un membre spécifique |
| **Pièces jointes** | Images, documents, archives |
| **Réponse citée** | Citer une réponse précédente |
| **Émoticônes** | Réactions rapides |

## Visibilité et permissions

### Drapeaux de visibilité

| Drapeau | Qui voit | Usage |
|---------|----------|-------|
| **Interne** | Membres de l'équipe uniquement | Discussions internes, décisions techniques |
| **Externe** | Membres + Clients | Retours client, validations, questions |

### Permissions par rôle

| Action | Admin | Manager | Membre | Client |
|--------|-------|---------|--------|--------|
| Créer catégorie | ✅ | ✅ | ❌ | ❌ |
| Créer sujet | ✅ | ✅ | ✅ | ✅ (externe) |
| Répondre | ✅ | ✅ | ✅ | ✅ |
| Modifier son sujet | ✅ | ✅ | ✅ | ✅ |
| Supprimer un sujet | ✅ | ✅ | ❌ | ❌ |
| Épingler un sujet | ✅ | ✅ | ❌ | ❌ |

## Fonctionnalités avancées

### Épingler un sujet

```
Sujet → Menu (⋮) → Épingler
→ Le sujet reste en haut de la catégorie
→ Utile pour les annonces importantes ou guidelines
```

### Verrouiller un sujet

```
Sujet → Menu (⋮) → Verrouiller
→ Plus personne ne peut répondre
→ Utile pour les décisions finalisées
```

### Suivre un sujet

```
Sujet → Bouton "Suivre"
→ Recevoir une notification à chaque nouvelle réponse
→ Par défaut : on suit les sujets où on a participé
```

## Notifications des forums

```
Paramètres de notification :
☑ Nouveau sujet dans mes projets
☑ Réponse sur un sujet que je suis
☑ Mention @mon_nom
☐ Toute activité des forums

Canal : Email + Notification in-app
```

## Lien avec les tâches

Il est possible de lier un sujet de forum à une tâche :

```
1. Dans un sujet → "Convertir en tâche"
   → Crée une tâche à partir du contenu du sujet

2. Dans une tâche → Commentaire → "Lien vers le forum"
   → Référencer un sujet de discussion dans une tâche
```

## Recherche dans les forums

```
Forums → 🔍 Rechercher
→ Par mots-clés
→ Filtrer par : catégorie, auteur, date, drapeau
→ Trier par : date, nombre de réponses, pertinence
```

## Bonnes pratiques

1. **Un sujet = un sujet** : Éviter de mélanger plusieurs discussions dans un même thread
2. **Titres clairs** : Utiliser des titres descriptifs (pas "Question" ou "Aide")
3. **Catégoriser** : Créer des catégories cohérentes dès le début du projet
4. **Épingler les décisions** : Les décisions importantes doivent être facilement retrouvables
5. **Utiliser les drapeaux** : Bien distinguer discussions internes et externes
6. **Mentionner** : Utiliser les @mentions pour impliquer les bonnes personnes
7. **Fermer les sujets résolus** : Verrouiller les discussions terminées
