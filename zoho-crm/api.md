# 🔌 Zoho CRM - API REST

> Documentation complète de l'API REST Zoho CRM v6 : authentification, endpoints, exemples.

## Vue d'ensemble

- **Base URL** : `https://www.zohoapis.eu/crm/v6/` (EU) ou `https://www.zohoapis.com/crm/v6/` (US)
- **Format** : JSON
- **Authentification** : OAuth 2.0
- **Rate limit** : Selon l'édition (1 000 à 10 000 calls/jour/org)

## Authentification OAuth 2.0

### Flux d'authentification

```
1. Enregistrer l'application → Client ID + Client Secret
2. Obtenir un Authorization Code (via navigateur)
3. Échanger le code contre Access Token + Refresh Token
4. Utiliser l'Access Token pour les appels API
5. Rafraîchir avec le Refresh Token quand expiré (1h)
```

### Étape 1 : Enregistrer l'application

```
URL : https://api-console.zoho.eu/
Type : Self Client (pour scripts serveur) ou Server-based (pour apps web)
```

### Étape 2 : Obtenir le code d'autorisation

```
GET https://accounts.zoho.eu/oauth/v2/auth
  ?scope=ZohoCRM.modules.ALL,ZohoCRM.settings.ALL
  &client_id=YOUR_CLIENT_ID
  &response_type=code
  &access_type=offline
  &redirect_uri=YOUR_REDIRECT_URI
```

### Étape 3 : Obtenir les tokens

```bash
curl -X POST "https://accounts.zoho.eu/oauth/v2/token" \
  -d "grant_type=authorization_code" \
  -d "client_id=YOUR_CLIENT_ID" \
  -d "client_secret=YOUR_CLIENT_SECRET" \
  -d "code=AUTH_CODE" \
  -d "redirect_uri=YOUR_REDIRECT_URI"
```

**Réponse :**
```json
{
  "access_token": "1000.xxxx.yyyy",
  "refresh_token": "1000.aaaa.bbbb",
  "api_domain": "https://www.zohoapis.eu",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

### Étape 4 : Rafraîchir le token

```bash
curl -X POST "https://accounts.zoho.eu/oauth/v2/token" \
  -d "grant_type=refresh_token" \
  -d "client_id=YOUR_CLIENT_ID" \
  -d "client_secret=YOUR_CLIENT_SECRET" \
  -d "refresh_token=YOUR_REFRESH_TOKEN"
```

### Scopes disponibles

| Scope | Description |
|-------|-------------|
| `ZohoCRM.modules.ALL` | Accès à tous les modules |
| `ZohoCRM.modules.{Module}.READ` | Lecture d'un module spécifique |
| `ZohoCRM.modules.{Module}.WRITE` | Écriture d'un module |
| `ZohoCRM.modules.{Module}.DELETE` | Suppression |
| `ZohoCRM.settings.ALL` | Accès aux paramètres |
| `ZohoCRM.coql.READ` | Requêtes COQL |
| `ZohoCRM.bulk.ALL` | Opérations en masse |
| `ZohoCRM.notifications.ALL` | Webhooks/notifications |

## Endpoints principaux

### CRUD sur les enregistrements

#### Lister les enregistrements

```bash
GET /v6/{module}
  ?fields=Last_Name,Email,Phone
  &sort_by=Created_Time
  &sort_order=desc
  &per_page=50
  &page=1

# Headers
Authorization: Zoho-oauthtoken {access_token}
```

**Réponse :**
```json
{
  "data": [
    {
      "id": "5234876000000123456",
      "Last_Name": "Dupont",
      "Email": "jean@acme.com",
      "Phone": "+33612345678",
      "Created_Time": "2026-02-15T10:30:00+01:00"
    }
  ],
  "info": {
    "per_page": 50,
    "count": 50,
    "page": 1,
    "more_records": true
  }
}
```

#### Obtenir un enregistrement

```bash
GET /v6/{module}/{record_id}
```

#### Créer un enregistrement

```bash
POST /v6/{module}

