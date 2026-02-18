# Intégrations - OAuth 2.0

## Flux OAuth 2.0 pour Zoho

### Étape 1 : Enregistrer une Application
```
1. Aller sur https://api-console.zoho.com
2. Cliquer "Add Client"
3. Choisir le type :
   - Server-based Application (recommandé pour les serveurs)
   - Client-based Application (SPA, mobile)
   - Self Client (scripts, usage personnel)

4. Renseigner :
   Client Name : Mon Application
   Homepage URL : https://app.entreprise.com
   Authorized Redirect URI : https://app.entreprise.com/callback

5. Récupérer :
   Client ID : 1000.XXXXXXXXXXXX
   Client Secret : abcdef1234567890
```

### Étape 2 : Obtenir le Code d'Autorisation
```
URL d'autorisation :
https://accounts.zoho.com/oauth/v2/auth
  ?scope=ZohoCRM.modules.ALL,ZohoBooks.fullaccess.ALL
  &client_id=1000.XXXXXXXXXXXX
  &response_type=code
  &access_type=offline
  &redirect_uri=https://app.entreprise.com/callback
  &prompt=consent

L'utilisateur se connecte et autorise → Redirection vers :
https://app.entreprise.com/callback?code=1000.abcdef123456.ghijkl789012
```

### Étape 3 : Échanger le Code contre des Tokens
```bash
curl -X POST "https://accounts.zoho.com/oauth/v2/token" \
  -d "grant_type=authorization_code" \
  -d "client_id=1000.XXXXXXXXXXXX" \
  -d "client_secret=abcdef1234567890" \
  -d "code=1000.abcdef123456.ghijkl789012" \
  -d "redirect_uri=https://app.entreprise.com/callback"
```

**Réponse :**
```json
{
  "access_token": "1000.abc123.def456",
  "refresh_token": "1000.xyz789.uvw012",
  "token_type": "Bearer",
  "expires_in": 3600,
  "scope": "ZohoCRM.modules.ALL ZohoBooks.fullaccess.ALL"
}
```

### Étape 4 : Rafraîchir le Token
```bash
curl -X POST "https://accounts.zoho.com/oauth/v2/token" \
  -d "grant_type=refresh_token" \
  -d "client_id=1000.XXXXXXXXXXXX" \
  -d "client_secret=abcdef1234567890" \
  -d "refresh_token=1000.xyz789.uvw012"
```

## Scopes Principaux

### Zoho CRM
```
ZohoCRM.modules.ALL          → Tous les modules
ZohoCRM.modules.Leads.READ   → Lecture seule sur les Leads
ZohoCRM.modules.Leads.CREATE → Création de Leads
ZohoCRM.modules.Leads.UPDATE → Mise à jour de Leads
ZohoCRM.modules.Leads.DELETE → Suppression de Leads
ZohoCRM.settings.ALL         → Paramètres CRM
ZohoCRM.coql.READ            → Requêtes COQL
```

### Zoho Books
```
ZohoBooks.fullaccess.ALL     → Accès complet
ZohoBooks.invoices.READ      → Lecture des factures
ZohoBooks.invoices.CREATE    → Création de factures
ZohoBooks.contacts.ALL       → Gestion des contacts
```

### Multi-Applications
```
Combiner les scopes avec des virgules :
scope=ZohoCRM.modules.ALL,ZohoBooks.fullaccess.ALL,ZohoDesk.tickets.ALL
```

## Self Client (Scripts / Automatisation)

Pour les scripts serveur sans interaction utilisateur :

```
1. API Console → Self Client
2. Générer un Grant Token :
   Scope : ZohoCRM.modules.ALL
   Durée : 10 minutes (usage unique)
   
3. Échanger immédiatement :
   curl -X POST "https://accounts.zoho.com/oauth/v2/token" \
     -d "grant_type=authorization_code" \
     -d "client_id=CLIENT_ID" \
     -d "client_secret=CLIENT_SECRET" \
     -d "code=GRANT_TOKEN"

4. Sauvegarder le refresh_token (ne expire pas)
```

## Domaines par Région

| Région | Accounts URL | API URL |
|--------|-------------|---------|
| US | accounts.zoho.com | www.zohoapis.com |
| EU | accounts.zoho.eu | www.zohoapis.eu |
| IN | accounts.zoho.in | www.zohoapis.in |
| AU | accounts.zoho.com.au | www.zohoapis.com.au |
| JP | accounts.zoho.jp | www.zohoapis.jp |

**Important :** Utiliser le domaine correspondant à la région de votre compte.

## Gestion Sécurisée des Tokens

```python
# Ne JAMAIS stocker les tokens en clair dans le code
# Utiliser des variables d'environnement ou un vault

import os
from cryptography.fernet import Fernet

class TokenManager:
    def __init__(self):
        self.key = os.environ['ENCRYPTION_KEY']
        self.cipher = Fernet(self.key)
    
    def store_token(self, token):
        encrypted = self.cipher.encrypt(token.encode())
        # Stocker dans une base de données
        db.save("zoho_refresh_token", encrypted)
    
    def get_token(self):
        encrypted = db.load("zoho_refresh_token")
        return self.cipher.decrypt(encrypted).decode()
```

## Bonnes Pratiques OAuth

1. **Ne jamais exposer le Client Secret** côté client (navigateur)
2. **Stocker les refresh tokens de façon sécurisée** (chiffrés, vault)
3. **Rafraîchir proactivement** — avant l'expiration du access token
4. **Demander le minimum de scopes** nécessaires
5. **Utiliser le bon domaine** selon la région du compte
6. **Révoquer les tokens** des applications inutilisées
