# API Zoho Projects

## Présentation

L'API REST de Zoho Projects (v3) permet d'interagir programmatiquement avec les projets, tâches, jalons, feuilles de temps et autres entités.

## Informations générales

| Paramètre | Valeur |
|-----------|--------|
| **URL de base** | `https://projectsapi.zoho.eu/restapi` (EU) |
| **Version** | v3 |
| **Authentification** | OAuth 2.0 |
| **Format** | JSON |
| **Rate limit** | 100 requêtes / minute |

### Domaines par région

| Région | URL |
|--------|-----|
| US | `https://projectsapi.zoho.com/restapi` |
| EU | `https://projectsapi.zoho.eu/restapi` |
| IN | `https://projectsapi.zoho.in/restapi` |
| AU | `https://projectsapi.zoho.com.au/restapi` |

## Authentification OAuth 2.0

### 1. Enregistrer l'application

```
Zoho API Console : https://api-console.zoho.eu/

Type : Server-based Application
Client Name : Mon App
Redirect URI : https://monapp.com/callback
```

### 2. Obtenir le code d'autorisation

```
GET https://accounts.zoho.eu/oauth/v2/auth
  ?scope=ZohoProjects.portals.READ,ZohoProjects.projects.ALL,ZohoProjects.tasks.ALL
  &client_id={client_id}
  &response_type=code
  &access_type=offline
  &redirect_uri=https://monapp.com/callback
```

### 3. Échanger contre un access token

```bash
curl -X POST "https://accounts.zoho.eu/oauth/v2/token" \
  -d "grant_type=authorization_code" \
  -d "client_id={client_id}" \
  -d "client_secret={client_secret}" \
  -d "code={authorization_code}" \
  -d "redirect_uri=https://monapp.com/callback"
```

**Réponse :**
```json
{
  "access_token": "1000.xxxxxxxx",
  "refresh_token": "1000.yyyyyyyy",
  "expires_in": 3600,
  "token_type": "Bearer"
}
```

### 4. Rafraîchir le token

```bash
curl -X POST "https://accounts.zoho.eu/oauth/v2/token" \
  -d "grant_type=refresh_token" \
  -d "client_id={client_id}" \
  -d "client_secret={client_secret}" \
  -d "refresh_token={refresh_token}"
```

### Scopes disponibles

| Scope | Description |
|-------|-------------|
| `ZohoProjects.portals.READ` | Lire les portails |
| `ZohoProjects.projects.ALL` | CRUD projets |
| `ZohoProjects.tasks.ALL` | CRUD tâches |
| `ZohoProjects.milestones.ALL` | CRUD jalons |
| `ZohoProjects.timesheets.ALL` | CRUD feuilles de temps |
| `ZohoProjects.forums.ALL` | CRUD forums |
| `ZohoProjects.users.READ` | Lire les utilisateurs |
| `ZohoProjects.documents.ALL` | CRUD documents |

## Portails

### Lister les portails

```bash
GET /restapi/portals/

# Réponse
{
  "portals": [
    {
      "id": "123456",
      "name": "Mon Entreprise",
      "default": true,
      "project_count": 12,
      "role": "admin"
    }
  ]
}
```

## Projets

### Lister les projets

```bash
GET /restapi/portal/{portalId}/projects/
  ?status=active
  &sort_column=created_time
  &sort_order=descending
  &index=0
  &range=50
```

### Créer un projet

```bash
POST /restapi/portal/{portalId}/projects/
Content-Type: application/json

{
  "name": "Nouvelle Plateforme Web",
  "description": "Refonte complète du site corporate",
  "start_date": "2026-03-01",
  "end_date": "2026-06-30",
  "owner": "987654",
  "template_id": "111222",
  "custom_fields": {
    "client": "Entreprise ABC",
    "budget": "50000"
  }
}
```

### Obtenir un projet