{
  "data": [
    {
      "Last_Name": "Dupont",
      "First_Name": "Jean",
      "Email": "jean@acme.com",
      "Company": "Acme Corp",
      "Lead_Source": "Site Web",
      "Industry": "Technology",
      "Annual_Revenue": 500000
    }
  ],
  "trigger": ["workflow", "blueprint"]
}
```

**Note :** Le champ `trigger` contrôle si les automatisations se déclenchent. Valeurs : `workflow`, `blueprint`, `approval`.

#### Créer en masse (jusqu'à 100)

```bash
POST /v6/{module}

{
  "data": [
    {"Last_Name": "Dupont", "Email": "jean@acme.com"},
    {"Last_Name": "Martin", "Email": "alice@beta.com"},
    {"Last_Name": "Bernard", "Email": "bob@gamma.com"}
  ]
}
```

#### Mettre à jour

```bash
PUT /v6/{module}/{record_id}

{
  "data": [
    {
      "id": "5234876000000123456",
      "Stage": "Négociation",
      "Amount": 35000
    }
  ]
}
```

#### Supprimer

```bash
DELETE /v6/{module}/{record_id}
```

#### Supprimer en masse

```bash
DELETE /v6/{module}?ids=id1,id2,id3
```

### Recherche

#### Par critères

```bash
GET /v6/{module}/search
  ?criteria=(Stage:equals:Négociation)and(Amount:greater_than:10000)
  &per_page=50
```

#### Par email

```bash
GET /v6/{module}/search?email=jean@acme.com
```

#### Par téléphone

```bash
GET /v6/{module}/search?phone=+33612345678
```

#### Par mot-clé

```bash
GET /v6/{module}/search?word=SquarePage
```

### COQL (CRM Query Language)

```bash
POST /v6/coql

{
  "select_query": "SELECT Deal_Name, Amount, Stage, Account_Name.Account_Name FROM Deals WHERE Stage = 'Négociation' AND Amount > 10000 ORDER BY Amount DESC LIMIT 100"
}
```

**Opérateurs COQL :**
```sql
=, !=, <, >, <=, >=
LIKE, NOT LIKE
IN, NOT IN
BETWEEN
IS NULL, IS NOT NULL
AND, OR
```

### Enregistrements liés (Related Records)

```bash
# Obtenir les contacts d'un compte
GET /v6/Accounts/{account_id}/Contacts

# Obtenir les deals d'un contact
GET /v6/Contacts/{contact_id}/Deals

# Obtenir les notes d'un deal
GET /v6/Deals/{deal_id}/Notes

# Lier des enregistrements
PUT /v6/{module}/{record_id}/{related_module}/{related_id}
```

### Conversion de lead

```bash
POST /v6/Leads/{lead_id}/actions/convert

{
  "data": [
    {
      "overwrite": true,
      "notify_lead_owner": true,
      "notify_new_entity_owner": true,
      "Accounts": {
        "Account_Name": "Acme Corp",
        "Website": "https://acme.com"
      },
      "Deals": {
        "Deal_Name": "Projet CRM Acme",
        "Closing_Date": "2026-06-30",
        "Stage": "Qualification",
        "Amount": 25000
      }
    }
  ]
}
```

### Blueprint API

```bash
# Obtenir le blueprint d'un enregistrement
GET /v6/{module}/{record_id}/actions/blueprint

# Exécuter une transition
PUT /v6/{module}/{record_id}/actions/blueprint

{
  "blueprint": [
    {
      "transition_id": "5234876000000456789",
      "data": {
        "Budget_estime": 25000,
        "Notes": "Client qualifié lors de l'appel du 15/02"
      }
    }
  ]
}
```

### Notes

```bash
# Créer une note
POST /v6/{module}/{record_id}/Notes

{
  "data": [
    {
      "Note_Title": "Compte-rendu appel",
      "Note_Content": "Discussion productive sur le projet CRM..."
    }
  ]
}
```

### Pièces jointes

```bash
# Upload
POST /v6/{module}/{record_id}/Attachments
Content-Type: multipart/form-data
file: @document.pdf

