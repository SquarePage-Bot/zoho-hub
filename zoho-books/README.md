# 📒 Zoho Books - Guide

> Logiciel de comptabilité en ligne : facturation, dépenses, rapprochement bancaire, TVA.

## Vue d'ensemble

Zoho Books gère la comptabilité complète d'une entreprise :
- Devis, factures, avoirs
- Dépenses et achats
- Rapprochement bancaire
- Déclarations de TVA
- Rapports financiers (P&L, bilan, trésorerie)

## Modules principaux

| Module | Description |
|--------|-------------|
| **Contacts** | Clients et fournisseurs |
| **Articles** | Produits et services |
| **Factures** (Invoices) | Factures clients |
| **Devis** (Estimates) | Propositions commerciales |
| **Avoirs** (Credit Notes) | Notes de crédit |
| **Dépenses** (Expenses) | Frais et achats |
| **Factures fournisseurs** (Bills) | Factures reçues |
| **Paiements** (Payments) | Encaissements et décaissements |
| **Rapprochement bancaire** | Matching avec les relevés |
| **Journal** | Écritures comptables manuelles |

## Intégration CRM ↔ Books

### Synchronisation automatique

```
CRM: Deal gagné → Books: Facture créée automatiquement
CRM: Contact créé → Books: Client créé
Books: Facture payée → CRM: Deal mis à jour
```

### Via Deluge

```deluge
// Créer une facture Books depuis un deal CRM
deal = zoho.crm.getRecordById("Deals", dealId);
contact = zoho.crm.getRecordById("Contacts", deal.get("Contact_Name").get("id"));

invoiceData = Map();
invoiceData.put("customer_name", contact.get("Full_Name"));
invoiceData.put("customer_email", contact.get("Email"));
invoiceData.put("date", zoho.currentdate.toString("yyyy-MM-dd"));
invoiceData.put("due_date", zoho.currentdate.addDay(30).toString("yyyy-MM-dd"));

lineItems = List();
item = Map();
item.put("name", deal.get("Deal_Name"));
item.put("rate", deal.get("Amount"));
item.put("quantity", 1);
item.put("tax_id", "TVA_20"); // ID de la taxe configurée
lineItems.add(item);
invoiceData.put("line_items", lineItems);

response = invokeurl
[
    url: "https://www.zohoapis.eu/books/v3/invoices?organization_id=" + orgId
    type: POST
    headers: {"Content-Type": "application/json"}
    parameters: invoiceData.toString()
    connection: "zoho_books_conn"
];
```

## API Books

```
Base URL : https://www.zohoapis.eu/books/v3

GET    /invoices                → Lister les factures
POST   /invoices                → Créer une facture
GET    /invoices/{id}           → Détail d'une facture
PUT    /invoices/{id}           → Mettre à jour
DELETE /invoices/{id}           → Supprimer
POST   /invoices/{id}/email     → Envoyer par email
POST   /invoices/{id}/status/sent → Marquer comme envoyée
```

## Gestion de la TVA

| Taux | Description |
|------|-------------|
| 20% | Taux normal |
| 10% | Taux intermédiaire |
| 5.5% | Taux réduit |
| 2.1% | Taux super-réduit |
| 0% | Exonéré / Export |

## 📚 Documentation détaillée

| Fichier | Contenu |
|---------|---------|
| [configuration.md](configuration.md) | Paramétrage initial, TVA, devises, modèles |
| [factures.md](factures.md) | Création, récurrence, relances, mentions légales |
| [contacts.md](contacts.md) | Clients, fournisseurs, portail client |
| [bancaire.md](bancaire.md) | Rapprochement bancaire, flux, règles |
| [rapports.md](rapports.md) | Rapports financiers, tableau de bord |
| [automatisations.md](automatisations.md) | Workflows, webhooks, scripts Deluge |
| [api.md](api.md) | API REST Books, endpoints, exemples |

---
*Voir aussi : [../zoho-crm/](../zoho-crm/) pour l'intégration CRM, [../integrations/](../integrations/) pour les flux.*
