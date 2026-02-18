# Sources de Données

## Présentation

Zoho Analytics peut ingérer des données depuis de multiples sources : fichiers, bases de données, applications cloud et API. La synchronisation peut être ponctuelle ou automatique.

## Import de fichiers

### Formats supportés

| Format | Extension | Taille max |
|--------|-----------|-----------|
| **CSV** | .csv | 50 Mo (100 Mo plan Enterprise) |
| **Excel** | .xls, .xlsx | 50 Mo |
| **JSON** | .json | 50 Mo |
| **HTML** | .html (tables) | 50 Mo |
| **TSV** | .tsv | 50 Mo |

### Importer un fichier CSV

```
Workspace → + Importer → Fichier local

1. Sélectionner le fichier
2. Configurer :
   - Nom de la table : "Ventes_2026"
   - Délimiteur : Virgule / Point-virgule / Tab
   - Encodage : UTF-8
   - Première ligne = en-têtes : Oui
   
3. Mapper les colonnes :
   ┌────────────────┬──────────────┬────────────┐
   │ Colonne CSV    │ Type détecté │ Nom table  │
   ├────────────────┼──────────────┼────────────┤
   │ date_vente     │ Date         │ Date       │
   │ client         │ Texte        │ Client     │
   │ produit        │ Texte        │ Produit    │
   │ montant        │ Nombre       │ Montant    │
   │ quantite       │ Nombre       │ Quantité   │
   │ region         │ Texte        │ Région     │
   └────────────────┴──────────────┴────────────┘

4. Options d'import :
   ☑ Créer une nouvelle table
   ☐ Ajouter à une table existante
   ☐ Mettre à jour les lignes existantes (par clé)
```

### Import récurrent de fichiers

```
Sources de données → Import programmé → + Nouveau

Sources :
- FTP / SFTP : ftp://monserveur.fr/exports/ventes.csv
- URL HTTP : https://api.monsite.fr/export/ventes.csv
- Google Drive / Dropbox / OneDrive : Fichier partagé
- Email : Pièce jointe d'un email automatique

Fréquence : Toutes les heures / Quotidien / Hebdomadaire / Mensuel

Options :
☑ Ajouter les nouvelles lignes
☑ Mettre à jour les lignes existantes (clé : "id_vente")
☐ Remplacer toutes les données
```

## Connecteurs d'applications

### Applications Zoho

#### Zoho CRM

```
+ Nouvelle connexion → Zoho CRM

Modules à synchroniser :
☑ Leads (Prospects)
☑ Contacts
☑ Deals (Transactions)
☑ Accounts (Comptes)
☑ Tasks (Tâches)
☑ Calls (Appels)
☐ Products (Produits)
☐ Quotes (Devis)

Fréquence de sync : Toutes les heures
Champs : Tous / Sélection personnalisée

Tables créées automatiquement :
- Leads
- Contacts
- Deals
- Accounts
- Tasks
- Calls
+ Tables de liaison automatiques
```

#### Zoho Books

```
+ Nouvelle connexion → Zoho Books

Modules :
☑ Factures (Invoices)
☑ Paiements (Payments)
☑ Dépenses (Expenses)
☑ Articles (Items)
☑ Contacts
☑ Comptes bancaires

Sync : Quotidienne à 2h00
```

### Applications tierces

#### Google Analytics

```
+ Nouvelle connexion → Google Analytics

Propriété : UA-XXXXXXX / GA4
Vues : Site principal

Métriques importées :
- Sessions, Utilisateurs, Pages vues
- Taux de rebond, Durée moyenne
- Sources de trafic
- Pages les plus visitées
- Conversions et objectifs

Dimensions : Date, Source, Pays, Appareil, Page

Sync : Quotidienne
```

#### Shopify

```
+ Nouvelle connexion → Shopify

Données importées :
- Commandes (détail, statut, montant)
- Produits (nom, catégorie, stock, prix)
- Clients (nom, email, historique)
- Inventaire
- Remboursements

Sync : Toutes les 6 heures
```

## Bases de données

### Connecteurs disponibles

| Base de données | Connexion |
|----------------|-----------|
| **MySQL** | Directe / SSH tunnel |
| **PostgreSQL** | Directe / SSH tunnel |
| **Microsoft SQL Server** | Directe |
| **Oracle** | Directe |
| **MariaDB** | Directe |
| **Amazon RDS** | Via endpoint |
| **Google Cloud SQL** | Via endpoint |
| **Azure SQL** | Via endpoint |

### Configurer une connexion MySQL

```
+ Nouvelle connexion → Base de données → MySQL

Paramètres :
- Hôte : db.monserveur.fr
- Port : 3306
- Base de données : app_production
- Utilisateur : analytics_readonly
- Mot de passe : ********

Options avancées :
☑ Connexion SSL
☐ SSH Tunnel (si base non accessible publiquement)
   - Hôte SSH : bastion.monserveur.fr
   - Port SSH : 22
   - Utilisateur SSH : admin
   - Clé privée : [upload]
```

### Requête SQL personnalisée

```sql
-- Importer les ventes du dernier trimestre avec jointures
SELECT 
    v.id,
    v.date_vente,
    c.nom AS client,
    c.ville,
    p.nom AS produit,
    p.categorie,
    v.quantite,
    v.montant_ht,
    v.montant_ttc
FROM ventes v
JOIN clients c ON v.client_id = c.id
JOIN produits p ON v.produit_id = p.id
WHERE v.date_vente >= DATE_SUB(CURDATE(), INTERVAL 3 MONTH)
ORDER BY v.date_vente DESC
```

