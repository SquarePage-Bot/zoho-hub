# 🔌 Zoho Desk - API REST

> Endpoints, authentification et exemples pour l'API Zoho Desk.

## Table des matières

- [Authentification](#authentification)
- [Base URL](#base-url)
- [Modules et endpoints](#modules-et-endpoints)
- [Exemples complets](#exemples-complets)
- [Pagination et filtres](#pagination-et-filtres)
- [Limites](#limites)

---

## Authentification

### OAuth 2.0

```
Scopes Desk :
Desk.tickets.ALL              → CRUD tickets
Desk.tickets.READ             → Lecture tickets
Desk.tickets.CREATE           → Création tickets
Desk.tickets.UPDATE           → Modification tickets
Desk.tickets.DELETE           → Suppression tickets
Desk.contacts.ALL             → CRUD contacts
Desk.contacts.READ            → Lecture contacts
Desk.basic.READ               → Lecture infos de base
Desk.settings.ALL             → Paramètres
Desk.search.READ              → Recherche
Desk.articles.ALL             → Base de connaissances
```

**Headers :**
```
Authorization: Zoho-oauthtoken {access_token}
orgId: {organization_id}
Content-Type: application/json
```

**Note :** L'`orgId` est passé en header (pas en query parameter comme Books).

---

## Base URL

| Région | URL |
|--------|-----|
| Europe | `https://desk.zoho.eu/api/v1` |
| US | `https://desk.zoho.com/api/v1` |
| Inde | `https://desk.zoho.in/api/v1` |

---

## Modules et endpoints

### Tickets

| Méthode | Endpoint | Description |
|---------|----------|-------------|
| `GET` | `/tickets` | Lister les tickets |
| `GET` | `/tickets/{id}` | Détail d'un ticket |
| `POST` | `/tickets` | Créer un ticket |
| `PATCH` | `/tickets/{id}` | Modifier un ticket |
| `DELETE` | `/tickets/{id}` | Supprimer |
| `POST` | `/tickets/{id}/move` | Déplacer vers un département |
| `POST` | `/tickets/{id}/merge` | Fusionner des tickets |
| `GET` | `/tickets/{id}/threads` | Conversations du ticket |
| `GET` | `/tickets/{id}/comments` | Commentaires |
| `POST` | `/tickets/{id}/comments` | Ajouter un commentaire |
| `POST` | `/tickets/{id}/sendReply` | Répondre au client |
| `GET` | `/tickets/{id}/attachments` | Pièces jointes |
| `POST` | `/tickets/{id}/attachments` | Ajouter une pièce jointe |
| `GET` | `/tickets/{id}/timeEntry` | Temps passé |
| `POST` | `/tickets/{id}/timeEntry` | Ajouter du temps |
| `GET` | `/tickets/{id}/history` | Historique des modifications |

### Contacts

| Méthode | Endpoint | Description |
|---------|----------|-------------|
| `GET` | `/contacts` | Lister |
| `GET` | `/contacts/{id}` | Détail |
| `POST` | `/contacts` | Créer |
| `PATCH` | `/contacts/{id}` | Modifier |
| `DELETE` | `/contacts/{id}` | Supprimer |
| `GET` | `/contacts/{id}/tickets` | Tickets du contact |

### Comptes (Accounts)

| Méthode | Endpoint | Description |
|---------|----------|-------------|
| `GET` | `/accounts` | Lister les entreprises |
| `GET` | `/accounts/{id}` | Détail |
| `POST` | `/accounts` | Créer |
| `GET` | `/accounts/{id}/tickets` | Tickets de l'entreprise |

### Agents

| Méthode | Endpoint | Description |
|---------|----------|-------------|
| `GET` | `/agents` | Lister les agents |
| `GET` | `/agents/{id}` | Détail |

### Départements

| Méthode | Endpoint | Description |
|---------|----------|-------------|
| `GET` | `/departments` | Lister |
| `GET` | `/departments/{id}` | Détail |

### Base de connaissances

| Méthode | Endpoint | Description |
|---------|----------|-------------|
| `GET` | `/articles` | Lister les articles |
| `GET` | `/articles/{id}` | Détail |
| `POST` | `/articles` | Créer |
| `PATCH` | `/articles/{id}` | Modifier |
| `DELETE` | `/articles/{id}` | Supprimer |
| `GET` | `/articles/categories` | Catégories |
| `GET` | `/articles/sections` | Sections |

### Recherche

| Méthode | Endpoint | Description |
|---------|----------|-------------|
| `GET` | `/search` | Recherche globale |

---

## Exemples complets

### Créer un ticket

```bash
curl -X POST "https://desk.zoho.eu/api/v1/tickets" \
  -H "Authorization: Zoho-oauthtoken 1000.xxxx" \
  -H "orgId: 123456789" \
  -H "Content-Type: application/json" \
  -d '{
    "subject": "Impossible de me connecter",
    "description": "<p>Depuis ce matin, je reçois une erreur 403 lors de la connexion.</p>",
    "email": "client@acme.com",
    "contactId": "4000000012345",
    "departmentId": "4000000000001",
    "channel": "Email",
    "priority": "High",
    "status": "Open",
    "category": "Technique",
    "subCategory": "Connexion",
    "cf": {
      "cf_version_produit": "3.2",
      "cf_environnement": "Production"
    }
  }'
```

**Réponse :**
```json
{
  "id": "4000000098765",
  "ticketNumber": "1234",
  "subject": "Impossible de me connecter",
  "status": "Open",
  "priority": "High",
  "departmentId": "4000000000001",
  "contactId": "4000000012345",
  "createdTime": "2026-02-18T16:30:00.000Z",
  "channel": "Email"
}
```

### Lister les tickets ouverts

```bash
curl -X GET "https://desk.zoho.eu/api/v1/tickets?status=Open&limit=25&sortBy=createdTime&sortOrder=desc" \
  -H "Authorization: Zoho-oauthtoken 1000.xxxx" \
  -H "orgId: 123456789"
```

### Modifier un ticket

```bash
curl -X PATCH "https://desk.zoho.eu/api/v1/tickets/4000000098765" \
  -H "Authorization: Zoho-oauthtoken 1000.xxxx" \
  -H "orgId: 123456789" \
  -H "Content-Type: application/json" \
  -d '{
    "status": "On Hold",
    "priority": "Urgent",
    "assigneeId": "4000000054321",
    "cf": {
      "cf_cause_racine": "Bug identifié"
    }
  }'
```

### Ajouter un commentaire

```bash
curl -X POST "https://desk.zoho.eu/api/v1/tickets/4000000098765/comments" \
  -H "Authorization: Zoho-oauthtoken 1000.xxxx" \
  -H "orgId: 123456789" \
  -H "Content-Type: application/json" \
  -d '{
    "content": "Bug identifié dans le module auth. Fix en cours de déploiement.",
    "isPublic": false,
    "contentType": "plainText"
  }'
```

### Répondre au client

```bash
curl -X POST "https://desk.zoho.eu/api/v1/tickets/4000000098765/sendReply" \
  -H "Authorization: Zoho-oauthtoken 1000.xxxx" \
  -H "orgId: 123456789" \
  -H "Content-Type: application/json" \
  -d '{
    "channel": "EMAIL",
    "to": "client@acme.com",
    "fromEmailAddress": "support@entreprise.com",
    "contentType": "html",
    "content": "<p>Bonjour,</p><p>Le problème a été identifié et corrigé. Pouvez-vous réessayer ?</p><p>Cordialement,<br>L'\''équipe support</p>"
  }'
```

### Recherche

```bash
# Recherche globale
curl -X GET "https://desk.zoho.eu/api/v1/search?module=tickets&searchStr=connexion&limit=10" \
  -H "Authorization: Zoho-oauthtoken 1000.xxxx" \
  -H "orgId: 123456789"
```

### Créer un article KB

```bash
curl -X POST "https://desk.zoho.eu/api/v1/articles" \
  -H "Authorization: Zoho-oauthtoken 1000.xxxx" \
  -H "orgId: 123456789" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Comment réinitialiser son mot de passe",
    "answer": "<h2>Étapes</h2><ol><li>Aller sur la page de connexion</li><li>Cliquer sur Mot de passe oublié</li><li>Saisir votre email</li><li>Suivre le lien reçu par email</li></ol>",
    "categoryId": "4000000005678",
    "sectionId": "4000000005679",
    "status": "Published",
    "permission": "ALL"
  }'
```

---

## Pagination et filtres

### Pagination

```bash
# Paramètres
?from=0&limit=50        # Résultats 0 à 49
?from=50&limit=50       # Résultats 50 à 99

Limite max par requête : 100
```

### Filtres sur les tickets

```bash
# Par statut
?status=Open
?status=Open,On Hold              # Multiple

# Par priorité
?priority=High,Urgent

# Par département
?departmentId=4000000000001

# Par agent
?assignee=4000000054321

# Par canal
?channel=Email

# Par date
?createdTimeRange=2026-01-01T00:00:00.000Z,2026-02-18T23:59:59.000Z

# Tri
?sortBy=createdTime&sortOrder=desc
?sortBy=dueDate&sortOrder=asc

# Inclure des champs supplémentaires
?include=contacts,assignee,departments
```

---

## Limites

| Limite | Valeur |
|--------|--------|
| Requêtes par minute | 60 (Free), 200 (Standard), 500 (Professional/Enterprise) |
| Résultats par page | 100 max |
| Pièce jointe max | 20 Mo |

### Codes de réponse HTTP

| Code | Signification |
|------|---------------|
| `200` | Succès |
| `201` | Créé |
| `204` | Supprimé |
| `400` | Requête invalide |
| `401` | Non authentifié |
| `403` | Interdit |
| `404` | Ressource introuvable |
| `429` | Trop de requêtes (rate limit) |
| `500` | Erreur serveur |

---

## Bonnes pratiques

1. **orgId** : Toujours inclure dans les headers
2. **Pagination** : Itérer avec `from` et `limit` pour les grandes collections
3. **Rate limiting** : Implémenter un retry avec backoff exponentiel
4. **Webhooks** : Préférer les webhooks Desk au polling pour les mises à jour temps réel
5. **Champs personnalisés** : Préfixés par `cf_` dans l'API
6. **Include** : Utiliser le paramètre `include` pour réduire le nombre d'appels

---

*Voir aussi : [automatisations.md](automatisations.md) | [tickets.md](tickets.md) | [configuration.md](configuration.md)*
