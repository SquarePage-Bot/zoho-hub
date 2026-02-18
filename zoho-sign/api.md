# Zoho Sign - API REST

## Informations Générales

```
Base URL : https://sign.zoho.com/api/v1
Authentification : OAuth 2.0
Format : JSON
Scope : ZohoSign.documents.ALL, ZohoSign.templates.ALL
```

## Authentification

```bash
curl -X POST "https://accounts.zoho.com/oauth/v2/token" \
  -d "grant_type=refresh_token" \
  -d "client_id=YOUR_CLIENT_ID" \
  -d "client_secret=YOUR_CLIENT_SECRET" \
  -d "refresh_token=YOUR_REFRESH_TOKEN" \
  -d "scope=ZohoSign.documents.ALL,ZohoSign.templates.ALL"
```

## Documents

### Lister les documents
```bash
curl -X GET "https://sign.zoho.com/api/v1/requests" \
  -H "Authorization: Zoho-oauthtoken {access_token}" \
  -d "page_context[row_count]=20" \
  -d "page_context[start_index]=1"
```

### Récupérer un document
```bash
curl -X GET "https://sign.zoho.com/api/v1/requests/{request_id}" \
  -H "Authorization: Zoho-oauthtoken {access_token}"
```

### Envoyer un document pour signature
```bash
curl -X POST "https://sign.zoho.com/api/v1/requests" \
  -H "Authorization: Zoho-oauthtoken {access_token}" \
  -F "file=@contrat.pdf" \
  -F 'data={
    "requests": {
      "request_name": "Contrat Client ABC",
      "actions": [
        {
          "recipient_name": "Jean Dupont",
          "recipient_email": "jean@client.fr",
          "action_type": "SIGN",
          "signing_order": 1
        },
        {
          "recipient_name": "Marie Admin",
          "recipient_email": "marie@entreprise.fr",
          "action_type": "SIGN",
          "signing_order": 2
        }
      ],
      "expiration_days": 7,
      "email_reminders": true,
      "reminder_period": 2
    }
  }'
```

### Envoyer depuis un Template
```bash
curl -X POST "https://sign.zoho.com/api/v1/templates/{template_id}/createdocument" \
  -H "Authorization: Zoho-oauthtoken {access_token}" \
  -H "Content-Type: application/json" \
  -d '{
    "templates": {
      "actions": [
        {
          "recipient_name": "Jean Dupont",
          "recipient_email": "jean@client.fr",
          "action_type": "SIGN",
          "role": "Client"
        }
      ],
      "field_data": {
        "field_text_data": {
          "nom_client": "Entreprise ABC",
          "montant": "15000",
          "date_debut": "01/03/2026"
        }
      }
    }
  }'
```

### Télécharger un document signé
```bash
curl -X GET "https://sign.zoho.com/api/v1/requests/{request_id}/pdf" \
  -H "Authorization: Zoho-oauthtoken {access_token}" \
  -o "contrat-signe.pdf"
```

### Télécharger le certificat d'audit
```bash
curl -X GET "https://sign.zoho.com/api/v1/requests/{request_id}/completioncertificate" \
  -H "Authorization: Zoho-oauthtoken {access_token}" \
  -o "certificat-audit.pdf"
```

### Annuler un document
```bash
curl -X POST "https://sign.zoho.com/api/v1/requests/{request_id}/recall" \
  -H "Authorization: Zoho-oauthtoken {access_token}"
```

## Templates

### Lister les templates
```bash
curl -X GET "https://sign.zoho.com/api/v1/templates" \
  -H "Authorization: Zoho-oauthtoken {access_token}"
```

## Webhooks

### Événements
```
document.completed  → Tous les signataires ont signé
document.declined   → Un signataire a refusé
document.expired    → Le document a expiré
document.viewed     → Un signataire a ouvert le document
document.signed     → Un signataire individuel a signé
```

### Configuration
```bash
curl -X POST "https://sign.zoho.com/api/v1/webhooks" \
  -H "Authorization: Zoho-oauthtoken {access_token}" \
  -H "Content-Type: application/json" \
  -d '{
    "webhooks": {
      "webhook_name": "Signature completée",
      "webhook_url": "https://votre-serveur.com/webhook/sign",
      "events": ["document.completed", "document.declined"]
    }
  }'
```

## Bonnes Pratiques API

1. **Utiliser les templates** plutôt que d'uploader à chaque fois
2. **Configurer les webhooks** pour réagir aux signatures en temps réel
3. **Stocker les documents signés** dans votre système (ne pas dépendre uniquement de Zoho)
4. **Télécharger les certificats d'audit** pour archivage légal
5. **Gérer les erreurs** : vérifier les réponses et retry si nécessaire
