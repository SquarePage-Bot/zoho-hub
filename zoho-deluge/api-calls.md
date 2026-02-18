# 🌐 Zoho Deluge - Appels API (invokeurl)

> Appeler des API REST externes et internes depuis Deluge.

## invokeurl - Syntaxe de base

```deluge
response = invokeurl
[
    url: "https://api.example.com/endpoint"
    type: GET
    headers: {"Authorization": "Bearer TOKEN", "Content-Type": "application/json"}
    parameters: {"key": "value"}
    connection: "nom_connexion"
];

info response;
```

### Paramètres

| Paramètre | Description | Obligatoire |
|-----------|-------------|-------------|
| `url` | URL de l'API | ✅ |
| `type` | Méthode HTTP : GET, POST, PUT, PATCH, DELETE | ✅ |
| `headers` | Headers HTTP (Map) | ❌ |
| `parameters` | Body ou query params (Map ou String) | ❌ |
| `content-type` | Type de contenu (alternative à headers) | ❌ |
| `connection` | Nom de la connexion Zoho (OAuth) | ❌ |
| `detailed` | `true` pour obtenir headers + status code | ❌ |

## Méthodes HTTP

### GET

```deluge
// Simple
response = invokeurl
[
    url: "https://api.example.com/users?page=1&limit=10"
    type: GET
    headers: {"Authorization": "Bearer TOKEN"}
];

data = response.toMap();
info data;
```

### POST (JSON)

```deluge
payload = Map();
payload.put("name", "Jean Dupont");
payload.put("email", "jean@acme.com");

response = invokeurl
[
    url: "https://api.example.com/users"
    type: POST
    headers: {"Authorization": "Bearer TOKEN", "Content-Type": "application/json"}
    parameters: payload.toString()
];

info response;
```

### POST (Form Data)

```deluge
response = invokeurl
[
    url: "https://api.example.com/login"
    type: POST
    parameters: {"username": "admin", "password": "secret"}
];
```

### PUT

```deluge
updateData = Map();
updateData.put("name", "Jean-Pierre Dupont");

response = invokeurl
[
    url: "https://api.example.com/users/123"
    type: PUT
    headers: {"Content-Type": "application/json"}
    parameters: updateData.toString()
];
```

### DELETE

```deluge
response = invokeurl
[
    url: "https://api.example.com/users/123"
    type: DELETE
    headers: {"Authorization": "Bearer TOKEN"}
];
```

## Réponse détaillée

```deluge
response = invokeurl
[
    url: "https://api.example.com/data"
    type: GET
    headers: {"Authorization": "Bearer TOKEN"}
    detailed: true
];

// La réponse contient : statusCode, responseHeaders, responseText
statusCode = response.get("statusCode");
headers = response.get("responseHeaders");
body = response.get("responseText");

info "Status : " + statusCode;  // 200
info "Body : " + body;

if(statusCode == 200)
{
    data = body.toMap();
    info data;
}
else
{
    info "Erreur : " + statusCode;
}
```

## Connexions Zoho (OAuth)

### Concept

Les connexions permettent d'appeler les API Zoho avec l'authentification OAuth gérée automatiquement.

### Créer une connexion

```
Setup → Personnalisation → Connexions → Créer une connexion
  Service : Zoho OAuth
  Nom : "zoho_crm_connection"
  Scopes : ZohoCRM.modules.ALL, ZohoCRM.settings.ALL
```

### Utiliser une connexion

```deluge
// Appeler l'API CRM via connexion
response = invokeurl
[
    url: "https://www.zohoapis.eu/crm/v6/Deals"
    type: GET
    connection: "zoho_crm_connection"
];

deals = response.get("data");
for each deal in deals
{
    info deal.get("Deal_Name") + " - " + deal.get("Amount");
}
```

### Connexions vers services tiers

```
Setup → Connexions → Créer une connexion
  Service : Custom
  Nom : "slack_webhook"
  Base URL : https://hooks.slack.com
  Auth : Aucune (token dans l'URL)
```

## Exemples d'intégration

### Slack

```deluge
// Envoyer un message Slack
slackUrl = "https://hooks.slack.com/services/T00/B00/xxxx";

message = Map();
message.put("text", "🎉 Nouveau deal gagné : " + dealName + " - " + amount + "€");
message.put("channel", "#sales");

response = invokeurl
[
    url: slackUrl
    type: POST
    headers: {"Content-Type": "application/json"}
    parameters: message.toString()
];
```

### Stripe - Créer un client

