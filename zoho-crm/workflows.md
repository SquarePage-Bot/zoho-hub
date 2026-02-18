# ⚙️ Zoho CRM - Workflows

> Automatisation par règles de workflow : déclencheurs, conditions, actions.

## Concept

Un workflow est une règle d'automatisation qui s'exécute quand un enregistrement remplit certaines conditions. Il se compose de :

1. **Déclencheur** : Quand exécuter ?
2. **Condition** : Sur quels enregistrements ?
3. **Actions** : Que faire ?

## Déclencheurs (Triggers)

| Déclencheur | Description |
|-------------|-------------|
| **Création** | Quand un enregistrement est créé |
| **Modification** | Quand un enregistrement est modifié |
| **Création ou Modification** | Les deux |
| **Suppression** | Quand un enregistrement est supprimé |
| **Action sur un champ** | Quand un champ spécifique change de valeur |
| **Date/Heure** | Basé sur une date (ex: 2 jours avant la date de clôture) |
| **Score** | Quand le score atteint un seuil |
| **Note ajoutée** | Quand une note est attachée |

### Déclencheur basé sur une date

Permet des actions planifiées :
- X jours **avant** ou **après** une date
- Récurrence possible (quotidienne, hebdomadaire)
- Utile pour les relances et rappels

**Exemple :** Envoyer un email 3 jours avant la date de clôture d'un deal

## Conditions

### Critères simples

```
Module: Deals
Condition: Stage = "Négociation" AND Amount > 10000
```

### Opérateurs disponibles

| Opérateur | Description | Types compatibles |
|-----------|-------------|-------------------|
| `=` / `is` | Égal à | Tous |
| `!=` / `isn't` | Différent de | Tous |
| `<`, `>`, `<=`, `>=` | Comparaison | Nombre, Date |
| `contains` | Contient | Texte |
| `starts with` | Commence par | Texte |
| `ends with` | Finit par | Texte |
| `is empty` | Est vide | Tous |
| `is not empty` | N'est pas vide | Tous |
| `between` | Entre deux valeurs | Nombre, Date |
| `in` | Dans une liste | Picklist |
| `not in` | Pas dans une liste | Picklist |

### Combinaison de critères

- **ET** (AND) : Toutes les conditions doivent être vraies
- **OU** (OR) : Au moins une condition vraie
- **Parenthèses** : Groupement de critères
- Exemple : `(Critère1 AND Critère2) OR (Critère3 AND Critère4)`

## Actions

### 1. Notifications par email

Envoyer un email automatique à :
- L'utilisateur propriétaire de l'enregistrement
- Le contact/lead associé
- D'autres utilisateurs CRM
- Des adresses email spécifiques

**Templates d'email :** Utiliser les merge fields `${Module.Field_Name}`

### 2. Mise à jour de champ

Modifier automatiquement un ou plusieurs champs.

**Exemples :**
- Passer le statut à "Relancé" après envoi d'email
- Mettre à jour la date de dernière activité
- Assigner à un utilisateur spécifique

### 3. Créer une tâche

Créer automatiquement une tâche assignée à un utilisateur.

**Paramètres :**
- Sujet, Statut, Priorité
- Date d'échéance (relative ou absolue)
- Assigné à (propriétaire, manager, utilisateur spécifique)

### 4. Appel de fonction (Deluge)

Exécuter une fonction Deluge personnalisée.

```deluge
// Fonction appelée par workflow quand un deal est gagné
// Paramètres reçus : dealId

deal = zoho.crm.getRecordById("Deals", dealId);
accountId = deal.get("Account_Name").get("id");

// Mettre à jour le type de compte en "Client"
updateMap = Map();
updateMap.put("Account_Type", "Client");
zoho.crm.updateRecord("Accounts", accountId, updateMap);

// Créer une tâche d'onboarding
taskMap = Map();
taskMap.put("Subject", "Onboarding - " + deal.get("Deal_Name"));
taskMap.put("Due_Date", zoho.currentdate.addDay(7).toString("yyyy-MM-dd"));
taskMap.put("Status", "Not Started");
taskMap.put("Priority", "High");
taskMap.put("What_Id", dealId); // Lié au deal
taskMap.put("Owner", deal.get("Owner").get("id"));
zoho.crm.createRecord("Tasks", taskMap);

// Notifier sur Slack
slackMsg = "🎉 Deal gagné : " + deal.get("Deal_Name") + " - " + deal.get("Amount") + "€";
response = invokeurl
[
    url: "https://hooks.slack.com/services/XXX/YYY/ZZZ"
    type: POST
    parameters: {"text": slackMsg}
];
```

