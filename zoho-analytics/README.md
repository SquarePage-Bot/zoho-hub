# Zoho Analytics

## Présentation

Zoho Analytics est une plateforme de Business Intelligence (BI) et d'analyse de données qui permet de créer des rapports, tableaux de bord et visualisations interactives à partir de multiples sources de données.

## Fonctionnalités principales

- **Sources de données** : Import fichiers, bases de données, applications cloud, API
- **Préparation des données** : Nettoyage, transformation, fusion de tables
- **Rapports** : Graphiques, tableaux croisés, KPI, formules avancées
- **Tableaux de bord** : Dashboards interactifs avec filtres dynamiques
- **Partage** : Collaboration, export, intégration dans des applications
- **IA (Zia)** : Analyse prédictive, questions en langage naturel
- **Alertes** : Notifications basées sur des seuils

## Concepts clés

### Workspace (Espace de travail)
Un workspace regroupe toutes les données, rapports et tableaux de bord liés à un même domaine.

### Table
Une table contient des données structurées en lignes et colonnes, similaire à une feuille de calcul ou une table de base de données.

### Rapport
Un rapport est une visualisation des données (graphique, tableau, KPI) basée sur une ou plusieurs tables.

### Tableau de bord (Dashboard)
Un dashboard combine plusieurs rapports sur une seule page interactive.

## Architecture des données

```
Workspace : "Analyse Commerciale"
│
├── Tables
│   ├── Ventes (sync Zoho CRM)
│   ├── Produits (import CSV)
│   ├── Clients (sync Zoho CRM)
│   ├── Objectifs (saisie manuelle)
│   └── Dépenses (sync Zoho Books)
│
├── Relations (Lookup)
│   ├── Ventes.client_id → Clients.id
│   ├── Ventes.produit_id → Produits.id
│   └── Dépenses.categorie → Produits.categorie
│
├── Rapports
│   ├── CA mensuel (graphique barres)
│   ├── Top 10 clients (tableau)
│   ├── Marge par produit (graphique camembert)
│   └── Tendances (graphique lignes)
│
└── Tableaux de bord
    ├── Dashboard Direction
    ├── Dashboard Commercial
    └── Dashboard Finance
```

## Types de visualisations

| Type | Usage | Exemple |
|------|-------|---------|
| **Barres** | Comparaisons | CA par région |
| **Lignes** | Tendances temporelles | Évolution mensuelle |
| **Camembert** | Proportions | Répartition par catégorie |
| **Aire** | Tendances cumulées | CA cumulé par mois |
| **Scatter** | Corrélations | Prix vs volume |
| **Entonnoir** | Pipeline | Funnel de vente |
| **KPI** | Indicateur unique | CA total, NPS |
| **Tableau croisé** | Données détaillées | Ventes par produit × mois |
| **Carte** | Données géographiques | CA par département |
| **Jauge** | Progression | Objectif vs réalisé |
| **Heatmap** | Intensité | Activité par jour/heure |
| **Combiné** | Multiple | Barres + ligne de tendance |

## Navigation dans la documentation

| Fichier | Contenu |
|---------|---------|
| [sources-donnees.md](sources-donnees.md) | Import, synchronisation et connecteurs |
| [tableaux-bord.md](tableaux-bord.md) | Création de dashboards interactifs |
| [rapports.md](rapports.md) | Types de rapports, formules et KPI |
| [partage.md](partage.md) | Partage, collaboration et export |
| [api.md](api.md) | API REST Zoho Analytics |

## Tarification

| Plan | Lignes | Utilisateurs | Fonctionnalités clés |
|------|--------|--------------|----------------------|
| **Gratuit** | 10 000 | 2 | Rapports basiques, 1 workspace |
| **Basic** | 500 000 | 2 | Connecteurs, alertes |
| **Standard** | 1 M | 5 | Formules avancées, Zia |
| **Premium** | 5 M | 15 | Slides, audit, intégrations avancées |
| **Enterprise** | 50 M | 50 | White-label, API illimitée |

## Intégrations natives

### Applications Zoho
- **Zoho CRM** : Leads, contacts, deals, activités
- **Zoho Books** : Factures, paiements, dépenses
- **Zoho Desk** : Tickets, SLA, satisfaction client
- **Zoho Projects** : Tâches, temps, jalons
- **Zoho Campaigns** : Campagnes, ouvertures, clics
- **Zoho People** : RH, présences, congés

### Applications tierces
- **Google Analytics** : Trafic web, conversions
- **Salesforce** : CRM
- **HubSpot** : Marketing et ventes
- **Shopify** : E-commerce
- **Stripe** : Paiements
- **Google Ads / Facebook Ads** : Publicité
- **MySQL / PostgreSQL / SQL Server** : Bases de données

## Zia - Intelligence artificielle

### Fonctionnalités IA

| Fonction | Description |
|----------|-------------|
| **Ask Zia** | Poser des questions en langage naturel |
| **Prédictions** | Prévisions basées sur les données historiques |
| **Anomalies** | Détection automatique des valeurs anormales |
| **Narratifs** | Résumé textuel automatique des données |
| **Suggestions** | Recommandations de rapports pertinents |

### Exemple Ask Zia

```
Question : "Quel est le CA du mois dernier par région ?"
→ Zia génère automatiquement un graphique à barres
   avec le CA par région pour le mois précédent

Question : "Comparer les ventes Q1 2025 vs Q1 2026"
→ Zia crée un graphique comparatif avec variation en %
```
