# 🏗️ Zoho Creator - Guide

> Plateforme low-code pour créer des applications métier personnalisées.

## Vue d'ensemble

Zoho Creator permet de construire des applications sans (ou peu) coder :
- **Formulaires** : Saisie de données avec validations
- **Rapports** : Listes, calendriers, kanban, cartes
- **Pages** : Interfaces personnalisées (HTML/CSS/JS)
- **Workflows** : Automatisations Deluge
- **API** : Exposer les données via REST

## Concepts clés

### Structure d'une application

```
Application Creator
├── Formulaires (Forms)
│   ├── Champs
│   ├── Règles de validation
│   └── Actions on submit (Deluge)
├── Rapports (Reports)
│   ├── Liste (List)
│   ├── Résumé (Summary)
│   ├── Calendrier (Calendar)
│   ├── Kanban
│   └── Pivot
├── Pages (Pages)
│   ├── HTML
│   ├── CSS
│   └── JavaScript
├── Workflows
│   ├── On create
│   ├── On edit
│   ├── On delete
│   ├── On approval
│   └── Scheduled
└── Connexions & API
```

### Types de champs

| Type | Description |
|------|-------------|
| Single Line | Texte court |
| Multi Line | Texte long |
| Name | Prénom + Nom |
| Email | Email validé |
| Phone | Téléphone |
| Number | Nombre |
| Decimal | Décimal |
| Currency | Devise |
| Date | Date |
| Date-Time | Date et heure |
| Drop Down | Liste déroulante |
| Radio | Boutons radio |
| Checkbox | Cases à cocher |
| Multi Select | Sélection multiple |
| Lookup | Relation vers un autre formulaire |
| File Upload | Upload de fichier |
| Image | Image |
| Subform | Sous-formulaire (lignes enfant) |
| Rich Text | Éditeur HTML |
| Decision Box | Oui/Non |
| Formula | Champ calculé |

## Deluge dans Creator

### Événements de formulaire

```deluge
// On Add (à la création d'un enregistrement)
// Variables disponibles : input.Field_Name pour chaque champ

// Validation avant enregistrement
if(input.Montant < 0)
{
    cancel submit;
    alert "Le montant doit être positif";
}

// Action après enregistrement
if(input.Statut == "Urgent")
{
    sendmail
    [
        from: zoho.adminuserid
        to: "manager@entreprise.com"
        subject: "Nouvelle demande urgente"
        message: "Demande de " + input.Nom_du_demandeur + "\n" + input.Description
    ];
}
```

### Accès aux données

```deluge
// Récupérer des enregistrements
records = Nom_du_formulaire[Statut == "En cours"];
for each record in records
{
    info record.Nom + " - " + record.Date_creation;
}

// Récupérer par ID
record = Nom_du_formulaire[ID == input.ID_enregistrement];

// Mettre à jour
record.Statut = "Terminé";
record.Date_cloture = zoho.currentdate;

// Compter
count = Nom_du_formulaire[Statut == "En cours"].count();

// Somme
total = Nom_du_formulaire[Statut == "Validé"].sum(Montant);

// Agrégations
avg = Nom_du_formulaire[Type == "Projet"].avg(Budget);
maxVal = Nom_du_formulaire[Année == 2026].max(CA);
minVal = Nom_du_formulaire[Année == 2026].min(CA);
```

### Intégration CRM depuis Creator

```deluge
// Lire un contact CRM
contact = zoho.crm.getRecordById("Contacts", contactId);

// Créer un lead depuis Creator
leadMap = Map();
leadMap.put("Last_Name", input.Nom);
leadMap.put("Email", input.Email);
leadMap.put("Company", input.Entreprise);
leadMap.put("Lead_Source", "Application Creator");
response = zoho.crm.createRecord("Leads", leadMap);
```

## API Creator

### Endpoints

```
Base URL : https://creator.zoho.eu/api/v2/{owner}/{app}

GET    /report/{report_name}          → Lire les enregistrements
POST   /form/{form_name}              → Créer un enregistrement
PATCH  /report/{report_name}/{id}     → Mettre à jour
DELETE /report/{report_name}/{id}     → Supprimer
```

### Exemple : Créer un enregistrement

```bash
POST https://creator.zoho.eu/api/v2/squarepage/mon-app/form/Demandes

{
  "data": {
    "Nom": "Jean Dupont",
    "Email": "jean@acme.com",
    "Type": "Support",
    "Description": "Besoin d'aide avec le CRM"
  }
}
```

## Bonnes pratiques

1. **Nommage** : Noms de formulaires et champs en français clair
2. **Validation** : Valider côté formulaire ET côté workflow
3. **Lookups** : Utiliser des lookups plutôt que de dupliquer les données
4. **Performances** : Indexer les champs utilisés dans les filtres
5. **Sécurité** : Configurer les permissions par rôle
6. **Mobile** : Tester les formulaires sur mobile (responsive natif)

## 📚 Documentation détaillée

| Fichier | Contenu |
|---------|---------|
| [formulaires.md](formulaires.md) | Types de champs, validation, actions on submit, sous-formulaires |
| [rapports.md](rapports.md) | Types de rapports, filtres, groupements, export |
| [pages.md](pages.md) | Pages HTML personnalisées, SDK Creator, widgets |
| [workflows.md](workflows.md) | Workflow rules, schedules, blueprints, approbations |
| [deluge-creator.md](deluge-creator.md) | Scripts Deluge spécifiques à Creator |
| [api.md](api.md) | API REST Creator, endpoints, authentification |

---
*Voir aussi : [../zoho-deluge/](../zoho-deluge/) pour la syntaxe Deluge complète.*