# Download
GET /v6/{module}/{record_id}/Attachments/{attachment_id}

# Liste
GET /v6/{module}/{record_id}/Attachments
```

## Métadonnées

### Modules disponibles

```bash
GET /v6/settings/modules
```

### Champs d'un module

```bash
GET /v6/settings/fields?module={module}
```

### Layouts

```bash
GET /v6/settings/layouts?module={module}
```

### Utilisateurs

```bash
GET /v6/users?type=ActiveUsers
```

### Rôles et profils

```bash
GET /v6/settings/roles
GET /v6/settings/profiles
```

## Opérations en masse (Bulk)

### Bulk Read

```bash
# Créer un job de lecture en masse
POST /v6/bulk-read

{
  "callback": {
    "url": "https://myapp.com/webhook",
    "method": "post"
  },
  "query": {
    "module": {
      "api_name": "Deals"
    },
    "criteria": {
      "group_operator": "and",
      "group": [
        {
          "api_name": "Stage",
          "comparator": "equal",
          "value": "Fermée gagnée"
        }
      ]
    },
    "page": 1
  }
}

# Vérifier le statut
GET /v6/bulk-read/{job_id}

# Télécharger le résultat (CSV zippé)
GET /v6/bulk-read/{job_id}/result
```

### Bulk Write

```bash
# Upload du CSV
POST /v6/upload
Content-Type: multipart/form-data
file: @import.csv

# Créer le job d'écriture
POST /v6/bulk-write

{
  "operation": "insert", // ou "update", "upsert"
  "resource": [
    {
      "type": "data",
      "module": {
        "api_name": "Leads"
      },
      "file_id": "FILE_ID_FROM_UPLOAD",
      "field_mappings": [
        {"api_name": "Last_Name", "index": 0},
        {"api_name": "Email", "index": 1},
        {"api_name": "Company", "index": 2}
      ]
    }
  ]
}
```

## Webhooks (Notifications API)

Recevoir des notifications quand des enregistrements changent.

```bash
POST /v6/actions/watch

{
  "watch": [
    {
      "channel_id": "1000000068001",
      "events": ["Deals.create", "Deals.edit", "Deals.delete"],
      "channel_expiry": "2026-12-31T23:59:59+01:00",
      "token": "mon_token_secret",
      "notify_url": "https://myapp.com/zoho-webhook"
    }
  ]
}
```

**Payload reçu :**
```json
{
  "module": "Deals",
  "operation": "edit",
  "token": "mon_token_secret",
  "ids": ["5234876000000123456"]
}
```

## Gestion des erreurs

### Codes HTTP

| Code | Signification |
|------|--------------|
| 200 | Succès |
| 201 | Créé |
| 202 | Accepté (bulk) |
| 204 | Supprimé |
| 400 | Requête invalide |
| 401 | Non authentifié |
| 403 | Interdit (permissions) |
| 404 | Non trouvé |
| 429 | Rate limit dépassé |
| 500 | Erreur serveur |

### Codes d'erreur courants

```json
{
  "data": [
    {
      "code": "MANDATORY_NOT_FOUND",
      "details": {"api_name": "Last_Name"},
      "message": "required field not found",
      "status": "error"
    }
  ]
}
```

| Code erreur | Description |
|------------|-------------|
| `MANDATORY_NOT_FOUND` | Champ obligatoire manquant |
| `INVALID_DATA` | Donnée invalide |
| `DUPLICATE_DATA` | Doublon détecté |
| `RECORD_NOT_FOUND` | Enregistrement inexistant |
| `LIMIT_EXCEEDED` | Limite dépassée |
| `OAUTH_SCOPE_MISMATCH` | Scope insuffisant |
| `INVALID_TOKEN` | Token expiré ou invalide |

## Rate Limiting

| Édition | API calls/jour/org | Calls/minute |
|---------|-------------------|--------------|
| Gratuit | 0 | 0 |
| Standard | 1 000 | 100 |
| Professionnel | 2 000 | 100 |
| Enterprise | 5 000 | 100 |
| Ultimate | 10 000 | 100 |

**Calcul :** Base + (500 × nombre d'utilisateurs)

**Headers de rate limit :**
```
X-RATELIMIT-LIMIT: 100
X-RATELIMIT-REMAINING: 87
X-RATELIMIT-RESET: 1708272000
```

## Exemples complets

### Python : Créer un lead

```python
import requests

