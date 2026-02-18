# ⚙️ Zoho Desk - Configuration

> Départements, canaux de support, SLA, heures ouvrées et paramètres généraux.

## Table des matières

- [Paramétrage initial](#paramétrage-initial)
- [Départements](#départements)
- [Canaux de support](#canaux-de-support)
- [SLA (Service Level Agreements)](#sla)
- [Heures ouvrées](#heures-ouvrées)
- [Agents et équipes](#agents-et-équipes)
- [Rôles et profils](#rôles-et-profils)
- [Champs personnalisés](#champs-personnalisés)

---

## Paramétrage initial

```
Étapes de mise en place :
1. Créer les départements (ex : Support, Commercial, Technique)
2. Configurer les canaux (email, chat, téléphone, formulaire web)
3. Définir les heures ouvrées
4. Configurer les SLA
5. Ajouter les agents et les affecter aux départements
6. Créer les modèles d'email
7. Configurer les automatisations (workflows, blueprints)
8. Personnaliser le portail client (base de connaissances)
```

---

## Départements

### Création

```
Configuration → Départements → + Nouveau département

Champs :
- Nom du département
- Description
- Email associé (support@entreprise.com)
- Agents assignés
- Produit/service associé (optionnel)
```

### Exemples de structure

```
Organisation
├── Support Général
│   ├── Email : support@entreprise.com
│   └── Agents : 5
├── Support Technique
│   ├── Email : technique@entreprise.com
│   └── Agents : 3
├── Commercial
│   ├── Email : commercial@entreprise.com
│   └── Agents : 4
├── Facturation
│   ├── Email : facturation@entreprise.com
│   └── Agents : 2
└── RH (interne)
    ├── Email : rh@entreprise.com
    └── Agents : 2
```

### Département par défaut

```
Le département par défaut reçoit les tickets qui ne sont pas
automatiquement routés vers un département spécifique.
```

---

## Canaux de support

### Email

```
Configuration → Canaux → Email

Chaque département peut avoir son propre email :
- support@entreprise.com → Département Support
- tech@entreprise.com → Département Technique

Configuration :
1. Ajouter l'adresse email
2. Configurer la redirection (forwarding) ou IMAP
3. Zoho crée automatiquement un ticket pour chaque email reçu
4. Les réponses de l'agent sont envoyées depuis cette adresse

Options :
- Créer un ticket par email ou par conversation (thread)
- Signature par agent ou par département
- Modèle de réponse par défaut
```

### Chat en direct (Zoho SalesIQ)

```
Intégration avec Zoho SalesIQ :
- Widget de chat sur le site web
- Conversion chat → ticket si non résolu
- Historique de chat attaché au ticket
- Routing vers le bon département
```

### Formulaire web

```
Configuration → Canaux → Formulaire web

Générer un formulaire HTML intégrable :
- Champs : Nom, Email, Sujet, Description, Département, Priorité
- Personnalisation des champs
- CAPTCHA optionnel
- Redirection après soumission

Code à intégrer :
<iframe src="https://desk.zoho.eu/portal/entreprise/newticket" ...></iframe>
```

### Téléphone (Zoho Telephony)

```
Intégration avec PhoneBridge :
- Appels entrants → création de ticket
- Identification automatique du client
- Enregistrement des appels
- Click-to-call depuis un ticket
```

### Réseaux sociaux

```
Canaux supportés :
- Facebook (Messenger + publications)
- Twitter / X
- Instagram

Configuration :
- Connecter le compte social
- Chaque message/mention → ticket ou conversation
- Répondre directement depuis Zoho Desk
```

---

## SLA

### Configuration des SLA

```
Configuration → SLA → Nouvelle politique SLA

Paramètres :
- Nom de la politique
- Départements concernés
- Conditions d'application (priorité, canal, client...)
```

### Définition des temps cibles

```
Politique : "SLA Standard"

┌──────────┬───────────────────┬──────────────────┐
│ Priorité │ 1ère réponse      │ Résolution       │
├──────────┼───────────────────┼──────────────────┤
│ Urgente  │ 1h ouvrée         │ 4h ouvrées       │
│ Haute    │ 4h ouvrées        │ 8h ouvrées       │
│ Moyenne  │ 8h ouvrées        │ 24h ouvrées      │
│ Basse    │ 24h ouvrées       │ 48h ouvrées      │
└──────────┴───────────────────┴──────────────────┘

Politique : "SLA Premium"
┌──────────┬───────────────────┬──────────────────┐
│ Priorité │ 1ère réponse      │ Résolution       │
├──────────┼───────────────────┼──────────────────┤
│ Urgente  │ 15 min            │ 1h               │
│ Haute    │ 1h                │ 4h               │
│ Moyenne  │ 4h                │ 8h               │
│ Basse    │ 8h                │ 24h              │
└──────────┴───────────────────┴──────────────────┘
```

### Escalades SLA

```
Niveaux d'escalade :

Niveau 1 : 80% du temps écoulé
  → Notification à l'agent assigné
  → Notification au chef d'équipe

Niveau 2 : 100% du temps dépassé
  → Notification au manager
  → Réassignation automatique à un agent senior

Niveau 3 : 150% du temps dépassé
  → Notification au directeur
  → Priorité automatiquement relevée

Niveau 4 : 200% du temps dépassé
  → Notification à la direction générale
```

---

## Heures ouvrées

```
Configuration → Heures ouvrées

Exemple :
  Lundi à Vendredi : 9h00 - 18h00
  Samedi : Fermé
  Dimanche : Fermé

Jours fériés (France) :
  - 01 Janvier (Jour de l'an)
  - Lundi de Pâques
  - 01 Mai (Fête du travail)
  - 08 Mai (Victoire 1945)
  - Ascension
  - Lundi de Pentecôte
  - 14 Juillet (Fête nationale)
  - 15 Août (Assomption)
  - 01 Novembre (Toussaint)
  - 11 Novembre (Armistice)
  - 25 Décembre (Noël)

Impact : Les SLA ne comptent que les heures ouvrées.
  Ticket reçu vendredi 17h (SLA 4h) → Échéance lundi 12h
```

---

## Agents et équipes

### Ajouter un agent

```
Configuration → Agents → + Nouvel agent

Champs :
- Nom, Email
- Département(s) assigné(s)
- Rôle (Agent, Admin, Manager...)
- Profil de permissions
- Spécialités / Compétences
- Limite de tickets simultanés
```

### Équipes

```
Configuration → Équipes

Équipe "Support Niveau 1" :
  Agents : Alice, Bob, Charlie
  Département : Support Général
  Round-robin : Oui

Équipe "Support Niveau 2" :
  Agents : David, Eve
  Département : Support Technique
  Escalade depuis N1 : Oui
```

---

## Rôles et profils

### Rôles prédéfinis

| Rôle | Description |
|------|-------------|
| **CEO** | Accès complet à tout |
| **Manager** | Gestion d'équipe, rapports |
| **Agent** | Traitement des tickets |
| **Light Agent** | Lecture + commentaires internes uniquement |

### Profils de permissions

```
Profil "Agent Standard" :
  Tickets : Créer, Lire, Modifier (les siens + département)
  Base de connaissances : Lire
  Rapports : Lire (ses propres KPIs)
  Contacts : Lire

Profil "Manager Support" :
  Tickets : Tout (tous les départements)
  Base de connaissances : Tout
  Rapports : Tout
  Contacts : Tout
  Agents : Gérer son équipe
```

---

## Champs personnalisés

```
Configuration → Disposition et champs → Champs personnalisés

Types de champs ajoutables aux tickets :
- Texte (ligne unique, multiligne)
- Nombre, Décimal
- Date, Date-heure
- Liste déroulante, Cases à cocher
- Lookup (vers contacts)
- URL, Email, Téléphone

Exemples de champs personnalisés :
- "Numéro de contrat" (texte)
- "Version du produit" (liste déroulante)
- "Impact business" (liste : Faible, Moyen, Fort, Critique)
- "Date de première occurrence" (date)
- "Environnement" (liste : Production, Staging, Dev)
```

---

## Bonnes pratiques

1. **Départements** : Limiter à 5-7 pour éviter la confusion
2. **SLA** : Être réaliste, commencer conservateur puis améliorer
3. **Canaux** : Activer uniquement les canaux que l'équipe peut gérer
4. **Heures ouvrées** : Mettre à jour les jours fériés chaque année
5. **Agents** : Définir des limites de tickets pour éviter la surcharge
6. **Champs personnalisés** : Ne pas en abuser, rester pertinent

---

*Voir aussi : [tickets.md](tickets.md) | [automatisations.md](automatisations.md) | [rapports.md](rapports.md)*
