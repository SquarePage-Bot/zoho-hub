# 👁️ Zoho CRM - Vues et Filtres

> Organiser et retrouver les données avec les vues, filtres et critères.

## Types de vues

### Vues système

| Vue | Description |
|-----|-------------|
| **Tous les enregistrements** | Tous les records du module |
| **Mes enregistrements** | Records dont je suis propriétaire |
| **Créés récemment** | Créés dans les dernières 24h |
| **Modifiés récemment** | Modifiés dans les dernières 24h |
| **Non lus** | Records pas encore consultés |
| **Enregistrements non traités** | Records sans activité |

### Vues personnalisées (Custom Views)

Filtrées par des critères définis par l'utilisateur.

**Création :**
```
Module → Liste → "Créer une vue"
  Nom : "Deals chauds ce mois"
  Critères :
    - Stage NOT IN ("Fermée gagnée", "Fermée perdue")
    - Amount > 10000
    - Closing_Date = "Ce mois"
  Colonnes affichées : Deal_Name, Account_Name, Amount, Stage, Closing_Date, Owner
  Tri : Amount DESC
  Partagée avec : Mon équipe
```

### Vues Canvas

Affichage visuel personnalisé (cartes, tuiles) au lieu du tableau classique. Disponible en Enterprise+.

## Filtres avancés

### Opérateurs par type de champ

#### Texte
```
is / isn't / contains / doesn't contain / starts with / ends with
is empty / is not empty
```

#### Nombre / Devise
```
= / != / < / > / <= / >= / between / not between
is empty / is not empty
```

#### Date
```
is / isn't / is before / is after / between
= today / yesterday / tomorrow
= this week / last week / next week
= this month / last month / next month
= this year / last year
= last 7 days / last 30 days / last 90 days
= next 7 days / next 30 days
is empty / is not empty
```

#### Picklist
```
is / isn't / in / not in / is empty / is not empty
```

#### Lookup
```
is / isn't / is empty / is not empty
```

#### Booléen
```
is true / is false
```

### Combinaison de critères

```
Pattern : (1 AND 2) OR (3 AND 4)

1. Stage = "Négociation"
2. Amount > 50000
3. Stage = "Proposition"
4. Closing_Date = "This week"
```

## Filtres rapides (Quick Filters)

Barre de filtres en haut de la vue liste, permettant le filtrage interactif.

**Configuration :**
- Maximum 5 champs en filtre rapide
- Types supportés : Picklist, Lookup, Date, Propriétaire
- Combinaison automatique en AND

## Recherche

### Recherche globale

- Barre de recherche en haut
- Cherche dans tous les modules
- Indexe : Nom, Email, Téléphone, et champs configurés

### Recherche dans un module

```
Module → Barre de recherche
  - Recherche exacte : "Jean Dupont"
  - Recherche partielle : "Dup"
  - Par champ spécifique : Email = "jean@acme.com"
```

### Recherche via API

```
GET /v6/Deals/search?criteria=(Stage:equals:Negotiation)&per_page=50

GET /v6/Contacts/search?email=jean@acme.com

GET /v6/Leads/search?word=SquarePage
```

## Tags

### Utilisation

Les tags permettent une catégorisation flexible, complémentaire aux champs.

```
Exemples de tags :
- "VIP", "Partenaire", "Prospect chaud"
- "Relance Q1", "Salon 2026"
- "À nettoyer", "Doublon potentiel"
```

### Gestion en Deluge

```deluge
// Ajouter un tag
tagList = List();
tagList.add("VIP");
tagList.add("Priorité haute");
response = zoho.crm.addTags("Deals", dealId, tagList);

// Supprimer un tag
removeList = List();
removeList.add("À relancer");
response = zoho.crm.removeTags("Deals", dealId, removeList);
```

### Filtrer par tag

```
Vue personnalisée :
  Critère : Tag = "VIP"
```

## Territory Management (Gestion des territoires)

Organisation des données par zone géographique ou segment.

```
France
├── Île-de-France
│   ├── Paris
│   └── Banlieue
├── Sud
│   ├── PACA
│   └── Occitanie
└── Nord
    ├── Hauts-de-France
    └── Grand Est
```

**Règles d'attribution automatique par territoire :**
```
Si Country = "France" ET City = "Paris" → Territoire "Paris"
Si Country = "France" ET State = "PACA" → Territoire "PACA"
```

## Exemples de vues utiles

### Pour les commerciaux

| Vue | Critères |
|-----|----------|
| Mes deals à closer ce mois | `Owner = moi AND Stage != Fermée AND Closing_Date = This month` |
| Leads non contactés | `Owner = moi AND Lead_Status = "Non contacté" AND Created_Time = Last 7 days` |
| Tâches en retard | `Owner = moi AND Status != Completed AND Due_Date < Today` |
| Deals bloqués | `Owner = moi AND Modified_Time < Last 14 days AND Stage != Fermée` |

### Pour les managers

| Vue | Critères |
|-----|----------|
| Pipeline total équipe | `Owner IN (mon équipe) AND Stage != Fermée` |
| Deals > 50K€ | `Amount > 50000 AND Stage != Fermée` |
| Leads non traités > 48h | `Lead_Status = "Non contacté" AND Created_Time < 2 days ago` |
| Deals perdus ce trimestre | `Stage = "Fermée perdue" AND Closing_Date = This quarter` |

## Filtres via API (COQL)

Zoho CRM Query Language (COQL) pour des requêtes avancées :

```sql
-- Deals en cours avec montant > 10K, triés par montant décroissant
SELECT Deal_Name, Amount, Stage, Account_Name, Closing_Date
FROM Deals
WHERE Stage NOT IN ('Fermée gagnée', 'Fermée perdue')
AND Amount > 10000
ORDER BY Amount DESC
LIMIT 50
```

```
POST /v6/coql

{
  "select_query": "SELECT Deal_Name, Amount, Stage FROM Deals WHERE Stage = 'Négociation' AND Amount > 10000 ORDER BY Amount DESC LIMIT 50"
}
```

### Fonctions COQL disponibles

```sql
-- Agrégations
SELECT COUNT(id), SUM(Amount), AVG(Amount), MAX(Amount), MIN(Amount)
FROM Deals
WHERE Stage = 'Fermée gagnée'

-- Filtres de date
SELECT Deal_Name FROM Deals
WHERE Created_Time BETWEEN '2026-01-01T00:00:00+01:00' AND '2026-03-31T23:59:59+01:00'

-- LIKE
SELECT Last_Name, Email FROM Contacts
WHERE Email LIKE '%@acme.com'

-- IN
SELECT * FROM Leads
WHERE Lead_Source IN ('Site Web', 'LinkedIn', 'Référence')
```

## Bonnes pratiques

1. **Vues partagées** : Créer des vues standards pour l'équipe
2. **Colonnes** : Limiter à 6-8 colonnes pour la lisibilité
3. **Tri par défaut** : Choisir un tri pertinent (date, montant, priorité)
4. **Tags** : Convention de nommage cohérente
5. **Nettoyage** : Supprimer les vues inutilisées régulièrement
6. **COQL** : Privilégier COQL pour les requêtes complexes via API

---
*Voir aussi : [modules.md](modules.md) pour les modules filtrables, [api.md](api.md) pour les requêtes API.*
