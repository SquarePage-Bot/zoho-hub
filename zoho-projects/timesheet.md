# Feuilles de Temps (Timesheets)

## Présentation

Les feuilles de temps dans Zoho Projects permettent de suivre le temps passé sur chaque tâche. Elles sont essentielles pour la facturation, l'analyse de rentabilité et l'amélioration des estimations.

## Accéder aux feuilles de temps

```
Projet → Menu latéral → Feuilles de temps
ou
Tâche → Onglet "Temps"
```

## Méthodes de saisie du temps

### 1. Saisie manuelle

```
Feuilles de temps → + Enregistrer le temps

Champs :
- Tâche : Sélectionner la tâche concernée
- Date : Date du travail effectué
- Heures : Durée (ex : 3h30)
- Facturable : Oui / Non
- Note : Description du travail effectué
```

**Exemple :**
```
Tâche : Développer l'API d'authentification
Date : 05/03/2026
Heures : 4h15
Facturable : Oui
Note : Implémentation JWT + refresh tokens, tests unitaires
```

### 2. Chronomètre (Timer)

```
Depuis n'importe quelle tâche → Bouton ▶️ (Démarrer le chronomètre)

Le chronomètre fonctionne en temps réel :
▶️ Démarrer → ⏸️ Pause → ⏹️ Arrêter

À l'arrêt, une entrée de temps est automatiquement créée.
Un seul chronomètre actif à la fois.
```

### 3. Saisie hebdomadaire

```
Feuilles de temps → Vue hebdomadaire

┌──────────────────┬──────┬──────┬──────┬──────┬──────┬───────┐
│ Tâche            │ Lun  │ Mar  │ Mer  │ Jeu  │ Ven  │ Total │
├──────────────────┼──────┼──────┼──────┼──────┼──────┼───────┤
│ Dev API Auth     │ 4h   │ 6h   │ 2h   │      │      │ 12h   │
│ Design UI Login  │      │ 2h   │ 4h   │ 6h   │ 3h   │ 15h   │
│ Réunion Sprint   │ 1h   │      │      │      │ 1h   │ 2h    │
├──────────────────┼──────┼──────┼──────┼──────┼──────┼───────┤
│ Total jour       │ 5h   │ 8h   │ 6h   │ 6h   │ 4h   │ 29h   │
└──────────────────┴──────┴──────┴──────┴──────┴──────┴───────┘
```

## Facturation

### Configurer les taux horaires

```
Paramètres du projet → Budget → Taux horaires

Niveaux de configuration :
1. Taux par défaut du projet : 50€/h
2. Taux par utilisateur :
   - Marie (Senior) : 75€/h
   - Paul (Junior) : 40€/h
3. Taux par tâche (prioritaire) : Override possible
```

### Heures facturables vs non facturables

| Type | Exemples | Impact budget |
|------|----------|---------------|
| **Facturable** | Développement, design, tests | Comptabilisé dans la facturation |
| **Non facturable** | Réunions internes, formation, admin | Non facturé au client |

### Exemple de calcul de facturation

```
Projet : Application Mobile (Taux : 60€/h)

Semaine du 02/03/2026 :
Marie : 32h facturables × 75€ = 2 400€
Paul  : 35h facturables × 40€ = 1 400€
Sophie: 28h facturables × 60€ = 1 680€
                                ────────
Total facturable semaine :       5 480€
```

## Approbation des feuilles de temps

### Workflow d'approbation

```
Saisie par le membre → Soumission → Revue par le manager → Approbation/Rejet

Statuts :
- 📝 Brouillon : Non soumis, modifiable
- 📤 Soumis : En attente d'approbation
- ✅ Approuvé : Validé, non modifiable
- ❌ Rejeté : Retourné avec commentaire, à corriger
```

### Configurer l'approbation

```
Paramètres du projet → Feuilles de temps → Approbation

Options :
☑ Activer l'approbation des feuilles de temps
Approbateur : Manager du projet / Utilisateur spécifique
Fréquence de soumission : Hebdomadaire / Quotidienne / Libre
Rappel de soumission : Vendredi 17h
```