```bash
GET /restapi/portal/{portalId}/projects/{projectId}/

# Réponse
{
  "project": {
    "id": "456789",
    "name": "Nouvelle Plateforme Web",
    "status": "active",
    "created_date": "2026-03-01",
    "owner_name": "Marie Dupont",
    "task_count": {
      "open": 24,
      "closed": 8
    },
    "milestone_count": {
      "open": 3,
      "closed": 1
    }
  }
}
```

### Modifier un projet

```bash
PUT /restapi/portal/{portalId}/projects/{projectId}/
Content-Type: application/json

{
  "name": "Plateforme Web v2",
  "status": "active",
  "end_date": "2026-07-31"
}
```

### Supprimer un projet

```bash
DELETE /restapi/portal/{portalId}/projects/{projectId}/
```

## Tâches

### Lister les tâches d'un projet

```bash
GET /restapi/portal/{portalId}/projects/{projectId}/tasks/
  ?status=open
  &priority=high
  &owner=987654
  &milestone_id=333444
  &index=0
  &range=100
```

### Créer une tâche

```bash
POST /restapi/portal/{portalId}/projects/{projectId}/tasks/
Content-Type: application/json

{
  "name": "Développer le module de paiement",
  "tasklist_id": "555666",
  "start_date": "2026-04-01",
  "end_date": "2026-04-15",
  "owners": [{"id": "987654"}],
  "priority": "high",
  "description": "Intégrer Stripe et PayPal",
  "percent_complete": 0,
  "duration": "40",
  "custom_fields": {
    "complexite": "Haute"
  }
}
```

### Modifier une tâche

```bash
PUT /restapi/portal/{portalId}/projects/{projectId}/tasks/{taskId}/
Content-Type: application/json

{
  "status": "in_progress",
  "percent_complete": 50,
  "owners": [{"id": "987654"}, {"id": "111222"}]
}
```

### Sous-tâches

```bash
# Créer une sous-tâche
POST /restapi/portal/{portalId}/projects/{projectId}/tasks/{parentTaskId}/subtasks/

{
  "name": "Intégrer Stripe",
  "start_date": "2026-04-01",
  "end_date": "2026-04-07",
  "owners": [{"id": "987654"}]
}
```

## Jalons

### Lister les jalons

```bash
GET /restapi/portal/{portalId}/projects/{projectId}/milestones/
  ?status=notcompleted
```

### Créer un jalon

```bash
POST /restapi/portal/{portalId}/projects/{projectId}/milestones/
Content-Type: application/json

{
  "name": "Phase 2 : Développement",
  "start_date": "2026-04-01",
  "end_date": "2026-05-31",
  "owner": "987654",
  "flag": "internal"
}
```

## Feuilles de temps

### Enregistrer du temps

```bash
POST /restapi/portal/{portalId}/projects/{projectId}/tasks/{taskId}/logs/
Content-Type: application/json

{
  "date": "2026-03-15",
  "hours": "04:30",
  "bill_status": "Billable",
  "notes": "Développement de l'API REST produits"
}
```

### Lister les entrées de temps

```bash
GET /restapi/portal/{portalId}/projects/{projectId}/logs/
  ?users=987654
  &date=2026-03-15
  &bill_status=Billable
  &index=0
  &range=100
```

## Forums

### Lister les sujets

```bash
GET /restapi/portal/{portalId}/projects/{projectId}/forums/
```

### Créer un sujet

```bash
POST /restapi/portal/{portalId}/projects/{projectId}/forums/
Content-Type: application/json

{
  "name": "Discussion architecture microservices",
  "content": "Propositions pour l'architecture...",
  "category_id": "777888",
  "flag": "internal",
  "notify": "all"
}
```

### Ajouter un commentaire

```bash
POST /restapi/portal/{portalId}/projects/{projectId}/forums/{forumId}/comments/
Content-Type: application/json

{
  "content": "Je suis favorable à l'option 2."
}
```

## Dépendances

### Ajouter une dépendance

