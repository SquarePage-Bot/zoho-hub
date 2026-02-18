# Zoho Flow - Exemples Concrets

## Exemple 1 : Lead Web → CRM + Email de Bienvenue

**Scénario :** Un visiteur remplit un formulaire sur le site. Créer un lead dans CRM et envoyer un email automatique.

```
TRIGGER : Webhook (formulaire du site)
  Payload : { name, email, phone, company, source }

ÉTAPE 1 : Rechercher dans Zoho CRM
  Module : Leads
  Critère : Email == ${trigger.data.email}

ÉTAPE 2 : Condition
  SI ${step1.count} == 0 (nouveau lead)
    ÉTAPE 3a : Créer un Lead dans Zoho CRM
      Nom : ${trigger.data.name}
      Email : ${trigger.data.email}
      Téléphone : ${trigger.data.phone}
      Société : ${trigger.data.company}
      Source : ${trigger.data.source}
    
    ÉTAPE 4a : Envoyer un email
      À : ${trigger.data.email}
      Objet : "Bienvenue ${trigger.data.name} !"
      Corps : Template de bienvenue HTML
  
  SINON (lead existant)
    ÉTAPE 3b : Mettre à jour le Lead
      ID : ${step1.data[0].id}
      Dernière activité : ${system.current_datetime}
    
    ÉTAPE 4b : Ajouter une note
      "Nouvelle visite depuis ${trigger.data.source}"
```

## Exemple 2 : Ticket Desk → Slack + CRM

**Scénario :** Quand un ticket critique est créé dans Zoho Desk, notifier Slack et mettre à jour le CRM.

```
TRIGGER : Zoho Desk - Nouveau ticket
  Filtre : Priorité == "Haute" OU Priorité == "Urgente"

ÉTAPE 1 : Rechercher le contact dans Zoho CRM
  Critère : Email == ${trigger.data.contact_email}

ÉTAPE 2 (parallèle) :
  ├── Branche A : Slack
  │   Canal : #support-urgent
  │   Message : "🚨 Ticket critique #${trigger.data.ticket_number}
  │              Client : ${trigger.data.contact_name}
  │              Sujet : ${trigger.data.subject}
  │              Priorité : ${trigger.data.priority}"
  │
  └── Branche B : Zoho CRM (si contact trouvé)
      Mettre à jour le contact :
        Dernier ticket : ${trigger.data.ticket_number}
        Statut support : "En cours"
```

## Exemple 3 : Facture Impayée → Relance Automatique

**Scénario :** Chaque jour, vérifier les factures impayées de plus de 30 jours et envoyer des relances.

```
TRIGGER : Planifié - Chaque jour à 9h00

ÉTAPE 1 : Zoho Books - Rechercher les factures
  Statut : "Impayée"
  Date d'échéance : < ${addDays(system.current_date, -30)}

ÉTAPE 2 : Pour chaque facture
  ÉTAPE 3 : Condition
    SI ${facture.overdue_days} > 60
      → Email de relance urgente (template 3)
      → Notifier le commercial (Slack)
    SINON SI ${facture.overdue_days} > 30
      → Email de relance standard (template 2)
    
  ÉTAPE 4 : Logger dans Google Sheets
    Date : ${system.current_date}
    Facture : ${facture.invoice_number}
    Client : ${facture.customer_name}
    Montant : ${facture.total}
    Jours de retard : ${facture.overdue_days}
    Relance envoyée : Oui
```

## Exemple 4 : Synchronisation Bidirectionnelle CRM ↔ Google Contacts

**Scénario :** Maintenir les contacts synchronisés entre Zoho CRM et Google Contacts.

```
=== Flow A : CRM → Google ===
TRIGGER : Zoho CRM - Contact créé/modifié

ÉTAPE 1 : Rechercher dans Google Contacts
  Critère : email == ${trigger.data.email}

ÉTAPE 2 : Condition
  SI trouvé → Mettre à jour Google Contact
  SINON → Créer Google Contact
    Nom : ${trigger.data.full_name}
    Email : ${trigger.data.email}
    Téléphone : ${trigger.data.phone}
    Société : ${trigger.data.company}

=== Flow B : Google → CRM ===
TRIGGER : Google Contacts - Contact créé/modifié

ÉTAPE 1 : Rechercher dans Zoho CRM
  Critère : email == ${trigger.data.email}

ÉTAPE 2 : Condition
  SI trouvé → Mettre à jour CRM Contact
  SINON → Créer CRM Contact
```

## Exemple 5 : Onboarding Client Automatisé

**Scénario :** Quand un deal est gagné dans CRM, lancer le processus d'onboarding complet.

```
TRIGGER : Zoho CRM - Deal mis à jour
  Filtre : Stage == "Closed Won"

ÉTAPE 1 : Récupérer les détails du contact associé

ÉTAPE 2 (parallèle) :
  ├── A : Zoho Projects
  │   → Créer un projet "Onboarding - ${deal.account_name}"
  │   → Créer les tâches standard depuis un template
  │   → Assigner le chef de projet
  │
  ├── B : Zoho Books
  │   → Créer le client
  │   → Générer la première facture
  │
  ├── C : Zoho Desk
  │   → Créer le profil client
  │   → Assigner le plan de support
  │
  ├── D : Email
  │   → Envoyer le kit de bienvenue
  │   → Inclure les accès et la documentation
  │
  └── E : Slack
      → #onboarding : "Nouveau client : ${deal.account_name}"
      → Mentionner l'équipe CS

ÉTAPE 3 : Mettre à jour le CRM
  Contact.Statut : "Client actif"
  Contact.Date_onboarding : ${system.current_date}
```

## Exemple 6 : Webhook → Multi-Apps (E-commerce)

**Scénario :** Recevoir une commande depuis Shopify et dispatcher dans l'écosystème Zoho.

```
TRIGGER : Webhook - Nouvelle commande Shopify
  Payload : { order_id, customer, line_items[], total, shipping }

ÉTAPE 1 : Rechercher/Créer le client dans Zoho CRM
  Email : ${trigger.data.customer.email}

ÉTAPE 2 : Créer la commande dans Zoho Inventory
  Client : ${step1.data.id}
  Articles : POUR CHAQUE item DANS ${trigger.data.line_items}
    SKU : ${item.sku}
    Quantité : ${item.quantity}
    Prix : ${item.price}

ÉTAPE 3 : Créer la facture dans Zoho Books
  Référence commande : ${trigger.data.order_id}
  Lignes : copiées depuis l'inventaire

ÉTAPE 4 : Mettre à jour Zoho CRM
  Deal.Montant : ${trigger.data.total}
  Deal.Stage : "Commande confirmée"

ÉTAPE 5 : Notification
  Slack #commandes : "Nouvelle commande #${trigger.data.order_id} - ${trigger.data.total}€"
```

## Conseils pour Construire vos Flows

1. **Commencer simple** — un trigger, une action, puis enrichir
2. **Tester chaque étape** individuellement
3. **Utiliser la galerie** comme point de départ
4. **Monitorer les exécutions** pendant les premiers jours
5. **Prévoir les cas d'erreur** dès la conception
6. **Documenter** chaque flow avec un nom et une description clairs
