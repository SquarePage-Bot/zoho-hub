# Zoho Flow - Déclencheurs (Triggers)

## Types de Déclencheurs

### 1. Déclencheurs d'Application
Événements provenant d'une application connectée.

```
Exemples :
- Zoho CRM : "Quand un nouveau lead est créé"
- Zoho Desk : "Quand un ticket est mis à jour"
- Gmail : "Quand un email est reçu"
- Slack : "Quand un message est posté dans un canal"
```

### 2. Déclencheurs Webhook
Recevoir des données via une URL webhook personnalisée.

```
URL générée : https://flow.zoho.com/webhook/catch/<flow_id>/<webhook_id>

Méthodes supportées : POST, GET
Formats : JSON, XML, Form-data
```

**Exemple de configuration webhook :**
```json
{
  "trigger_type": "webhook",
  "method": "POST",
  "content_type": "application/json",
  "sample_payload": {
    "name": "Jean Dupont",
    "email": "jean@example.com",
    "source": "landing_page"
  }
}
```

### 3. Déclencheurs Planifiés (Schedule)
Exécution à intervalles réguliers.

```
Options de planification :
- Toutes les X minutes (minimum 5 min)
- Toutes les X heures
- Quotidien à une heure précise
- Hebdomadaire (jour + heure)
- Mensuel (date + heure)
```

**Exemple :** Vérifier chaque jour à 8h les factures impayées.

### 4. Déclencheurs Manuels
Exécution à la demande via le bouton "Run" ou via l'API.

## Déclencheurs par Application Zoho

### Zoho CRM
| Déclencheur | Description |
|-------------|-------------|
| Nouvel enregistrement | Quand un record est créé dans un module |
| Enregistrement mis à jour | Quand un champ spécifique change |
| Nouveau note | Quand une note est ajoutée |
| Blueprint transition | Quand une transition de blueprint se produit |
| Deal gagné | Quand un deal passe en "Closed Won" |

### Zoho Desk
| Déclencheur | Description |
|-------------|-------------|
| Nouveau ticket | Quand un ticket est créé |
| Ticket mis à jour | Quand le statut/priorité change |
| Nouveau commentaire | Quand un commentaire est ajouté |
| Ticket assigné | Quand un agent est assigné |

### Zoho Books
| Déclencheur | Description |
|-------------|-------------|
| Nouvelle facture | Quand une facture est créée |
| Paiement reçu | Quand un paiement est enregistré |
| Facture en retard | Quand une facture dépasse l'échéance |

## Configuration Avancée des Triggers

### Filtres sur les Déclencheurs
Appliquer des conditions directement sur le trigger pour ne déclencher le flow que si certains critères sont remplis.

```
Exemple : Déclencher uniquement si
- Lead.Source == "Website"
- Lead.Pays == "France"
- Lead.Score > 50
```

### Variables du Trigger
Chaque trigger expose des variables utilisables dans les actions :

```
${trigger.data.id}
${trigger.data.email}
${trigger.data.created_time}
${trigger.timestamp}
${trigger.app_name}
```

### Gestion des Doublons
Configurer la déduplication pour éviter les exécutions multiples :

```
Dédupliquer sur : email
Fenêtre : 24 heures
Action si doublon : Ignorer
```

## Bonnes Pratiques

1. **Spécifier les filtres au niveau du trigger** plutôt que dans la logique — économise des tâches
2. **Utiliser des webhooks** pour les intégrations personnalisées avec des systèmes externes
3. **Planifier aux heures creuses** pour les traitements en masse
4. **Tester avec des données réelles** en mode brouillon avant activation
5. **Documenter le payload attendu** pour les webhooks
