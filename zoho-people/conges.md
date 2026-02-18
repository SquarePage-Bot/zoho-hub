# Zoho People - Congés et Absences

## Types de Congés

### Congés Légaux (France)
| Type | Droits annuels | Calcul |
|------|---------------|--------|
| Congés payés | 25 jours ouvrés | 2.08 jours/mois travaillé |
| RTT | Variable (selon accord) | Forfait ou calcul mensuel |
| Congé maladie | Selon arrêt médical | Justificatif obligatoire |
| Congé maternité | 16 semaines | Légal |
| Congé paternité | 25 jours | Légal depuis 2021 |
| Congé sans solde | Sur demande | Accord employeur |

### Configuration dans Zoho People
```
Type : Congés payés
Code : CP
Droits annuels : 25 jours
Accumulation : Mensuelle (2.08 jours/mois)
Report possible : Oui (max 5 jours)
Date limite de report : 31/05 de l'année suivante
Délai de demande minimum : 14 jours
Approbateur : Manager direct
Approbateur de secours : DRH
```

## Processus de Demande

### Cycle de Vie d'une Demande
```
Employé soumet    →   Manager       →   Statut final
la demande            examine

  ┌─────────┐     ┌──────────┐     ┌──────────────┐
  │ Soumise │────▶│ En revue │────▶│ ✅ Approuvée │
  └─────────┘     └──────┬───┘     └──────────────┘
                         │
                         └────────▶┌──────────────┐
                                   │ ❌ Refusée   │
                                   └──────────────┘
```

### Exemple de Demande
```
Employé : Marie Durand (EMP-001)
Type : Congés payés
Du : 24/02/2026
Au : 28/02/2026
Durée : 5 jours ouvrés
Motif : Vacances
Solde avant : 18 jours
Solde après : 13 jours

Approbateur : Pierre Lambert
Statut : ✅ Approuvé le 10/02/2026
```

## Soldes et Compteurs

### Vue Employé
```
=== Soldes au 18/02/2026 — Marie Durand ===

Congés payés :
  Acquis : 20.83 jours (10 mois × 2.08)
  Pris : 8 jours
  En attente : 5 jours
  Disponible : 7.83 jours

RTT :
  Droits : 12 jours/an
  Pris : 4 jours
  Disponible : 8 jours

Maladie :
  Jours pris : 2 jours (janvier)
```

## Politiques de Congés

### Règles Configurables
```
Politique "CDI France" :
  - Congés payés : 25 jours/an, accumulation mensuelle
  - RTT : 12 jours/an, crédit au 1er janvier
  - Ancienneté : +1 jour après 5 ans, +2 jours après 10 ans
  - Fractionnement : +1 jour si 3-5 jours hors période légale
                     +2 jours si 6+ jours hors période légale

Politique "CDD / Stage" :
  - Congés payés : 2.08 jours/mois
  - RTT : au prorata
  - Pas de report
```

### Jours Fériés
```
Calendrier France 2026 :
  01/01 - Jour de l'an
  06/04 - Lundi de Pâques
  01/05 - Fête du travail
  08/05 - Victoire 1945
  14/05 - Ascension
  25/05 - Lundi de Pentecôte
  14/07 - Fête nationale
  15/08 - Assomption
  01/11 - Toussaint
  11/11 - Armistice
  25/12 - Noël
```

## Workflows d'Approbation

### Approbation Multi-Niveaux
```
Si durée ≤ 3 jours :
  → Manager direct uniquement

Si durée > 3 jours et ≤ 10 jours :
  → Manager direct → puis DRH

Si durée > 10 jours :
  → Manager direct → DRH → Direction

Congé maladie :
  → Approbation automatique (avec justificatif obligatoire sous 48h)
```

## Rapports

| Rapport | Description |
|---------|-------------|
| Soldes par employé | Vue détaillée des compteurs |
| Absences par département | Taux d'absentéisme |
| Planning des congés | Vue calendrier de l'équipe |
| Historique des demandes | Toutes les demandes avec statuts |
| Tendances d'absence | Analyse mensuelle/annuelle |

## Bonnes Pratiques

1. **Configurer les politiques avant le déploiement** — cohérence avec le droit du travail
2. **Automatiser les approbations** pour les cas simples
3. **Forcer le justificatif** pour les congés maladie
4. **Alerter les managers** des conflits de planning (trop d'absents en même temps)
5. **Rappeler les dates limites** de prise de congés