```deluge
response = invokeurl
[
    url: "https://api.stripe.com/v1/customers"
    type: POST
    headers: {"Authorization": "Bearer sk_live_xxxx"}
    parameters: {
        "email": email,
        "name": nomComplet,
        "metadata[crm_id]": crmId
    }
];

stripeCustomerId = response.toMap().get("id");
info "Client Stripe créé : " + stripeCustomerId;
```

### Google Sheets - Ajouter une ligne

```deluge
// Via connexion Google OAuth
sheetId = "1ABC...XYZ";
range = "Sheet1!A:D";

values = List();
row = List();
row.add(dealName);
row.add(amount.toString());
row.add(stage);
row.add(zoho.currentdate.toString("yyyy-MM-dd"));
values.add(row);

body = Map();
body.put("values", values);

response = invokeurl
[
    url: "https://sheets.googleapis.com/v4/spreadsheets/" + sheetId + "/values/" + encodeUrl(range) + ":append?valueInputOption=USER_ENTERED"
    type: POST
    headers: {"Content-Type": "application/json"}
    parameters: body.toString()
    connection: "google_sheets_connection"
];
```

### SendGrid - Envoyer un email

```deluge
emailBody = Map();
emailBody.put("personalizations", [{
    "to": [{"email": destinataire}],
    "subject": sujet
}]);
emailBody.put("from", {"email": "noreply@squarepage.fr", "name": "SquarePage"});
emailBody.put("content", [{"type": "text/html", "value": contenuHtml}]);

response = invokeurl
[
    url: "https://api.sendgrid.com/v3/mail/send"
    type: POST
    headers: {
        "Authorization": "Bearer SG.xxxx",
        "Content-Type": "application/json"
    }
    parameters: emailBody.toString()
];
```

### Webhook générique

```deluge
// Envoyer un webhook avec les données du CRM
deal = zoho.crm.getRecordById("Deals", dealId);

webhookData = Map();
webhookData.put("event", "deal_won");
webhookData.put("timestamp", zoho.currenttime.toString("yyyy-MM-dd'T'HH:mm:ssZ"));
webhookData.put("deal", Map());
webhookData.get("deal").put("id", dealId);
webhookData.get("deal").put("name", deal.get("Deal_Name"));
webhookData.get("deal").put("amount", deal.get("Amount"));
webhookData.get("deal").put("stage", deal.get("Stage"));

response = invokeurl
[
    url: "https://myapp.com/webhooks/zoho"
    type: POST
    headers: {
        "Content-Type": "application/json",
        "X-Webhook-Secret": "mon_secret_123"
    }
    parameters: webhookData.toString()
];
```

## Upload de fichier

```deluge
// Télécharger un fichier puis l'uploader ailleurs
file = invokeurl
[
    url: "https://example.com/document.pdf"
    type: GET
];

// Uploader vers une API
response = invokeurl
[
    url: "https://api.example.com/upload"
    type: POST
    headers: {"Authorization": "Bearer TOKEN"}
    files: file
];
```

## Gestion des erreurs API

```deluge
try
{
    response = invokeurl
    [
        url: "https://api.example.com/data"
        type: GET
        headers: {"Authorization": "Bearer TOKEN"}
        detailed: true
    ];
    
    statusCode = response.get("statusCode");
    
    if(statusCode == 200)
    {
        data = response.get("responseText").toMap();
        // Traiter les données
    }
    else if(statusCode == 401)
    {
        info "Token expiré, rafraîchir l'authentification";
    }
    else if(statusCode == 429)
    {
        info "Rate limit atteint, réessayer plus tard";
        // Optionnel : sleep et retry
        sleep(5000);
        // Retry...
    }
    else
    {
        info "Erreur " + statusCode + " : " + response.get("responseText");
    }
}
catch(e)
{
    info "Exception : " + e.getMessage();
}
```

## Limites

| Limite | Valeur |
|--------|--------|
| Timeout par appel | 10 secondes |
| Taille de réponse max | 50 MB |
| Appels invokeurl par exécution | 50 |
| Connexions par organisation | 30 |
| URL autorisées | Toute URL HTTPS publique |

## Bonnes pratiques

1. **Toujours `detailed: true`** pour les appels critiques (vérifier le status code)
2. **Try/catch** : Entourer chaque appel externe
3. **Timeout** : Prévoir le cas où l'API ne répond pas (10s max)
4. **Secrets** : Stocker les tokens dans les connexions Zoho, pas en dur dans le code
5. **Logs** : Logger les réponses pour le debug
6. **Retry** : Implémenter un retry pour les erreurs temporaires (429, 500)
7. **JSON** : Utiliser `.toString()` pour les maps en body JSON, pas la map directement

---
*Voir aussi : [crm-functions.md](crm-functions.md) pour les appels CRM natifs, [fonctions.md](fonctions.md) pour le parsing JSON.*
