# Intégrations - API REST Zoho

## Vue d'Ensemble des APIs

| Application | Base URL | Documentation |
|-------------|---------|---------------|
| Zoho CRM | `https://www.zohoapis.com/crm/v6` | developer.zoho.com/crm |
| Zoho Books | `https://www.zohoapis.com/books/v3` | developer.zoho.com/books |
| Zoho Desk | `https://desk.zoho.com/api/v1` | developer.zoho.com/desk |
| Zoho People | `https://people.zoho.com/people/api` | developer.zoho.com/people |
| Zoho Inventory | `https://www.zohoapis.com/inventory/v1` | developer.zoho.com/inventory |
| Zoho Projects | `https://projectsapi.zoho.com/restapi` | developer.zoho.com/projects |
| Zoho Campaigns | `https://campaigns.zoho.com/api` | developer.zoho.com/campaigns |
| Zoho Sign | `https://sign.zoho.com/api/v1` | developer.zoho.com/sign |
| Zoho Analytics | `https://analyticsapi.zoho.com/restapi/v2` | developer.zoho.com/analytics |

## Conventions Communes

### Headers Standards
```bash
Authorization: Zoho-oauthtoken {access_token}
Content-Type: application/json
```

### Pagination
```bash
# Zoho CRM
GET /crm/v6/Leads?page=1&per_page=200

# Zoho Books
GET /books/v3/invoices?page=1&per_page=200

# Réponse typique
{
  "info": {
    "page": 1,
    "per_page": 200,
    "count": 200,
    "more_records": true
  }
}
```

### Filtres et Recherche
```bash
# CRM - COQL (CRM Object Query Language)
POST /crm/v6/coql
{
  "select_query": "SELECT Last_Name, Email, Lead_Score 
                   FROM Leads 
                   WHERE Lead_Source = 'Website' 
                   AND Lead_Score > 50 
                   ORDER BY Lead_Score DESC 
                   LIMIT 100"
}

# Books - Filtres par paramètres
GET /books/v3/invoices?status=overdue&customer_id=123&date_start=2026-01-01

# Desk - Recherche
GET /api/v1/tickets?status=open&priority=high&sortBy=createdTime
```

### Codes de Réponse HTTP
| Code | Signification | Action |
|------|--------------|--------|
| 200 | Succès | Traiter la réponse |
| 201 | Créé | Récupérer l'ID créé |
| 204 | Supprimé | Confirmer la suppression |
| 400 | Requête invalide | Vérifier les paramètres |
| 401 | Non authentifié | Rafraîchir le token |
| 403 | Interdit | Vérifier les permissions |
| 404 | Non trouvé | Vérifier l'ID/URL |
| 429 | Rate limit | Attendre et réessayer |
| 500 | Erreur serveur | Réessayer plus tard |

## Rate Limits

### Par Application
| Application | Limite | Fenêtre |
|-------------|--------|---------|
| Zoho CRM | 100 req/min (Standard) | Par minute |
| Zoho CRM | 500 req/min (Entreprise) | Par minute |
| Zoho Books | 100 req/min | Par minute |
| Zoho Desk | 60 req/min | Par minute |
| Zoho People | 100 req/min | Par minute |

### Gestion des Rate Limits
```python
import requests
import time

def zoho_api_call(url, headers, max_retries=3):
    for attempt in range(max_retries):
        response = requests.get(url, headers=headers)
        
        if response.status_code == 429:
            retry_after = int(response.headers.get('Retry-After', 60))
            print(f"Rate limit atteint. Attente de {retry_after}s...")
            time.sleep(retry_after)
            continue
        
        if response.status_code == 401:
            headers['Authorization'] = f'Zoho-oauthtoken {refresh_token()}'
            continue
        
        return response
    
    raise Exception("Max retries atteint")
```

## Exemple Complet : Synchronisation CRM → ERP

```python
import requests
import json

class ZohoSync:
    def __init__(self, client_id, client_secret, refresh_token):
        self.base_url = "https://www.zohoapis.com/crm/v6"
        self.token = self.get_token(client_id, client_secret, refresh_token)
    
    def get_token(self, client_id, client_secret, refresh_token):
        resp = requests.post("https://accounts.zoho.com/oauth/v2/token", data={
            "grant_type": "refresh_token",
            "client_id": client_id,
            "client_secret": client_secret,
            "refresh_token": refresh_token
        })
        return resp.json()["access_token"]
    
    def get_deals(self, modified_since=None):
        headers = {"Authorization": f"Zoho-oauthtoken {self.token}"}
        if modified_since:
            headers["If-Modified-Since"] = modified_since
        
        deals = []
        page = 1
        while True:
            resp = requests.get(
                f"{self.base_url}/Deals?page={page}&per_page=200",
                headers=headers
            )
            data = resp.json()
            deals.extend(data.get("data", []))
            
            if not data.get("info", {}).get("more_records"):
                break
            page += 1
        
        return deals
    
    def sync_to_erp(self):
        deals = self.get_deals(modified_since="2026-02-17T00:00:00+01:00")
        for deal in deals:
            erp_data = {
                "reference": deal["Deal_Name"],
                "amount": deal["Amount"],
                "stage": deal["Stage"],
                "customer": deal["Account_Name"]["name"],
                "close_date": deal["Closing_Date"]
            }
            # Envoyer vers l'ERP
            requests.post("https://erp.entreprise.com/api/deals", json=erp_data)
            print(f"Deal synchronisé : {deal['Deal_Name']}")
```

## Bonnes Pratiques API

1. **Rafraîchir les tokens proactivement** — ne pas attendre le 401
2. **Implémenter le retry avec backoff** pour les erreurs temporaires
3. **Utiliser If-Modified-Since** pour ne récupérer que les changements
4. **Paginer correctement** — ne pas oublier les pages suivantes
5. **Logger toutes les erreurs** pour faciliter le débogage
6. **Utiliser COQL** (CRM) pour des requêtes complexes plutôt que des filtres multiples
