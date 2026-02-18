# API Zoho Campaigns

## Informations générales

| Paramètre | Valeur |
|-----------|--------|
| **URL de base** | `https://campaigns.zoho.eu/api/v1.1` |
| **Authentification** | OAuth 2.0 |
| **Format** | JSON |
| **Rate limit** | Variable selon le plan |

### Domaines par région

| Région | URL |
|--------|-----|
| US | `https://campaigns.zoho.com/api/v1.1` |
| EU | `https://campaigns.zoho.eu/api/v1.1` |
| IN | `https://campaigns.zoho.in/api/v1.1` |

## Authentification

### Scopes OAuth

| Scope | Description |
|-------|-------------|
| `ZohoCampaigns.contact.READ` | Lire les contacts |
| `ZohoCampaigns.contact.CREATE` | Créer/importer des contacts |
| `ZohoCampaigns.contact.UPDATE` | Modifier des contacts |
| `ZohoCampaigns.campaign.READ` | Lire les campagnes |
| `ZohoCampaigns.campaign.CREATE` | Créer des campagnes |
| `ZohoCampaigns.report.READ` | Lire les rapports |

### Obtenir un token

```bash
# 1. Code d'autorisation
GET https://accounts.zoho.eu/oauth/v2/auth
  ?scope=ZohoCampaigns.contact.ALL,ZohoCampaigns.campaign.ALL
  &client_id={client_id}
  &response_type=code
  &access_type=offline
  &redirect_uri={redirect_uri}

# 2. Échanger contre un access token
curl -X POST "https://accounts.zoho.eu/oauth/v2/token" \
  -d "grant_type=authorization_code" \
  -d "client_id={client_id}" \
  -d "client_secret={client_secret}" \
  -d "code={code}" \
  -d "redirect_uri={redirect_uri}"
```

## Listes de diffusion

### Lister les listes

```bash
GET /api/v1.1/getmailinglists
  ?resfmt=JSON
  &sort=asc
  &range=50

# Réponse
{
  "status": "success",
  "list_of_details": [
    {
      "listkey": "3z0123456789",
      "listname": "Newsletter principale",
      "createddate": "2026-01-15",
      "total_contacts": 9427
    }
  ]
}
```

### Créer une liste

```bash
POST /api/v1.1/json/listsubscribe
Content-Type: application/x-www-form-urlencoded

resfmt=JSON
&listkey=3z0123456789
&contactinfo={"First Name":"Marie","Last Name":"Dupont","Contact Email":"marie@example.fr"}
```

### Ajouter un contact à une liste

```bash
POST /api/v1.1/json/listsubscribe
Content-Type: application/x-www-form-urlencoded

resfmt=JSON
&listkey=3z0123456789
&contactinfo={
  "Contact Email": "paul@example.fr",
  "First Name": "Paul",
  "Last Name": "Martin",
  "Company": "TechCorp"
}
```

### Import en masse

```bash
POST /api/v1.1/json/listsubscribe
Content-Type: application/x-www-form-urlencoded

resfmt=JSON
&listkey=3z0123456789
&contactinfo=[
  {"Contact Email":"a@ex.fr","First Name":"Alice"},
  {"Contact Email":"b@ex.fr","First Name":"Bob"},
  {"Contact Email":"c@ex.fr","First Name":"Charlie"}
]
```

## Contacts

### Obtenir les détails d'un contact

```bash
GET /api/v1.1/json/getlistsubscribers
  ?resfmt=JSON
  &listkey=3z0123456789
  &status=active
  &sort=desc
  &range=50
  &fromindex=1

# Réponse
{
  "status": "success",
  "list_of_details": [
    {
      "contact_email": "marie@example.fr",
      "first_name": "Marie",
      "last_name": "Dupont",
      "company": "TechCorp",
      "added_time": "2026-01-20 10:30:00"
    }
  ]
}
```

### Désinscrire un contact

```bash
POST /api/v1.1/json/listunsubscribe
Content-Type: application/x-www-form-urlencoded

resfmt=JSON
&listkey=3z0123456789
&contactinfo={"Contact Email":"paul@example.fr"}
```

## Campagnes

### Lister les campagnes

