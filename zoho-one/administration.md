# Zoho One - Administration

## Panneau d'Administration

### Accès
```
URL : https://one.zoho.com/admin
Rôle requis : Administrateur Zoho One
```

### Vue d'Ensemble
```
Dashboard Admin :
  ├── Utilisateurs : 85 actifs / 100 licences
  ├── Applications : 32 activées / 45+ disponibles
  ├── Stockage : 2.1 TB / 5 TB utilisés
  ├── Sécurité : 2 alertes en cours
  └── Activité : 12 400 connexions ce mois
```

## Gestion des Applications

### Activer / Désactiver une Application
```
1. Admin → Applications
2. Sélectionner l'application
3. Activer / Désactiver pour toute l'organisation

Options par application :
  - Activer pour tous les utilisateurs
  - Activer pour certains départements
  - Activer pour certains rôles
```

### Configurer une Application
```
Chaque application a ses propres paramètres :
  - Zoho CRM → Modules, champs, workflows
  - Zoho Books → Plan comptable, TVA, devises
  - Zoho People → Politiques RH, congés
  
Accès : Admin → Applications → [App] → Paramètres
```

## Gestion du Domaine

### Domaine Personnalisé
```
Domaine email : @entreprise.fr
Domaine portail : portal.entreprise.fr

Configuration DNS :
  MX : mx.zoho.com (priorité 10)
  MX : mx2.zoho.com (priorité 20)
  SPF : v=spf1 include:zoho.com ~all
  DKIM : zoho._domainkey → clé publique
  DMARC : _dmarc → v=DMARC1; p=quarantine
```

## Annuaire (Directory)

### Structure de l'Organisation
```
Organisation : VotreSociété SAS
  ├── 📁 Direction
  │   └── 3 utilisateurs
  ├── 📁 Commercial
  │   ├── 📁 France
  │   └── 📁 International
  ├── 📁 Marketing
  │   └── 8 utilisateurs
  ├── 📁 Technique
  │   ├── 📁 Développement
  │   └── 📁 Infrastructure
  ├── 📁 Finance
  └── 📁 RH
```

### SSO (Single Sign-On)
```
Fournisseurs supportés :
  - Azure AD / Entra ID
  - Google Workspace
  - Okta
  - OneLogin
  - SAML 2.0 (générique)
  - LDAP / Active Directory

Configuration :
  1. Admin → Sécurité → SSO
  2. Choisir le fournisseur
  3. Configurer les URLs (login, logout, metadata)
  4. Mapper les attributs (email, nom, département)
  5. Tester avec un utilisateur pilote
  6. Activer pour tous
```

## Sauvegarde et Export

```
Export des données :
  - Par application (CRM, Books, etc.)
  - Format : CSV, JSON
  - Planification : hebdomadaire ou mensuelle

Conformité :
  - RGPD : Export des données personnelles sur demande
  - Droit à l'effacement : Suppression des données employé
  - Portabilité : Export complet au format standard
```

## Bonnes Pratiques

1. **Nommer un admin principal et un backup** — ne jamais dépendre d'une seule personne
2. **Activer uniquement les apps nécessaires** — moins de complexité
3. **Configurer le SSO** dès le départ pour la sécurité
4. **Documenter la configuration** de chaque application
5. **Planifier des sauvegardes régulières** des données critiques