### 5. Webhook

Appeler une URL externe (API tierce) avec les données de l'enregistrement.

**Configuration :**
- URL de destination
- Méthode : POST/GET
- Paramètres : champs de l'enregistrement en JSON ou form-data
- Headers personnalisés

### 6. Tag

Ajouter ou retirer un tag à l'enregistrement.

### 7. Notification push (Zia)

Envoyer une notification dans l'app Zoho CRM.

## Exemples de workflows courants

### Workflow 1 : Attribution automatique des leads

```
Déclencheur : Création de Lead
Condition : Lead_Source = "Site Web" AND Country = "France"
Actions :
  1. Mise à jour : Owner = "commercial-france@entreprise.com"
  2. Email : Notification au commercial
  3. Tâche : "Appeler le lead dans les 24h" (échéance J+1)
```

### Workflow 2 : Relance deal inactif

```
Déclencheur : Date/Heure (14 jours après Modified_Time)
Condition : Stage NOT IN ("Fermée gagnée", "Fermée perdue")
Actions :
  1. Tâche : "Relancer le prospect" (assignée au propriétaire)
  2. Email : Rappel au propriétaire du deal
  3. Mise à jour : Tag "Inactif"
```

### Workflow 3 : Escalade deal important

```
Déclencheur : Création ou Modification de Deal
Condition : Amount > 50000 AND Stage = "Proposition/Devis"
Actions :
  1. Email : Notification au directeur commercial
  2. Tâche : "Valider la proposition" (assignée au manager)
  3. Mise à jour : Priority = "High"
```

### Workflow 4 : Suivi post-vente

```
Déclencheur : Modification de Deal (champ Stage)
Condition : Stage = "Fermée gagnée"
Actions :
  1. Fonction Deluge : Créer un projet dans Zoho Projects
  2. Email : Bienvenue au client
  3. Tâche : "Planifier kickoff" (échéance J+3)
  4. Mise à jour du compte : Account_Type = "Client"
```

## Limites par édition

| Édition | Workflows/module | Actions/workflow | Conditions |
|---------|------------------|------------------|------------|
| Standard | 5 | 5 | 5 |
| Professionnel | 15 | 5 | 10 |
| Enterprise | 30 | 5 | 10 |
| Ultimate | 50 | 5 | 10 |

## Ordre d'exécution

Quand plusieurs automatisations se déclenchent :

```
1. Règles de validation
2. Règles de layout
3. Règles d'attribution
4. Règles d'approbation
5. Workflows
6. Blueprints (transitions)
7. Actions planifiées (schedules)
```

## Bonnes pratiques

1. **Nommage** : Préfixer par module et action (ex: `DEAL_Won_Onboarding`)
2. **Documentation** : Ajouter une description claire à chaque workflow
3. **Tests** : Tester avec des données de test avant activation
4. **Performance** : Éviter les boucles entre workflows (A met à jour B qui met à jour A)
5. **Logs** : Utiliser les logs d'audit pour le debug
6. **Limites API** : Les fonctions Deluge dans les workflows consomment des API calls
7. **Désactivation** : Désactiver plutôt que supprimer les workflows obsolètes

## Debug et monitoring

### Logs de workflow

- **Setup → Automatisation → Workflows → Logs**
- Affiche : date, enregistrement, workflow, résultat (succès/échec)
- Conservation : 3 mois

### Problèmes courants

| Problème | Cause | Solution |
|----------|-------|----------|
| Workflow ne se déclenche pas | Condition non remplie | Vérifier les critères |
| Email non envoyé | Limite quotidienne atteinte | Vérifier quota emails |
| Fonction échoue | Erreur dans le code Deluge | Vérifier les logs de fonction |
| Boucle infinie | Workflow A déclenche B qui déclenche A | Ajouter des conditions de sortie |

---
*Voir aussi : [blueprints.md](blueprints.md) pour les processus guidés, [automatisations.md](automatisations.md) pour la vue globale.*
