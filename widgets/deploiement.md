# Widgets Zoho - Déploiement

## Structure d'un Projet Widget

```
mon-widget/
├── app/
│   ├── index.html        ← Page principale
│   ├── css/
│   │   └── style.css     ← Styles
│   ├── js/
│   │   └── app.js        ← Logique JavaScript
│   └── images/
│       └── logo.png      ← Ressources
├── plugin-manifest.json  ← Configuration du widget
└── README.md
```

### plugin-manifest.json
```json
{
  "service": "CRM",
  "components": {
    "widgets": [
      {
        "widgetNamespace": "monwidget",
        "location": "crm.contacts.widgets.related_list",
        "name": "Mon Widget Personnalisé",
        "url": "/app/index.html",
        "logo": "/app/images/logo.png"
      }
    ]
  },
  "connectors": [],
  "version": "1.0.0"
}
```

### Emplacements Disponibles (location)
```
CRM :
  crm.{module}.widgets.related_list    → Related List dans un record
  crm.{module}.widgets.detail_page     → Page de détail
  crm.{module}.widgets.button          → Bouton custom
  crm.{module}.widgets.sidebar         → Panneau latéral
  crm.webtab                           → Onglet web complet

Desk :
  desk.ticket.widgets.sidebar          → Sidebar du ticket
  desk.ticket.widgets.topbar           → Barre supérieure
  desk.background                      → Widget en arrière-plan
```

## Installation avec Zoho CLI (ZET)

### Prérequis
```bash
# Installer Node.js (v14+)
# Installer Zoho Extension Toolkit
npm install -g zoho-extension-toolkit
```

### Créer un Projet
```bash
# Créer un nouveau widget
zet init

# Répondre aux questions :
#   Service : CRM
#   Widget name : mon-widget
#   Location : Related List
#   Module : Contacts

# Structure créée automatiquement
```

### Développement Local
```bash
# Lancer le serveur de développement
zet run

# Ouvre http://localhost:5000
# Le widget tourne dans un simulateur local
# Hot reload activé
```

### Packager et Déployer
```bash
# Créer le package (.zip)
zet pack

# Le fichier mon-widget.zip est créé dans dist/
```

## Déploiement dans Zoho

### Via l'Interface Web
```
1. Zoho CRM → Paramètres → Marketplace → Mes Extensions
2. Cliquer "Installer une extension"
3. Uploader le fichier .zip
4. Configurer les permissions
5. Activer pour les profils souhaités
```

### Via la CLI
```bash
# Se connecter
zet login

# Publier directement
zet publish

# Mettre à jour une extension existante
zet update
```

## Mise à Jour

```
1. Modifier le code
2. Incrémenter la version dans plugin-manifest.json
3. zet pack (ou zet publish)
4. Uploader la nouvelle version
5. Les utilisateurs reçoivent automatiquement la mise à jour
```

## Marketplace Zoho

### Publier sur le Marketplace
```
Pour rendre votre widget disponible à tous les utilisateurs Zoho :

1. Soumettre via https://marketplace.zoho.com
2. Fournir :
   - Description détaillée
   - Captures d'écran (min 3)
   - Documentation
   - Politique de confidentialité
   - Support email
3. Revue par l'équipe Zoho (5-10 jours ouvrés)
4. Publication si approuvé
```

## Bonnes Pratiques

1. **Tester localement** avec `zet run` avant de déployer
2. **Versionner** le code (Git) et le manifeste
3. **Design responsive** — les widgets s'affichent dans des espaces variés
4. **Performances** — charger les données de façon asynchrone
5. **Sécurité** — ne jamais stocker de secrets dans le code client
6. **Documenter** l'installation et la configuration