```
Synchronisation :
- Fréquence : Toutes les heures
- Mode : Incrémental (basé sur la date de modification)
- Clé primaire : id
```

## API et webhooks

### Import via API

```bash
# Ajouter des lignes à une table via l'API
curl -X POST "https://analyticsapi.zoho.eu/restapi/v2/workspaces/{workspaceId}/views/{viewId}/rows" \
  -H "Authorization: Bearer {token}" \
  -H "Content-Type: application/json" \
  -d '{
    "columns": [
      {"columnName": "Date", "value": "2026-03-15"},
      {"columnName": "Produit", "value": "Licence Pro"},
      {"columnName": "Montant", "value": "299.99"},
      {"columnName": "Client", "value": "TechCorp"}
    ]
  }'
```

### Webhook entrant

```
Paramètres → Webhooks entrants → + Nouveau

URL générée : https://analytics.zoho.eu/webhook/v1/xxxxx

Format attendu :
{
  "data": [
    {"date": "2026-03-15", "event": "signup", "user": "marie@ex.fr"},
    {"date": "2026-03-15", "event": "purchase", "user": "paul@ex.fr"}
  ]
}

→ Les données sont ajoutées automatiquement à la table cible
```

## Relations entre tables (Lookup)

### Créer une relation

```
Table "Ventes" → Clic droit sur colonne "client_id"
→ Lookup → Table "Clients" → Colonne "id"

Types de relation :
- Lookup (N:1) : Chaque vente a un client
- Multi-lookup (N:N) : via table de liaison

Résultat :
La table Ventes peut maintenant accéder aux colonnes de Clients
dans les rapports (nom, ville, secteur, etc.)
```

### Exemple de modèle relationnel

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│  Clients    │     │   Ventes    │     │  Produits   │
├─────────────┤     ├─────────────┤     ├─────────────┤
│ id (PK)     │◄────│ client_id   │     │ id (PK)     │
│ nom         │     │ produit_id  │────►│ nom         │
│ ville       │     │ date_vente  │     │ categorie   │
│ secteur     │     │ quantite    │     │ prix_unit   │
│ commercial  │     │ montant     │     │ stock       │
└─────────────┘     │ remise      │     └─────────────┘
                    └─────────────┘
                          │
                          │ lookup
                          ▼
                    ┌─────────────┐
                    │ Commerciaux │
                    ├─────────────┤
                    │ id (PK)     │
                    │ nom         │
                    │ region      │
                    │ equipe      │
                    └─────────────┘
```

## Préparation des données

### Colonnes de formule

```
Ajouter une colonne calculée à une table :

Exemples :
- Marge : "Montant" - "Coût"
- Marge % : ("Montant" - "Coût") / "Montant" * 100
- Trimestre : quarter("Date")
- Année-Mois : year("Date") & "-" & month("Date")
- Catégorie CA :
    if("Montant" > 10000, "Grand compte",
       if("Montant" > 1000, "Moyen", "Petit"))
```

### Nettoyage et transformation

```
Opérations disponibles :
- Supprimer les doublons
- Remplacer les valeurs (rechercher/remplacer)
- Changer le type de données
- Fusionner des colonnes
- Séparer une colonne (split)
- Remplir les valeurs manquantes
- Convertir les formats de date
- Normaliser le texte (majuscules, trim)
```

### Fusion de tables (Query Table)

```sql
-- Créer une Query Table combinant plusieurs sources
SELECT 
    c.nom AS client,
    c.secteur,
    SUM(v.montant) AS ca_total,
    COUNT(v.id) AS nb_commandes,
    AVG(v.montant) AS panier_moyen,
    MAX(v.date_vente) AS derniere_commande
FROM Ventes v
JOIN Clients c ON v."client_id" = c."id"
GROUP BY c.nom, c.secteur
ORDER BY ca_total DESC
```

## Synchronisation

### Modes de synchronisation

| Mode | Description | Usage |
|------|-------------|-------|
| **Remplacement total** | Supprime et réimporte tout | Petites tables, données volatiles |
| **Ajout** | Ajoute les nouvelles lignes | Logs, événements |
| **Incrémental** | Met à jour les lignes modifiées | Tables transactionnelles |

### Monitorer les syncs

```
Paramètres → Sources de données → Historique de synchronisation

┌─────────────────┬──────────┬──────────┬──────────┬──────────┐
│ Source           │ Dernière │ Statut   │ Lignes   │ Durée    │
├─────────────────┼──────────┼──────────┼──────────┼──────────┤
│ Zoho CRM        │ 10h30    │ ✅ OK    │ +45      │ 12s      │
│ Google Analytics │ 02h00    │ ✅ OK    │ +1 200   │ 45s      │
│ MySQL prod      │ 11h00    │ ⚠️ Partiel│ +890    │ 2m30s    │
│ CSV FTP          │ 06h00    │ ❌ Erreur │ 0       │ -        │
└─────────────────┴──────────┴──────────┴──────────┴──────────┘
```

## Bonnes pratiques

1. **Planifier les syncs hors heures de pointe** : Réduire l'impact sur les systèmes sources
2. **Utiliser le mode incrémental** : Plus rapide et moins gourmand
3. **Créer des relations** : Lier les tables pour des rapports cross-données
4. **Nettoyer à la source** : Corriger les données avant import quand possible
5. **Documenter les sources** : Noter l'origine et la fréquence de chaque table
6. **Monitorer les erreurs** : Vérifier l'historique de sync quotidiennement
7. **Limiter les colonnes** : N'importer que les colonnes utiles pour l'analyse
