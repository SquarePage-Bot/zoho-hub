# Zoho Social - API

## Informations Générales

```
Base URL : https://social.zoho.com/api/v1
Authentification : OAuth 2.0
Format : JSON
Scope : ZohoSocial.publishmessage.ALL, ZohoSocial.dashboard.READ
```

## Authentification

```bash
curl -X POST "https://accounts.zoho.com/oauth/v2/token" \
  -d "grant_type=refresh_token" \
  -d "client_id=YOUR_CLIENT_ID" \
  -d "client_secret=YOUR_CLIENT_SECRET" \
  -d "refresh_token=YOUR_REFRESH_TOKEN" \
  -d "scope=ZohoSocial.publishmessage.ALL"
```

## Publications

### Créer une publication
```bash
curl -X POST "https://social.zoho.com/api/v1/publishmessage" \
  -H "Authorization: Zoho-oauthtoken {access_token}" \
  -H "Content-Type: application/json" \
  -d '{
    "message": "🚀 Nouvelle fonctionnalité disponible ! #update",
    "channels": ["facebook_page_id", "twitter_profile_id", "linkedin_page_id"],
    "schedule_time": "2026-02-20T10:00:00+01:00",
    "media_url": "https://example.com/image.png"
  }'
```

### Lister les publications planifiées
```bash
curl -X GET "https://social.zoho.com/api/v1/scheduledposts" \
  -H "Authorization: Zoho-oauthtoken {access_token}" \
  -d "from=2026-02-18" \
  -d "to=2026-02-28"
```

### Supprimer une publication
```bash
curl -X DELETE "https://social.zoho.com/api/v1/publishmessage/{post_id}" \
  -H "Authorization: Zoho-oauthtoken {access_token}"
```

## Statistiques

### Récupérer les stats d'un post
```bash
curl -X GET "https://social.zoho.com/api/v1/postanalytics/{post_id}" \
  -H "Authorization: Zoho-oauthtoken {access_token}"
```

**Réponse :**
```json
{
  "post_id": "12345",
  "reach": 12500,
  "impressions": 18200,
  "engagement": {
    "likes": 340,
    "comments": 28,
    "shares": 15,
    "clicks": 180
  },
  "engagement_rate": 4.5
}
```

### Stats globales d'un canal
```bash
curl -X GET "https://social.zoho.com/api/v1/channelanalytics/{channel_id}" \
  -H "Authorization: Zoho-oauthtoken {access_token}" \
  -d "from=2026-02-01" \
  -d "to=2026-02-28"
```

## Bonnes Pratiques API

1. **Respecter les limites** de chaque réseau social (ex: Twitter rate limits)
2. **Planifier plutôt que publier immédiatement** via l'API
3. **Vérifier les dimensions media** avant upload
4. **Utiliser les webhooks** pour les notifications d'engagement
