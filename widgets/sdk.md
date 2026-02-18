# Widgets Zoho - SDK

## Installation du SDK

### Inclure le SDK
```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Mon Widget</title>
  <script src="https://live.zwidgets.com/js-sdk/1.2/ZohoEmbeddedAppSDK.min.js"></script>
</head>
<body>
  <div id="widget-content"></div>
  <script src="app.js"></script>
</body>
</html>
```

### Initialisation
```javascript
// app.js
ZOHO.embeddedApp.on("PageLoad", function(data) {
  // data contient le contexte (module, record ID, etc.)
  console.log("Widget chargé avec :", data);
  
  // Votre logique ici
  initWidget(data);
});

// Initialiser le SDK
ZOHO.embeddedApp.init();
```

## API du SDK

### Récupérer des Données CRM

```javascript
// Récupérer l'enregistrement courant
ZOHO.CRM.API.getRecord({
  Entity: "Leads",
  RecordID: "123456789"
}).then(function(data) {
  var lead = data.data[0];
  console.log("Nom :", lead.Last_Name);
  console.log("Email :", lead.Email);
  console.log("Score :", lead.Lead_Score);
});

// Rechercher des enregistrements
ZOHO.CRM.API.searchRecord({
  Entity: "Contacts",
  Type: "email",
  Query: "jean@example.com"
}).then(function(data) {
  console.log("Résultats :", data.data);
});

// Créer un enregistrement
ZOHO.CRM.API.insertRecord({
  Entity: "Leads",
  APIData: {
    Last_Name: "Dupont",
    Email: "jean@example.com",
    Company: "ABC Corp",
    Lead_Source: "Widget"
  }
}).then(function(data) {
  console.log("Lead créé :", data.data[0].details.id);
});

// Mettre à jour un enregistrement
ZOHO.CRM.API.updateRecord({
  Entity: "Leads",
  RecordID: "123456789",
  APIData: {
    Lead_Status: "Contacté",
    Description: "Mis à jour via widget"
  }
});
```

### Récupérer le Contexte

```javascript
// Informations sur l'utilisateur courant
ZOHO.CRM.CONFIG.getCurrentUser().then(function(data) {
  var user = data.users[0];
  console.log("Utilisateur :", user.full_name);
  console.log("Rôle :", user.role.name);
  console.log("Profil :", user.profile.name);
});

// Informations sur l'organisation
ZOHO.CRM.CONFIG.getOrgInfo().then(function(data) {
  console.log("Organisation :", data.org[0].company_name);
});
```

### Appels HTTP (vers des APIs externes)

```javascript
// Appel vers une API externe via le proxy Zoho
ZOHO.CRM.HTTP.get({
  url: "https://api.externe.com/data",
  headers: {
    "Authorization": "Bearer TOKEN_EXTERNE"
  }
}).then(function(response) {
  var data = JSON.parse(response);
  console.log(data);
});

// POST vers une API externe
ZOHO.CRM.HTTP.post({
  url: "https://api.externe.com/webhook",
  headers: {
    "Content-Type": "application/json"
  },
  body: {
    event: "widget_action",
    record_id: "123456789"
  }
}).then(function(response) {
  console.log("Réponse :", response);
});
```

### Interface Utilisateur

```javascript
// Afficher une notification
ZOHO.CRM.UI.Popup.alert({
  message: "Action effectuée avec succès !"
});

// Boîte de dialogue de confirmation
ZOHO.CRM.UI.Popup.confirm({
  message: "Êtes-vous sûr de vouloir continuer ?"
}).then(function(response) {
  if (response === "yes") {
    // Action confirmée
  }
});

// Redimensionner le widget
ZOHO.CRM.UI.Resize({
  height: "500",
  width: "100%"
});

// Fermer le widget (si popup)
ZOHO.CRM.UI.Popup.close();

// Naviguer vers un enregistrement
ZOHO.CRM.UI.Record.open({
  Entity: "Contacts",
  RecordID: "987654321"
});
```

### Connexions (pour les tokens sécurisés)

```javascript
// Utiliser une connexion Zoho (OAuth stocké côté serveur)
ZOHO.CRM.CONNECTOR.invokeAPI("ma_connexion.api_externe", {
  method: "GET",
  url: "/endpoint"
}).then(function(response) {
  console.log(response);
});
```

## SDK Zoho Desk

```javascript
// Initialisation Desk
ZOHO.embeddedApp.on("PageLoad", function(data) {
  // data.ticket_id pour un widget de ticket
});

// Récupérer le ticket courant
ZOHO.DESK.API.getTicket().then(function(data) {
  console.log("Sujet :", data.ticket.subject);
  console.log("Statut :", data.ticket.status);
});

// Ajouter un commentaire
ZOHO.DESK.API.addComment({
  ticketId: "TICKET_ID",
  comment: "Commentaire depuis le widget",
  isPublic: false
});
```

## Bonnes Pratiques SDK

1. **Toujours initialiser** avec `ZOHO.embeddedApp.init()`
2. **Gérer les erreurs** avec `.catch()` sur chaque appel
3. **Utiliser les connexions** plutôt que des tokens en dur
4. **Minimiser les appels API** — mettre en cache quand possible
5. **Tester dans le sandbox** avant déploiement en production
