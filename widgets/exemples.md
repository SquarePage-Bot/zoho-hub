# Widgets Zoho - Exemples Concrets

## Exemple 1 : Widget de Score Client (CRM Sidebar)

Afficher un score de santé client calculé depuis des données externes.

### index.html
```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <script src="https://live.zwidgets.com/js-sdk/1.2/ZohoEmbeddedAppSDK.min.js"></script>
  <style>
    body { font-family: Arial, sans-serif; padding: 15px; margin: 0; }
    .score-circle {
      width: 120px; height: 120px; border-radius: 50%;
      display: flex; align-items: center; justify-content: center;
      font-size: 36px; font-weight: bold; color: white; margin: 20px auto;
    }
    .score-high { background: #4CAF50; }
    .score-mid { background: #FF9800; }
    .score-low { background: #F44336; }
    .metric { display: flex; justify-content: space-between; padding: 8px 0;
              border-bottom: 1px solid #eee; }
    .metric-label { color: #666; }
    .metric-value { font-weight: bold; }
    h3 { color: #333; margin-top: 20px; }
  </style>
</head>
<body>
  <h3>Score de Santé Client</h3>
  <div id="score-display"></div>
  <div id="metrics"></div>

  <script>
    ZOHO.embeddedApp.on("PageLoad", function(data) {
      var entityId = data.EntityId;
      loadClientScore(entityId);
    });

    function loadClientScore(contactId) {
      ZOHO.CRM.API.getRecord({
        Entity: "Contacts",
        RecordID: contactId
      }).then(function(response) {
        var contact = response.data[0];
        
        // Calculer le score (exemple simplifié)
        var score = calculateScore(contact);
        displayScore(score);
        displayMetrics(contact, score);
      });
    }

    function calculateScore(contact) {
      var score = 50; // Base
      if (contact.Last_Activity_Time) {
        var daysSinceActivity = daysBetween(
          new Date(contact.Last_Activity_Time), new Date()
        );
        if (daysSinceActivity < 7) score += 20;
        else if (daysSinceActivity < 30) score += 10;
        else score -= 10;
      }
      if (contact.Number_of_Deals > 0) score += 15;
      if (contact.Email_Opt_Out === false) score += 5;
      return Math.min(100, Math.max(0, score));
    }

    function displayScore(score) {
      var level = score >= 70 ? 'high' : score >= 40 ? 'mid' : 'low';
      document.getElementById('score-display').innerHTML =
        '<div class="score-circle score-' + level + '">' + score + '</div>';
    }

    function displayMetrics(contact, score) {
      var html = '';
      html += metric("Dernière activité", contact.Last_Activity_Time || "Aucune");
      html += metric("Deals actifs", contact.Number_of_Deals || 0);
      html += metric("Opt-in email", contact.Email_Opt_Out ? "Non" : "Oui");
      html += metric("Score", score + "/100");
      document.getElementById('metrics').innerHTML = html;
    }

    function metric(label, value) {
      return '<div class="metric"><span class="metric-label">' + label +
             '</span><span class="metric-value">' + value + '</span></div>';
    }

    function daysBetween(d1, d2) {
      return Math.floor((d2 - d1) / (1000 * 60 * 60 * 24));
    }

    ZOHO.embeddedApp.init();
  </script>
</body>
</html>
```

### plugin-manifest.json
```json
{
  "service": "CRM",
  "components": {
    "widgets": [{
      "widgetNamespace": "client_score",
      "location": "crm.contacts.widgets.sidebar",
      "name": "Score Client",
      "url": "/app/index.html",
      "logo": "/app/images/score-icon.png"
    }]
  },
  "version": "1.0.0"
}
```

## Exemple 2 : Widget de Recherche Entreprise (CRM Button)

Rechercher des informations d'entreprise via une API externe.

