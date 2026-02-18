# 📊 Zoho CRM - Guide Complet

> Documentation exhaustive de Zoho CRM : modules, champs, automatisations, API.

## Vue d'ensemble

Zoho CRM est une plateforme de gestion de la relation client qui permet de :
- Gérer les leads, contacts, comptes et transactions
- Automatiser les processus de vente
- Suivre les interactions clients
- Analyser les performances commerciales

## Architecture

```
Zoho CRM
├── Modules standards (Leads, Contacts, Accounts, Deals, etc.)
├── Modules personnalisés
├── Automatisations
│   ├── Workflows
│   ├── Blueprints
│   ├── Actions planifiées
│   └── Fonctions Deluge
├── API REST v2 / v6
└── Intégrations (Zoho Books, Desk, Campaigns, etc.)
```

## Fichiers de cette section

| Fichier | Contenu |
|---------|---------|
| [modules.md](modules.md) | Tous les modules standards et personnalisés |
| [champs.md](champs.md) | Types de champs, validation, formules |
| [workflows.md](workflows.md) | Règles de workflow et déclencheurs |
| [blueprints.md](blueprints.md) | Processus Blueprint (états/transitions) |
| [scoring.md](scoring.md) | Scoring des leads et contacts |
| [vues-filtres.md](vues-filtres.md) | Vues, filtres et critères |
| [automatisations.md](automatisations.md) | Vue globale des automatisations |
| [api.md](api.md) | API REST v2/v6, OAuth, endpoints |

## Concepts clés

### Hiérarchie des données
```
Organisation (compte Zoho)
└── Utilisateurs (rôles + profils)
    └── Modules
        └── Enregistrements (records)
            ├── Champs standards
            ├── Champs personnalisés
            ├── Sous-formulaires
            ├── Notes
            ├── Pièces jointes
            └── Relations (lookups)
```

### Limites importantes

| Élément | Édition Gratuite | Standard | Professionnel | Enterprise | Ultimate |
|---------|-------------------|----------|---------------|------------|----------|
| Utilisateurs | 3 | Illimité | Illimité | Illimité | Illimité |
| Enregistrements | 5 000 | 100 000 | Illimité | Illimité | Illimité |
| Modules personnalisés | 0 | 0 | 10 | 25 | 100 |
| Workflows | 0 | 5/module | 15/module | 30/module | 50/module |
| Blueprints | 0 | 0 | 1/module | 5/module | 20/module |
| API calls/jour | 0 | 1 000 | 2 000 | 5 000 | 10 000 |

### Rôles et Profils

- **Rôle** : Position hiérarchique (détermine la visibilité des données)
- **Profil** : Ensemble de permissions (détermine les actions possibles)
- Un utilisateur = 1 rôle + 1 profil

---
*Voir aussi : [zoho-deluge/](../zoho-deluge/) pour le scripting, [api.md](api.md) pour l'intégration.*
