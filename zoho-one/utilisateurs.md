# Zoho One - Gestion des Utilisateurs

## Ajouter un Utilisateur

### Invitation par Email
```
1. Admin → Utilisateurs → Ajouter
2. Renseigner :
   - Prénom : Marie
   - Nom : Durand
   - Email : marie.durand@entreprise.fr
   - Département : Marketing
   - Rôle : Utilisateur standard
3. Sélectionner les applications autorisées
4. Envoyer l'invitation

L'utilisateur reçoit un email pour activer son compte.
```

### Import en Masse
```csv
First Name,Last Name,Email,Department,Role
Marie,Durand,marie@entreprise.fr,Marketing,User
Luc,Bernard,luc@entreprise.fr,IT,Admin
Anne,Martin,anne@entreprise.fr,Finance,User
```

### Provisionnement Automatique (via SSO)
```
Si SSO configuré (Azure AD, Google Workspace) :
  - Les utilisateurs sont créés automatiquement à la première connexion
  - Les attributs sont synchronisés (nom, département, photo)
  - La désactivation dans l'annuaire source désactive dans Zoho
```

## Rôles et Permissions

### Rôles par Défaut
| Rôle | Description | Permissions |
|------|-------------|-------------|
| Super Admin | Propriétaire du compte | Toutes |
| Admin | Administrateur | Gestion utilisateurs, apps, paramètres |
| Utilisateur | Employé standard | Accès aux apps autorisées |
| Invité | Accès limité | Applications spécifiques uniquement |

### Rôles Personnalisés
```
Créer un rôle "Manager Marketing" :
  Applications autorisées :
    ✅ Zoho CRM (accès complet)
    ✅ Zoho Campaigns (accès complet)
    ✅ Zoho Social (accès complet)
    ✅ Zoho Analytics (lecture seule)
    ✅ Zoho Projects (son équipe uniquement)
    ❌ Zoho Books (pas d'accès)
    ❌ Zoho People Admin (pas d'accès)
```

## Groupes

```
Groupes :
  - "Équipe Marketing" → 8 membres
  - "Managers" → 12 membres
  - "Développeurs" → 15 membres
  - "Comité Direction" → 5 membres

Utilisation :
  - Partage de documents (WorkDrive)
  - Canaux Cliq
  - Règles d'accès CRM
  - Politiques de sécurité
```

## Cycle de Vie Utilisateur

```
1. Création / Invitation
2. Activation (premier login)
3. Utilisation active
4. Modification (changement de rôle, département)
5. Suspension temporaire (congé longue durée)
6. Désactivation (départ)
7. Suppression (après période de rétention)

Note : La désactivation conserve les données mais bloque l'accès.
       La suppression est irréversible.
```

## Rapports Utilisateurs

| Rapport | Description |
|---------|-------------|
| Utilisateurs actifs | Connexions sur les 30 derniers jours |
| Utilisateurs inactifs | Aucune connexion depuis 30+ jours |
| Utilisation par app | Nombre d'utilisateurs par application |
| Licences | Licences utilisées vs disponibles |
| Activité de connexion | Journal des connexions (date, IP, appareil) |

## Bonnes Pratiques

1. **Désactiver immédiatement** les comptes des employés partis
2. **Utiliser les groupes** plutôt que les permissions individuelles
3. **Auditer les accès** trimestriellement (supprimer les inactifs)
4. **Principe du moindre privilège** — ne donner que les accès nécessaires
5. **Provisionnement automatique** via SSO pour éviter les oublis
