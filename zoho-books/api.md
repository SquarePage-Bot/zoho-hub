# 🔌 Zoho Books - API REST

> Endpoints, authentification et exemples pour l'API Zoho Books.

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
Scopes Books :
ZohoBooks.fullaccess.all          → Accès complet
ZohoBooks.invoices.READ           → Lecture factures
ZohoBooks.invoices.CREATE         → Création factures
ZohoBooks.invoices.UPDATE         → Modification factures
ZohoBooks.invoices.DELETE         → Suppression factures
ZohoBooks.contacts.READ           → Lecture contacts
ZohoBooks.contacts.CREATE         → Création contacts
ZohoBooks.settings.READ           → Lecture paramètres
ZohoBooks.bankaccounts.READ       → Lecture comptes bancaires
```

**Headers :**
```
Authorization: Zoho-oauthtoken {access_token}
Content-Type: application/json
```

**Paramètre obligatoire :** `organization_id` sur chaque requête.

---

## Base URL

| Région | URL |
|--------|-----|
| Europe | `https://www.zohoapis.eu/books/v3` |
| US | `https://www.zohoapis.com/books/v3` |
| Inde | `https://www.zohoapis.in/books/v3` |

---

## Modules et endpoints

### Factures (Invoices)

| Méthode | Endpoint | Description |
|---------|----------|-------------|
| `GET` | `/invoices` | Lister les factures |
| `GET` | `/invoices/{id}` | Détail d'une facture |
| `POST` | `/invoices` | Créer une facture |
| `PUT` | `/invoices/{id}` | Modifier une facture |
| `DELETE` | `/invoices/{id}` | Supprimer une facture |
| `POST` | `/invoices/{id}/status/sent` | Marquer comme envoyée |
| `POST` | `/invoices/{id}/status/void` | Annuler |
| `POST` | `/invoices/{id}/email` | Envoyer par email |
| `GET` | `/invoices/{id}/payments` | Paiements liés |
| `GET` | `/invoices/{id}/credits` | Crédits appliqués |

### Contacts

| Méthode | Endpoint | Description |
|---------|----------|-------------|
| `GET` | `/contacts` | Lister |
| `GET` | `/contacts/{id}` | Détail |
| `POST` | `/contacts` | Créer |
| `PUT` | `/contacts/{id}` | Modifier |
| `DELETE` | `/contacts/{id}` | Supprimer |
| `GET` | `/contacts/{id}/statements` | Relevé de compte |

### Devis (Estimates)

| Méthode | Endpoint | Description |
|---------|----------|-------------|
| `GET` | `/estimates` | Lister |
| `POST` | `/estimates` | Créer |
| `POST` | `/estimates/{id}/status/accepted` | Accepter |
| `POST` | `/estimates/{id}/status/declined` | Refuser |

### Articles (Items)

| Méthode | Endpoint | Description |
|---------|----------|-------------|
| `GET` | `/items` | Lister |
| `POST` | `/items` | Créer |
| `PUT` | `/items/{id}` | Modifier |
| `DELETE` | `/items/{id}` | Supprimer |

### Paiements (Customer Payments)

| Méthode | Endpoint | Description |
|---------|----------|-------------|
| `GET` | `/customerpayments` | Lister |
| `POST` | `/customerpayments` | Enregistrer un paiement |
| `DELETE` | `/customerpayments/{id}` | Supprimer |

### Dépenses (Expenses)

| Méthode | Endpoint | Description |
|---------|----------|-------------|
| `GET` | `/expenses` | Lister |
| `POST` | `/expenses` | Créer |
| `PUT` | `/expenses/{id}` | Modifier |

### Comptes bancaires

| Méthode | Endpoint | Description |
|---------|----------|-------------|
| `GET` | `/bankaccounts` | Lister |
| `GET` | `/bankaccounts/{id}` | Détail |
| `GET` | `/banktransactions` | Transactions bancaires |

### Taxes

| Méthode | Endpoint | Description |
|---------|----------|-------------|
| `GET` | `/settings/taxes` | Lister les taxes |
| `POST` | `/settings/taxes` | Créer une taxe |

---

## Exemples complets

### Créer une facture

