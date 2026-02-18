# 🧾 Zoho Books - Factures

> Création, personnalisation, récurrence, relances et cycle de vie des factures.

## Table des matières

- [Création d'une facture](#création-dune-facture)
- [Cycle de vie](#cycle-de-vie)
- [Factures récurrentes](#factures-récurrentes)
- [Relances automatiques](#relances-automatiques)
- [Devis et conversion](#devis-et-conversion)
- [Avoirs (Credit Notes)](#avoirs)
- [Paiements](#paiements)
- [Mentions légales France](#mentions-légales-france)

---

## Création d'une facture

### Champs principaux

```
Client                → Sélection dans les contacts
Numéro de facture     → Auto-incrémenté (préfixe configurable : FAC-0001)
Date de facture       → Date d'émission
Date d'échéance       → Calculée selon les conditions de paiement
Référence             → Numéro de commande client, PO number
Vendeur               → Agent commercial (optionnel)

Lignes de facture :
├── Article / Description
├── Quantité
├── Prix unitaire HT
├── Remise (% ou montant)
├── Taxe applicable
└── Montant HT ligne

Sous-total HT
Remise globale (optionnelle)
Frais de port (optionnels)
Total TVA (par taux)
Total TTC
```

### Conditions de paiement

| Condition | Description |
|-----------|-------------|
| À réception | Échéance = date de facture |
| Net 15 | 15 jours |
| Net 30 | 30 jours (standard B2B France) |
| Net 45 | 45 jours |
| Net 60 | 60 jours (maximum légal France) |
| Fin de mois + 30 | Fin de mois de facturation + 30 jours |
| Personnalisé | Nombre de jours configurable |

---

## Cycle de vie

```
Brouillon (Draft)
    ↓  [Envoyer / Marquer comme envoyée]
Envoyée (Sent)
    ↓  [Client paie]
    ├── Partiellement payée (Partially Paid)
    │       ↓  [Solde payé]
    └── Payée (Paid)

Autres statuts :
- En retard (Overdue) → Date d'échéance dépassée
- Annulée (Void) → Facture annulée (traçabilité conservée)
```

### Actions disponibles

| Action | Description |
|--------|-------------|
| **Envoyer par email** | Email au client avec PDF en PJ |
| **Imprimer** | Génération PDF pour impression |
| **Marquer comme envoyée** | Changer le statut sans envoyer |
| **Enregistrer un paiement** | Saisir un paiement reçu |
| **Dupliquer** | Créer une copie de la facture |
| **Créer un avoir** | Générer un avoir lié |
| **Annuler (Void)** | Annuler la facture |
| **Écrire au client** | Ajouter un commentaire/message |

---

## Factures récurrentes

### Configuration

```
Ventes → Factures récurrentes → Nouvelle

Paramètres :
- Client
- Profil récurrent (nom du profil)
- Fréquence : Hebdomadaire, Mensuelle, Trimestrielle, Annuelle, Personnalisée
- Date de début
- Date de fin (ou nombre d'occurrences, ou jamais)
- Lignes de facture (articles, prix, taxes)

Options :
- Envoyer automatiquement au client : Oui/Non
- Créer en brouillon : Oui/Non
- Activer le paiement en ligne
- Appliquer les crédits clients automatiquement
```

### Exemples de fréquences

```
Abonnement mensuel :
  Fréquence = Mensuelle
  Début = 01/03/2026
  Fin = Jamais
  → Facture générée le 1er de chaque mois

Maintenance trimestrielle :
  Fréquence = Tous les 3 mois
  Début = 01/01/2026
  Occurrences = 4
  → 4 factures : Jan, Avr, Jul, Oct

Location hebdomadaire :
  Fréquence = Hebdomadaire
  Jour = Lundi
  → Facture chaque lundi
```

---

## Relances automatiques

### Configuration des rappels

```
Paramètres → Rappels de paiement

Types de rappels :
1. Rappel avant échéance (reminder before due date)
2. Rappel le jour de l'échéance
3. Rappels après échéance (overdue reminders)
```

### Scénario de relance type

```
J-7  : Rappel courtois avant échéance
       "Votre facture {invoice.number} arrive à échéance le {invoice.due_date}."

J+0  : Rappel le jour de l'échéance
       "La facture {invoice.number} est due aujourd'hui."

J+7  : Première relance
       "La facture {invoice.number} est en retard de 7 jours."

J+15 : Deuxième relance
       "Sauf erreur de notre part, la facture {invoice.number} reste impayée."

J+30 : Relance ferme
       "Nous vous rappelons que la facture {invoice.number} est en retard de 30 jours.
        Des pénalités de retard seront appliquées conformément à nos CGV."

J+45 : Dernière relance avant recouvrement
       "Sans règlement sous 10 jours, nous transmettrons le dossier à notre
        service contentieux."
```

### Personnalisation des emails de relance

```
Variables disponibles :
${customer.name}        → Nom du client
${invoice.number}       → Numéro de facture
${invoice.date}         → Date d'émission
${invoice.due_date}     → Date d'échéance
${invoice.total}        → Montant total
${invoice.balance_due}  → Reste à payer
${invoice.overdue_days} → Nombre de jours de retard
${payment_link}         → Lien de paiement en ligne
```

---

## Devis et conversion

### Création d'un devis

```
Ventes → Devis → Nouveau

Mêmes champs qu'une facture :
- Client, lignes, taxes, conditions
- Date d'expiration du devis
- Notes au client
```

### Cycle de vie du devis

```
Brouillon → Envoyé → Accepté → Converti en facture
                   → Refusé
                   → Expiré
```

### Conversion en facture

```
Devis accepté → Bouton "Convertir en facture"
  - Tous les champs sont pré-remplis
  - Possibilité de modifier avant validation
  - Lien tracé entre le devis et la facture
```

---

## Avoirs

### Créer un avoir

```
Ventes → Avoirs → Nouveau

Cas d'utilisation :
- Retour de marchandise
- Erreur de facturation
- Remise exceptionnelle
- Annulation partielle

L'avoir peut être :
- Appliqué à une facture existante (déduit du solde)
- Remboursé au client (virement, CB...)
- Conservé en crédit client
```

### Lier un avoir à une facture

```
Avoir → Appliquer aux factures

Sélectionner la facture concernée :
  Facture FAC-0042 : 1 200,00 €
  Avoir AVR-0005 : -200,00 €
  Nouveau solde : 1 000,00 €
```

---

## Paiements

### Modes de paiement

```
Paramètres → Modes de paiement

Modes prédéfinis :
- Virement bancaire
- Chèque
- Carte bancaire
- Espèces
- PayPal
- Prélèvement SEPA
- Personnalisé

Chaque paiement est lié à un compte bancaire.
```

### Enregistrer un paiement

```
Facture → Enregistrer un paiement

Champs :
- Montant reçu
- Date du paiement
- Mode de paiement
- Référence (numéro de chèque, réf virement...)
- Compte bancaire de destination
- Notes

Paiement partiel : le solde restant est calculé automatiquement.
```

### Paiement en ligne

```
Sur la facture envoyée par email :
- Bouton "Payer maintenant"
- Redirection vers la passerelle (Stripe, PayPal...)
- Paiement automatiquement enregistré dans Books
- Email de confirmation au client
```

---

## Mentions légales France

### Mentions obligatoires sur une facture

```
✅ Date d'émission
✅ Numéro de facture (séquentiel, sans rupture)
✅ Identité du vendeur (nom/raison sociale, adresse, SIRET, RCS)
✅ Numéro de TVA intracommunautaire (vendeur et acheteur si B2B)
✅ Identité de l'acheteur (nom, adresse)
✅ Désignation des produits/services
✅ Quantité et prix unitaire HT
✅ Taux de TVA applicable
✅ Montant total HT et TTC
✅ Date d'échéance
✅ Conditions d'escompte (ou mention "Pas d'escompte pour paiement anticipé")
✅ Taux de pénalités de retard
✅ Indemnité forfaitaire de recouvrement : 40€
```

### Modèle de pied de page

```
Conditions de paiement : {payment_terms}
Pénalités de retard : 3 fois le taux d'intérêt légal
Indemnité forfaitaire pour frais de recouvrement : 40,00 €
Pas d'escompte pour paiement anticipé

{organization.name} — SIRET : {organization.siret}
RCS {organization.rcs} — TVA : {organization.tax_id}
Capital social : {organization.capital} €
```

---

## Bonnes pratiques

1. **Numérotation** : Ne jamais modifier la séquence une fois lancée (obligation légale)
2. **Envoi** : Privilégier l'envoi par email avec lien de paiement en ligne
3. **Relances** : Automatiser les rappels pour réduire les impayés
4. **Récurrentes** : Utiliser pour les abonnements afin d'éviter les oublis
5. **Avoirs** : Toujours créer un avoir plutôt que de supprimer une facture
6. **Archive** : Conserver les factures 10 ans (obligation légale France)

---

*Voir aussi : [contacts.md](contacts.md) | [bancaire.md](bancaire.md) | [rapports.md](rapports.md)*