## Budget et suivi financier

### Configurer le budget du projet

```
Paramètres du projet → Budget

Type de budget :
1. Basé sur les heures : Budget = 500h
2. Basé sur le coût : Budget = 30 000€
3. Les deux : 500h ET 30 000€

Alertes :
- ⚠️ À 75% du budget : Notification au manager
- 🔴 À 90% du budget : Notification au manager + admin
- 🛑 À 100% : Alerte critique
```

### Tableau de suivi budgétaire

```
Projet : Plateforme E-commerce
Budget : 600h / 36 000€

┌───────────────────┬──────────┬──────────┬──────────┬──────────┐
│ Phase             │ Estimé   │ Réel     │ Reste    │ Statut   │
├───────────────────┼──────────┼──────────┼──────────┼──────────┤
│ Spécifications    │ 80h      │ 72h      │ +8h      │ ✅ Sous  │
│ Design            │ 120h     │ 135h     │ -15h     │ ⚠️ Dépassé│
│ Développement     │ 300h     │ 210h     │ 90h      │ 🔄 En cours│
│ Tests             │ 100h     │ 0h       │ 100h     │ ⏳ À venir│
├───────────────────┼──────────┼──────────┼──────────┼──────────┤
│ Total             │ 600h     │ 417h     │ 183h     │ 69.5%    │
└───────────────────┴──────────┴──────────┴──────────┴──────────┘
```

## Rapports de temps

### Rapports disponibles

| Rapport | Description |
|---------|-------------|
| **Par utilisateur** | Heures par membre de l'équipe |
| **Par tâche** | Temps passé par tâche |
| **Par projet** | Vue consolidée multi-projets |
| **Par date** | Répartition quotidienne/hebdomadaire |
| **Facturation** | Heures facturables et montants |
| **Estimé vs réel** | Comparaison estimation/temps passé |

### Exemple de rapport estimé vs réel

```
┌─────────────────────────┬──────────┬──────────┬──────────┐
│ Tâche                   │ Estimé   │ Réel     │ Écart    │
├─────────────────────────┼──────────┼──────────┼──────────┤
│ Design page d'accueil   │ 16h      │ 22h      │ +37.5%   │
│ Développement API       │ 40h      │ 35h      │ -12.5%   │
│ Tests d'intégration     │ 24h      │ 30h      │ +25.0%   │
│ Documentation           │ 8h       │ 6h       │ -25.0%   │
├─────────────────────────┼──────────┼──────────┼──────────┤
│ Total                   │ 88h      │ 93h      │ +5.7%    │
└─────────────────────────┴──────────┴──────────┴──────────┘
```

## Intégration avec Zoho Books / Invoice

### Facturer les heures

```
Feuilles de temps → Sélectionner les entrées approuvées
→ Actions → Créer une facture

La facture est automatiquement créée dans Zoho Books / Invoice avec :
- Détail des heures par tâche
- Taux horaire appliqué
- Montant total
- Période couverte
```

### Synchronisation automatique

```
Paramètres → Intégrations → Zoho Books

Options :
☑ Synchroniser les heures approuvées automatiquement
☑ Inclure les notes dans la facture
Fréquence : Hebdomadaire / Mensuelle / Manuelle
```

## Bonnes pratiques

1. **Saisir quotidiennement** : Ne pas attendre la fin de semaine pour saisir ses heures
2. **Utiliser le chronomètre** : Plus précis que la saisie manuelle rétrospective
3. **Ajouter des notes** : Décrire le travail effectué pour la traçabilité
4. **Distinguer facturable/non facturable** : Important pour la rentabilité
5. **Estimer avant de commencer** : Comparer ensuite avec le temps réel
6. **Soumettre à temps** : Respecter le cycle d'approbation hebdomadaire
7. **Revoir les rapports** : Analyser les écarts pour améliorer les futures estimations
