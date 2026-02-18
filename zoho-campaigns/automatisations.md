# Automatisations

## Présentation

Les automatisations de Zoho Campaigns permettent d'envoyer des emails ciblés au bon moment, sans intervention manuelle. Elles incluent les workflows, les autoresponders et les drip campaigns.

## Autoresponders

### Concept

Un autoresponder est un email (ou une série) envoyé automatiquement suite à un événement simple.

### Types d'autoresponders

| Type | Déclencheur | Exemple |
|------|-------------|---------|
| **Bienvenue** | Inscription à une liste | Email de bienvenue |
| **Anniversaire** | Date du champ anniversaire | Offre spéciale anniversaire |
| **Date personnalisée** | N'importe quel champ date | Rappel de renouvellement |
| **Basé sur un champ** | Valeur d'un champ spécifique | Contenu par segment |

### Exemple : Série de bienvenue

```
Déclencheur : Inscription à la liste "Newsletter"

Jour 0 (immédiat) :
  📧 "Bienvenue chez TechCorp !"
  → Présentation de l'entreprise
  → Lien vers les meilleurs articles

Jour 2 :
  📧 "Nos ressources gratuites pour vous"
  → Ebook gratuit
  → Lien vers les webinars

Jour 5 :
  📧 "Découvrez nos solutions"
  → Présentation des produits
  → CTA : Demander une démo

Jour 10 :
  📧 "Une question ? On est là"
  → Témoignages clients
  → CTA : Planifier un appel
```

### Configurer un autoresponder

```
Automatisations → Autoresponders → + Nouveau

1. Nom : "Série bienvenue newsletter"
2. Déclencheur : Ajout à la liste "Newsletter"
3. Planifier les emails :
   Email 1 : Immédiat
   Email 2 : 2 jours après l'email 1
   Email 3 : 3 jours après l'email 2
4. Choisir/créer le template pour chaque email
5. Activer
```

## Workflows avancés

### Concept

Les workflows sont des automatisations visuelles avec logique conditionnelle : branchements, délais, actions multiples et déclencheurs variés.

### Déclencheurs de workflow

| Déclencheur | Description |
|-------------|-------------|
| **Inscription à une liste** | Contact ajouté à une liste |
| **Ouverture d'email** | Contact ouvre un email spécifique |
| **Clic sur un lien** | Contact clique dans un email |
| **Champ modifié** | Valeur d'un champ mise à jour |
| **Tag ajouté** | Un tag est appliqué au contact |
| **Score atteint** | Le score du contact dépasse un seuil |
| **Événement e-commerce** | Achat, abandon de panier |
| **Date** | Date anniversaire ou personnalisée |
| **API / Webhook** | Déclenchement externe |

### Actions disponibles

| Action | Description |
|--------|-------------|
| **Envoyer un email** | Envoyer un email spécifique |
| **Attendre** | Délai (heures, jours, date spécifique) |
| **Condition (If/Else)** | Branchement selon critères |
| **Ajouter à une liste** | Déplacer vers une autre liste |
| **Retirer d'une liste** | Supprimer d'une liste |
| **Ajouter un tag** | Appliquer un tag |
| **Modifier un champ** | Changer la valeur d'un champ |
| **Envoyer un webhook** | Appeler une URL externe |
| **Créer une tâche CRM** | Créer une tâche dans Zoho CRM |
| **Notifier** | Envoyer une notification interne |
| **Envoyer un SMS** | Envoyer un SMS (si module SMS actif) |

### Exemple 1 : Workflow d'onboarding SaaS

```
[Inscription essai gratuit]
        │
        ▼
[📧 Email : Bienvenue + Guide démarrage]
        │
        ▼
[⏳ Attendre 1 jour]
        │
        ▼
[❓ A connecté son compte ?]
    │           │
   OUI         NON
    │           │
    ▼           ▼
[📧 Tutoriel   [📧 Rappel : 
 avancé]        "Besoin d'aide ?"]
    │           │
    ▼           ▼
[⏳ 3 jours]   [⏳ 2 jours]
    │           │
    ▼           ▼
[📧 Étude de   [❓ A connecté ?]
 cas client]     │         │
    │           OUI       NON
    ▼           │         │
[⏳ 5 jours]   ▼         ▼
    │        [📧 Tutoriel] [📧 Offre d'aide
    ▼                       personnalisée]
[❓ A upgradé en payant ?]    │
    │           │              ▼
   OUI         NON     [🏷️ Tag: "besoin-aide"]
    │           │       [📋 Tâche CRM pour
    ▼           ▼        le commercial]
[📧 Merci +  [📧 Rappel fin
 onboarding   d'essai J-3]
 premium]        │
                 ▼
            [⏳ 3 jours]
                 │
                 ▼
            [📧 Dernière chance
             offre -20%]
```

