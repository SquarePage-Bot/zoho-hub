# 🤖 Zoho Desk - Automatisations

> Workflows, macros, blueprints et règles d'automatisation.

## Table des matières

- [Workflow Rules](#workflow-rules)
- [Macros](#macros)
- [Blueprints](#blueprints)
- [Règles d'assignation](#règles-dassignation)
- [Superviseur (Time-Based)](#superviseur)
- [Intégrations et webhooks](#intégrations-et-webhooks)

---

## Workflow Rules

### Structure

```
Déclencheur :
  - À la création du ticket
  - À la modification du ticket
  - À la suppression
  - Basé sur le temps (time-based)

Conditions :
  - Champs du ticket (statut, priorité, département...)
  - Champs du contact (type, pays...)
  - Combinaisons AND/OR

Actions :
  - Envoyer un email
  - Notification in-app
  - Mettre à jour un champ
  - Déclencher un webhook
  - Assigner un agent
  - Ajouter un tag
  - Créer une tâche
  - Exécuter une Custom Function
```

### Exemples de workflows

**1. Notification ticket urgent**
```
Déclencheur : Création de ticket
Condition : Priorité = "Urgente"
Actions :
  - Email au manager : "🚨 Ticket urgent #{ticket.number} : {ticket.subject}"
  - Notification in-app à l'équipe
  - Tag "urgent"
```

**2. Escalade automatique**
```
Déclencheur : Modification de ticket
Condition : Priorité passe à "Urgente" ET Agent non assigné
Actions :
  - Assigner au chef d'équipe
  - Email au chef d'équipe
  - Mettre statut = "Escaladé"
```

**3. Fermeture automatique**
```
Déclencheur : Modification de ticket
Condition : Statut = "Résolu" depuis plus de 48h
Actions :
  - Mettre statut = "Fermé"
  - Envoyer enquête de satisfaction
```

**4. Routing par sujet**
```
Déclencheur : Création de ticket
Conditions (OR) :
  - Sujet contient "facture" OU "paiement" → Département Facturation
  - Sujet contient "bug" OU "erreur" → Département Technique
  - Sujet contient "devis" OU "prix" → Département Commercial
```

**5. Enrichissement via CRM**
```
Déclencheur : Création de ticket
Action : Custom Function (Deluge)
  → Chercher le contact dans Zoho CRM
  → Remplir le champ "Type de client"
  → Ajuster la priorité si client Premium
```

---

## Macros

### Concept

Les macros sont des combinaisons d'actions exécutables en un clic par un agent.

```
Macro = Action 1 + Action 2 + Action 3 (en un clic)
```

### Exemples de macros

**Macro "Escalader au N2"**
```
Actions :
1. Changer département → Support Technique
2. Changer priorité → Haute
3. Ajouter tag "escalade-n2"
4. Commentaire interne : "Escaladé au support N2 par {agent}"
5. Email au client : "Votre demande a été transférée à notre équipe technique."
```

**Macro "Demander info client"**
```
Actions :
1. Changer statut → En attente
2. Répondre au client (modèle "Demande d'information")
3. Ajouter tag "info-manquante"
```

**Macro "Clôturer résolu"**
```
Actions :
1. Changer statut → Fermé
2. Répondre : "Ce ticket est maintenant clôturé. N'hésitez pas à nous recontacter."
3. Envoyer enquête CSAT
```

**Macro "Spam"**
```
Actions :
1. Changer statut → Fermé
2. Ajouter tag "spam"
3. Supprimer le ticket de la vue
```

---

## Blueprints

### Concept

Un Blueprint définit un processus structuré pour les transitions d'état des tickets. Il force les agents à suivre un workflow précis.

### Exemple : Processus de support standard

```
États et transitions :

Ouvert
  ├── "Prendre en charge" → En cours
  │     Conditions : Agent doit être assigné
  │     Actions : Timer SLA démarre
  │
  └── "Rejeter" → Fermé
        Conditions : Motif obligatoire
        Actions : Email au client avec motif

En cours
  ├── "Demander info" → En attente
  │     Actions : Email au client, SLA suspendu
  │
  ├── "Escalader" → Escaladé
  │     Conditions : Commentaire obligatoire
  │     Actions : Notifier le manager
  │
  └── "Résoudre" → Résolu
        Conditions : Description de la solution obligatoire
        Actions : Email au client avec solution

En attente
  └── "Client a répondu" → En cours
        Déclencheur : Réponse du client
        Actions : SLA reprend

Escaladé
  ├── "Résoudre" → Résolu
  └── "Renvoyer au N1" → En cours

Résolu
  ├── "Client confirme" → Fermé
  │     Actions : Enquête CSAT
  └── "Réouvrir" → Ouvert
        Déclencheur : Client conteste
```

### Configuration d'une transition

```
Transition "Résoudre" :
  De : En cours
  Vers : Résolu

  Avant la transition (formulaire) :
  ├── Champ "Cause racine" : obligatoire (liste déroulante)
  ├── Champ "Solution appliquée" : obligatoire (texte multiligne)
  └── Champ "Temps passé" : optionnel (nombre)

  Après la transition :
  ├── Email au client avec la solution
  ├── Mise à jour du champ "Date de résolution"
  └── Notification au manager si ticket était escaladé

  Permissions :
  └── Agents + Managers du département concerné
```

---

## Règles d'assignation

### Direct Assignment

```
Configuration → Assignation → Règles directes

Règle 1 : "Clients Premium"
  Condition : Contact.Type = "Premium"
  Assigner à : Équipe Premium (round-robin)

Règle 2 : "Canal téléphone"
  Condition : Canal = "Téléphone"
  Assigner à : Agents disponibles (load balancing)

Règle 3 : "Par langue"
  Condition : Langue du ticket = "Anglais"
  Assigner à : Équipe internationale
```

### Skills-Based Routing

```
Compétences définies par agent :
  Alice : [CRM, Technique, Français, Anglais]
  Bob : [Facturation, Français]
  Charlie : [CRM, Technique, Espagnol]

Ticket entrant :
  Catégorie = "CRM", Langue = "Anglais"
  → Assigné à Alice (seule avec CRM + Anglais)
```

---

## Superviseur

### Règles basées sur le temps

```
Configuration → Automatisation → Superviseur

Exemples :

Règle 1 : "Ticket dormant"
  Si statut = "Ouvert" depuis > 24h ET agent non assigné
  → Assigner au chef d'équipe
  → Notification manager

Règle 2 : "Relance client"
  Si statut = "En attente" depuis > 72h
  → Envoyer un rappel au client
  → Si > 7 jours → Fermer automatiquement

Règle 3 : "SLA bientôt dépassé"
  Si temps restant SLA < 20%
  → Notification à l'agent
  → Notification au manager

Règle 4 : "Satisfaction basse"
  Si CSAT = "Insatisfait" ET statut = "Fermé"
  → Réouvrir le ticket
  → Assigner au manager
  → Notification direction
```

### Fréquence d'exécution

```
Le superviseur s'exécute toutes les heures.
Il vérifie toutes les conditions et applique les actions correspondantes.
```

---

## Intégrations et webhooks

### Webhooks

```
Configuration → Automatisation → Webhooks

Exemple : Notification Slack
URL : https://hooks.slack.com/services/T00/B00/XXXX
Méthode : POST
Body :
{
  "text": "Nouveau ticket #${ticket.ticketNumber}",
  "blocks": [{
    "type": "section",
    "fields": [
      {"type": "mrkdwn", "text": "*Sujet :* ${ticket.subject}"},
      {"type": "mrkdwn", "text": "*Client :* ${ticket.contact.name}"},
      {"type": "mrkdwn", "text": "*Priorité :* ${ticket.priority}"},
      {"type": "mrkdwn", "text": "*Département :* ${ticket.department}"}
    ]
  }]
}
```

### Custom Functions (Deluge)

```deluge
// Enrichir un ticket avec les données CRM
email = ticket.get("email");
contacts = zoho.crm.searchRecords("Contacts", "(Email:equals:" + email + ")");

if (contacts.size() > 0)
{
    contact = contacts.get(0);
    accountType = contact.get("Account_Name").get("Account_Type");

    if (accountType == "Premium")
    {
        // Mettre le ticket en haute priorité
        updateMap = Map();
        updateMap.put("priority", "High");
        invokeurl
        [
            url: "https://desk.zoho.eu/api/v1/tickets/" + ticket.get("id")
            type: PATCH
            headers: {"Content-Type": "application/json"}
            parameters: updateMap.toString()
            connection: "zoho_desk"
        ];
    }
}
```

---

## Bonnes pratiques

1. **Blueprints** : Utiliser pour les processus critiques (force le respect des étapes)
2. **Macros** : Créer pour les actions répétitives (gain de temps agents)
3. **Superviseur** : Configurer pour les tickets dormants et les violations SLA
4. **Workflows** : Tester en environnement sandbox avant activation
5. **Webhooks** : Sécuriser avec des tokens d'authentification
6. **Custom Functions** : Logger les erreurs pour le débogage

---

*Voir aussi : [tickets.md](tickets.md) | [configuration.md](configuration.md) | [api.md](api.md)*
