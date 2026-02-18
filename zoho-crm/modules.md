# 📦 Zoho CRM - Modules

> Référence complète des modules standards et personnalisés.

## Modules standards

### Modules de vente (Sales)

| Module | Nom API | Description |
|--------|---------|-------------|
| **Leads** | `Leads` | Prospects non qualifiés |
| **Contacts** | `Contacts` | Personnes qualifiées |
| **Comptes** | `Accounts` | Entreprises/organisations |
| **Transactions** | `Deals` | Opportunités commerciales |
| **Devis** | `Quotes` | Propositions commerciales |
| **Commandes** | `Sales_Orders` | Bons de commande client |
| **Factures** | `Invoices` | Factures clients |
| **Bons de commande** | `Purchase_Orders` | Commandes fournisseurs |
| **Fournisseurs** | `Vendors` | Entreprises fournisseurs |

### Modules d'activité

| Module | Nom API | Description |
|--------|---------|-------------|
| **Tâches** | `Tasks` | Actions à réaliser |
| **Événements** | `Events` | Rendez-vous, réunions |
| **Appels** | `Calls` | Appels téléphoniques |

### Modules de support

| Module | Nom API | Description |
|--------|---------|-------------|
| **Dossiers** | `Cases` | Tickets de support |
| **Solutions** | `Solutions` | Base de connaissances |

### Autres modules standards

| Module | Nom API | Description |
|--------|---------|-------------|
| **Produits** | `Products` | Catalogue produits |
| **Livres de prix** | `Price_Books` | Tarification |
| **Campagnes** | `Campaigns` | Campagnes marketing |
| **Notes** | `Notes` | Notes attachées aux enregistrements |
| **Pièces jointes** | `Attachments` | Fichiers joints |

## Détail des modules principaux

### Leads (Prospects)

Point d'entrée du pipeline commercial. Un lead représente une personne ou entreprise ayant montré un intérêt.

**Champs clés :**
- Prénom, Nom, Entreprise, Email, Téléphone
- Source du lead, Statut du lead
- Propriétaire du lead
- Score (si scoring activé)

**Conversion de lead :**
```
Lead → Contact + Compte + Transaction (optionnel)
```

La conversion peut être :
- **Manuelle** : Bouton "Convertir"
- **Automatique** : Via workflow ou blueprint
- **Par API** : Endpoint `POST /Leads/{id}/actions/convert`

**Exemple Deluge - Conversion de lead :**
```deluge
// Convertir un lead en contact + compte + deal
leadId = "5234876000000123456";

convertData = Map();
convertData.put("overwrite", true);
convertData.put("notify_lead_owner", true);
convertData.put("notify_new_entity_owner", true);

dealData = Map();
dealData.put("Deal_Name", "Nouveau deal depuis lead");
dealData.put("Closing_Date", zoho.currentdate.addMonth(1).toString("yyyy-MM-dd"));
dealData.put("Stage", "Qualification");
dealData.put("Amount", 10000.0);
convertData.put("Deals", dealData);

response = zoho.crm.convertLead(leadId, convertData);
info response;
```

**Statuts de lead standards :**
1. Non contacté
2. Contacté
3. Intéressé
4. Non intéressé
5. Converti
6. Perdu

### Contacts

Personnes avec lesquelles vous avez une relation commerciale établie.

**Relations :**
- Appartient à un **Compte** (lookup)
- Peut avoir plusieurs **Transactions**
- Peut avoir des **Activités** (tâches, événements, appels)
- Peut être lié à des **Campagnes**

**Champs clés :**
- Prénom, Nom, Email, Téléphone, Mobile
- Compte (lookup vers Accounts)
- Titre, Département
- Adresse de correspondance, Autre adresse
- Date de naissance
- Propriétaire

### Accounts (Comptes)

Entreprises ou organisations clientes/prospects.

**Relations :**
- Contient plusieurs **Contacts**
- Peut avoir un **Compte parent** (hiérarchie)
- Contient des **Transactions**
- Lié aux **Activités**

