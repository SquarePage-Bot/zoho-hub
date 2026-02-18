# 🔌 Zoho Creator - API REST

> Endpoints, authentification, exemples de requêtes et réponses.

## Table des matières

- [Authentification](#authentification)
- [Base URL](#base-url)
- [Endpoints](#endpoints)
- [Exemples complets](#exemples-complets)
- [Pagination et filtres](#pagination-et-filtres)
- [Gestion des fichiers](#gestion-des-fichiers)
- [Limites et quotas](#limites-et-quotas)

---

## Authentification

### OAuth 2.0

```
1. Créer un client OAuth sur https://api-console.zoho.eu
2. Obtenir un Authorization Code
3. Échanger contre un Access Token + Refresh Token
```

**Scopes Creator :**
```
ZohoCreator.report.READ
ZohoCreator.report.CREATE
ZohoCreator.report.UPDATE
ZohoCreator.report.DELETE
ZohoCreator.form.CREATE
ZohoCreator.meta.form.READ
ZohoCreator.meta.application.READ
ZohoCreator.dashboard.READ
```

**Obtenir un token :**
```bash
# Étape 1 : Authorization Code (navigateur)
https://accounts.zoho.eu/oauth/v2/auth?scope=ZohoCreator.report.ALL,ZohoCreator.form.CREATE&client_id=CLIENT_ID&response_type=code&redirect_uri=REDIRECT_URI&access_type=offline

# Étape 2 : Échanger le code
curl -X POST "https://accounts.zoho.eu/oauth/v2/token" \
  -d "grant_type=authorization_code" \
  -d "client_id=CLIENT_ID" \
  -d "client_secret=CLIENT_SECRET" \
  -d "redirect_uri=REDIRECT_URI" \
  -d "code=AUTH_CODE"

# Réponse
{
  "access_token": "1000.xxxx",
  "refresh_token": "1000.yyyy",
  "expires_in": 3600,
  "token_type": "Bearer"
}

# Étape 3 : Rafraîchir le token
curl -X POST "https://accounts.zoho.eu/oauth/v2/token" \
  -d "grant_type=refresh_token" \
  -d "client_id=CLIENT_ID" \
  -d "client_secret=CLIENT_SECRET" \
  -d "refresh_token=REFRESH_TOKEN"
```

### Headers

```
Authorization: Zoho-oauthtoken {access_token}
Content-Type: application/json
```

---

## Base URL

| Région | URL |
|--------|-----|
| Europe (EU) | `https://creator.zoho.eu/api/v2` |
| États-Unis (US) | `https://creator.zoho.com/api/v2` |
| Inde (IN) | `https://creator.zoho.in/api/v2` |
| Australie (AU) | `https://creator.zoho.com.au/api/v2` |

**Format des endpoints :**
```
https://creator.zoho.eu/api/v2/{owner_name}/{app_link_name}/report/{report_link_name}
https://creator.zoho.eu/api/v2/{owner_name}/{app_link_name}/form/{form_link_name}
```

---

## Endpoints

### Enregistrements

| Méthode | Endpoint | Description |
|---------|----------|-------------|
| `GET` | `/report/{report}` | Lister les enregistrements |
| `GET` | `/report/{report}/{id}` | Obtenir un enregistrement |
| `POST` | `/form/{form}` | Créer un enregistrement |
| `PATCH` | `/report/{report}/{id}` | Mettre à jour un enregistrement |
| `DELETE` | `/report/{report}/{id}` | Supprimer un enregistrement |
| `DELETE` | `/report/{report}` | Supprimer en masse (avec critères) |

### Métadonnées

| Méthode | Endpoint | Description |
|---------|----------|-------------|
| `GET` | `/meta/applications` | Lister les applications |
| `GET` | `/meta/application` | Détails de l'application |
| `GET` | `/meta/application/forms` | Lister les formulaires |
| `GET` | `/meta/application/form/{form}` | Détails d'un formulaire |
| `GET` | `/meta/application/reports` | Lister les rapports |
| `GET` | `/meta/application/pages` | Lister les pages |

### Fichiers

| Méthode | Endpoint | Description |
|---------|----------|-------------|
| `GET` | `/report/{report}/{id}/{field}/download` | Télécharger un fichier |
| `POST` | `/form/{form}` (multipart) | Upload avec fichier |

---

## Exemples complets

### Lister les enregistrements

```bash
curl -X GET "https://creator.zoho.eu/api/v2/owner/mon-app/report/Tous_les_Clients" \
  -H "Authorization: Zoho-oauthtoken 1000.xxxx"
```

**Réponse :**
```json
{
  "code": 3000,
  "data": [
    {
      "ID": "3860000000012345",
      "Nom": "Jean Dupont",
      "Email": "jean@acme.com",
      "Entreprise": "ACME",
      "Statut": "Actif",
      "Date_Creation": "18-Feb-2026"
    },
    {
      "ID": "3860000000012346",
      "Nom": "Marie Martin",
      "Email": "marie@beta.com",
      "Entreprise": "Beta Corp",
      "Statut": "Actif",
      "Date_Creation": "15-Feb-2026"
    }
  ],
  "message": "Data fetched successfully"
}
```

### Obtenir un enregistrement

```bash
curl -X GET "https://creator.zoho.eu/api/v2/owner/mon-app/report/Tous_les_Clients/3860000000012345" \
  -H "Authorization: Zoho-oauthtoken 1000.xxxx"
```

### Créer un enregistrement

```bash
curl -X POST "https://creator.zoho.eu/api/v2/owner/mon-app/form/Clients" \
  -H "Authorization: Zoho-oauthtoken 1000.xxxx" \
  -H "Content-Type: application/json" \
  -d '{
    "data": {
      "Nom": "Pierre Bernard",
      "Email": "pierre@gamma.com",
      "Entreprise": "Gamma SA",
      "Telephone": "+33612345678",
      "Statut": "Prospect",
      "Adresse": {
        "address_line_1": "12 rue de la Paix",
        "city": "Paris",
        "postal_code": "75002",
        "country": "France"
      }
    }
  }'
```

**Réponse :**
```json
{
  "code": 3000,
  "data": {
    "ID": "3860000000012347"
  },
  "message": "Data Added Successfully"
}
```

### Mettre à jour

```bash
curl -X PATCH "https://creator.zoho.eu/api/v2/owner/mon-app/report/Tous_les_Clients/3860000000012345" \
  -H "Authorization: Zoho-oauthtoken 1000.xxxx" \
  -H "Content-Type: application/json" \
  -d '{
    "data": {
      "Statut": "Client",
      "Date_Conversion": "18-Feb-2026"
    }
  }'
```

### Supprimer

```bash
# Un enregistrement
curl -X DELETE "https://creator.zoho.eu/api/v2/owner/mon-app/report/Tous_les_Clients/3860000000012345" \
  -H "Authorization: Zoho-oauthtoken 1000.xxxx"

# En masse avec critères
curl -X DELETE "https://creator.zoho.eu/api/v2/owner/mon-app/report/Tous_les_Clients" \
  -H "Authorization: Zoho-oauthtoken 1000.xxxx" \
  -d '{
    "criteria": "Statut == \"Archivé\" && Date_Modification < \"01-Jan-2025\""
  }'
```

---

## Pagination et filtres

### Pagination

```bash
# Page 1, 20 enregistrements
GET /report/Tous_les_Clients?from=0&limit=20

# Page 2
GET /report/Tous_les_Clients?from=20&limit=20

# Page 3
GET /report/Tous_les_Clients?from=40&limit=20
```

**Paramètres :**
| Paramètre | Description | Défaut | Max |
|-----------|-------------|--------|-----|
| `from` | Index de départ | 0 | - |
| `limit` | Nombre d'enregistrements | 200 | 200 |

### Filtres (criteria)

```bash
# Filtre simple
GET /report/Tous?criteria=Statut=="Actif"

# Filtre multiple (AND)
GET /report/Tous?criteria=Statut=="Actif" && Ville=="Paris"

# Filtre multiple (OR)
GET /report/Tous?criteria=Statut=="Actif" || Statut=="Prospect"

# Comparaisons numériques
GET /report/Tous?criteria=Montant > 5000

# Filtre de date
GET /report/Tous?criteria=Date_Creation >= "01-Jan-2026"

# Contient
GET /report/Tous?criteria=Nom.contains("Dup")

# Commence par
GET /report/Tous?criteria=Code_Postal.startsWith("75")
```

### Tri

```bash
# Tri ascendant
GET /report/Tous?sort_by=Nom&sort_order=asc

# Tri descendant
GET /report/Tous?sort_by=Date_Creation&sort_order=desc
```

### Sélection de champs

```bash
# Ne retourner que certains champs
GET /report/Tous?fields=Nom,Email,Statut
```

---

## Gestion des fichiers

### Upload

```bash
curl -X POST "https://creator.zoho.eu/api/v2/owner/mon-app/form/Documents" \
  -H "Authorization: Zoho-oauthtoken 1000.xxxx" \
  -F "data={\"Titre\":\"Mon document\",\"Type\":\"PDF\"}" \
  -F "Fichier=@/chemin/vers/document.pdf"
```

### Download

```bash
curl -X GET "https://creator.zoho.eu/api/v2/owner/mon-app/report/Documents/3860000000012345/Fichier/download" \
  -H "Authorization: Zoho-oauthtoken 1000.xxxx" \
  -o document.pdf
```

---

## Limites et quotas

| Limite | Valeur |
|--------|--------|
| Requêtes par minute (par utilisateur) | 200 |
| Requêtes par jour | 50 000 (plan gratuit) à 1 000 000 (plan premium) |
| Enregistrements par requête GET | 200 max |
| Taille du body POST | 10 Mo |
| Taille de fichier upload | 50 Mo |
| Champs par requête POST/PATCH | Pas de limite documentée |

### Codes de réponse

| Code | Signification |
|------|---------------|
| `3000` | Succès |
| `3001` | Enregistrement ajouté |
| `3002` | Enregistrement mis à jour |
| `3100` | Erreur de validation |
| `3101` | Champ obligatoire manquant |
| `3200` | Authentification échouée |
| `3300` | Limite de requêtes atteinte |
| `3500` | Erreur serveur |

---

## Bonnes pratiques

1. **Toujours paginer** les requêtes GET sur les gros volumes
2. **Utiliser le Refresh Token** pour renouveler automatiquement l'Access Token
3. **Limiter les champs** retournés avec le paramètre `fields`
4. **Gérer les erreurs** : vérifier le code de réponse à chaque appel
5. **Rate limiting** : Implémenter un retry avec backoff exponentiel
6. **Cache** : Mettre en cache les données qui changent peu

---

*Voir aussi : [../../zoho-deluge/](../../zoho-deluge/) | [deluge-creator.md](deluge-creator.md) | [formulaires.md](formulaires.md)*
