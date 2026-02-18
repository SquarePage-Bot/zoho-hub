# 🤖 Zoho Books - Automatisations

> Workflows, règles, webhooks et intégrations automatiques.

## Table des matières

- [Workflow Rules](#workflow-rules)
- [Webhooks](#webhooks)
- [Intégration Zoho Flow](#intégration-zoho-flow)
- [Intégration CRM](#intégration-crm)
- [Scripts Deluge (Custom Functions)](#scripts-deluge)
- [Notifications automatiques](#notifications-automatiques)

---

## Workflow Rules

### Vue d'ensemble

```
Paramètres → Automatisation → Workflow Rules

Modules supportés :
- Factures (Invoices)
- Devis (Estimates)
- Dépenses (Expenses)
- Factures fournisseurs (Bills)
- Bons de commande (Purchase Orders)
- Avoirs (Credit Notes)
- Ordres de vente (Sales Orders)
```

### Structure d'un workflow

```
Déclencheur (Trigger)
  → Quand ? (création, modification, suppression, ou basé sur date)
  → Conditions (filtres)
    → Actions
```

### Exemples de workflows

**1. Notification facture en retard**
```
Module : Factures
Déclencheur : Basé sur la date (Date d'échéance)
Quand : 1 jour après la date d'échéance
Condition : Statut ≠ "Payée"
Actions :
  - Email au responsable : "La facture {invoice.number} de {customer.name} est en retard"
  - Email au client : rappel de paiement
```

**2. Alerte gros montant**
```
Module : Factures
Déclencheur : À la création
Condition : Montant > 10 000 €
Actions :
  - Email au directeur financier
  - Webhook vers Slack
```

**3. Mise à jour CRM lors du paiement**
```
Module : Factures
Déclencheur : À la modification
Condition : Statut devient "Payée"
Actions :
  - Custom Function (Deluge) : mettre à jour le deal CRM
  - Email de remerciement au client
```

### Types d'actions

| Action | Description |
|--------|-------------|
| **Email Alert** | Envoyer un email à un ou plusieurs destinataires |
| **In-App Notification** | Notification dans Zoho Books |
| **Webhook** | Appel HTTP POST vers une URL externe |
| **Custom Function** | Script Deluge personnalisé |
| **Field Update** | Mise à jour automatique d'un champ |

---

## Webhooks

### Configuration

```
Paramètres → Automatisation → Webhooks → + Nouveau

Champs :
- Nom du webhook
- Module (Factures, Contacts...)
- URL cible
- Méthode : POST
- Headers personnalisés
- Body (JSON avec variables)
```

### Exemple : Notification Slack

```json
URL : https://hooks.slack.com/services/T00/B00/XXXX
Méthode : POST
Content-Type : application/json

Body :
{
  "text": "💰 Paiement reçu !",
  "blocks": [
    {
      "type": "section",
      "text": {
        "type": "mrkdwn",
        "text": "*Facture ${invoice.number}*\nClient : ${customer.name}\nMontant : ${invoice.total} €\nStatut : ${invoice.status}"
      }
    }
  ]
}
```

### Exemple : Webhook vers API interne

```json
URL : https://api.entreprise.com/webhooks/zoho-books
Méthode : POST
Headers :
  Authorization: Bearer SECRET_TOKEN
  Content-Type: application/json

Body :
{
  "event": "invoice.paid",
  "invoice_id": "${invoice.invoice_id}",
  "invoice_number": "${invoice.number}",
  "customer_name": "${customer.name}",
  "customer_email": "${customer.email}",
  "amount": "${invoice.total}",
  "payment_date": "${invoice.last_payment_date}",
  "currency": "${invoice.currency_code}"
}
```

### Variables disponibles

```
${invoice.number}           → Numéro de facture
${invoice.total}            → Montant TTC
${invoice.balance_due}      → Reste à payer
${invoice.status}           → Statut
${invoice.date}             → Date
${invoice.due_date}         → Échéance
${customer.name}            → Nom du client
${customer.email}           → Email du client
${organization.name}        → Nom de l'entreprise
```

---

## Intégration Zoho Flow

```
Zoho Flow permet des automatisations avancées multi-apps :

Exemples de flux :
1. Books : Facture payée → Flow → Slack : notification
2. Books : Nouveau client → Flow → Mailchimp : ajout à la liste
3. CRM : Deal gagné → Flow → Books : créer facture
4. Books : Facture en retard → Flow → Trello : créer carte
5. Stripe : Paiement reçu → Flow → Books : enregistrer paiement
```

---

## Intégration CRM

### Synchronisation automatique

```
Paramètres → Intégrations → Zoho CRM → Configurer

Sync Contacts :
  CRM Contacts → Books Clients (auto-création)
  CRM Vendors → Books Fournisseurs

Sync Transactions :
  CRM Deal gagné → Books Facture (création automatique)
  Books Facture payée → CRM Deal statut mis à jour

Sync Articles :
  CRM Products → Books Items
```

### Mapping des champs

```
CRM                    →    Books
Last_Name              →    Contact Name
Email                  →    Email
Phone                  →    Phone
Mailing_Street         →    Billing Address
Account_Name           →    Company Name
```

---

## Scripts Deluge

### Custom Functions dans Books

```
Paramètres → Automatisation → Custom Functions → Nouvelle

Modules : Factures, Contacts, Dépenses, etc.
Événement : Création, Modification, Suppression
```

### Exemple : Créer un deal CRM à la création d'une facture

```deluge
// Trigger : Facture créée
invoiceId = invoice.get("invoice_id");
customerName = invoice.get("customer_name");
total = invoice.get("total");
invoiceNumber = invoice.get("invoice_number");

// Chercher le contact CRM
contacts = zoho.crm.searchRecords("Contacts", "(Email:equals:" + invoice.get("email") + ")");

if (contacts.size() > 0)
{
    contactId = contacts.get(0).get("id");

    dealMap = Map();
    dealMap.put("Deal_Name", "Facture " + invoiceNumber);
    dealMap.put("Stage", "Closed Won");
    dealMap.put("Amount", total);
    dealMap.put("Contact_Name", contactId);
    dealMap.put("Closing_Date", zoho.currentdate.toString("yyyy-MM-dd"));

    zoho.crm.createRecord("Deals", dealMap);
}
```

### Exemple : Alerte trésorerie basse

```deluge
// Schedule : tous les jours à 9h
// Vérifier le solde bancaire

orgId = "ORG_ID";
response = invokeurl
[
    url: "https://www.zohoapis.eu/books/v3/bankaccounts?organization_id=" + orgId
    type: GET
    connection: "zoho_books"
];

accounts = response.get("bankaccounts");
for each account in accounts
{
    solde = account.get("balance").toDecimal();
    if (solde < 5000)
    {
        sendmail
        [
            from: zoho.adminuserid
            to: "directeur@entreprise.com"
            subject: "⚠️ Solde bas : " + account.get("account_name")
            message: "Le compte " + account.get("account_name") + " a un solde de " + solde + " €."
        ];
    }
}
```

---

## Notifications automatiques

### Types de notifications

```
Paramètres → Notifications

Notifications par email :
- Facture envoyée → Confirmation au vendeur
- Paiement reçu → Reçu au client + notification interne
- Devis accepté → Notification au commercial
- Facture en retard → Alerte au gestionnaire

Notifications in-app :
- Bell icon dans Zoho Books
- Zoho Cliq (chat d'équipe)
```

### Configuration des emails automatiques

```
Paramètres → Emails

Templates personnalisables pour :
- Envoi de facture
- Rappel de paiement
- Confirmation de paiement
- Envoi de devis
- Remerciement

Chaque template peut être personnalisé avec les variables du document.
```

---

## Bonnes pratiques

1. **Workflows** : Commencer par les cas simples (notifications) puis complexifier
2. **Webhooks** : Toujours vérifier la sécurité (tokens, IP whitelisting)
3. **Custom Functions** : Tester dans un environnement sandbox
4. **CRM Sync** : Définir clairement quelle source est prioritaire
5. **Notifications** : Ne pas surcharger les utilisateurs, cibler les alertes
6. **Logs** : Vérifier régulièrement les logs d'exécution des workflows

---

*Voir aussi : [configuration.md](configuration.md) | [factures.md](factures.md) | [api.md](api.md)*
