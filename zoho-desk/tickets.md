# 🎫 Zoho Desk - Tickets

> Création, assignation, statuts, vues et gestion complète des tickets.

## Table des matières

- [Création d'un ticket](#création-dun-ticket)
- [Cycle de vie](#cycle-de-vie)
- [Assignation](#assignation)
- [Vues et filtres](#vues-et-filtres)
- [Conversations et réponses](#conversations-et-réponses)
- [Fusion et relation](#fusion-et-relation)
- [Tags et classification](#tags-et-classification)

---

## Création d'un ticket

### Canaux de création

| Canal | Création |
|-------|----------|
| **Email** | Automatique à réception d'un email |
| **Formulaire web** | Soumission via portail/formulaire |
| **Chat** | Conversion d'un chat non résolu |
| **Téléphone** | Création manuelle par l'agent |
| **Réseaux sociaux** | Automatique (Facebook, Twitter) |
| **API** | Création programmatique |
| **Manuel** | Agent crée dans l'interface Desk |

### Champs d'un ticket

```
Champs obligatoires :
- Sujet (Subject)
- Email ou Contact
- Département

Champs standards :
- Description (corps du message)
- Priorité : Basse, Moyenne, Haute, Urgente
- Statut : Ouvert, En attente, Escaladé, Fermé
- Canal : Email, Téléphone, Chat, Web, Social
- Catégorie / Sous-catégorie / Type de problème
- Agent assigné
- Date d'échéance
- Pièces jointes

Champs personnalisés :
- Ajoutés selon les besoins métier
```

---

## Cycle de vie

### Statuts par défaut

```
Ouvert (Open)
  → Le ticket vient d'être créé ou réouvert
  → SLA en cours

En cours (In Progress)
  → Un agent travaille dessus
  → SLA en cours

En attente (On Hold)
  → En attente d'information du client
  → SLA suspendu (configurable)

Escaladé (Escalated)
  → Transféré à un niveau supérieur
  → SLA en cours (nouveau timer)

Résolu (Resolved)
  → Solution fournie, en attente de confirmation
  → Timer de clôture automatique (ex : 48h)

Fermé (Closed)
  → Ticket définitivement clos
  → Réouvrable si le client répond
```

### Statuts personnalisés

```
Exemples de statuts ajoutés :
- "En attente fournisseur" (SLA suspendu)
- "En test" (SLA en cours)
- "En attente de déploiement"
- "Doublon"
```

### Flux typique

```
1. Client envoie un email → Ticket créé (Ouvert)
2. Round-robin → Assigné à Agent Alice
3. Alice répond → Statut : En attente (réponse client)
4. Client répond → Statut : Ouvert
5. Alice résout → Statut : Résolu
6. 48h sans réponse → Statut : Fermé automatiquement
   OU Client confirme → Fermé
   OU Client conteste → Réouvert
```

---

## Assignation

### Méthodes d'assignation

| Méthode | Description |
|---------|-------------|
| **Round Robin** | Distribution équitable et cyclique |
| **Load Balancing** | Basé sur le nombre de tickets ouverts par agent |
| **Skill-Based** | Basé sur les compétences de l'agent |
| **Manual** | Assignation manuelle par un superviseur |
| **Workflow** | Règle automatique selon des critères |

### Round Robin

```
Configuration → Assignation → Round Robin

Options :
- Département concerné
- Agents participants
- Limite de tickets par agent (ex : max 20 ouverts)
- Considérer la charge actuelle : Oui/Non

Exemple :
  Agents : Alice (5 tickets), Bob (3 tickets), Charlie (7 tickets)
  Prochain ticket → Bob (charge la plus faible)
```

### Assignation par workflow

```
Si Sujet contient "facturation" → Département Facturation
Si Canal = "Téléphone" → Agent senior
Si Client.Type = "Premium" → Équipe Premium
Si Langue = "Anglais" → Équipe internationale
Si Priorité = "Urgente" → Manager de garde
```

---

## Vues et filtres

### Vues prédéfinies

| Vue | Description |
|-----|-------------|
| **Mes tickets ouverts** | Tickets assignés à l'agent connecté |
| **Tickets non assignés** | Sans agent assigné |
| **En retard** | SLA dépassé |
| **Tous les tickets** | Tout le département |
| **Fermés récemment** | Fermés dans les 7 derniers jours |
| **Haute priorité** | Priorité Haute + Urgente |

### Vues personnalisées

```
Créer une vue → Filtres

Exemples :
Vue "Mes tickets urgents" :
  Agent = Moi ET Priorité IN (Haute, Urgente) ET Statut ≠ Fermé

Vue "En attente client > 3 jours" :
  Statut = "En attente" ET Dernière mise à jour < J-3

Vue "Gros clients non résolus" :
  Contact.Type = "Premium" ET Statut IN (Ouvert, En cours, Escaladé)
```

### Modes d'affichage

```
- Liste classique (tableau)
- Mode compact (résumé)
- Mode détaillé (aperçu dans le panneau droit)
- Kanban (colonnes par statut)
- Mode comptoir (Countdown : affiche le SLA restant)
```

### Mode Comptoir (Countdown)

```
Affichage spécial orienté SLA :
- Tickets triés par temps restant avant violation SLA
- Code couleur : Vert (OK), Orange (bientôt), Rouge (dépassé)
- Idéal pour les équipes avec des SLA stricts
```

---

## Conversations et réponses

### Types de réponses

| Type | Visible client | Description |
|------|---------------|-------------|
| **Réponse** | ✅ | Réponse envoyée au client par email |
| **Commentaire** | ❌ | Note interne (visible uniquement par les agents) |
| **Transfert** | ❌ | Transfert à un autre agent/département |

### Modèles de réponse

```
Configuration → Modèles → Modèles de tickets

Exemple "Accusé de réception" :
  Sujet : "Re: ${ticket.subject}"
  Corps :
  "Bonjour ${contact.firstName},

  Nous avons bien reçu votre demande (Ticket #${ticket.ticketNumber}).
  Un de nos agents va la traiter dans les plus brefs délais.

  Cordialement,
  L'équipe support ${department.name}"

Exemple "Demande d'information" :
  "Bonjour ${contact.firstName},

  Merci pour votre message. Afin de mieux vous aider, pourriez-vous
  nous fournir les informations suivantes :
  - ...
  - ...

  Cordialement,
  ${agent.firstName}"
```

### Snippets (réponses rapides)

```
Raccourcis pour insérer du texte pré-écrit :
- #merci → "Merci pour votre patience, nous revenons vers vous rapidement."
- #cloture → "Ce ticket va être clôturé. N'hésitez pas à nous recontacter."
- #reset → "Pour réinitialiser votre mot de passe, rendez-vous sur..."
```

### Satisfaction client (CSAT)

```
À la fermeture du ticket, email automatique :
"Comment évaluez-vous notre support ?"
  😊 Satisfait | 😐 Neutre | 😞 Insatisfait

+ Commentaire optionnel

Résultats visibles dans les rapports.
```

---

## Fusion et relation

### Fusionner des tickets

```
Tickets → Sélectionner 2+ tickets → Fusionner

Cas d'usage :
- Le client a envoyé plusieurs emails pour le même problème
- Doublons créés par différents canaux

Le ticket fusionné conserve toutes les conversations.
```

### Tickets liés

```
Ticket parent → Tickets enfants

Cas d'usage : Un incident global affecte plusieurs clients
  Ticket parent : "Panne serveur X"
  Tickets enfants :
    - "Client A : site indisponible"
    - "Client B : erreurs 500"
    - "Client C : lenteurs"

Quand le parent est résolu → notification sur tous les enfants.
```

---

## Tags et classification

### Tags

```
Tags libres appliqués aux tickets :
- "bug", "feature-request", "documentation"
- "client-premium", "contrat-sla"
- "produit-x", "version-3.2"

Utilisation :
- Filtrer les vues
- Statistiques par tag
- Déclencheurs de workflow
```

### Classification hiérarchique

```
Catégorie → Sous-catégorie → Type de problème

Exemple :
  Catégorie : Technique
    Sous-catégorie : Connexion
      Type : Mot de passe oublié
      Type : Erreur de login
      Type : Compte bloqué
    Sous-catégorie : Performance
      Type : Lenteurs
      Type : Timeout
  Catégorie : Facturation
    Sous-catégorie : Paiement
      Type : Remboursement
      Type : Erreur de facturation
```

---

## Bonnes pratiques

1. **Réponse rapide** : Toujours envoyer un accusé de réception automatique
2. **Commentaires internes** : Documenter les étapes de résolution pour l'historique
3. **Tags** : Utiliser des tags cohérents pour l'analyse future
4. **Modèles** : Préparer des modèles pour les réponses fréquentes
5. **CSAT** : Activer les enquêtes de satisfaction pour mesurer la qualité
6. **Fusion** : Fusionner les doublons rapidement pour éviter le double traitement

---

*Voir aussi : [configuration.md](configuration.md) | [automatisations.md](automatisations.md) | [base-connaissances.md](base-connaissances.md)*
