# 👥 Zoho Books - Contacts

> Gestion des clients, fournisseurs et personnes de contact.

## Table des matières

- [Types de contacts](#types-de-contacts)
- [Création d'un contact](#création-dun-contact)
- [Personnes de contact](#personnes-de-contact)
- [Conditions et crédits](#conditions-et-crédits)
- [Portail client](#portail-client)
- [Import et synchronisation](#import-et-synchronisation)

---

## Types de contacts

| Type | Description | Usage |
|------|-------------|-------|
| **Client** | Acheteur de biens/services | Factures, devis, avoirs |
| **Fournisseur** | Vendeur de biens/services | Factures fournisseurs, bons de commande |
| **Client et fournisseur** | Les deux rôles | Contact mixte |

---

## Création d'un contact

### Champs principaux

```
Onglet "Général" :
- Type : Client / Fournisseur / Les deux
- Civilité (M., Mme, Dr...)
- Prénom, Nom
- Nom de l'entreprise
- Nom affiché (sur les documents)
- Email
- Téléphone fixe, mobile
- Site web

Onglet "Autres détails" :
- SIRET / SIREN
- Numéro de TVA intracommunautaire
- Devise (si multi-devises activé)
- Conditions de paiement par défaut
- Remise par défaut (%)
- Portail client activé : Oui/Non

Onglet "Adresses" :
- Adresse de facturation
- Adresse de livraison (peut être différente)
- Plusieurs adresses possibles

Onglet "Personnes de contact" :
- Liste des interlocuteurs (nom, email, téléphone, rôle)

Onglet "Notes" :
- Notes internes (non visibles sur les documents)

Onglet "Champs personnalisés" :
- Champs ajoutés par l'utilisateur
```

---

## Personnes de contact

Chaque contact peut avoir plusieurs personnes de contact (interlocuteurs) :

```
Contact : ACME Corp
├── Jean Dupont (Directeur) — jean@acme.com — Principal
├── Marie Martin (Comptable) — compta@acme.com — Reçoit les factures
└── Paul Bernard (Acheteur) — achats@acme.com — Reçoit les devis
```

### Configuration

```
Pour chaque personne de contact :
- Prénom, Nom
- Email
- Téléphone
- Fonction/Rôle
- Principal : Oui/Non (destinataire par défaut)
- Reçoit les factures : Oui/Non
- Reçoit les devis : Oui/Non
- Reçoit les relevés : Oui/Non
```

---

## Conditions et crédits

### Conditions de paiement par contact

```
Client "ACME Corp" :
  Conditions = Net 30
  Remise par défaut = 5%
  Limite de crédit = 50 000 €
  Devise = EUR
  Taxe par défaut = TVA 20%
```

### Solde client

```
Informations automatiques :
- Factures impayées (nombre et montant)
- Factures en retard
- Crédits disponibles (avoirs non utilisés)
- Total des paiements reçus
- Relevé de compte
```

### Relevé de compte (Statement)

```
Contacts → Client → Relevé de compte

Contenu :
- Toutes les factures de la période
- Tous les paiements reçus
- Avoirs appliqués
- Solde ouvert

Envoi par email au client pour rapprochement.
```

---

## Portail client

### Activation

```
Paramètres → Préférences → Portail client → Activer

Fonctionnalités du portail :
- Le client consulte ses factures
- Il effectue des paiements en ligne
- Il accepte/refuse les devis
- Il consulte ses relevés de compte
- Il télécharge les PDF

Accès :
- Invitation par email
- URL personnalisée : https://books.zoho.eu/portal/votreentreprise
```

### Personnalisation du portail

```
Options :
- Logo et couleurs
- Modules visibles (factures, devis, paiements)
- Mentions sur la page d'accueil
- Langue du portail
```

---

## Import et synchronisation

### Import CSV

```
Contacts → Importer

Format CSV requis :
Nom,Email,Téléphone,Entreprise,Adresse,Ville,CP,Pays,TVA,Type
"Jean Dupont","jean@acme.com","+33612345678","ACME","12 rue de la Paix","Paris","75002","France","FR12345678901","client"

Mapping des colonnes :
- Associer chaque colonne CSV à un champ Books
- Gérer les doublons (ignorer, écraser, fusionner)
```

### Synchronisation CRM

```
Paramètres → Intégrations → Zoho CRM

Synchronisation bidirectionnelle :
CRM Contacts ↔ Books Clients
CRM Vendors ↔ Books Fournisseurs

Options :
- Sync automatique ou manuelle
- Champs synchronisés (mapping personnalisable)
- Gestion des conflits (CRM prioritaire ou Books prioritaire)
```

### API

```bash
# Lister les contacts
GET /contacts?organization_id=ORG_ID

# Créer un contact
POST /contacts?organization_id=ORG_ID
{
  "contact_name": "ACME Corp",
  "company_name": "ACME Corp",
  "contact_type": "customer",
  "billing_address": {
    "address": "12 rue de la Paix",
    "city": "Paris",
    "zip": "75002",
    "country": "France"
  },
  "contact_persons": [
    {
      "first_name": "Jean",
      "last_name": "Dupont",
      "email": "jean@acme.com",
      "is_primary_contact": true
    }
  ],
  "payment_terms": 30,
  "currency_id": "EUR_ID",
  "tax_id": "TVA_20_ID"
}
```

---

## Bonnes pratiques

1. **Nommage** : Utiliser le nom d'entreprise comme nom principal pour les B2B
2. **TVA** : Toujours renseigner le numéro de TVA pour les clients B2B EU
3. **Personnes de contact** : Séparer les destinataires de factures et de devis
4. **Portail client** : Activer pour les clients récurrents (réduit les relances)
5. **Doublons** : Vérifier régulièrement et fusionner les contacts en double
6. **Sync CRM** : Configurer dès le début pour éviter les décalages

---

*Voir aussi : [factures.md](factures.md) | [configuration.md](configuration.md) | [api.md](api.md)*
