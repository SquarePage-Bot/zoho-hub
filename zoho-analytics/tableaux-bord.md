# Tableaux de Bord (Dashboards)

## Présentation

Les tableaux de bord de Zoho Analytics combinent plusieurs rapports sur une seule page interactive, offrant une vue d'ensemble de vos indicateurs clés.

## Créer un tableau de bord

```
Workspace → + Nouveau → Tableau de bord

1. Nom : "Dashboard Direction Générale"
2. Layout : Grille libre (drag & drop)
3. Ajouter des composants (rapports existants ou nouveaux)
```

## Interface du dashboard

```
┌──────────────────────────────────────────────────────────────────────┐
│  Dashboard Direction Générale          [Filtres] [Partager] [Export] │
├──────────────────────────────────────────────────────────────────────┤
│  Période : [Ce mois ▼]  Région : [Toutes ▼]  Produit : [Tous ▼]   │
├─────────────────────┬─────────────────────┬──────────────────────────┤
│  💰 CA Total        │  📈 Croissance      │  👥 Nouveaux clients     │
│  245 800 €          │  +12.3%             │  87                      │
│  ▲ +8% vs M-1      │  vs +9.1% M-1       │  ▲ +15 vs M-1           │
├─────────────────────┴─────────────────────┴──────────────────────────┤
│                                                                      │
│  📊 CA par mois (barres + ligne de tendance)                        │
│  ████                                                                │
│  ████ ████                                                           │
│  ████ ████ ████                                                      │
│  ████ ████ ████ ████                                                 │
│  Jan  Fév  Mars Avr                                                  │
│                                                                      │
├──────────────────────────────┬───────────────────────────────────────┤
│  🥧 Répartition par produit │  🗺️ CA par région (carte)             │
│                              │                                       │
│  ◉ SaaS Pro     45%        │  IDF ████████ 42%                     │
│  ◉ SaaS Basic   30%        │  AURA ██████ 22%                      │
│  ◉ Consulting   15%        │  PACA ████ 15%                        │
│  ◉ Formation    10%        │  Autres ███ 21%                       │
│                              │                                       │
├──────────────────────────────┴───────────────────────────────────────┤
│  📋 Top 10 clients par CA                                           │
│  ┌──────┬──────────────┬──────────┬──────────┬──────────┐           │
│  │ Rang │ Client       │ CA       │ Commandes│ Tendance │           │
│  ├──────┼──────────────┼──────────┼──────────┼──────────┤           │
│  │ 1    │ TechCorp     │ 45 200€  │ 12       │ ▲ +15%  │           │
│  │ 2    │ DataFlow     │ 32 100€  │ 8        │ ▼ -3%   │           │
│  │ 3    │ DesignLab    │ 28 500€  │ 15       │ ▲ +22%  │           │
│  └──────┴──────────────┴──────────┴──────────┴──────────┘           │
└──────────────────────────────────────────────────────────────────────┘
```

## Composants de dashboard

### Widgets KPI

```
Ajouter → Widget KPI

Configuration :
- Table source : Ventes
- Valeur : SUM("Montant")
- Comparaison : Mois précédent
- Format : Devise (€)
- Couleur : Vert si positif, Rouge si négatif
- Icône : 💰

Exemples de KPI :
- CA total : SUM(Montant) → 245 800€
- Nombre de commandes : COUNT(ID) → 342
- Panier moyen : AVG(Montant) → 718€
- Taux de conversion : COUNT(Deals gagnés) / COUNT(Deals) × 100 → 28%
```

### Graphiques

```
Ajouter → Rapport existant / Nouveau rapport

Types de graphiques disponibles :
- Barres (verticales / horizontales / empilées)
- Lignes (simple / multi-séries / aire)
- Camembert / Donut
- Scatter / Bulles
- Carte géographique
- Jauge / Cadran
- Entonnoir
- Heatmap
- Combiné (barres + lignes)
- Tableau croisé dynamique
```

### Filtres globaux

Les filtres du dashboard s'appliquent à tous les rapports simultanément.

```
Configurer les filtres :
Dashboard → Paramètres → Filtres globaux

Filtres courants :
- Période : Ce mois / Ce trimestre / Cette année / Personnalisé
- Région : Multi-sélection
- Produit : Liste déroulante
- Commercial : Multi-sélection
- Segment client : B2B / B2C / Tous

Les rapports du dashboard se mettent à jour en temps réel
quand l'utilisateur change un filtre.
```

## Exemples de dashboards

### Dashboard Commercial

