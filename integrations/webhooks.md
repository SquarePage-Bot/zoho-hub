# Intégrations - Webhooks

## Webhooks Sortants (Zoho → Externe)

### Configuration dans Zoho CRM
```
Paramètres → Actions → Webhooks → Nouveau

Nom : Notifier ERP - Nouveau Deal
URL : https://erp.entreprise.com/api/webhook/zoho-deal
Méthode : POST
Module : Deals
Événement : Création

Headers :
  Content-Type: application/json
  X-Webhook-Secret: votre_secret_123

Corps (JSON) :
{
  "event": "deal.created",
  "data": {
    "id": "${Deals.Deal Id}",
    "name": "${Deals.Deal Name}",
    "amount": "${Deals.Amount}",
    "stage": "${Deals.Stage}",
    "account": "${Deals.Account Name}",
    "owner": "${Deals.Deal Owner}",
    "created": "${Deals.Created Time}"
  }
}
```

### Configuration dans Zoho Books
```
Paramètres → Automatisation → Webhooks

Événements disponibles :
  - invoice.created
  - invoice.updated
  - payment.received
  - expense.created
  - contact.created

Exemple :
  Événement : payment.received
  URL : https://app.entreprise.com/webhook/payment
  Corps : Données complètes du paiement (JSON automatique)
```

### Configuration dans Zoho Desk
```
Paramètres → Automatisation → Webhooks

Événements :
  - ticket.created
  - ticket.updated
  - ticket.resolved
  - comment.added

Avec conditions :
  SI ticket.priority == "Urgente"
  → Envoyer le webhook
```

## Webhooks Entrants (Externe → Zoho)

### Via Zoho Flow
```
Créer un flow avec trigger Webhook :
  URL générée : https://flow.zoho.com/webhook/catch/{flow_id}/{key}
  Méthode : POST
  Format : JSON

Exemple : Recevoir une commande Shopify
  Payload :
  {
    "order_id": "1234",
    "customer": { "email": "client@test.com" },
    "total": 99.99
  }
  
  → Action : Créer un Deal dans CRM
  → Action : Créer une commande dans Inventory
```

### Via Zoho CRM (Fonctions Personnalisées)
```
Paramètres → Développeur → APIs → Fonctions

Fonction : recevoir_commande_externe
Type : REST API
Méthode : POST

Code Deluge :
void recevoir_commande_externe(Map params)
{
    order_id = params.get("order_id");
    email = params.get("customer_email");
    total = params.get("total");
    
    // Rechercher le contact
    contacts = zoho.crm.searchRecords("Contacts", "(Email:equals:" + email + ")");
    
    if (contacts.size() > 0)
    {
        contact_id = contacts.get(0).get("id");
        
        // Créer un deal
        dealMap = Map();
        dealMap.put("Deal_Name", "Commande #" + order_id);
        dealMap.put("Amount", total);
        dealMap.put("Stage", "Closed Won");
        dealMap.put("Contact_Name", contact_id);
        
        zoho.crm.createRecord("Deals", dealMap);
    }
}

URL d'appel : https://www.zohoapis.com/crm/v6/functions/recevoir_commande_externe/actions/execute?auth_type=apikey&zapikey=VOTRE_CLE
```

## Sécurisation des Webhooks

### Vérifier la Signature
```python
# Côté récepteur (votre serveur)
import hmac
import hashlib

def verify_webhook(payload, signature, secret):
    expected = hmac.new(
        secret.encode('utf-8'),
        payload.encode('utf-8'),
        hashlib.sha256
    ).hexdigest()
    
    return hmac.compare_digest(expected, signature)

# Dans votre endpoint Flask/Django
@app.route('/webhook/zoho', methods=['POST'])
def handle_webhook():
    signature = request.headers.get('X-Webhook-Signature')
    payload = request.get_data(as_text=True)
    
    if not verify_webhook(payload, signature, WEBHOOK_SECRET):
        return "Unauthorized", 401
    
    data = request.get_json()
    process_webhook(data)
    return "OK", 200
```

### IP Whitelisting
```
IPs Zoho à autoriser :
  136.143.188.0/24
  136.143.189.0/24
  (vérifier la doc officielle pour la liste à jour)
```

## Bonnes Pratiques

1. **Toujours vérifier la signature** des webhooks entrants
2. **Répondre rapidement** (< 10 secondes) — traiter en arrière-plan si long
3. **Idempotence** — gérer les doublons (le même webhook peut être envoyé 2 fois)
4. **Logger tous les webhooks** reçus pour le débogage
5. **Monitorer les échecs** — configurer des alertes si un webhook échoue
6. **Utiliser HTTPS** obligatoirement pour les URLs de webhook
