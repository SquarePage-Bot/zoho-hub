# Rapports, Formules et KPI

## Créer un rapport

### Étapes

```
Workspace → + Nouveau → Rapport

1. Type de rapport : Graphique / Tableau / KPI / Tableau croisé
2. Table(s) source(s) : Sélectionner une ou plusieurs tables
3. Axes / Colonnes :
   - Axe X (dimensions) : Champs de regroupement
   - Axe Y (mesures) : Champs numériques avec agrégation
4. Filtres : Conditions de filtrage
5. Personnalisation : Couleurs, titres, légendes
```

## Types de rapports

### Graphique à barres

```
Nom : "CA mensuel par produit"
Type : Barres empilées
Table : Ventes

Axe X : Mois(Date_vente)
Axe Y : SUM(Montant)
Couleur : Produit

Résultat :
        Jan     Fév     Mars    Avril
SaaS    ████    █████   ██████  ███████
Consult ██      ███     ██      ████
Format  █       █       ██      █
```

### Graphique en lignes

```
Nom : "Tendance du nombre de leads"
Type : Lignes multi-séries
Table : Leads

Axe X : Semaine(Date_creation)
Axe Y : COUNT(ID)
Séries : Source (Google Ads, SEO, Social, Email)
```

### Tableau croisé dynamique (Pivot)

```
Nom : "Ventes croisées Produit × Région"
Type : Tableau croisé

Lignes : Produit
Colonnes : Région
Valeurs : SUM(Montant)
Sous-totaux : Oui

Résultat :
┌──────────────┬──────────┬──────────┬──────────┬──────────┐
│ Produit      │ IDF      │ AURA     │ PACA     │ Total    │
├──────────────┼──────────┼──────────┼──────────┼──────────┤
│ SaaS Pro     │ 45 200€  │ 22 100€  │ 15 800€  │ 83 100€  │
│ SaaS Basic   │ 28 400€  │ 18 500€  │ 12 200€  │ 59 100€  │
│ Consulting   │ 18 900€  │ 8 200€   │ 5 400€   │ 32 500€  │
│ Formation    │ 12 100€  │ 6 800€   │ 4 300€   │ 23 200€  │
├──────────────┼──────────┼──────────┼──────────┼──────────┤
│ Total        │ 104 600€ │ 55 600€  │ 37 700€  │ 197 900€ │
└──────────────┴──────────┴──────────┴──────────┴──────────┘
```

### Widget KPI

```
Nom : "Chiffre d'affaires mensuel"
Type : KPI unique

Valeur : SUM("Montant") WHERE Mois = Mois courant
Comparaison : SUM("Montant") WHERE Mois = Mois précédent
Format : Devise (€)
Indicateur : ▲ vert si hausse, ▼ rouge si baisse

Affichage :
┌─────────────────────┐
│  💰 CA du mois      │
│  245 800 €          │
│  ▲ +8.2% vs M-1    │
└─────────────────────┘
```

### Carte géographique

```
Nom : "CA par département"
Type : Carte de France

Dimension : Département
Mesure : SUM(Montant)
Couleur : Dégradé (blanc → bleu foncé)

Interactions :
- Survol : Affiche le montant
- Clic : Drill-down vers les villes du département
```

### Entonnoir

```
Nom : "Funnel de conversion"
Type : Entonnoir

Étapes (manuelles ou calculées) :
1. Visiteurs : 50 000
2. Leads : 2 500 (5.0%)
3. MQL : 800 (32.0%)
4. SQL : 320 (40.0%)
5. Opportunités : 150 (46.9%)
6. Clients : 45 (30.0%)

Taux de conversion global : 0.09%
```

## Formules

### Types de formules

| Type | Contexte | Exemple |
|------|----------|---------|
| **Colonne de formule** | Ajoutée à la table, calculée par ligne | Marge = Montant - Coût |
| **Formule agrégée** | Dans les rapports, sur un groupe | SUM, AVG, COUNT |
| **Formule de rapport** | Calcul entre mesures agrégées | Taux = SUM(Gagnés)/COUNT(Total) |

### Fonctions disponibles

#### Mathématiques
```
abs(x)          → Valeur absolue
ceil(x)         → Arrondi supérieur
floor(x)        → Arrondi inférieur
round(x, n)     → Arrondi à n décimales
power(x, n)     → Puissance
sqrt(x)         → Racine carrée
mod(x, y)       → Modulo
log(x)          → Logarithme
```

#### Texte
```
concat(a, b)    → Concaténation
upper(x)        → Majuscules
lower(x)        → Minuscules
trim(x)         → Supprimer espaces
left(x, n)      → N premiers caractères
right(x, n)     → N derniers caractères
length(x)       → Longueur
contains(x, y)  → Contient (booléen)
replace(x,a,b)  → Remplacer
```

#### Dates
```
year(d)             → Année
month(d)            → Mois (1-12)
day(d)              → Jour (1-31)
quarter(d)          → Trimestre (1-4)
weekday(d)          → Jour de semaine
datediff(d1, d2)    → Différence en jours
dateadd(d, n, unit) → Ajouter n unités
now()               → Date et heure actuelles
today()             → Date du jour
```

#### Logique
```
if(condition, valeur_vrai, valeur_faux)
ifnull(x, valeur_defaut)
case when condition1 then val1 when condition2 then val2 else val3 end
```

