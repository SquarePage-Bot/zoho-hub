# Zoho One - Politiques de Sécurité

## Authentification

### Politique de Mots de Passe
```
Configuration recommandée :
  Longueur minimale : 12 caractères
  Complexité : Majuscule + minuscule + chiffre + caractère spécial
  Expiration : 90 jours
  Historique : 10 derniers mots de passe interdits
  Verrouillage : Après 5 tentatives échouées (30 min)
```

### Authentification Multi-Facteurs (MFA)
```
Méthodes MFA disponibles :
  - Zoho OneAuth (app mobile) ← Recommandé
  - Google Authenticator / Authy
  - SMS (code par texto)
  - Email (code par email)
  - YubiKey (clé physique)

Politique :
  MFA obligatoire : Oui, pour tous les utilisateurs
  Appareils de confiance : 30 jours max
  Méthode de secours : Codes de récupération (10 codes à usage unique)
```

## Contrôle d'Accès

### Restrictions IP
```
Autoriser la connexion uniquement depuis :
  - Réseau bureau : 82.123.45.0/24
  - VPN entreprise : 10.0.0.0/8
  - Plage admin : 82.123.45.10-82.123.45.20

Exception : Application mobile (avec MFA obligatoire)
```

### Restrictions Géographiques
```
Pays autorisés :
  ✅ France
  ✅ Belgique
  ✅ Suisse
  ❌ Tous les autres → Bloquer + alerter l'admin
```

### Restrictions Horaires
```
Connexion autorisée :
  Lundi - Vendredi : 06:00 - 22:00
  Samedi : 08:00 - 18:00
  Dimanche : Bloqué (sauf admins)
```

## Gestion des Appareils

### Mobile Device Management (MDM)
```
Politiques mobiles :
  - Verrouillage par PIN/biométrie obligatoire
  - Chiffrement du stockage obligatoire
  - Effacement à distance en cas de perte
  - Interdiction du jailbreak/root
  - VPN obligatoire pour les données sensibles
  
Appareils autorisés :
  - iOS 15+ 
  - Android 12+
  - Appareils gérés par l'entreprise uniquement (optionnel)
```

## Journaux d'Audit

### Types d'Événements Tracés
```
Connexions :
  - Login réussi / échoué
  - Logout
  - Changement de mot de passe
  - Réinitialisation MFA

Administration :
  - Ajout/suppression d'utilisateur
  - Changement de rôle/permission
  - Activation/désactivation d'application
  - Modification de politique

Données :
  - Export de données
  - Suppression en masse
  - Partage externe
```

### Consultation des Logs
```
Admin → Sécurité → Journaux d'audit

Filtres :
  - Par utilisateur
  - Par type d'événement
  - Par date
  - Par résultat (succès/échec)

Rétention : 1 an (configurable)
Export : CSV pour analyse externe
```

## Conformité RGPD

```
Outils intégrés :
  - Consentement : Formulaires de consentement dans CRM
  - Portabilité : Export des données au format standard
  - Droit à l'oubli : Anonymisation/suppression des données
  - DPO : Désignation du responsable dans les paramètres
  - Registre des traitements : Documentable dans Zoho
  - Notification de violation : Procédure configurable
```

## Bonnes Pratiques

1. **MFA obligatoire** pour tous, sans exception
2. **Mots de passe forts** — 12+ caractères, complexité requise
3. **Principe du moindre privilège** — accès minimal nécessaire
4. **Auditer régulièrement** — vérifier les logs mensuellement
5. **Former les utilisateurs** — phishing, bonnes pratiques
6. **Plan de réponse aux incidents** — procédure documentée
