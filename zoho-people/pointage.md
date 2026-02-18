# Zoho People - Pointage et Présence

## Méthodes de Pointage

### 1. Pointage Web
```
Connexion au portail Zoho People :
  → Clic sur "Pointer" (Check-in / Check-out)
  → Enregistrement automatique : heure, IP, navigateur
```

### 2. Application Mobile
```
Zoho People (iOS / Android) :
  → Pointage avec géolocalisation
  → Photo selfie optionnelle
  → Mode hors-ligne (sync ultérieure)
  → Géofencing : pointage uniquement dans un périmètre défini
```

### 3. Bornes / Matériel
```
Intégrations biométriques :
  - Lecteur d'empreintes digitales
  - Badge RFID / NFC
  - Reconnaissance faciale

Marques compatibles :
  - ZKTeco
  - Hikvision
  - Suprema
  
Sync automatique vers Zoho People via API
```

### 4. Kiosque
```
Tablette/PC partagé à l'accueil :
  - Employé entre son ID ou badge
  - Pointage rapide sans connexion individuelle
  - Idéal pour les ateliers / chantiers
```

## Configuration de la Présence

### Horaires de Travail
```
Shift "Bureau Standard" :
  Lundi-Vendredi : 09:00 - 18:00
  Pause déjeuner : 12:30 - 13:30
  Heures par jour : 8h
  Heures par semaine : 40h

Shift "Support" :
  Rotation : Matin (6h-14h) / Après-midi (14h-22h) / Nuit (22h-6h)
  Cycle : 3 jours travaillés, 1 jour repos
```

### Règles de Pointage
```
Tolérance de retard : 15 minutes
  → Arrivée avant 09:15 = Présent
  → Arrivée entre 09:15 et 09:30 = Retard mineur
  → Arrivée après 09:30 = Retard

Heures supplémentaires :
  → Comptabilisées après 8h/jour
  → Autorisation préalable du manager requise
  → Majoration : +25% (heures 1-8), +50% (au-delà)

Départ anticipé :
  → Avant 17:30 = Départ anticipé (sauf autorisation)
```

## Feuilles de Temps (Timesheets)

### Saisie des Temps
```
Employé : Marie Durand
Semaine : 17/02/2026 - 21/02/2026

| Jour    | Arrivée | Départ | Pause | Heures | Projet          |
|---------|---------|--------|-------|--------|-----------------|
| Lundi   | 08:55   | 18:10  | 1h00  | 8h15   | Campagne Q1     |
| Mardi   | 09:02   | 18:30  | 1h00  | 8h28   | Campagne Q1     |
| Mercredi| 08:45   | 17:00  | 0h45  | 7h30   | Formation       |
| Jeudi   | 09:10   | 19:00  | 1h00  | 8h50   | Refonte site    |
| Vendredi| 09:00   | 17:30  | 1h00  | 7h30   | Campagne Q1     |

Total semaine : 40h33 (dont 0h33 heures sup.)
```

### Approbation des Feuilles de Temps
```
Cycle mensuel :
  1. Employé saisit/vérifie ses temps (avant le 2 du mois suivant)
  2. Manager valide (avant le 5)
  3. RH contrôle et finalise (avant le 8)
  4. Export vers la paie
```

## Régularisations

### Demande de Régularisation
```
Employé : Marie Durand
Date : 19/02/2026
Type : Oubli de pointage
Heure réelle d'arrivée : 08:50
Motif : Badge oublié à la maison

Approuvé par : Pierre Lambert
```

## Rapports de Présence

| Rapport | Description |
|---------|-------------|
| Présence quotidienne | Qui est présent/absent aujourd'hui |
| Rapport mensuel | Heures travaillées par employé sur le mois |
| Retards et départs anticipés | Liste des anomalies |
| Heures supplémentaires | Détail des heures sup. par employé |
| Taux de présence | Pourcentage par département |
| Géolocalisation | Carte des pointages mobiles |

## Intégration avec la Paie

```
Export mensuel :
  - Jours travaillés
  - Heures normales
  - Heures supplémentaires (25%, 50%)
  - Jours d'absence (type)
  - Retards (nombre et durée)

Formats d'export : CSV, XLS, API directe vers logiciel de paie
```

## Bonnes Pratiques

1. **Choisir la méthode de pointage adaptée** au contexte (bureau vs terrain)
2. **Configurer le géofencing** pour le pointage mobile
3. **Automatiser le calcul des heures sup.** selon la législation
4. **Permettre les régularisations** avec approbation manager
5. **Archiver les données** conformément au droit du travail (5 ans)