```
Composants :
1. KPI : CA du mois, Nombre de deals, Taux de conversion, Panier moyen
2. Graphique barres : CA par commercial
3. Entonnoir : Pipeline des ventes (Prospect → Qualifié → Proposition → Négociation → Gagné)
4. Graphique lignes : Évolution CA mensuel (12 derniers mois)
5. Carte : CA par département
6. Tableau : Deals en cours (montant, probabilité, date de clôture prévue)
7. Camembert : Répartition CA par source (site web, salon, recommandation)

Filtres : Période, Commercial, Produit
```

### Dashboard Support Client

```
Composants :
1. KPI : Tickets ouverts, Temps moyen résolution, Satisfaction (CSAT), SLA respecté
2. Graphique lignes : Volume de tickets par jour
3. Barres empilées : Tickets par catégorie et statut
4. Heatmap : Volume par jour de semaine × heure
5. Tableau : Tickets les plus anciens non résolus
6. Jauge : SLA (% de tickets résolus dans les délais)
7. Camembert : Répartition par canal (email, chat, téléphone)

Filtres : Période, Agent, Priorité, Catégorie
```

### Dashboard Marketing

```
Composants :
1. KPI : Leads générés, Coût par lead, Taux de conversion lead→client, ROI
2. Graphique barres : Leads par source (Google Ads, SEO, Réseaux sociaux, Email)
3. Entonnoir : Funnel marketing (Visiteur → Lead → MQL → SQL → Client)
4. Graphique lignes : Trafic web vs leads (corrélation)
5. Tableau : Performance des campagnes email (ouvertures, clics, conversions)
6. Comparatif : Budget vs dépenses réelles par canal
7. Géographique : Leads par pays/région

Filtres : Période, Canal, Campagne
```

## Personnalisation

### Thèmes et styles

```
Dashboard → Paramètres → Apparence

Options :
- Thème : Clair / Sombre / Personnalisé
- Couleurs : Palette personnalisée
- Police : Inter, Roboto, Open Sans...
- Logo : Ajouter le logo de l'entreprise
- Fond : Couleur unie / Dégradé / Image
```

### Layout et grille

```
Disposition :
- Grille libre : Placer les composants n'importe où
- Grille fixe : Alignement automatique sur une grille
- Responsive : Adaptation automatique à l'écran

Tailles de composants :
- Petit : 1/4 de la largeur (KPI)
- Moyen : 1/2 de la largeur (graphique)
- Grand : Pleine largeur (tableau, graphique détaillé)

Redimensionner : Glisser les bords du composant
Déplacer : Glisser-déposer le composant
```

## Interactivité

### Drill-down (Exploration)

```
Cliquer sur une barre "Paris" dans le graphique CA par ville
→ Drill-down automatique : Détail par client à Paris
→ Cliquer sur "TechCorp"
→ Drill-down : Détail des commandes de TechCorp à Paris
```

### Filtrage croisé

```
Cliquer sur un élément dans un graphique
→ Tous les autres graphiques du dashboard se filtrent

Exemple :
Cliquer sur "SaaS Pro" dans le camembert
→ Le graphique CA par mois affiche seulement SaaS Pro
→ Le tableau Top clients affiche seulement clients SaaS Pro
→ La carte montre la répartition géo de SaaS Pro
```

### Alertes et seuils

```
Dashboard → Rapport → Paramètres → Alerte

Condition : SI CA mensuel < 200 000€
Action : Envoyer email au directeur commercial
Fréquence : Vérification quotidienne

Condition : SI taux de satisfaction < 80%
Action : Notification in-app au responsable support
```

## Actualisation des données

```
Options d'actualisation :
- Manuelle : Bouton "Actualiser" sur le dashboard
- Automatique : Selon la fréquence de sync des sources
- Programmée : Actualiser toutes les heures / jours

Indicateur de fraîcheur :
"Dernière mise à jour : il y a 15 minutes"
```

## Présentation (Slideshow)

```
Dashboard → Présentation

Créer un diaporama de dashboards :
1. Sélectionner les dashboards à inclure
2. Définir la durée par slide (30s, 1min, 2min)
3. Activer le défilement automatique
4. Mode plein écran

Usage : Affichage sur écran TV dans le bureau
URL de présentation : https://analytics.zoho.eu/slideshow/xxxxx
```

## Bonnes pratiques

1. **Un dashboard = un objectif** : Ne pas mélanger commercial et support
2. **KPI en haut** : Les indicateurs clés immédiatement visibles
3. **5-8 composants max** : Trop d'infos = pas d'info
4. **Filtres pertinents** : Période + 2-3 dimensions clés
5. **Couleurs cohérentes** : Même code couleur partout
6. **Mobile-friendly** : Tester l'affichage sur smartphone
7. **Nommer clairement** : Titres explicites sur chaque composant
8. **Actualiser régulièrement** : Un dashboard obsolète est pire qu'aucun dashboard