#### Agrégation
```
sum(x)          → Somme
avg(x)          → Moyenne
count(x)        → Nombre
countif(x, cond)→ Nombre conditionnel
min(x)          → Minimum
max(x)          → Maximum
distinctcount(x)→ Nombre de valeurs uniques
```

### Exemples de formules complexes

#### Marge bénéficiaire
```
Colonne : Marge_pct
Formule : round(("Montant_HT" - "Cout") / "Montant_HT" * 100, 2)
```

#### Catégorie de client (RFM)
```
Colonne : Segment_client
Formule :
  if(datediff("Derniere_commande", today()) < 30 
     AND "CA_total" > 10000, "VIP",
  if(datediff("Derniere_commande", today()) < 90, "Actif",
  if(datediff("Derniere_commande", today()) < 180, "À risque",
     "Perdu")))
```

#### Évolution en pourcentage (dans un rapport)
```
Formule de rapport :
(SUM("Montant" pour "Mois courant") - SUM("Montant" pour "Mois précédent"))
/ SUM("Montant" pour "Mois précédent") * 100
```

#### Score de santé client
```
Colonne : Score_sante
Formule :
  (if("NPS" >= 9, 40, if("NPS" >= 7, 25, 10)))
  + (if("Tickets_ouverts" = 0, 30, if("Tickets_ouverts" <= 2, 15, 0)))
  + (if(datediff("Derniere_connexion", today()) < 7, 30, 
       if(datediff("Derniere_connexion", today()) < 30, 15, 0)))
```

## KPI et indicateurs

### KPI commerciaux courants

| KPI | Formule | Cible type |
|-----|---------|-----------|
| **Chiffre d'affaires** | SUM(Montant) | Budget annuel |
| **MRR** (Monthly Recurring Revenue) | SUM(Montant_mensuel) où Type = Abonnement | Croissance +5%/mois |
| **ARR** (Annual Recurring Revenue) | MRR × 12 | - |
| **Panier moyen** | AVG(Montant) | > 500€ |
| **Taux de conversion** | COUNT(Deals gagnés) / COUNT(Deals) × 100 | > 25% |
| **Cycle de vente** | AVG(datediff(Date_creation, Date_cloture)) | < 30 jours |
| **CAC** (Coût d'acquisition) | Dépenses_marketing / Nb_nouveaux_clients | < LTV/3 |
| **LTV** (Lifetime Value) | Panier_moyen × Fréquence × Durée_vie | > 3× CAC |
| **Churn rate** | Clients_perdus / Clients_début_période × 100 | < 5%/mois |
| **NPS** (Net Promoter Score) | % Promoteurs - % Détracteurs | > 50 |

### KPI support

| KPI | Formule | Cible |
|-----|---------|-------|
| **Temps moyen de résolution** | AVG(datediff(Date_creation, Date_resolution)) | < 4h |
| **Taux de résolution 1er contact** | COUNT(Résolu_premier_contact) / COUNT(Total) × 100 | > 70% |
| **CSAT** | Nb_satisfaits / Nb_réponses × 100 | > 85% |
| **SLA respecté** | COUNT(Dans_SLA) / COUNT(Total) × 100 | > 95% |

## Filtres de rapport

### Types de filtres

```
Filtres statiques (au moment de la création) :
- Région = "Île-de-France"
- Date >= "01/01/2026"
- Montant > 1000

Filtres dynamiques (modifiables par l'utilisateur) :
- Période : Liste déroulante (Ce mois, Ce trimestre, Cette année)
- Commercial : Multi-sélection
- Produit : Recherche
```

### Filtres de date relatifs

```
Filtres temporels prédéfinis :
- Aujourd'hui
- Hier
- Cette semaine / Semaine dernière
- Ce mois / Mois dernier
- Ce trimestre / Trimestre dernier
- Cette année / Année dernière
- 7 derniers jours / 30 derniers jours / 90 derniers jours
- Personnalisé (du JJ/MM/AAAA au JJ/MM/AAAA)
```

## Export et planification

### Formats d'export

```
Rapport → Exporter

Formats :
- PDF (avec mise en page)
- Excel (.xlsx)
- CSV
- Image (PNG, JPG)
- HTML
```

### Rapports programmés

```
Rapport → Planifier → + Nouvelle planification

Fréquence : Quotidien à 8h / Hebdo le lundi / Mensuel le 1er
Format : PDF
Destinataires : direction@entreprise.fr, commercial@entreprise.fr
Objet : "Rapport CA hebdomadaire - Semaine {week_number}"
Message : "Veuillez trouver ci-joint le rapport de la semaine."
```

## Bonnes pratiques

1. **Nommer clairement** : "CA mensuel par produit 2026" plutôt que "Rapport 1"
2. **Choisir le bon type** : Barres pour comparer, lignes pour les tendances
3. **Limiter les dimensions** : Max 2-3 dimensions par graphique
4. **Ajouter du contexte** : Comparaison avec période précédente ou objectif
5. **Formater les nombres** : Devise, pourcentage, séparateurs de milliers
6. **Trier intelligemment** : Du plus grand au plus petit par défaut
7. **Légende claire** : Titres d'axes et légendes explicites
8. **Tester les formules** : Vérifier sur un échantillon avant de déployer