**Champs clés :**
- Nom du compte
- Site web, Industrie, Chiffre d'affaires annuel
- Nombre d'employés
- Type de compte (Analyste, Compétiteur, Client, etc.)
- Propriétaire

### Deals (Transactions)

Opportunités commerciales avec un montant et une probabilité.

**Pipeline et étapes standards :**

| Étape | Probabilité | Type |
|-------|-------------|------|
| Qualification | 10% | Ouverte |
| Analyse des besoins | 20% | Ouverte |
| Proposition de valeur | 40% | Ouverte |
| Identifier les décideurs | 60% | Ouverte |
| Proposition/Devis | 75% | Ouverte |
| Négociation/Révision | 90% | Ouverte |
| Fermée gagnée | 100% | Gagnée |
| Fermée perdue | 0% | Perdue |

**Champs clés :**
- Nom de la transaction
- Montant, Étape, Date de clôture
- Compte (lookup), Contact (lookup)
- Type, Source du lead
- Probabilité (auto selon étape)
- Propriétaire

**Exemple Deluge - Créer un deal :**
```deluge
dealMap = Map();
dealMap.put("Deal_Name", "Projet CRM SquarePage");
dealMap.put("Stage", "Qualification");
dealMap.put("Amount", 25000);
dealMap.put("Closing_Date", "2026-06-30");
dealMap.put("Account_Name", "5234876000000456789"); // ID du compte
dealMap.put("Contact_Name", "5234876000000789012"); // ID du contact

response = zoho.crm.createRecord("Deals", dealMap);
info response;
```

## Modules personnalisés

### Création

- Disponible à partir de l'édition **Professionnelle**
- Maximum selon l'édition (10/25/100)
- Nom API : `Custom_Module_Name` (espaces remplacés par `_`)

### Bonnes pratiques

1. **Nommage** : Utiliser des noms métier clairs (ex: "Projets", "Contrats")
2. **Relations** : Définir les lookups vers les modules standards
3. **Layouts** : Créer des mises en page adaptées au module
4. **Permissions** : Configurer les profils d'accès

### Exemple - Créer un enregistrement dans un module personnalisé

```deluge
// Module personnalisé "Projets"
projetMap = Map();
projetMap.put("Name", "Refonte site web");
projetMap.put("Client", "5234876000000456789"); // Lookup vers Accounts
projetMap.put("Date_debut", "2026-03-01");
projetMap.put("Date_fin", "2026-06-30");
projetMap.put("Statut", "En cours");
projetMap.put("Budget", 15000);

response = zoho.crm.createRecord("Projets", projetMap);
info response;
```

## Relations entre modules

```
Leads ──(conversion)──→ Contacts + Accounts + Deals
                              │           │        │
                              │           │        ├── Quotes
                              │           │        ├── Sales_Orders
                              │           │        └── Invoices
                              │           │
                              │           ├── Tasks
                              │           ├── Events
                              │           └── Calls
                              │
                              ├── Campaigns (Many-to-Many)
                              └── Cases
```

## Lookups et relations

### Types de relations

| Type | Description | Exemple |
|------|-------------|---------|
| **Lookup** | Relation N:1 | Contact → Compte |
| **Multi-select lookup** | Relation N:N | Contact ↔ Campagne |
| **Sous-formulaire** | Relation parent-enfant | Deal → Lignes de produits |

### Accéder aux données liées (Deluge)

```deluge
// Récupérer le compte d'un contact
contact = zoho.crm.getRecordById("Contacts", contactId);
accountId = contact.get("Account_Name").get("id");
account = zoho.crm.getRecordById("Accounts", accountId);
info account.get("Account_Name");

// Récupérer les contacts d'un compte
contacts = zoho.crm.getRelatedRecords("Contacts", "Accounts", accountId);
for each contact in contacts
{
    info contact.get("Full_Name");
}

// Récupérer les deals d'un contact
deals = zoho.crm.getRelatedRecords("Deals", "Contacts", contactId);
for each deal in deals
{
    info deal.get("Deal_Name") + " - " + deal.get("Amount");
}
```

---
*Voir aussi : [champs.md](champs.md) pour les types de champs, [api.md](api.md) pour manipuler les modules via API.*
