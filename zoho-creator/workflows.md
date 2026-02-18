# ⚙️ Zoho Creator - Workflows

> Règles d'automatisation, schedules, actions et intégrations.

## Table des matières

- [Vue d'ensemble](#vue-densemble)
- [Workflow Rules](#workflow-rules)
- [Schedules (Planifications)](#schedules)
- [Actions disponibles](#actions-disponibles)
- [Blueprints](#blueprints)
- [Approbations](#approbations)
- [Notifications push](#notifications-push)

---

## Vue d'ensemble

Les workflows dans Creator automatisent des actions en réponse à des événements sur les données.

```
Événement (Trigger)
    ↓
Condition (optionnelle)
    ↓
Action(s)
```

---

## Workflow Rules

### Événements déclencheurs

| Événement | Description |
|-----------|-------------|
| **On Create** | Création d'un enregistrement |
| **On Edit** | Modification d'un enregistrement |
| **On Create or Edit** | Création ou modification |
| **On Delete** | Suppression |
| **On Approval** | Lors d'une action d'approbation |

### Structure d'un workflow

```deluge
// Workflow : On Create du formulaire "Commandes"
// Condition : Montant > 5000

// Actions :
// 1. Notifier le manager
sendmail
[
    from: zoho.adminuserid
    to: "manager@entreprise.com"
    subject: "Nouvelle commande importante : " + input.Numero
    message: "Montant : " + input.Montant + " €<br>Client : " + input.Client
];

// 2. Créer une tâche de validation
taskMap = Map();
taskMap.put("Titre", "Valider commande " + input.Numero);
taskMap.put("Assignee", "manager@entreprise.com");
taskMap.put("Priorite", "Haute");
taskMap.put("Date_Echeance", zoho.currentdate.addDay(2));
insert into Taches values taskMap;

// 3. Mettre à jour un champ
input.Statut = "En attente de validation";
```

### Conditions avancées

```deluge
// Condition sur le changement d'un champ spécifique
if (input.Statut != input.Statut_old)
{
    // Le statut a changé
    if (input.Statut == "Terminé" && input.Statut_old == "En cours")
    {
        // Passage de "En cours" à "Terminé"
        input.Date_Cloture = zoho.currentdate;
    }
}

// Vérifier si un champ spécifique a été modifié (On Edit)
if (isFieldModified("Montant"))
{
    // Recalculer la TVA
    input.TVA = input.Montant * 0.20;
    input.Total_TTC = input.Montant + input.TVA;
}
```

---

## Schedules

### Types de planification

| Type | Description |
|------|-------------|
| **Once** | Exécution unique à une date/heure |
| **Hourly** | Toutes les N heures |
| **Daily** | Tous les jours à une heure fixe |
| **Weekly** | Certains jours de la semaine |
| **Monthly** | Certains jours du mois |
| **Yearly** | Certains jours de l'année |

### Exemples de schedules

```deluge
// Schedule : Tous les jours à 8h00
// Nom : "Rappel_Echeances"
// Envoyer un rappel pour les tâches qui arrivent à échéance

taches = Taches[Date_Echeance == zoho.currentdate && Statut != "Terminé"];
for each tache in taches
{
    sendmail
    [
        from: zoho.adminuserid
        to: tache.Assignee
        subject: "⚠️ Tâche arrivant à échéance aujourd'hui"
        message: "La tâche \"" + tache.Titre + "\" arrive à échéance aujourd'hui."
    ];
}
```

```deluge
// Schedule : Tous les lundis à 9h00
// Nom : "Rapport_Hebdomadaire"

// Compter les nouveaux enregistrements de la semaine passée
lundi = zoho.currentdate.subDay(7);
nouveaux = Commandes[Date_Creation >= lundi].count();
totalCA = Commandes[Date_Creation >= lundi].sum(Montant);
fermes = Commandes[Date_Creation >= lundi && Statut == "Terminé"].count();

sendmail
[
    from: zoho.adminuserid
    to: "direction@entreprise.com"
    subject: "📊 Rapport hebdomadaire - Semaine du " + lundi.toString("dd/MM/yyyy")
    message: "<h2>Rapport hebdomadaire</h2>" +
             "<ul>" +
             "<li>Nouvelles commandes : " + nouveaux + "</li>" +
             "<li>CA total : " + totalCA + " €</li>" +
             "<li>Commandes terminées : " + fermes + "</li>" +
             "</ul>"
];
```

```deluge
// Schedule : Le 1er de chaque mois à 6h00
// Nom : "Archivage_Mensuel"

// Archiver les enregistrements anciens
anciens = Tickets[Statut == "Fermé" && Date_Cloture < zoho.currentdate.subMonth(6)];
for each ticket in anciens
{
    // Copier dans le formulaire d'archive
    archiveMap = Map();
    archiveMap.put("Numero", ticket.Numero);
    archiveMap.put("Sujet", ticket.Sujet);
    archiveMap.put("Date_Creation", ticket.Date_Creation);
    archiveMap.put("Date_Cloture", ticket.Date_Cloture);
    insert into Archives_Tickets values archiveMap;

    // Supprimer l'original
    delete ticket;
}
```

---

## Actions disponibles

### Envoi d'emails

```deluge
sendmail
[
    from: zoho.adminuserid
    to: "destinataire@email.com"
    cc: "copie@email.com"
    bcc: "copie-cachee@email.com"
    subject: "Sujet de l'email"
    message: "<html><body><h1>Contenu HTML</h1></body></html>"
];
```

### SMS (via intégration)

```deluge
// Via Zoho SMS ou Twilio
response = invokeurl
[
    url: "https://api.twilio.com/2010-04-01/Accounts/ACCOUNT_SID/Messages.json"
    type: POST
    parameters: {
        "From": "+33XXXXXXXXX",
        "To": input.Telephone,
        "Body": "Votre commande " + input.Numero + " est prête."
    }
    connection: "twilio_conn"
];
```

### Webhook

```deluge
payload = Map();
payload.put("event", "commande_creee");
payload.put("id", input.ID);
payload.put("numero", input.Numero);
payload.put("montant", input.Montant);
payload.put("timestamp", zoho.currenttime.toString());

response = invokeurl
[
    url: "https://hooks.slack.com/services/T00/B00/XXXX"
    type: POST
    headers: {"Content-Type": "application/json"}
    parameters: {"text": "Nouvelle commande " + input.Numero + " - " + input.Montant + "€"}.toString()
];
```

### Intégrations Zoho

```deluge
// Créer un contact CRM
crmMap = Map();
crmMap.put("Last_Name", input.Nom);
crmMap.put("Email", input.Email);
crmMap.put("Phone", input.Telephone);
zoho.crm.createRecord("Contacts", crmMap);

// Créer une facture Books
invoiceMap = Map();
invoiceMap.put("customer_name", input.Client);
invoiceMap.put("date", zoho.currentdate.toString("yyyy-MM-dd"));
response = invokeurl
[
    url: "https://www.zohoapis.eu/books/v3/invoices?organization_id=ORG_ID"
    type: POST
    parameters: invoiceMap.toString()
    connection: "zoho_books"
];

// Créer un ticket Desk
ticketMap = Map();
ticketMap.put("subject", input.Sujet);
ticketMap.put("email", input.Email);
ticketMap.put("departmentId", "DEPT_ID");
response = invokeurl
[
    url: "https://desk.zoho.eu/api/v1/tickets"
    type: POST
    headers: {"Content-Type": "application/json"}
    parameters: ticketMap.toString()
    connection: "zoho_desk"
];
```

---

## Blueprints

Les blueprints définissent un flux de travail structuré pour les transitions d'état.

### Configuration

```
États : Brouillon → Soumis → En revue → Approuvé → Rejeté
                                  ↓
                              Modification demandée → Soumis

Transition "Soumettre" :
  - De : Brouillon
  - Vers : Soumis
  - Conditions : Tous les champs obligatoires remplis
  - Actions : Notifier le reviewer
  - Permissions : Créateur de l'enregistrement

Transition "Approuver" :
  - De : En revue
  - Vers : Approuvé
  - Conditions : Commentaire obligatoire
  - Actions : Notifier le demandeur, mettre à jour la date
  - Permissions : Rôle Manager uniquement
```

### Script de transition

```deluge
// Before transition "Approuver"
if (input.Montant > 10000 && input.Justificatif == "")
{
    cancel transition;
    alert "Un justificatif est obligatoire pour les montants > 10 000€";
}

// After transition "Approuver"
input.Date_Approbation = zoho.currentdate;
input.Approbateur = zoho.loginuser;
sendmail
[
    from: zoho.adminuserid
    to: input.Demandeur_Email
    subject: "✅ Votre demande a été approuvée"
    message: "Votre demande " + input.Numero + " a été approuvée par " + zoho.loginuser
];
```

---

## Approbations

### Configuration du processus d'approbation

```
Formulaire : Demandes_Achat

Critères : Montant > 500

Niveaux d'approbation :
  Niveau 1 : Chef de service (si Montant <= 5000)
  Niveau 2 : Directeur (si Montant > 5000)
  Niveau 3 : DG (si Montant > 20000)

Actions On Approve :
  - Statut = "Approuvé"
  - Notifier le demandeur

Actions On Reject :
  - Statut = "Rejeté"
  - Notifier le demandeur avec le motif
```

### Script d'approbation

```deluge
// On Approval Submit
input.Date_Soumission = zoho.currentdate;
input.Statut = "En attente d'approbation";

// On Approve
input.Statut = "Approuvé";
input.Date_Approbation = zoho.currentdate;

// On Reject (le commentaire de rejet est dans approval.comments)
input.Statut = "Rejeté";
input.Motif_Rejet = approval.comments;
```

---

## Notifications push

```deluge
// Notification dans l'application Creator
zoho.creator.sendNotification(
    "Nouvelle commande",
    "Commande " + input.Numero + " de " + input.Client,
    "user@entreprise.com"
);
```

---

## Bonnes pratiques

1. **Nommer clairement** chaque workflow avec son objectif
2. **Conditions précises** : Éviter les workflows qui se déclenchent trop souvent
3. **Logs** : Utiliser `info` pour tracer les exécutions dans les logs
4. **Erreurs** : Toujours gérer les cas d'erreur des API externes
5. **Performances** : Les schedules lourds doivent tourner en heures creuses
6. **Tests** : Tester chaque workflow dans un environnement sandbox avant la prod
7. **Limites** : Max 5 workflows par formulaire/événement (recommandé)

---

*Voir aussi : [formulaires.md](formulaires.md) | [deluge-creator.md](deluge-creator.md) | [../../zoho-deluge/](../../zoho-deluge/)*
