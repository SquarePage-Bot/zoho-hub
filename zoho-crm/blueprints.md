# 🗺️ Zoho CRM - Blueprints

> Processus guidés avec états et transitions pour structurer le cycle de vente.

## Concept

Un Blueprint définit un **processus métier structuré** sous forme de machine à états. Il force les utilisateurs à suivre un chemin défini, contrairement aux workflows qui sont invisibles.

```
État A ──(Transition 1)──→ État B ──(Transition 2)──→ État C
                              │
                              └──(Transition 3)──→ État D
```

### Blueprint vs Workflow

| Aspect | Workflow | Blueprint |
|--------|----------|-----------|
| Visibilité | Invisible (arrière-plan) | Visible (boutons de transition) |
| Contrôle | Réactif | Proactif (guide l'utilisateur) |
| Séquence | Actions indépendantes | Processus séquentiel obligatoire |
| Données | N'impose pas de saisie | Peut exiger des champs à chaque étape |
| Approbation | Non | Oui (transitions conditionnelles) |

## Composants

### 1. États (States)

Les états correspondent aux valeurs d'un champ **picklist** (généralement le champ "Stage" ou "Status").

**Exemple pour les Deals :**
```
[Qualification] → [Analyse des besoins] → [Proposition] → [Négociation] → [Fermée gagnée]
                                                                       ↘ [Fermée perdue]
```

### 2. Transitions

Une transition est le passage d'un état à un autre. Elle peut contenir :

| Élément | Description |
|---------|-------------|
| **Avant** (Before) | Champs obligatoires à remplir, conditions |
| **Pendant** (During) | Formulaire de transition (saisie utilisateur) |
| **Après** (After) | Actions automatiques (comme un workflow) |

### 3. Conditions de transition

Qui peut exécuter la transition :
- Rôles spécifiques
- Profils spécifiques
- Propriétaire de l'enregistrement
- Conditions sur les champs

## Création d'un Blueprint

### Étape 1 : Choisir le module et le champ

- Module : Deals
- Champ : Stage
- Layout : Standard (ou spécifique)

### Étape 2 : Définir les états

Glisser-déposer les états (valeurs du picklist) sur le canvas.

### Étape 3 : Créer les transitions

Relier les états par des flèches (transitions).

### Étape 4 : Configurer chaque transition

## Exemple complet : Pipeline de vente

### Schéma

```
┌──────────────┐     Qualifier     ┌──────────────┐
│ Qualification │ ───────────────→ │   Analyse    │
└──────────────┘                   └──────────────┘
                                         │
                                   Proposer │
                                         ▼
                                   ┌──────────────┐
                                   │  Proposition  │
                                   └──────────────┘
                                    │           │
                            Négocier│           │ Perdre
                                    ▼           ▼
                              ┌──────────┐ ┌──────────┐
                              │Négociation│ │  Perdue   │
                              └──────────┘ └──────────┘
                                    │
                              Gagner│
                                    ▼
                              ┌──────────┐
                              │  Gagnée   │
                              └──────────┘
```

### Transition "Qualifier"

**Avant :**
- Le propriétaire du deal doit avoir le rôle "Commercial" ou supérieur
- Le champ "Account_Name" ne doit pas être vide

**Pendant (formulaire de transition) :**
```
Champs obligatoires :
- Budget estimé (Currency) → Obligatoire
- Décideur identifié (Boolean) → Obligatoire  
- Besoin principal (Multi-line) → Obligatoire
- Prochain rendez-vous (Date) → Optionnel
- Notes de qualification (Multi-line) → Optionnel
```

**Après :**
- Mise à jour : `Probability` = 20%
- Tâche : "Préparer analyse des besoins" (J+3)
- Email : Notification au manager

### Transition "Proposer"

**Avant :**
- Le champ "Amount" doit être > 0
- Au moins un produit dans le sous-formulaire

**Pendant :**
```
Champs obligatoires :
- Date de la proposition (Date)
- Fichier de proposition (File Upload)
- Remise accordée (Percent) → Max 30%
```

**Après :**
- Email au contact : envoi de la proposition
- Tâche : "Relancer dans 5 jours" (J+5)
- Webhook : notifier le système de facturation

### Transition "Gagner"

**Avant :**
- `Amount` > 0
- `Closing_Date` renseignée
- Approbation du manager si `Amount` > 50 000€

**Pendant :**
```
Champs obligatoires :
- Numéro de commande (Single Line)
- Date de signature (Date)
- Commentaire de victoire (Multi-line)
```

**Après :**
- Mise à jour : `Stage` = "Fermée gagnée", `Probability` = 100%
- Fonction Deluge : créer le projet, facturer
- Email : félicitations à l'équipe
- Tag : "Client 2026"

### Transition "Perdre"

**Pendant :**
```
Champs obligatoires :
- Raison de la perte (Picklist) : Prix, Concurrent, Pas de budget, Timing, Autre
- Concurrent choisi (Single Line) → Si raison = "Concurrent"
- Commentaire (Multi-line)
```

**Après :**
- Mise à jour : `Stage` = "Fermée perdue", `Probability` = 0%
- Tâche : "Analyser la perte" (assignée au manager)

## Fonctions Deluge dans les Blueprints

### Bouton de transition avec fonction

```deluge
// Fonction exécutée lors de la transition "Gagner"
// Paramètre : dealId

deal = zoho.crm.getRecordById("Deals", dealId);
accountId = deal.get("Account_Name").get("id");
contactId = deal.get("Contact_Name").get("id");

// 1. Mettre à jour le compte
updateAccount = Map();
updateAccount.put("Account_Type", "Client");
updateAccount.put("Date_premier_contrat", zoho.currentdate.toString("yyyy-MM-dd"));
zoho.crm.updateRecord("Accounts", accountId, updateAccount);

// 2. Créer une facture
invoiceMap = Map();
invoiceMap.put("Subject", "Facture - " + deal.get("Deal_Name"));
invoiceMap.put("Account_Name", accountId);
invoiceMap.put("Contact_Name", contactId);
invoiceMap.put("Due_Date", zoho.currentdate.addDay(30).toString("yyyy-MM-dd"));
invoiceMap.put("Status", "Créée");

// Copier les lignes de produits du deal vers la facture
productDetails = deal.get("Product_Details");
invoiceMap.put("Product_Details", productDetails);

zoho.crm.createRecord("Invoices", invoiceMap);

// 3. Planifier le kickoff
eventMap = Map();
eventMap.put("Event_Title", "Kickoff - " + deal.get("Deal_Name"));
eventMap.put("Start_DateTime", zoho.currentdate.addDay(5).toString("yyyy-MM-dd'T'10:00:00+01:00"));
eventMap.put("End_DateTime", zoho.currentdate.addDay(5).toString("yyyy-MM-dd'T'11:00:00+01:00"));
eventMap.put("Participants", [{participant: contactId, type: "contact"}]);
zoho.crm.createRecord("Events", eventMap);
```

## Blueprints avec approbation

### Configuration d'une transition avec approbation

```
Transition : "Valider la remise exceptionnelle"
De : Proposition → Négociation
Condition : Discount > 30%

Approbateur : Manager du propriétaire
Délai d'approbation : 48h
Action si approuvé : Continuer la transition
Action si rejeté : Retour à "Proposition"
Action si timeout : Notifier le directeur
```

## Limites

| Édition | Blueprints/module | Transitions/blueprint | États/blueprint |
|---------|-------------------|----------------------|-----------------|
| Professionnel | 1 | 20 | 10 |
| Enterprise | 5 | 50 | 25 |
| Ultimate | 20 | 100 | 50 |

## API Blueprint

### Récupérer les blueprints d'un enregistrement

```
GET /v6/Deals/{record_id}/actions/blueprint
```

**Réponse :**
```json
{
  "blueprint": {
    "process_info": {
      "field_id": "5234876000000000123",
      "escalation": null,
      "is_continuous": false,
      "api_name": "Stage",
      "continuous": false,
      "field_label": "Stage",
      "name": "Pipeline Standard",
      "field_name": "Stage"
    },
    "transitions": [
      {
        "next_transitions": [],
        "data": [],
        "next_field_value": "Analyse des besoins",
        "name": "Qualifier",
        "criteria_matched": true,
        "id": "5234876000000456789",
        "fields": [
          {
            "api_name": "Budget_estime",
            "required": true
          }
        ],
        "type": "manual"
      }
    ]
  }
}
```

### Exécuter une transition via API

```
PUT /v6/Deals/{record_id}/actions/blueprint

{
  "blueprint": [
    {
      "transition_id": "5234876000000456789",
      "data": {
        "Budget_estime": 25000,
        "Decideur_identifie": true,
        "Besoin_principal": "Refonte du CRM"
      }
    }
  ]
}
```

## Bonnes pratiques

1. **Simplicité** : Maximum 6-8 états, éviter les processus trop complexes
2. **Documentation** : Nommer clairement chaque transition (verbe d'action)
3. **Champs minimaux** : Ne demander que les champs essentiels à chaque étape
4. **Approbations** : Réserver aux cas critiques (montants élevés, remises)
5. **SLA** : Définir des délais pour chaque état (utiliser les actions planifiées)
6. **Tests** : Tester chaque chemin possible avant déploiement
7. **Formation** : Former les utilisateurs au processus avant activation

---
*Voir aussi : [workflows.md](workflows.md) pour l'automatisation, [automatisations.md](automatisations.md) pour la vue d'ensemble.*
