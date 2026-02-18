# 📊 Zoho Creator - Rapports

> Types de rapports, filtres, groupements et personnalisation de l'affichage des données.

## Table des matières

- [Types de rapports](#types-de-rapports)
- [Filtres](#filtres)
- [Groupements et tri](#groupements-et-tri)
- [Personnalisation des colonnes](#personnalisation-des-colonnes)
- [Actions sur les rapports](#actions-sur-les-rapports)
- [Recherche et pagination](#recherche-et-pagination)

---

## Types de rapports

### List Report (Rapport Liste)

Le rapport le plus courant. Affiche les enregistrements sous forme de tableau.

```
Colonnes configurables :
- Choix des champs affichés
- Ordre des colonnes (drag & drop)
- Largeur des colonnes
- Formatage conditionnel (couleurs, icônes)
```

**Cas d'usage** : Liste de clients, suivi des commandes, inventaire.

### Summary Report (Rapport Résumé)

Agrège les données avec des calculs (somme, moyenne, nombre, min, max).

```
Configuration :
- Lignes : champ de groupement (ex : Catégorie)
- Colonnes : champ de sous-groupement (ex : Mois)
- Valeurs : champ numérique + fonction d'agrégation

Exemple :
  Lignes = Département
  Colonnes = Trimestre
  Valeurs = SUM(Chiffre_Affaires)
```

**Cas d'usage** : CA par département et trimestre, nombre de tickets par agent.

### Calendar Report (Rapport Calendrier)

Affiche les enregistrements sur un calendrier basé sur un champ date.

```
Configuration :
- Champ date de début
- Champ date de fin (optionnel)
- Titre de l'événement (champ texte)
- Couleur (basée sur un champ)
- Vues : jour, semaine, mois
```

**Cas d'usage** : Planning des rendez-vous, échéancier, congés.

### Kanban Report

Affiche les enregistrements en colonnes de type Kanban (drag & drop).

```
Configuration :
- Champ de catégorisation (dropdown) → colonnes
- Titre de la carte
- Champs affichés sur la carte
- Champ de tri dans chaque colonne
```

**Cas d'usage** : Suivi de projet, pipeline commercial, gestion des tickets.

**Exemple de configuration :**
```
Colonnes = Statut (À faire | En cours | En revue | Terminé)
Titre = Nom_Tache
Sous-titre = Assignee
Couleur = Priorité (Rouge=Haute, Orange=Moyenne, Vert=Basse)
```

### Pivot Report (Tableau Croisé)

Analyse multidimensionnelle avec lignes, colonnes et valeurs.

```
Configuration :
- Lignes : un ou plusieurs champs
- Colonnes : un ou plusieurs champs
- Valeurs : agrégations (SUM, COUNT, AVG, MIN, MAX)
- Filtres globaux
```

### Map Report (Rapport Carte)

Affiche les enregistrements sur une carte géographique.

```
Configuration :
- Champ adresse ou coordonnées GPS
- Marqueur personnalisé (couleur, icône)
- Info-bulle au survol
```

### Timeline Report

Affiche les enregistrements sous forme de chronologie.

```
Configuration :
- Champ date
- Titre et description
- Regroupement par période
```

---

## Filtres

### Filtres statiques (définition du rapport)

```
Critères prédéfinis appliqués au rapport :
- Statut == "Actif"
- Date_Creation >= "01-Jan-2026"
- Département == zoho.loginuser.department
```

### Filtres dynamiques (barre de filtres)

Les utilisateurs peuvent filtrer en temps réel :

```
Types de filtres disponibles :
- Texte : contient, commence par, est exactement
- Nombre : égal, supérieur, inférieur, entre
- Date : aujourd'hui, cette semaine, ce mois, personnalisé
- Liste : sélection dans les valeurs existantes
- Lookup : sélection dans le formulaire lié
- Checkbox : coché/non coché
```

### Filtres via Deluge (On Load du rapport)

```deluge
// Filtrer selon l'utilisateur connecté
if (zoho.loginuserrole == "Agent")
{
    // L'agent ne voit que ses propres enregistrements
    filter by (Assignee == zoho.loginuser);
}

// Filtre basé sur la date
filter by (Date_Creation >= zoho.currentdate.subDay(30));
```

### Filtres par URL (paramètres GET)

```
https://creatorapp.zoho.eu/owner/app/#Report:Rapport?Statut=Actif&Departement=IT

Paramètres supportés :
- Nom_Champ=Valeur
- startdate=2026-01-01 (pour les calendriers)
- enddate=2026-12-31
```

---

## Groupements et tri

### Groupement

```
Configuration :
- Grouper par : un ou plusieurs champs
- Ordre du groupe : croissant/décroissant
- Résumé par groupe : somme, moyenne, nombre
- Groupes repliables : oui/non
```

**Exemple : Commandes groupées par client et statut**
```
Groupe 1 : Client (A → Z)
  Groupe 2 : Statut (Personnalisé : En cours → Livré → Annulé)
    Lignes : détail des commandes
  Résumé : Total = SUM(Montant)
Résumé global : Total général
```

### Tri

```
Tri principal : Date_Creation (Décroissant)
Tri secondaire : Priorité (Personnalisé : Haute → Moyenne → Basse)
Tri tertiaire : Nom (A → Z)
```

### Formatage conditionnel

```
Règles de couleur :
- Si Montant > 10000 → Fond vert
- Si Statut == "En retard" → Texte rouge, gras
- Si Priorité == "Urgente" → Icône ⚠️ + fond jaune
- Si Date_Echeance < aujourd'hui → Fond rouge clair
```

---

## Personnalisation des colonnes

### Configuration des colonnes

```
Pour chaque colonne :
- Visible/masquée
- Largeur (pixels ou %)
- Alignement (gauche, centre, droite)
- Format (date, nombre, devise)
- Lien cliquable (ouvrir l'enregistrement)
```

### Colonnes calculées (Formula)

```deluge
// Dans la configuration du rapport, ajouter une colonne formule :
Total_TTC = Montant_HT * 1.20
Jours_Restants = Date_Echeance - zoho.currentdate
Statut_SLA = if(Jours_Restants < 0, "En retard", "Dans les temps")
```

---

## Actions sur les rapports

### Actions en masse (Bulk Actions)

```
Sélectionner plusieurs enregistrements puis :
- Modifier en masse (changer un champ)
- Supprimer en masse
- Exécuter un workflow personnalisé
- Exporter (CSV, PDF)
- Envoyer par email
```

### Boutons personnalisés

```deluge
// Bouton "Valider la commande" dans le rapport
// Déclenche un script Deluge sur l'enregistrement sélectionné
record = Commandes[ID == input.ID];
record.Statut = "Validée";
record.Date_Validation = zoho.currentdate;
record.Valideur = zoho.loginuser;

// Envoyer notification
sendmail
[
    from: zoho.adminuserid
    to: record.Email_Client
    subject: "Commande " + record.Numero + " validée"
    message: "Votre commande a été validée."
];
```

### Export

```
Formats disponibles :
- CSV
- XLS (Excel)
- PDF
- JSON (via API)

Options :
- Tous les enregistrements ou sélection
- Avec/sans en-têtes
- Encodage (UTF-8, ISO-8859-1)
```

---

## Recherche et pagination

### Recherche

```
Recherche globale :
- Recherche dans tous les champs texte
- Résultats en surbrillance

Recherche par colonne :
- Filtre spécifique à chaque colonne
- Autocomplétion pour les lookups
```

### Pagination

```
Configuration :
- Enregistrements par page : 10, 20, 50, 100, 200
- Navigation : première, précédente, suivante, dernière
- Affichage du total d'enregistrements
```

---

## Intégration dans les pages

```html
<!-- Intégrer un rapport dans une page Creator -->
<div id="report-container"></div>

<script>
// Charger un rapport dans un conteneur
ZOHO.CREATOR.init()
.then(function() {
    var config = {
        reportName: "Mes_Commandes",
        appName: "mon-app",
        criteria: "Statut == \"Actif\"",
        fields: ["Numero", "Client", "Montant", "Date"],
        startIndex: 0,
        limit: 50
    };
    ZOHO.CREATOR.API.getAllRecords(config)
    .then(function(response) {
        // Traiter les données
        console.log(response.data);
    });
});
</script>
```

---

## Bonnes pratiques

1. **Performance** : Limiter le nombre de colonnes affichées (< 15)
2. **Filtres** : Toujours proposer des filtres sur les champs les plus utilisés
3. **Groupement** : Limiter à 2 niveaux de groupement pour la lisibilité
4. **Pagination** : 20-50 enregistrements par page pour un bon équilibre
5. **Export** : Prévoir un bouton d'export pour les rapports fréquemment demandés
6. **Mobile** : Les rapports liste sont les plus adaptés au mobile

---

*Voir aussi : [formulaires.md](formulaires.md) | [pages.md](pages.md) | [api.md](api.md)*