```bash
GET /api/v1.1/json/recentcampaigns
  ?resfmt=JSON
  &sort=desc
  &range=20

# Réponse
{
  "status": "success",
  "recent_campaigns": [
    {
      "campaign_key": "abc123",
      "campaign_name": "Newsletter Mars 2026",
      "campaign_type": "regular",
      "status": "Sent",
      "sent_time": "2026-03-10 10:00:00",
      "total_recipients": 9750
    }
  ]
}
```

### Obtenir les statistiques d'une campagne

```bash
GET /api/v1.1/json/campaigndetails
  ?resfmt=JSON
  &campaignkey=abc123

# Réponse
{
  "status": "success",
  "campaign_details": {
    "campaign_name": "Newsletter Mars 2026",
    "total_sent": 10000,
    "total_delivered": 9750,
    "unique_opens": 2438,
    "unique_clicks": 487,
    "hard_bounces": 150,
    "soft_bounces": 100,
    "unsubscribes": 45,
    "spam_complaints": 3,
    "open_rate": "25.0%",
    "click_rate": "5.0%"
  }
}
```

### Récupérer les contacts ayant ouvert

```bash
GET /api/v1.1/json/campaignopendetails
  ?resfmt=JSON
  &campaignkey=abc123
  &range=100
  &fromindex=1
```

### Récupérer les contacts ayant cliqué

```bash
GET /api/v1.1/json/campaignlinkclickdetails
  ?resfmt=JSON
  &campaignkey=abc123
```

## Webhooks

### Configurer un webhook

```bash
POST /api/v1.1/json/webhook
Content-Type: application/x-www-form-urlencoded

resfmt=JSON
&webhookurl=https://monapp.fr/webhook/campaigns
&listkey=3z0123456789
&event=subscribe,unsubscribe,bounce
```

### Événements webhook

| Événement | Payload |
|-----------|---------|
| `subscribe` | Contact ajouté à la liste |
| `unsubscribe` | Contact désinscrit |
| `bounce` | Email rebondi |
| `open` | Email ouvert |
| `click` | Lien cliqué |

### Payload webhook reçu

```json
{
  "event": "subscribe",
  "list_key": "3z0123456789",
  "list_name": "Newsletter principale",
  "contact": {
    "email": "nouveau@example.fr",
    "first_name": "Jean",
    "last_name": "Durand",
    "subscribed_time": "2026-03-15T14:30:00Z"
  }
}
```

## Exemple complet : Script d'import et suivi

```python
import requests

BASE_URL = "https://campaigns.zoho.eu/api/v1.1"
TOKEN = "1000.xxxxxxxx"
LIST_KEY = "3z0123456789"

headers = {"Authorization": f"Bearer {TOKEN}"}

# 1. Importer des contacts
contacts = [
    {"Contact Email": "alice@example.fr", "First Name": "Alice", "Company": "TechCorp"},
    {"Contact Email": "bob@example.fr", "First Name": "Bob", "Company": "DesignLab"},
]

import json
resp = requests.post(
    f"{BASE_URL}/json/listsubscribe",
    headers=headers,
    data={
        "resfmt": "JSON",
        "listkey": LIST_KEY,
        "contactinfo": json.dumps(contacts)
    }
)
print("Import:", resp.json())

# 2. Vérifier les stats de la dernière campagne
resp = requests.get(
    f"{BASE_URL}/json/recentcampaigns",
    headers=headers,
    params={"resfmt": "JSON", "range": 1}
)
campaign = resp.json()["recent_campaigns"][0]
campaign_key = campaign["campaign_key"]

# 3. Récupérer les détails
resp = requests.get(
    f"{BASE_URL}/json/campaigndetails",
    headers=headers,
    params={"resfmt": "JSON", "campaignkey": campaign_key}
)
stats = resp.json()["campaign_details"]

print(f"Campagne: {stats['campaign_name']}")
print(f"Taux d'ouverture: {stats['open_rate']}")
print(f"Taux de clic: {stats['click_rate']}")
print(f"Désinscriptions: {stats['unsubscribes']}")
```

## Gestion des erreurs

| Code | Signification |
|------|---------------|
| `200` | Succès |
| `400` | Paramètres manquants ou invalides |
| `401` | Token expiré ou invalide |
| `403` | Scope insuffisant |
| `429` | Rate limit dépassé |
| `500` | Erreur serveur |

```json
{
  "status": "error",
  "code": "2001",
  "message": "Invalid mailing list key"
}
```