### Exemple 2 : Workflow d'abandon de panier

```
[🛒 Abandon de panier détecté]
        │
        ▼
[⏳ Attendre 1 heure]
        │
        ▼
[❓ A finalisé l'achat ?]
    │           │
   OUI         NON
    │           │
    ▼           ▼
[FIN]    [📧 "Votre panier vous attend"
          + Produits du panier
          + Bouton "Finaliser"]
                │
                ▼
         [⏳ Attendre 24h]
                │
                ▼
         [❓ A acheté ?]
            │       │
           OUI     NON
            │       │
            ▼       ▼
          [FIN]  [📧 "Dernière chance"
                  + Code promo -10%
                  + Urgence : stock limité]
                    │
                    ▼
               [⏳ 48h]
                    │
                    ▼
               [❓ A acheté ?]
                  │       │
                 OUI     NON
                  │       │
                  ▼       ▼
                [FIN]  [🏷️ Tag: "abandon-panier"]
                       [📋 Rapport mensuel]
```

### Exemple 3 : Workflow de lead nurturing B2B

```
[📥 Téléchargement d'un ebook]
        │
        ▼
[📧 Email : Ebook + ressources complémentaires]
[🏷️ Tag : "intérêt-{sujet-ebook}"]
        │
        ▼
[⏳ 3 jours]
        │
        ▼
[❓ Score du lead > 50 ?]
    │           │
   OUI         NON
    │           │
    ▼           ▼
[📧 Étude de  [📧 Article de blog
 cas sectoriel] complémentaire]
    │           │
    ▼           ▼
[⏳ 5 jours]  [⏳ 7 jours]
    │           │
    ▼           ▼
[📧 Invitation [📧 Webinar replay
 démo gratuite] sur le sujet]
    │           │
    ▼           ▼
[❓ A demandé  [❓ Score > 50 ?]
 une démo ?]      │       │
   │    │        OUI     NON
  OUI  NON       │       │
   │    │        ▼       ▼
   ▼    ▼    [Revenir   [📧 Email mensuel
[CRM:  [📧    branche     "Restez informé"]
Tâche   Relance OUI]       │
commer- douce]              ▼
cial]                    [FIN - reste dans
                          la newsletter]
```

## Lead Scoring

### Concept

Le scoring attribue des points aux contacts en fonction de leur engagement, permettant d'identifier les leads les plus chauds.

### Configurer le scoring

```
Automatisations → Lead Scoring → Configurer

Actions positives :
  +5  points → Ouvre un email
  +10 points → Clique sur un lien
  +20 points → Visite la page tarifs
  +15 points → Télécharge un ebook
  +30 points → Demande une démo

Actions négatives :
  -5  points → N'ouvre pas un email
  -10 points → Se désabonne d'une liste
  -2  points → Par semaine d'inactivité

Seuils :
  0-30   → Lead froid (nurturing)
  31-60  → Lead tiède (engagement)
  61-100 → Lead chaud (transférer au commercial)
```

### Utiliser le score dans les workflows

```
Condition dans un workflow :
SI score > 60
  → Ajouter au segment "Leads chauds"
  → Créer une tâche CRM pour le commercial
  → Envoyer notification au sales manager
SINON
  → Continuer le nurturing
```

## Drip Campaigns (Campagnes goutte à goutte)

### Concept

Une série d'emails envoyés à intervalles réguliers pour guider le contact dans un parcours.

### Exemple : Drip campaign éducative

```
Sujet : "Maîtrisez le marketing digital en 7 jours"

Jour 1 : "Les bases du marketing digital"
Jour 2 : "Créer votre stratégie de contenu"
Jour 3 : "SEO : les fondamentaux"
Jour 4 : "Email marketing efficace"
Jour 5 : "Publicité en ligne (Google Ads, Facebook)"
Jour 6 : "Analytics et mesure de performance"
Jour 7 : "Plan d'action personnalisé + offre formation"

Conditions de sortie :
- Se désabonne → Sortie immédiate
- Achète la formation → Sortie + workflow client
- N'ouvre aucun des 3 premiers emails → Sortie + tag "inactif"
```

## Bonnes pratiques

1. **Mapper le parcours client** : Dessiner le workflow sur papier avant de le créer
2. **Commencer simple** : Un workflow de 3-4 étapes est plus efficace qu'un monstre complexe
3. **Tester** : Envoyer à une liste test avant d'activer en production
4. **Monitorer** : Vérifier les taux à chaque étape et optimiser
5. **Limiter la fréquence** : Ne pas envoyer plus d'un email par jour par contact
6. **Prévoir les sorties** : Toujours définir des conditions de sortie du workflow
7. **Scorer intelligemment** : Adapter les points au cycle de vente de votre business
8. **Combiner avec le CRM** : Les workflows les plus puissants connectent marketing et ventes
