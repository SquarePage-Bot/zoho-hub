# ⚙️ Zoho Books - Configuration

> Paramétrage initial, TVA, devises, modèles de documents et préférences.

## Table des matières

- [Paramétrage initial](#paramétrage-initial)
- [Informations de l'organisation](#informations-de-lorganisation)
- [TVA et taxes](#tva-et-taxes)
- [Devises](#devises)
- [Modèles de documents](#modèles-de-documents)
- [Plan comptable](#plan-comptable)
- [Passerelles de paiement](#passerelles-de-paiement)
- [Utilisateurs et rôles](#utilisateurs-et-rôles)

---

## Paramétrage initial

### Étapes de configuration

```
1. Créer l'organisation (nom, pays, secteur)
2. Configurer l'adresse et les coordonnées
3. Paramétrer l'exercice fiscal
4. Configurer la TVA (numéro TVA, taux)
5. Définir la devise principale
6. Personnaliser les modèles de documents
7. Configurer les passerelles de paiement
8. Importer les données existantes (contacts, articles)
```

### Exercice fiscal

```
Paramètres → Préférences → Exercice fiscal

Options :
- Janvier à Décembre (année civile)
- Avril à Mars (UK/Inde)
- Juillet à Juin
- Personnalisé

Méthode comptable :
- Comptabilité d'engagement (accrual) — recommandée
- Comptabilité de trésorerie (cash)
```

---

## Informations de l'organisation

```
Paramètres → Organisation

Champs :
- Nom de l'organisation
- Adresse complète
- Numéro SIRET / SIREN
- Numéro de TVA intracommunautaire (FR XX XXXXXXXXX)
- Téléphone, email, site web
- Logo (affiché sur les documents)
- Fuseau horaire
- Format de date (jj/mm/aaaa recommandé pour la France)
```

---

## TVA et taxes

### Configuration de la TVA française

```
Paramètres → Taxes

Taxes à créer :
┌──────────────────────┬───────┬─────────────────────────┐
│ Nom                  │ Taux  │ Usage                   │
├──────────────────────┼───────┼─────────────────────────┤
│ TVA 20%              │ 20%   │ Taux normal             │
│ TVA 10%              │ 10%   │ Taux intermédiaire      │
│ TVA 5.5%             │ 5.5%  │ Taux réduit             │
│ TVA 2.1%             │ 2.1%  │ Taux super-réduit       │
│ Exonéré              │ 0%    │ Export, DOM-TOM         │
│ Autoliquidation      │ 0%    │ Achats intracommunautaires │
└──────────────────────┴───────┴─────────────────────────┘
```

### TVA intracommunautaire

```
Pour les ventes B2B intracommunautaires :
- Taux = 0% (exonération art. 262 ter du CGI)
- Mention obligatoire sur la facture :
  "Exonération de TVA - Art. 262 ter I du CGI"
- Numéro de TVA du client obligatoire

Pour les achats intracommunautaires :
- Mécanisme d'autoliquidation
- Configurer une taxe "Reverse Charge" dans Books
```

### Groupes de taxes

```
Créer des groupes pour les taxes composées :

Groupe "TVA sur marge" :
  - TVA 20% sur la marge uniquement

Groupe "Taxes multiples" :
  - Taxe 1 + Taxe 2 (cumul)
```

---

## Devises

### Configuration multi-devises

```
Paramètres → Devises

Devise principale : EUR (€)
Devises supplémentaires :
- USD ($) — Dollar américain
- GBP (£) — Livre sterling
- CHF (CHF) — Franc suisse

Taux de change :
- Automatique (Zoho récupère les taux journaliers)
- Manuel (saisie du taux lors de la création du document)
```

### Gestion des gains/pertes de change

```
Comptes automatiques :
- Gains de change réalisés → Produit financier
- Pertes de change réalisées → Charge financière
- Gains/pertes de change non réalisés → Écarts de conversion
```

---

## Modèles de documents

### Types de modèles

| Document | Description |
|----------|-------------|
| Facture (Invoice) | Facture client |
| Devis (Estimate) | Proposition commerciale |
| Avoir (Credit Note) | Note de crédit |
| Bon de commande (Purchase Order) | Commande fournisseur |
| Reçu de paiement (Payment Receipt) | Confirmation de paiement |
| Bon de livraison (Delivery Challan) | Bon de livraison |

### Personnalisation des modèles

```
Paramètres → Modèles → Factures

Éléments personnalisables :
- En-tête : Logo, nom, adresse
- Corps : Colonnes affichées, libellés
- Pied de page : Conditions générales, mentions légales
- Couleurs et polices
- Champs personnalisés

Mentions légales obligatoires (France) :
- "TVA non applicable, art. 293 B du CGI" (micro-entreprise)
- Conditions de paiement
- Pénalités de retard
- Indemnité forfaitaire pour frais de recouvrement : 40€
- Numéro SIRET, RCS, capital social
```

### Variables disponibles dans les modèles

```
${organization.name}          → Nom de l'entreprise
${organization.address}       → Adresse
${organization.tax_id}        → Numéro de TVA
${invoice.number}             → Numéro de facture
${invoice.date}               → Date
${invoice.due_date}           → Date d'échéance
${customer.name}              → Nom du client
${customer.billing_address}   → Adresse de facturation
${line_item.name}             → Désignation de la ligne
${line_item.quantity}         → Quantité
${line_item.rate}             → Prix unitaire
${invoice.sub_total}          → Sous-total HT
${invoice.tax_total}          → Total TVA
${invoice.total}              → Total TTC
${invoice.balance_due}        → Reste à payer
```

---

## Plan comptable

### Plan comptable par défaut (France)

```
Zoho Books fournit un plan comptable adapté au pays.
Personnalisation possible dans :
Paramètres → Plan comptable

Catégories principales :
├── Actifs
│   ├── Actifs courants (512 Banque, 411 Clients...)
│   └── Actifs immobilisés
├── Passifs
│   ├── Passifs courants (401 Fournisseurs...)
│   └── Passifs long terme
├── Capitaux propres
├── Revenus (70x Ventes...)
├── Coût des ventes
├── Charges (60x Achats, 61x Services...)
└── Autres revenus/charges
```

### Ajouter un compte

```
Paramètres → Plan comptable → + Nouveau compte

Champs :
- Nom du compte
- Code comptable (ex : 411000)
- Type de compte (Actif, Passif, Revenu, Charge)
- Sous-type
- Description
- Compte parent (pour la hiérarchie)
```

---

## Passerelles de paiement

### Passerelles disponibles

| Passerelle | Cartes | Prélèvement | Régions |
|------------|--------|-------------|---------|
| **Stripe** | ✅ | ✅ SEPA | Europe, monde |
| **PayPal** | ✅ | ❌ | Monde |
| **GoCardless** | ❌ | ✅ SEPA | Europe |
| **Authorize.net** | ✅ | ✅ | US, Canada |
| **Zoho Payments** | ✅ | ❌ | US, Inde |

### Configuration Stripe

```
Paramètres → Passerelles de paiement en ligne → Stripe

1. Connecter le compte Stripe
2. Activer les modes de paiement (CB, SEPA)
3. Configurer les frais (absorbés ou facturés au client)
4. Activer le paiement en ligne sur les factures
```

---

## Utilisateurs et rôles

### Rôles prédéfinis

| Rôle | Droits |
|------|--------|
| **Admin** | Accès complet |
| **Comptable** | Toutes les transactions, rapports |
| **Facturier** | Factures, devis, contacts |
| **Consultant** | Lecture seule sur tout |
| **Saisie des dépenses** | Dépenses uniquement |

### Rôles personnalisés

```
Paramètres → Utilisateurs et rôles → Rôles → Nouveau rôle

Permissions granulaires :
- Par module (Factures, Dépenses, Rapports...)
- Par action (Créer, Lire, Modifier, Supprimer, Approuver)
- Par périmètre (Tous les enregistrements, les siens uniquement)
```

---

## Bonnes pratiques

1. **Exercice fiscal** : Configurer avant toute saisie, difficile à changer ensuite
2. **Plan comptable** : Adapter dès le début aux besoins spécifiques
3. **Modèles** : Vérifier les mentions légales obligatoires
4. **Multi-devises** : Activer uniquement si nécessaire (complexifie les rapports)
5. **Sauvegardes** : Exporter régulièrement les données (Paramètres → Export de données)

---

*Voir aussi : [factures.md](factures.md) | [contacts.md](contacts.md) | [bancaire.md](bancaire.md)*