```bash
curl -X POST "https://www.zohoapis.eu/books/v3/invoices?organization_id=ORG_ID" \
  -H "Authorization: Zoho-oauthtoken 1000.xxxx" \
  -H "Content-Type: application/json" \
  -d '{
    "customer_id": "460000000012345",
    "invoice_number": "FAC-0042",
    "date": "2026-02-18",
    "due_date": "2026-03-20",
    "payment_terms": 30,
    "line_items": [
      {
        "item_id": "460000000054321",
        "name": "Prestation de conseil",
        "description": "Audit SI - Février 2026",
        "rate": 1500.00,
        "quantity": 3,
        "tax_id": "460000000099999"
      },
      {
        "name": "Frais de déplacement",
        "rate": 250.00,
        "quantity": 1,
        "tax_id": "460000000099999"
      }
    ],
    "notes": "Merci pour votre confiance.",
    "terms": "Paiement à 30 jours. Pénalités de retard : 3x le taux légal."
  }'
```

**Réponse :**
```json
{
  "code": 0,
  "message": "The invoice has been created.",
  "invoice": {
    "invoice_id": "460000000078901",
    "invoice_number": "FAC-0042",
    "status": "draft",
    "total": 5250.00,
    "tax_total": 1050.00,
    "sub_total": 4750.00,
    "balance": 5700.00
  }
}
```

### Lister les factures impayées

```bash
curl -X GET "https://www.zohoapis.eu/books/v3/invoices?organization_id=ORG_ID&status=unpaid&sort_column=due_date&sort_order=A" \
  -H "Authorization: Zoho-oauthtoken 1000.xxxx"
```

### Enregistrer un paiement

```bash
curl -X POST "https://www.zohoapis.eu/books/v3/customerpayments?organization_id=ORG_ID" \
  -H "Authorization: Zoho-oauthtoken 1000.xxxx" \
  -H "Content-Type: application/json" \
  -d '{
    "customer_id": "460000000012345",
    "payment_mode": "Bank Transfer",
    "amount": 5700.00,
    "date": "2026-03-15",
    "reference_number": "VIR-20260315",
    "invoices": [
      {
        "invoice_id": "460000000078901",
        "amount_applied": 5700.00
      }
    ],
    "account_id": "460000000000123"
  }'
```

### Créer un contact

```bash
curl -X POST "https://www.zohoapis.eu/books/v3/contacts?organization_id=ORG_ID" \
  -H "Authorization: Zoho-oauthtoken 1000.xxxx" \
  -H "Content-Type: application/json" \
  -d '{
    "contact_name": "ACME Corp",
    "company_name": "ACME Corp",
    "contact_type": "customer",
    "payment_terms": 30,
    "currency_id": "460000000000097",
    "billing_address": {
      "address": "12 rue de la Paix",
      "city": "Paris",
      "zip": "75002",
      "country": "France"
    },
    "contact_persons": [
      {
        "first_name": "Jean",
        "last_name": "Dupont",
        "email": "jean@acme.com",
        "phone": "+33612345678",
        "is_primary_contact": true
      }
    ]
  }'
```

---

## Pagination et filtres

### Pagination

```bash
# Page 1 (défaut : 200 résultats)
GET /invoices?organization_id=ORG_ID&page=1&per_page=25

# Page suivante
GET /invoices?organization_id=ORG_ID&page=2&per_page=25
```

### Filtres courants

```bash
# Par statut
GET /invoices?status=unpaid          # sent, draft, unpaid, paid, overdue, void
GET /invoices?status=overdue

# Par client
GET /invoices?customer_id=460000000012345

# Par date
GET /invoices?date_start=2026-01-01&date_end=2026-12-31

# Par montant
GET /invoices?total_start=1000&total_end=5000

# Tri
GET /invoices?sort_column=date&sort_order=D    # D=desc, A=asc

# Recherche
GET /contacts?search_text=acme
```

---

## Limites

| Limite | Valeur |
|--------|--------|
| Requêtes par minute (org) | 100 |
| Requêtes par jour | 2 500 (gratuit) à 100 000 (enterprise) |
| Résultats par page | 200 max |
| Taille body | 10 Mo |

### Codes de réponse

| Code | Signification |
|------|---------------|
| `0` | Succès |
| `1` | Erreur interne |
| `2` | Validation échouée |
| `3` | Autorisation refusée |
| `4` | Ressource introuvable |
| `5` | Méthode non autorisée |
| `14` | Limite de requêtes atteinte |

---

## Bonnes pratiques

1. **organization_id** : Toujours inclure ce paramètre
2. **Pagination** : Itérer avec `page` pour les gros volumes
3. **Webhooks** : Préférer les webhooks au polling pour les mises à jour temps réel
4. **Batch** : Regrouper les opérations quand possible
5. **Cache** : Mettre en cache les données de référence (taxes, items)

---

*Voir aussi : [automatisations.md](automatisations.md) | [factures.md](factures.md) | [contacts.md](contacts.md)*