BASE_URL = "https://www.zohoapis.eu/crm/v6"
ACCESS_TOKEN = "1000.xxxx.yyyy"

headers = {
    "Authorization": f"Zoho-oauthtoken {ACCESS_TOKEN}",
    "Content-Type": "application/json"
}

data = {
    "data": [{
        "Last_Name": "Dupont",
        "First_Name": "Jean",
        "Email": "jean@acme.com",
        "Company": "Acme Corp",
        "Phone": "+33612345678",
        "Lead_Source": "Site Web",
        "Industry": "Technology"
    }],
    "trigger": ["workflow"]
}

response = requests.post(f"{BASE_URL}/Leads", json=data, headers=headers)
result = response.json()
print(f"Lead créé : {result['data'][0]['details']['id']}")
```

### JavaScript (Node.js) : Rechercher des deals

```javascript
const axios = require('axios');

const BASE_URL = 'https://www.zohoapis.eu/crm/v6';
const ACCESS_TOKEN = '1000.xxxx.yyyy';

async function searchDeals() {
  const response = await axios.get(`${BASE_URL}/Deals/search`, {
    headers: { 'Authorization': `Zoho-oauthtoken ${ACCESS_TOKEN}` },
    params: {
      criteria: '(Stage:equals:Négociation)and(Amount:greater_than:10000)',
      per_page: 50
    }
  });
  
  response.data.data.forEach(deal => {
    console.log(`${deal.Deal_Name} - ${deal.Amount}€ - ${deal.Stage}`);
  });
}

searchDeals();
```

### cURL : Pipeline complet

```bash
# 1. Créer un compte
ACCOUNT=$(curl -s -X POST "$BASE/Accounts" \
  -H "Authorization: Zoho-oauthtoken $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"data":[{"Account_Name":"Acme Corp","Industry":"Technology"}]}')

ACCOUNT_ID=$(echo $ACCOUNT | jq -r '.data[0].details.id')

# 2. Créer un contact lié
CONTACT=$(curl -s -X POST "$BASE/Contacts" \
  -H "Authorization: Zoho-oauthtoken $TOKEN" \
  -H "Content-Type: application/json" \
  -d "{\"data\":[{\"Last_Name\":\"Dupont\",\"First_Name\":\"Jean\",\"Account_Name\":\"$ACCOUNT_ID\"}]}")

CONTACT_ID=$(echo $CONTACT | jq -r '.data[0].details.id')

# 3. Créer un deal
curl -X POST "$BASE/Deals" \
  -H "Authorization: Zoho-oauthtoken $TOKEN" \
  -H "Content-Type: application/json" \
  -d "{\"data\":[{\"Deal_Name\":\"Projet CRM\",\"Stage\":\"Qualification\",\"Amount\":25000,\"Account_Name\":\"$ACCOUNT_ID\",\"Contact_Name\":\"$CONTACT_ID\",\"Closing_Date\":\"2026-06-30\"}]}"
```

## Bonnes pratiques API

1. **Refresh Token** : Stocker de manière sécurisée, ne jamais exposer côté client
2. **Rate limiting** : Implémenter un retry avec backoff exponentiel
3. **Bulk** : Utiliser les opérations batch pour > 10 enregistrements
4. **Champs** : Spécifier `fields` pour limiter la taille des réponses
5. **Triggers** : Contrôler `trigger` pour éviter les workflows non souhaités
6. **Pagination** : Toujours gérer `more_records` pour les listes
7. **Erreurs** : Logger les erreurs API avec le `request_id` pour le support

---
*Voir aussi : [../zoho-deluge/api-calls.md](../zoho-deluge/api-calls.md) pour les appels API depuis Deluge.*
