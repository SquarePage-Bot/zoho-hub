# 🌐 Zoho Creator - Pages

> Pages personnalisées, HTML/CSS/JS, widgets et intégrations front-end.

## Table des matières

- [Vue d'ensemble](#vue-densemble)
- [Types de pages](#types-de-pages)
- [HTML, CSS et JavaScript](#html-css-et-javascript)
- [SDK Creator (ZOHO.CREATOR)](#sdk-creator)
- [Widgets](#widgets)
- [Exemples pratiques](#exemples-pratiques)

---

## Vue d'ensemble

Les pages Creator permettent de créer des interfaces personnalisées en HTML/CSS/JS au sein d'une application Creator. Elles offrent un contrôle total sur l'UI, contrairement aux formulaires et rapports standards.

**Cas d'usage :**
- Tableaux de bord personnalisés
- Portails clients
- Interfaces de saisie avancées
- Intégrations avec des bibliothèques JS externes
- Landing pages internes

---

## Types de pages

### Page HTML (Custom Page)

Page entièrement codée en HTML/CSS/JS avec accès au SDK Creator.

```
Éditeur intégré :
- Onglet HTML
- Onglet CSS (feuille de style dédiée)
- Onglet JavaScript (Deluge côté client impossible, utiliser le SDK)
```

### Page de formulaire embarqué

Intègre un formulaire existant dans une mise en page personnalisée.

### Page avec widgets

Utilise des widgets prédéfinis ou des widgets Zoho (ZET - Zoho Extension Toolkit).

---

## HTML, CSS et JavaScript

### Structure de base

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <style>
        body {
            font-family: 'Lato', sans-serif;
            margin: 20px;
            background: #f5f5f5;
        }
        .dashboard-card {
            background: white;
            border-radius: 8px;
            padding: 20px;
            margin: 10px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
            display: inline-block;
            width: 250px;
            text-align: center;
        }
        .card-value {
            font-size: 36px;
            font-weight: bold;
            color: #1a73e8;
        }
        .card-label {
            font-size: 14px;
            color: #666;
            margin-top: 8px;
        }
    </style>
</head>
<body>
    <h1>Tableau de bord</h1>
    <div id="dashboard"></div>

    <script src="https://js.zohostatic.com/creator/widgets/api/v2.js"></script>
    <script>
        ZOHO.CREATOR.init()
        .then(function() {
            chargerDonnees();
        });

        function chargerDonnees() {
            var config = {
                appName: "mon-app",
                reportName: "Tous_les_Tickets",
                criteria: "",
                page: 1,
                pageSize: 200
            };

            ZOHO.CREATOR.API.getAllRecords(config)
            .then(function(response) {
                var records = response.data;
                afficherTableauDeBord(records);
            });
        }

        function afficherTableauDeBord(records) {
            var ouverts = records.filter(r => r.Statut === "Ouvert").length;
            var enCours = records.filter(r => r.Statut === "En cours").length;
            var fermes = records.filter(r => r.Statut === "Fermé").length;
            var total = records.length;

            var html = `
                <div class="dashboard-card">
                    <div class="card-value">${total}</div>
                    <div class="card-label">Total tickets</div>
                </div>
                <div class="dashboard-card">
                    <div class="card-value" style="color:#e74c3c">${ouverts}</div>
                    <div class="card-label">Ouverts</div>
                </div>
                <div class="dashboard-card">
                    <div class="card-value" style="color:#f39c12">${enCours}</div>
                    <div class="card-label">En cours</div>
                </div>
                <div class="dashboard-card">
                    <div class="card-value" style="color:#27ae60">${fermes}</div>
                    <div class="card-label">Fermés</div>
                </div>
            `;
            document.getElementById("dashboard").innerHTML = html;
        }
    </script>
</body>
</html>
```

---

## SDK Creator

### Initialisation

```javascript
// Toujours initialiser le SDK avant d'appeler les API
ZOHO.CREATOR.init()
.then(function() {
    console.log("SDK prêt");
});
```

### Lecture d'enregistrements

```javascript
// Tous les enregistrements d'un rapport
ZOHO.CREATOR.API.getAllRecords({
    appName: "mon-app",
    reportName: "Tous_les_Clients",
    criteria: "Statut == \"Actif\"",
    page: 1,
    pageSize: 50
})
.then(function(response) {
    console.log(response.data);       // Array d'enregistrements
    console.log(response.totalCount); // Nombre total
});

// Un enregistrement par ID
ZOHO.CREATOR.API.getRecordById({
    appName: "mon-app",
    reportName: "Tous_les_Clients",
    id: "3860000000012345"
})
.then(function(response) {
    console.log(response.data);
});
```

### Création d'enregistrements

```javascript
ZOHO.CREATOR.API.addRecord({
    appName: "mon-app",
    formName: "Clients",
    data: {
        data: {
            Nom: "Jean Dupont",
            Email: "jean@acme.com",
            Entreprise: "ACME",
            Statut: "Prospect"
        }
    }
})
.then(function(response) {
    console.log("Créé avec ID:", response.data.ID);
});
```

### Mise à jour

```javascript
ZOHO.CREATOR.API.updateRecord({
    appName: "mon-app",
    reportName: "Tous_les_Clients",
    id: "3860000000012345",
    data: {
        data: {
            Statut: "Client",
            Date_Conversion: "2026-02-18"
        }
    }
})
.then(function(response) {
    console.log("Mis à jour");
});
```

### Suppression

```javascript
ZOHO.CREATOR.API.deleteRecord({
    appName: "mon-app",
    reportName: "Tous_les_Clients",
    criteria: "Statut == \"Archivé\" && Date_Modification < \"01-Jan-2025\""
})
.then(function(response) {
    console.log("Supprimé");
});
```

### Informations contextuelles

```javascript
// Utilisateur connecté
ZOHO.CREATOR.UTIL.getInitParams()
.then(function(params) {
    console.log(params.loginUser);     // Email
    console.log(params.userRole);      // Rôle
    console.log(params.appLinkName);   // Nom de l'app
});

// Naviguer vers un autre rapport/formulaire
ZOHO.CREATOR.UTIL.navigateParentURL("/mon-app/#Report:Tous_les_Clients");
```

---

## Widgets

### Widget Zoho (ZET)

Les widgets permettent d'étendre Creator avec du code hébergé (comme un mini-site web intégré dans Creator).

**Structure d'un widget :**
```
mon-widget/
├── plugin-manifest.json
├── app/
│   ├── index.html
│   ├── css/
│   │   └── style.css
│   └── js/
│       └── app.js
└── resources/
    └── icon.png
```

**plugin-manifest.json :**
```json
{
    "service": "CREATOR",
    "cspDomains": {
        "connect-src": ["https://api.externe.com"]
    },
    "modules": {
        "widgets": {
            "MonWidget": {
                "type": "page",
                "index": "app/index.html",
                "name": "Mon Widget Personnalisé"
            }
        }
    }
}
```

### Bibliothèques JS externes

Vous pouvez charger des librairies externes dans vos pages :

```html
<!-- Chart.js pour les graphiques -->
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<!-- DataTables pour les tableaux interactifs -->
<link rel="stylesheet" href="https://cdn.datatables.net/1.13.6/css/jquery.dataTables.min.css">
<script src="https://code.jquery.com/jquery-3.7.0.min.js"></script>
<script src="https://cdn.datatables.net/1.13.6/js/jquery.dataTables.min.js"></script>

<!-- Leaflet pour les cartes -->
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css">
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
```

---

## Exemples pratiques

### Tableau de bord avec Chart.js

```html
<canvas id="chart" width="600" height="400"></canvas>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script src="https://js.zohostatic.com/creator/widgets/api/v2.js"></script>
<script>
ZOHO.CREATOR.init().then(function() {
    ZOHO.CREATOR.API.getAllRecords({
        appName: "mon-app",
        reportName: "Ventes_Par_Mois",
        page: 1, pageSize: 12
    }).then(function(resp) {
        var labels = resp.data.map(r => r.Mois);
        var values = resp.data.map(r => parseFloat(r.CA));

        new Chart(document.getElementById("chart"), {
            type: "bar",
            data: {
                labels: labels,
                datasets: [{
                    label: "Chiffre d'affaires (€)",
                    data: values,
                    backgroundColor: "#1a73e8"
                }]
            },
            options: {
                responsive: true,
                plugins: { legend: { display: true } }
            }
        });
    });
});
</script>
```

### Portail client avec recherche

```html
<div style="max-width:800px; margin:0 auto; padding:20px;">
    <h2>Suivi de vos commandes</h2>
    <input type="text" id="searchRef" placeholder="Numéro de commande..."
           style="width:100%; padding:12px; font-size:16px; border:1px solid #ddd; border-radius:4px;">
    <div id="results" style="margin-top:20px;"></div>
</div>

<script src="https://js.zohostatic.com/creator/widgets/api/v2.js"></script>
<script>
ZOHO.CREATOR.init();

document.getElementById("searchRef").addEventListener("keyup", function(e) {
    if (e.key === "Enter") {
        var ref = this.value.trim();
        ZOHO.CREATOR.API.getAllRecords({
            appName: "mon-app",
            reportName: "Commandes",
            criteria: "Numero == \"" + ref + "\""
        }).then(function(resp) {
            if (resp.data.length === 0) {
                document.getElementById("results").innerHTML = "<p>Aucune commande trouvée.</p>";
                return;
            }
            var cmd = resp.data[0];
            document.getElementById("results").innerHTML = `
                <div style="background:white; padding:20px; border-radius:8px; box-shadow:0 2px 4px rgba(0,0,0,0.1);">
                    <h3>Commande ${cmd.Numero}</h3>
                    <p><strong>Statut :</strong> ${cmd.Statut}</p>
                    <p><strong>Date :</strong> ${cmd.Date}</p>
                    <p><strong>Montant :</strong> ${cmd.Montant} €</p>
                </div>
            `;
        });
    }
});
</script>
```

---

## Bonnes pratiques

1. **Toujours initialiser le SDK** avec `ZOHO.CREATOR.init()` avant tout appel
2. **Gestion d'erreurs** : Ajouter des `.catch()` sur chaque appel API
3. **Performance** : Limiter les appels API, mettre en cache les données statiques
4. **Responsive** : Utiliser Flexbox/Grid pour le mobile
5. **Sécurité** : Ne jamais stocker de tokens dans le code HTML côté client
6. **CSP** : Déclarer les domaines externes dans le manifest pour les widgets

---

*Voir aussi : [rapports.md](rapports.md) | [api.md](api.md) | [../../widgets/](../../widgets/)*