```javascript
// app.js
ZOHO.embeddedApp.on("PageLoad", function(data) {
  var entityId = data.EntityId;
  
  ZOHO.CRM.API.getRecord({
    Entity: "Accounts",
    RecordID: entityId
  }).then(function(response) {
    var account = response.data[0];
    var siret = account.SIRET;
    
    if (siret) {
      // Appel API entreprise (ex: API Sirene)
      ZOHO.CRM.HTTP.get({
        url: "https://entreprise.data.gouv.fr/api/sirene/v3/etablissements/" + siret
      }).then(function(apiResponse) {
        var data = JSON.parse(apiResponse);
        var etab = data.etablissement;
        
        // Afficher les infos
        document.getElementById("result").innerHTML = `
          <h3>${etab.denomination}</h3>
          <p><strong>SIRET :</strong> ${etab.siret}</p>
          <p><strong>Adresse :</strong> ${etab.adresse_complete}</p>
          <p><strong>NAF :</strong> ${etab.activite_principale}</p>
          <p><strong>Effectif :</strong> ${etab.tranche_effectifs}</p>
          <p><strong>Date création :</strong> ${etab.date_creation}</p>
        `;
        
        // Proposer la mise à jour CRM
        document.getElementById("update-btn").style.display = "block";
        document.getElementById("update-btn").onclick = function() {
          ZOHO.CRM.API.updateRecord({
            Entity: "Accounts",
            RecordID: entityId,
            APIData: {
              Billing_Street: etab.adresse_complete,
              Employees: etab.tranche_effectifs,
              SIC_Code: etab.activite_principale
            }
          }).then(function() {
            ZOHO.CRM.UI.Popup.alert({ message: "Fiche mise à jour !" });
          });
        };
      });
    } else {
      document.getElementById("result").innerHTML = 
        "<p>Aucun SIRET renseigné sur cette fiche.</p>";
    }
  });
});

ZOHO.embeddedApp.init();
```

## Exemple 3 : Widget Desk - Historique Commandes

Afficher l'historique des commandes du client directement dans Zoho Desk.

```javascript
ZOHO.embeddedApp.on("PageLoad", function(data) {
  var ticketId = data.ticket_id;
  
  // Récupérer le ticket
  ZOHO.DESK.API.getTicket().then(function(ticketData) {
    var email = ticketData.ticket.email;
    
    // Chercher le contact dans CRM
    ZOHO.CRM.API.searchRecord({
      Entity: "Contacts",
      Type: "email",
      Query: email
    }).then(function(crmData) {
      if (crmData.data && crmData.data.length > 0) {
        var contactId = crmData.data[0].id;
        
        // Récupérer les commandes associées
        ZOHO.CRM.API.getRelatedRecords({
          Entity: "Contacts",
          RecordID: contactId,
          RelatedList: "Sales_Orders"
        }).then(function(orders) {
          displayOrders(orders.data);
        });
      }
    });
  });
});

function displayOrders(orders) {
  var html = '<table><tr><th>N°</th><th>Date</th><th>Montant</th><th>Statut</th></tr>';
  orders.forEach(function(order) {
    html += '<tr>';
    html += '<td>' + order.SO_Number + '</td>';
    html += '<td>' + order.Date + '</td>';
    html += '<td>' + order.Grand_Total + '€</td>';
    html += '<td>' + order.Status + '</td>';
    html += '</tr>';
  });
  html += '</table>';
  document.getElementById("orders-list").innerHTML = html;
}

ZOHO.embeddedApp.init();
```

## Bonnes Pratiques pour les Widgets

1. **UI simple et claire** — l'espace est limité (sidebar = ~300px de large)
2. **Chargement asynchrone** — afficher un loader pendant les appels API
3. **Gestion d'erreurs** — toujours un message si les données sont indisponibles
4. **Responsive** — adapter l'affichage à l'emplacement du widget
5. **Pas de dépendances lourdes** — éviter les gros frameworks (React complet) pour un widget simple
6. **Cacher les tokens** — utiliser les connexions Zoho, jamais de secrets en clair