```bash
POST /restapi/portal/{portalId}/projects/{projectId}/tasks/{taskId}/dependencies/
Content-Type: application/json

{
  "predecessor": {
    "task_id": "111222",
    "type": "FS",
    "lag": 2
  }
}
```

### Types de dépendances dans l'API

| Type | Valeur API |
|------|-----------|
| Fin-Début | `FS` |
| Début-Début | `SS` |
| Fin-Fin | `FF` |
| Début-Fin | `SF` |

## Utilisateurs

### Lister les utilisateurs du projet

```bash
GET /restapi/portal/{portalId}/projects/{projectId}/users/
```

### Ajouter un utilisateur

```bash
POST /restapi/portal/{portalId}/projects/{projectId}/users/
Content-Type: application/json

{
  "email": "nouveau@entreprise.fr",
  "role": "member"
}
```

## Pagination

```bash
# Paramètres de pagination
?index=0      # Index de départ (0-based)
&range=100    # Nombre d'éléments par page (max 200)

# Réponse avec métadonnées de pagination
{
  "tasks": [...],
  "page_info": {
    "index": 0,
    "range": 100,
    "total_count": 342,
    "has_next": true
  }
}
```

## Gestion des erreurs

| Code HTTP | Signification |
|-----------|---------------|
| `200` | Succès |
| `201` | Créé avec succès |
| `400` | Requête invalide |
| `401` | Non authentifié (token expiré) |
| `403` | Accès refusé (permissions insuffisantes) |
| `404` | Ressource non trouvée |
| `429` | Rate limit dépassé |
| `500` | Erreur serveur |

### Exemple d'erreur

```json
{
  "error": {
    "code": 6504,
    "message": "Invalid Task ID"
  }
}
```

## Exemple complet : Script de création de projet

```python
import requests

BASE_URL = "https://projectsapi.zoho.eu/restapi"
PORTAL_ID = "123456"
TOKEN = "1000.xxxxxxxx"

headers = {
    "Authorization": f"Bearer {TOKEN}",
    "Content-Type": "application/json"
}

# 1. Créer le projet
projet = requests.post(
    f"{BASE_URL}/portal/{PORTAL_ID}/projects/",
    headers=headers,
    json={
        "name": "App Mobile v2",
        "start_date": "2026-04-01",
        "end_date": "2026-08-31"
    }
).json()

project_id = projet["project"]["id"]

# 2. Créer un jalon
jalon = requests.post(
    f"{BASE_URL}/portal/{PORTAL_ID}/projects/{project_id}/milestones/",
    headers=headers,
    json={
        "name": "Sprint 1",
        "start_date": "2026-04-01",
        "end_date": "2026-04-30"
    }
).json()

# 3. Créer une liste de tâches
tasklist = requests.post(
    f"{BASE_URL}/portal/{PORTAL_ID}/projects/{project_id}/tasklists/",
    headers=headers,
    json={
        "name": "Développement Backend",
        "milestone_id": jalon["milestone"]["id"]
    }
).json()

# 4. Créer des tâches
taches = [
    {"name": "Setup base de données", "duration": "8"},
    {"name": "API authentification", "duration": "16"},
    {"name": "API produits", "duration": "24"},
]

task_ids = []
for tache in taches:
    result = requests.post(
        f"{BASE_URL}/portal/{PORTAL_ID}/projects/{project_id}/tasks/",
        headers=headers,
        json={
            **tache,
            "tasklist_id": tasklist["tasklist"]["id"],
            "priority": "high"
        }
    ).json()
    task_ids.append(result["task"]["id"])

# 5. Créer des dépendances (chaîne séquentielle)
for i in range(1, len(task_ids)):
    requests.post(
        f"{BASE_URL}/portal/{PORTAL_ID}/projects/{project_id}/tasks/{task_ids[i]}/dependencies/",
        headers=headers,
        json={
            "predecessor": {
                "task_id": task_ids[i-1],
                "type": "FS"
            }
        }
    )

print(f"Projet créé : {project_id} avec {len(task_ids)} tâches chaînées")
```
