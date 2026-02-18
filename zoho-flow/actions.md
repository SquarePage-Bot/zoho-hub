# Zoho Flow - Actions

## Types d'Actions

### 1. Actions d'Application
Opérations effectuées dans une application connectée.

#### Actions CRUD courantes
```
- Créer un enregistrement
- Mettre à jour un enregistrement
- Supprimer un enregistrement
- Rechercher un enregistrement
- Récupérer un enregistrement par ID
```

### 2. Actions Utilitaires

#### Formatter des données
```
Fonctions disponibles :
- Texte : majuscule, minuscule, trim, split, replace, concat
- Nombre : arrondir, formater, convertir
- Date : formater, ajouter/soustraire, différence
- Liste : filtrer, trier, mapper, réduire
```

**Exemples de formatage :**
```
# Texte
${uppercase(trigger.data.name)}           → "JEAN DUPONT"
${lowercase(trigger.data.email)}           → "jean@example.com"
${trim(trigger.data.description)}          → supprime espaces
${replace(trigger.data.phone, "+33", "0")} → "0612345678"

# Date
${formatDate(trigger.data.created, "dd/MM/yyyy")}  → "15/02/2026"
${addDays(trigger.data.date, 30)}                    → date + 30 jours
${dateDiff(now(), trigger.data.created, "days")}     → nombre de jours

# Nombre
${round(trigger.data.amount, 2)}           → 1234.57
${toNumber("1 234,56")}                    → 1234.56
```

#### Envoyer un Email
```
À : ${trigger.data.email}
Objet : Bienvenue ${trigger.data.first_name} !
Corps : HTML ou texte brut
Pièces jointes : URL ou fichier
```

#### Appel HTTP (Custom API)
```json
{
  "method": "POST",
  "url": "https://api.externe.com/endpoint",
  "headers": {
    "Authorization": "Bearer ${connection.token}",
    "Content-Type": "application/json"
  },
  "body": {
    "name": "${trigger.data.name}",
    "email": "${trigger.data.email}"
  }
}
```

#### Délai (Delay)
```
- Attendre X minutes/heures/jours
- Attendre jusqu'à une date/heure précise
- Attendre qu'une condition soit remplie (polling)
```

### 3. Actions Multi-Étapes

#### Créer puis Mettre à Jour
```
Étape 1 : Créer un contact dans Zoho CRM
  → Récupérer l'ID créé : ${step1.data.id}

Étape 2 : Mettre à jour le contact
  → ID : ${step1.data.id}
  → Champ : "Statut" = "Vérifié"
```

## Actions par Application

### Zoho CRM
| Action | Description |
|--------|-------------|
| Créer un enregistrement | Créer lead, contact, deal, etc. |
| Mettre à jour | Modifier les champs d'un record |
| Rechercher | Trouver par critères (email, nom...) |
| Upsert | Créer ou mettre à jour si existant |
| Convertir un lead | Transformer un lead en contact+deal |
| Ajouter une note | Attacher une note à un record |

### Zoho Desk
| Action | Description |
|--------|-------------|
| Créer un ticket | Nouveau ticket avec sujet, description |
| Mettre à jour un ticket | Changer statut, priorité, assigné |
| Ajouter un commentaire | Commentaire public ou privé |
| Envoyer une réponse | Répondre au client |

### Zoho Books
| Action | Description |
|--------|-------------|
| Créer une facture | Avec lignes d'articles |
| Enregistrer un paiement | Paiement sur facture |
| Créer un contact | Client ou fournisseur |
| Créer un avoir | Note de crédit |

### Applications Tierces
| App | Actions disponibles |
|-----|-------------------|
| Slack | Envoyer un message, créer un canal |
| Gmail | Envoyer un email, créer un brouillon |
| Google Sheets | Ajouter/modifier une ligne |
| Trello | Créer une carte, déplacer |
| Mailchimp | Ajouter un abonné, envoyer |
| Stripe | Créer un client, une facture |

## Gestion des Erreurs

### Retry (Nouvelle tentative)
```
Configuration :
- Nombre de tentatives : 1-5
- Délai entre tentatives : 1-60 min
- Action en cas d'échec final : Notifier / Ignorer / Arrêter
```

### Fallback (Action de secours)
```
Si l'action principale échoue :
  → Exécuter une action alternative
  → Exemple : Si CRM indisponible, logger dans Google Sheets
```

## Bonnes Pratiques

1. **Toujours gérer les erreurs** avec des actions de fallback
2. **Utiliser "Rechercher puis Créer/Mettre à jour"** pour éviter les doublons
3. **Limiter les appels HTTP** — vérifier les rate limits de l'API cible
4. **Logger les résultats** importants pour le débogage
5. **Utiliser les variables** plutôt que les valeurs en dur
