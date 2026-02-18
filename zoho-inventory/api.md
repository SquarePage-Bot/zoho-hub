# Zoho Inventory - API REST

## Informations Générales

```
Base URL : https://www.zohoapis.com/inventory/v1
Authentification : OAuth 2.0
Format : JSON
Rate Limit : 100 requêtes / minute (varie selon le plan)
```

## Authentification

```bash
# Obtenir un access token
curl -X POST "https://accounts.zoho.com/oauth/v2/token" \
  -d "grant_type=refresh_token" \
  -d "client_id=YOUR_CLIENT_ID" \
  -d "client_secret=YOUR_CLIENT_SECRET" \
  -d "refresh_token=YOUR_REFRESH_TOKEN"
```

## Articles (Items)

### Lister les articles
```bash
curl -X GET "https://www.zohoapis.com/inventory/v1/items" \
  -H "Authorization: Zoho-oauthtoken {access_token}" \
  -d "organization_id={org_id}"
```

### Créer un article
```bash
curl -X POST "https://www.zohoapis.com/inventory/v1/items" \
  -H "Authorization: Zoho-oauthtoken {access_token}" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "T-shirt Premium",
    "sku": "TSH-PRE-001",
    "unit": "pcs",
    "rate": 29.99,
    "purchase_rate": 12.00,
    "item_type": "inventory",
    "product_type": "goods",
    "initial_stock": 100,
    "initial_stock_rate": 12.00,
    "reorder_level": 25,
    "tax_id": "TAX_ID_TVA20",
    "description": "T-shirt premium coton bio"
  }'
```

### Récupérer un article
```bash
curl -X GET "https://www.zohoapis.com/inventory/v1/items/{item_id}" \
  -H "Authorization: Zoho-oauthtoken {access_token}"
```

### Mettre à jour un article
```bash
curl -X PUT "https://www.zohoapis.com/inventory/v1/items/{item_id}" \
  -H "Authorization: Zoho-oauthtoken {access_token}" \
  -H "Content-Type: application/json" \
  -d '{
    "rate": 34.99,
    "reorder_level": 30
  }'
```

### Supprimer un article
```bash
curl -X DELETE "https://www.zohoapis.com/inventory/v1/items/{item_id}" \
  -H "Authorization: Zoho-oauthtoken {access_token}"
```

## Commandes de Vente (Sales Orders)

### Créer une commande
```bash
curl -X POST "https://www.zohoapis.com/inventory/v1/salesorders" \
  -H "Authorization: Zoho-oauthtoken {access_token}" \
  -H "Content-Type: application/json" \
  -d '{
    "customer_id": "CUSTOMER_ID",
    "date": "2026-02-18",
    "shipment_date": "2026-02-25",
    "reference_number": "REF-CLIENT-001",
    "line_items": [
      {
        "item_id": "ITEM_ID_1",
        "quantity": 50,
        "rate": 12.00,
        "discount": "5%"
      },
      {
        "item_id": "ITEM_ID_2",
        "quantity": 20,
        "rate": 10.00
      }
    ],
    "notes": "Livraison avant vendredi SVP",
    "shipping_charge": 15.00
  }'
```

### Lister les commandes
```bash
curl -X GET "https://www.zohoapis.com/inventory/v1/salesorders?status=confirmed" \
  -H "Authorization: Zoho-oauthtoken {access_token}"
```

### Confirmer une commande
```bash
curl -X POST "https://www.zohoapis.com/inventory/v1/salesorders/{so_id}/status/confirmed" \
  -H "Authorization: Zoho-oauthtoken {access_token}"
```

## Commandes d'Achat (Purchase Orders)

### Créer un bon de commande
```bash
curl -X POST "https://www.zohoapis.com/inventory/v1/purchaseorders" \
  -H "Authorization: Zoho-oauthtoken {access_token}" \
  -H "Content-Type: application/json" \
  -d '{
    "vendor_id": "VENDOR_ID",
    "date": "2026-02-18",
    "delivery_date": "2026-03-10",
    "line_items": [
      {
        "item_id": "ITEM_ID_1",
        "quantity": 500,
        "rate": 8.50
      }
    ]
  }'
```

## Gestion des Stocks

### Ajustement de stock
```bash
curl -X POST "https://www.zohoapis.com/inventory/v1/inventoryadjustments" \
  -H "Authorization: Zoho-oauthtoken {access_token}" \
  -H "Content-Type: application/json" \
  -d '{
    "date": "2026-02-18",
    "reason": "Inventaire annuel",
    "adjustment_type": "quantity",
    "line_items": [
      {
        "item_id": "ITEM_ID",
        "quantity_adjusted": -3,
        "warehouse_id": "WH_PARIS_ID"
      }
    ]
  }'
```

### Transfert entre entrepôts
```bash
curl -X POST "https://www.zohoapis.com/inventory/v1/transferorders" \
  -H "Authorization: Zoho-oauthtoken {access_token}" \
  -H "Content-Type: application/json" \
  -d '{
    "date": "2026-02-18",
    "from_warehouse_id": "WH_PARIS_ID",
    "to_warehouse_id": "WH_LYON_ID",
    "line_items": [
      {
        "item_id": "ITEM_ID",
        "quantity": 100
      }
    ]
  }'
```

## Contacts

### Créer un client
```bash
curl -X POST "https://www.zohoapis.com/inventory/v1/contacts" \
  -H "Authorization: Zoho-oauthtoken {access_token}" \
  -H "Content-Type: application/json" \
  -d '{
    "contact_name": "Entreprise ABC",
    "contact_type": "customer",
    "billing_address": {
      "address": "10 rue du Commerce",
      "city": "Paris",
      "zip": "75001",
      "country": "France"
    },
    "contact_persons": [
      {
        "first_name": "Jean",
        "last_name": "Dupont",
        "email": "jean@abc.fr",
        "phone": "+33612345678",
        "is_primary_contact": true
      }
    ]
  }'
```

## Webhooks

### Événements Disponibles
```
Items : item.created, item.updated, item.deleted
Sales Orders : salesorder.created, salesorder.confirmed, salesorder.shipped
Purchase Orders : purchaseorder.created, purchaseorder.received
Inventory : inventory.adjusted
Shipments : shipment.created, shipment.delivered
```

### Configurer un Webhook
```
Paramètres → Automatisation → Webhooks

URL : https://votre-serveur.com/webhook/inventory
Événement : salesorder.confirmed
Méthode : POST
Format : JSON
```

## Pagination

```bash
# Page 1 (défaut : 200 résultats par page)
GET /inventory/v1/items?page=1&per_page=200

# Réponse inclut :
{
  "page_context": {
    "page": 1,
    "per_page": 200,
    "has_more_page": true
  }
}
```

## Codes d'Erreur

| Code | Description |
|------|-------------|
| 200 | Succès |
| 201 | Créé |
| 400 | Requête invalide |
| 401 | Non authentifié |
| 403 | Interdit |
| 404 | Non trouvé |
| 429 | Rate limit atteint |
| 500 | Erreur serveur |
