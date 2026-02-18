# 🏦 Zoho Books - Bancaire

> Rapprochement bancaire, flux bancaires, comptes et réconciliation.

## Table des matières

- [Comptes bancaires](#comptes-bancaires)
- [Flux bancaires](#flux-bancaires)
- [Rapprochement bancaire](#rapprochement-bancaire)
- [Règles bancaires](#règles-bancaires)
- [Transferts entre comptes](#transferts-entre-comptes)

---

## Comptes bancaires

### Ajouter un compte

```
Banque → + Nouveau compte

Types :
- Compte courant (Checking)
- Compte d'épargne (Savings)
- Carte de crédit (Credit Card)
- Caisse (Cash)
- PayPal / Stripe (comptes en ligne)

Informations :
- Nom du compte
- Code comptable
- Devise
- Numéro de compte / IBAN
- BIC / SWIFT
- Solde d'ouverture
- Date du solde d'ouverture
```

### Connexion bancaire automatique

```
Banque → Connecter une banque

Banques supportées en France :
- BNP Paribas
- Société Générale
- Crédit Agricole
- Crédit Mutuel / CIC
- La Banque Postale
- Boursorama
- LCL
- HSBC
- Et d'autres via agrégateur

Fonctionnement :
1. Sélectionner la banque
2. S'authentifier (identifiants bancaires)
3. Zoho récupère les transactions automatiquement
4. Sync quotidienne (ou manuelle)
```

---

## Flux bancaires

### Import manuel

```
Banque → Compte → Importer un relevé

Formats supportés :
- OFX (Open Financial Exchange) — recommandé
- QIF (Quicken Interchange Format)
- CSV (avec mapping des colonnes)

Colonnes CSV attendues :
Date, Description, Montant (ou Débit/Crédit séparés), Référence
```

### Transactions bancaires

```
Chaque transaction importée peut être :

1. CATÉGORISÉE → Associée à un compte comptable
   Exemple : "ORANGE SA" → Charge Téléphone (626000)

2. MATCHÉE → Rapprochée avec une transaction existante
   Exemple : Virement de 1200€ ↔ Facture FAC-0042

3. EXCLUE → Ignorée (transaction personnelle, erreur...)
```

---

## Rapprochement bancaire

### Principe

```
Le rapprochement consiste à vérifier que :
  Solde du relevé bancaire = Solde dans Zoho Books

Pour chaque transaction du relevé :
  → Trouver la correspondance dans Books
  → Valider le rapprochement
```

### Processus

```
Banque → Compte → Rapprochement

Étape 1 : Saisir le solde du relevé bancaire (à la date souhaitée)
Étape 2 : Zoho affiche les transactions non rapprochées
Étape 3 : Cocher les transactions qui correspondent au relevé
Étape 4 : Vérifier que l'écart est de 0,00 €
Étape 5 : Valider le rapprochement
```

### Matching automatique

```
Zoho Books tente de matcher automatiquement :

Critères de matching :
- Montant identique
- Date proche (± 3 jours)
- Référence identique

Types de match :
- Transaction Books ↔ Transaction bancaire (1:1)
- Plusieurs transactions Books ↔ 1 transaction bancaire (N:1)
  Exemple : 3 factures payées par un seul virement
```

### Résolution des écarts

```
Causes courantes d'écart :
- Frais bancaires non enregistrés
- Chèques émis non encaissés
- Virements en transit
- Erreurs de saisie

Solutions :
- Créer la transaction manquante dans Books
- Ajuster le montant si erreur
- Reporter au rapprochement suivant si en transit
```

---

## Règles bancaires

### Création de règles automatiques

```
Banque → Règles bancaires → Nouvelle règle

Condition :
  Si la description CONTIENT "ORANGE"
  ET le montant EST ENTRE 30 et 50

Action :
  Catégoriser comme : Charge Téléphone (626000)
  TVA : 20%
  Référence : "Abonnement Orange"

Options :
  - Appliquer automatiquement : Oui
  - Demander confirmation : Non
```

### Exemples de règles

```
Règle 1 : Loyer
  Condition : Description contient "SCI IMMOBILIER" ET Montant = 2500
  Action : Charge Loyer (613000)

Règle 2 : Salaires
  Condition : Description contient "VIR SALAIRE"
  Action : Charge Salaires (641000)

Règle 3 : Encaissement client
  Condition : Description contient "VIR RECU" ET Montant > 0
  Action : Tenter de matcher avec une facture client

Règle 4 : Frais bancaires
  Condition : Description contient "FRAIS" OU "COMMISSION"
  Action : Charge Services bancaires (627000)

Règle 5 : Abonnements SaaS
  Condition : Description contient "STRIPE" OU "PAYPAL"
  Action : Charge Logiciels (615000), TVA 20%
```

---

## Transferts entre comptes

### Enregistrer un transfert

```
Banque → Transfert entre comptes

Champs :
- Compte source
- Compte destination
- Montant
- Date
- Référence
- Description

Le transfert crée automatiquement :
- Un débit sur le compte source
- Un crédit sur le compte destination
```

### Cas courants

```
- Virement du compte courant vers le compte épargne
- Alimentation de la caisse depuis le compte courant
- Transfert entre comptes en devises différentes (avec taux de change)
- Remise de chèques (caisse chèques → compte courant)
```

---

## Bonnes pratiques

1. **Rapprochement régulier** : Au minimum mensuel, idéalement hebdomadaire
2. **Règles bancaires** : Créer des règles pour les transactions récurrentes
3. **Connexion automatique** : Privilégier la connexion bancaire directe
4. **Écarts** : Ne jamais valider un rapprochement avec un écart non expliqué
5. **Multi-comptes** : Créer un compte par compte bancaire réel
6. **Clôture** : Rapprocher avant de clôturer un exercice

---

*Voir aussi : [configuration.md](configuration.md) | [rapports.md](rapports.md) | [automatisations.md](automatisations.md)*
