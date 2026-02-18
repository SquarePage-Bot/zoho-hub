# Zoho Flow - Connecteurs et Applications

## Applications Zoho (Connecteurs Natifs)

### Suite CRM & Ventes
| Application | Triggers | Actions | Notes |
|-------------|----------|---------|-------|
| Zoho CRM | ✅ 15+ | ✅ 20+ | Tous modules, champs personnalisés |
| Zoho SalesIQ | ✅ 5+ | ✅ 8+ | Chat en direct, visiteurs |
| Zoho Bookings | ✅ 3+ | ✅ 5+ | Rendez-vous, créneaux |
| Zoho Forms | ✅ 2+ | ✅ 3+ | Soumissions de formulaires |

### Suite Finance
| Application | Triggers | Actions | Notes |
|-------------|----------|---------|-------|
| Zoho Books | ✅ 10+ | ✅ 15+ | Factures, paiements, contacts |
| Zoho Invoice | ✅ 5+ | ✅ 8+ | Facturation simple |
| Zoho Inventory | ✅ 8+ | ✅ 12+ | Stock, commandes |
| Zoho Expense | ✅ 3+ | ✅ 5+ | Notes de frais |
| Zoho Subscriptions | ✅ 5+ | ✅ 8+ | Abonnements récurrents |

### Suite Support & Communication
| Application | Triggers | Actions | Notes |
|-------------|----------|---------|-------|
| Zoho Desk | ✅ 10+ | ✅ 12+ | Tickets, agents, SLA |
| Zoho Mail | ✅ 3+ | ✅ 5+ | Emails, dossiers |
| Zoho Cliq | ✅ 5+ | ✅ 8+ | Messages, canaux |

### Suite Projets & Productivité
| Application | Triggers | Actions | Notes |
|-------------|----------|---------|-------|
| Zoho Projects | ✅ 8+ | ✅ 10+ | Tâches, jalons, bugs |
| Zoho Sprints | ✅ 5+ | ✅ 8+ | Sprints, backlog |
| Zoho WorkDrive | ✅ 3+ | ✅ 5+ | Fichiers, dossiers |

### Suite Marketing
| Application | Triggers | Actions | Notes |
|-------------|----------|---------|-------|
| Zoho Campaigns | ✅ 5+ | ✅ 8+ | Listes, campagnes |
| Zoho Social | ✅ 3+ | ✅ 5+ | Publications |
| Zoho Survey | ✅ 2+ | ✅ 3+ | Réponses aux sondages |
| Zoho PageSense | ✅ 2+ | ✅ 3+ | A/B testing |

## Applications Tierces Populaires

### Communication
| Application | Description |
|-------------|-------------|
| Slack | Messages, canaux, réactions |
| Microsoft Teams | Messages, canaux, réunions |
| Discord | Messages, webhooks |
| Telegram | Messages, bots |
| Twilio | SMS, appels |

### Email & Marketing
| Application | Description |
|-------------|-------------|
| Gmail | Emails, libellés, brouillons |
| Mailchimp | Listes, campagnes, abonnés |
| SendGrid | Emails transactionnels |
| HubSpot | CRM, marketing, contacts |

### Productivité
| Application | Description |
|-------------|-------------|
| Google Sheets | Lignes, feuilles, formules |
| Google Calendar | Événements, rappels |
| Trello | Cartes, listes, tableaux |
| Asana | Tâches, projets |
| Notion | Pages, bases de données |
| Airtable | Enregistrements, vues |

### Paiement & E-commerce
| Application | Description |
|-------------|-------------|
| Stripe | Paiements, clients, abonnements |
| PayPal | Transactions, remboursements |
| Shopify | Commandes, produits, clients |
| WooCommerce | Commandes, produits |

### Développement
| Application | Description |
|-------------|-------------|
| GitHub | Issues, PR, commits |
| GitLab | Issues, merge requests |
| Jira | Tickets, sprints |
| Bitbucket | Repositories, PR |

## Créer une Connexion

### Étape 1 : Autoriser l'application
```
1. Aller dans Paramètres → Connexions
2. Cliquer sur "+ Nouvelle connexion"
3. Sélectionner l'application
4. Autoriser via OAuth (ou entrer la clé API)
5. Nommer la connexion (ex: "CRM Production")
```

### Étape 2 : Tester la connexion
```
1. Cliquer sur "Tester"
2. Vérifier que le statut est "Connecté"
3. Vérifier les permissions accordées
```

### Connexions Personnalisées (Custom)
Pour les APIs non listées, créer un connecteur personnalisé :

```json
{
  "name": "Mon API Personnalisée",
  "base_url": "https://api.monservice.com/v2",
  "auth_type": "oauth2",
  "auth_url": "https://api.monservice.com/oauth/authorize",
  "token_url": "https://api.monservice.com/oauth/token",
  "client_id": "votre_client_id",
  "client_secret": "votre_client_secret",
  "scopes": ["read", "write"]
}
```

## Webhooks Entrants

Recevoir des données de n'importe quel système :

```
URL : https://flow.zoho.com/webhook/catch/<id>/<key>

Payload attendu (exemple) :
{
  "event": "order.completed",
  "data": {
    "order_id": "ORD-12345",
    "customer_email": "client@example.com",
    "total": 99.99
  }
}
```

## Bonnes Pratiques

1. **Nommer les connexions clairement** : "CRM Production", "CRM Sandbox"
2. **Utiliser un compte de service** dédié pour les connexions
3. **Renouveler les tokens** avant expiration
4. **Limiter les permissions** au strict nécessaire
5. **Documenter les connexions** utilisées par chaque flow
6. **Tester en sandbox** avant de connecter la production
