# 🎫 Zoho Desk - Guide

> Plateforme de support client : tickets, base de connaissances, SLA, automatisations.

## Vue d'ensemble

Zoho Desk centralise le support client multicanal :
- Email, téléphone, chat, réseaux sociaux, formulaire web
- Gestion des tickets avec SLA
- Base de connaissances (self-service)
- Automatisations et IA (Zia)

## Concepts clés

### Structure

```
Département
├── Agents
├── Tickets
│   ├── Statut (Ouvert, En attente, Escaladé, Fermé)
│   ├── Priorité (Basse, Moyenne, Haute, Urgente)
│   ├── Canal (Email, Téléphone, Chat, Web)
│   ├── Conversations (fils de discussion)
│   └── Pièces jointes
├── SLA
│   ├── Temps de première réponse
│   └── Temps de résolution
└── Base de connaissances
    ├── Catégories
    └── Articles
```

### Cycle de vie d'un ticket

```
Nouveau → Ouvert → En cours → En attente (client) → Résolu → Fermé
                                    ↓
                               Escaladé → Résolu
```

## Intégration CRM ↔ Desk

```deluge
// Quand un ticket est créé dans Desk, enrichir avec les données CRM
email = ticket.get("email");

// Chercher le contact dans le CRM
contacts = zoho.crm.searchRecords("Contacts", "(Email:equals:" + email + ")", 1, 1);
if(contacts.size() > 0)
{
    contact = contacts.get(0);
    accountName = contact.get("Account_Name").get("name");
    
    // Ajouter le contexte au ticket
    info "Client : " + accountName;
    info "Type de compte : " + contact.get("Account_Type");
    
    // Priorité haute pour les clients VIP
    if(contact.get("Account_Type") == "Client Premium")
    {
        // Mettre le ticket en haute priorité
        updateMap = Map();
        updateMap.put("priority", "High");
        // Update ticket via API Desk
    }
}
```

## API Desk

```
Base URL : https://desk.zoho.eu/api/v1

GET    /tickets                 → Lister les tickets
POST   /tickets                 → Créer un ticket
GET    /tickets/{id}            → Détail
PATCH  /tickets/{id}            → Mettre à jour
DELETE /tickets/{id}            → Supprimer
POST   /tickets/{id}/comments   → Ajouter un commentaire
GET    /tickets/{id}/threads    → Conversations
```

### Créer un ticket via API

```bash
POST /api/v1/tickets

{
  "subject": "Problème de connexion",
  "description": "Je n'arrive plus à me connecter depuis ce matin",
  "email": "client@acme.com",
  "departmentId": "123456",
  "channel": "Email",
  "priority": "High",
  "status": "Open"
}
```

## Automatisations

### Règles d'attribution

```
Si Canal = "Email" ET Sujet contient "facturation" → Département Comptabilité
Si Priorité = "Urgente" → Agent senior
Si Langue = "Anglais" → Équipe internationale
```

### SLA

```
Client Standard :
  Première réponse : 8h ouvrées
  Résolution : 48h ouvrées

Client Premium :
  Première réponse : 2h ouvrées
  Résolution : 8h ouvrées

Escalade :
  Après 80% du SLA → Notification manager
  Après 100% du SLA → Réassignation + Alerte direction
```

## 📚 Documentation détaillée

| Fichier | Contenu |
|---------|---------|
| [configuration.md](configuration.md) | Départements, canaux, SLA, heures ouvrées |
| [tickets.md](tickets.md) | Création, assignation, statuts, vues |
| [automatisations.md](automatisations.md) | Workflows, macros, blueprints |
| [base-connaissances.md](base-connaissances.md) | Articles, catégories, portail client |
| [rapports.md](rapports.md) | Tableaux de bord, métriques, KPIs |
| [api.md](api.md) | API REST Desk, endpoints, exemples |

---
*Voir aussi : [../zoho-crm/](../zoho-crm/) pour le contexte client, [../integrations/](../integrations/).*
