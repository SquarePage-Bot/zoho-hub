# 📊 Zoho Books - Rapports

> Rapports financiers, tableaux de bord et analyse de la performance.

## Table des matières

- [Catégories de rapports](#catégories-de-rapports)
- [Rapports financiers principaux](#rapports-financiers-principaux)
- [Rapports de ventes](#rapports-de-ventes)
- [Rapports d'achats](#rapports-dachats)
- [Rapports de taxes](#rapports-de-taxes)
- [Tableau de bord](#tableau-de-bord)
- [Rapports personnalisés](#rapports-personnalisés)
- [Export et planification](#export-et-planification)

---

## Catégories de rapports

| Catégorie | Rapports clés |
|-----------|---------------|
| **Comptabilité** | Bilan, Compte de résultat, Grand livre, Balance |
| **Ventes** | Factures par client, Ventes par article, Âge des créances |
| **Achats** | Factures fournisseurs, Achats par article, Âge des dettes |
| **Taxes** | Déclaration de TVA, TVA collectée, TVA déductible |
| **Trésorerie** | Flux de trésorerie, Prévisions |
| **Bancaire** | Rapprochement, Transactions non rapprochées |
| **Budget** | Budget vs Réel |

---

## Rapports financiers principaux

### Compte de résultat (Profit & Loss)

```
Rapports → Comptabilité → Compte de résultat

Affiche :
  Revenus
  ├── Ventes de produits          120 000 €
  ├── Prestations de services      80 000 €
  └── Autres revenus                5 000 €
  Total Revenus                   205 000 €

  Charges
  ├── Coût des ventes              60 000 €
  ├── Salaires                     80 000 €
  ├── Loyers                       24 000 €
  ├── Services externes            15 000 €
  └── Autres charges                6 000 €
  Total Charges                   185 000 €

  Résultat net                     20 000 €

Filtres :
- Période (mois, trimestre, année, personnalisé)
- Comparaison avec période précédente
- Par département / projet (si tags activés)
- Comptabilité d'engagement ou de trésorerie
```

### Bilan (Balance Sheet)

```
Rapports → Comptabilité → Bilan

Affiche :
  ACTIF
  ├── Actifs courants
  │   ├── Banque                   45 000 €
  │   ├── Clients (créances)       28 000 €
  │   └── Stocks                   12 000 €
  ├── Actifs immobilisés
  │   └── Matériel                 35 000 €
  Total Actif                     120 000 €

  PASSIF
  ├── Passifs courants
  │   ├── Fournisseurs (dettes)    15 000 €
  │   └── TVA à payer              8 000 €
  ├── Capitaux propres
  │   ├── Capital                  50 000 €
  │   ├── Réserves                 27 000 €
  │   └── Résultat de l'exercice   20 000 €
  Total Passif                    120 000 €
```

### Grand livre (General Ledger)

```
Détail de toutes les écritures par compte comptable.

Filtres :
- Compte spécifique ou tous les comptes
- Période
- Type d'écriture
```

### Balance générale (Trial Balance)

```
Liste de tous les comptes avec soldes débiteurs et créditeurs.
Permet de vérifier l'équilibre comptable.

Total Débits = Total Crédits ✓
```

### Flux de trésorerie (Cash Flow)

```
Rapports → Comptabilité → Flux de trésorerie

Trois sections :
1. Activités opérationnelles
   (encaissements clients, paiements fournisseurs)
2. Activités d'investissement
   (achats d'immobilisations)
3. Activités de financement
   (emprunts, apports en capital)

Variation nette de trésorerie sur la période.
```

---

## Rapports de ventes

### Ventes par client

```
Top clients par CA, nombre de factures, montant moyen.
Filtres : période, vendeur, article.
```

### Âge des créances (Aging)

```
Rapports → Ventes → Âge des créances

Tranches :
┌─────────────┬──────────┬──────────┬──────────┬──────────┬──────────┐
│ Client      │ Courant  │ 1-30 j   │ 31-60 j  │ 61-90 j  │ > 90 j   │
├─────────────┼──────────┼──────────┼──────────┼──────────┼──────────┤
│ ACME Corp   │ 5 000 €  │ 2 000 €  │          │          │          │
│ Beta SA     │          │          │ 3 500 €  │          │          │
│ Gamma SARL  │          │          │          │          │ 1 200 €  │
└─────────────┴──────────┴──────────┴──────────┴──────────┴──────────┘

→ Identifier les clients en retard de paiement
→ Prioriser les actions de recouvrement
```

### Ventes par article

```
Performance de chaque produit/service :
- Quantité vendue
- CA généré
- Marge
```

---

## Rapports d'achats

### Âge des dettes fournisseurs

```
Même principe que l'âge des créances, côté fournisseurs.
→ Identifier les factures fournisseurs à payer en priorité.
```

### Achats par fournisseur / article

```
Analyse des dépenses par fournisseur ou catégorie.
```

---

## Rapports de taxes

### Déclaration de TVA

```
Rapports → Taxes → Résumé de TVA

Période : Mensuelle ou Trimestrielle

TVA collectée (ventes)           24 000 €
TVA déductible (achats)          -16 000 €
TVA autoliquidée (intracom)       -2 000 €
═══════════════════════════════════════════
TVA nette à payer                  6 000 €
```

### Détail par taux de TVA

```
┌──────────┬────────────┬─────────────┬───────────┐
│ Taux     │ Base HT    │ TVA collectée│ TVA déduite│
├──────────┼────────────┼─────────────┼───────────┤
│ 20%      │ 100 000 €  │ 20 000 €    │ 14 000 €  │
│ 10%      │ 30 000 €   │ 3 000 €     │ 1 500 €   │
│ 5.5%     │ 10 000 €   │ 550 €       │ 275 €     │
│ 0% (exp) │ 15 000 €   │ 0 €         │ 0 €       │
└──────────┴────────────┴─────────────┴───────────┘
```

---

## Tableau de bord

### Dashboard principal

```
Page d'accueil Books → Tableau de bord

Widgets :
- Revenus totaux (graphique)
- Dépenses totales (graphique)
- Flux de trésorerie (graphique barres)
- Factures impayées (montant + nombre)
- Factures en retard (montant + nombre)
- Dépenses par catégorie (camembert)
- Top 5 clients
- Soldes bancaires
```

### Personnalisation

```
Options :
- Choisir la période affichée
- Ajouter/retirer des widgets
- Réorganiser la disposition
- Comparer avec N-1
```

---

## Rapports personnalisés

### Créer un rapport personnalisé

```
Rapports → + Nouveau rapport personnalisé

Étapes :
1. Choisir le module (Factures, Contacts, Articles...)
2. Sélectionner les colonnes
3. Définir les filtres
4. Configurer les groupements
5. Ajouter des formules de calcul
6. Choisir le format (tableau, graphique)
```

### Tags et projets

```
Utiliser les tags pour des rapports par :
- Projet
- Département
- Centre de coût
- Activité

Exemple : CA par projet
  Projet Alpha : 45 000 €
  Projet Beta : 32 000 €
  Projet Gamma : 18 000 €
```

---

## Export et planification

### Export

```
Formats :
- PDF
- Excel (XLS)
- CSV

Options :
- Période sélectionnée
- Tous les détails ou résumé
```

### Planification automatique

```
Rapports → Rapport → Planifier

Configuration :
- Fréquence : quotidienne, hebdomadaire, mensuelle
- Destinataires (emails)
- Format : PDF ou Excel
- Heure d'envoi
```

---

## Bonnes pratiques

1. **Mensuel** : Consulter le P&L et le bilan chaque mois
2. **Hebdomadaire** : Vérifier l'âge des créances et relancer
3. **TVA** : Préparer la déclaration avant la date limite
4. **Trésorerie** : Suivre le flux de trésorerie pour anticiper
5. **Tags** : Taguer les transactions dès la saisie pour des analyses précises
6. **Planification** : Automatiser l'envoi des rapports clés à l'équipe dirigeante

---

*Voir aussi : [configuration.md](configuration.md) | [factures.md](factures.md) | [bancaire.md](bancaire.md)*
