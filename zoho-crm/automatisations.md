# 🤖 Zoho CRM - Automatisations (Vue d'ensemble)

> Toutes les méthodes d'automatisation disponibles dans Zoho CRM.

## Panorama des automatisations

```
Automatisations Zoho CRM
├── Workflows (règles conditionnelles)
├── Blueprints (processus guidés)
├── Règles d'attribution (lead assignment)
├── Règles d'approbation (validation hiérarchique)
├── Règles de validation (contrôle des données)
├── Règles d'escalade (SLA et relances)
├── Actions planifiées (schedules)
├── Fonctions Deluge (scripts personnalisés)
├── CommandCenter (orchestration multi-modules)
├── Zia AI (suggestions et prédictions)
└── Zoho Flow (intégrations externes)
```

## 1. Règles d'attribution (Assignment Rules)

Attribution automatique des enregistrements aux utilisateurs.

### Méthodes

| Méthode | Description |
|---------|-------------|
| **Round Robin** | Distribution équitable entre les commerciaux |
| **Par critères** | Attribution selon des règles conditionnelles |
| **Par territoire** | Attribution selon la zone géographique |

### Exemple Round Robin

```
Module : Leads
Source : Formulaire web

Distribution :
  1. Alice Martin (33%)
  2. Bob Dupont (33%)
  3. Claire Bernard (34%)

Exceptions :
  Si Industry = "Tech" → Directement à Alice
  Si Country != "France" → Directement à Bob
```

### Exemple par critères

```
Règle 1 : Si Annual_Revenue > 1M€ → Équipe Grands Comptes
Règle 2 : Si Lead_Source = "Partenaire" → Responsable partenariats
Règle 3 : Si City = "Paris" → Équipe Île-de-France
Règle 4 : Défaut → Round Robin équipe générale
```

## 2. Règles d'approbation (Approval Rules)

Soumettre un enregistrement à validation avant traitement.

### Configuration

```
Module : Deals
Nom : "Approbation remise > 20%"

Déclencheur : Création ou Modification
Condition : Discount_Percentage > 20

Approbateurs :
  Niveau 1 : Manager direct
  Niveau 2 : Directeur commercial (si Discount > 40%)

Actions si approuvé :
  - Stage → "Proposition envoyée"
  - Email au commercial : "Remise approuvée"

Actions si rejeté :
  - Stage → "Révision nécessaire"
  - Tâche : "Revoir la proposition"

Délai : 48h
Action si délai dépassé : Escalade au niveau 2
```

### Statuts d'approbation

```
En attente → Approuvé
          → Rejeté
          → Délégué
          → Escaladé
```

## 3. Règles de validation

Empêcher la sauvegarde si les données ne respectent pas les critères.

```deluge
// Le montant du deal doit être positif
if(${Amount} != null && ${Amount} <= 0)
{
    alert "Le montant doit être supérieur à 0";
}

// La date de clôture doit être dans le futur
if(${Closing_Date} != null && ${Closing_Date} < today())
{
    alert "La date de clôture ne peut pas être dans le passé";
}

// Si le stage est "Proposition", un montant est obligatoire
if(${Stage} == "Proposition/Devis" && (${Amount} == null || ${Amount} == 0))
{
    alert "Le montant est obligatoire pour les propositions";
}

// Numéro SIRET français : 14 chiffres
if(${SIRET} != null && len(${SIRET}) != 14)
{
    alert "Le SIRET doit contenir exactement 14 chiffres";
}
```

## 4. Règles d'escalade

Escalade automatique quand un SLA n'est pas respecté.

```
Module : Cases (Tickets)
Nom : "Escalade ticket non résolu"

Niveaux :
  Après 4h sans réponse → Notifier le propriétaire
  Après 8h sans réponse → Réassigner au manager
  Après 24h sans résolution → Notifier le directeur
  Après 48h → Priorité = Urgente + Email au CEO
```

## 5. Actions planifiées (Scheduled Actions)

Exécution de fonctions à intervalles réguliers.

### Types

| Type | Description |
|------|-------------|
| **Quotidien** | Tous les jours à une heure précise |
| **Hebdomadaire** | Un jour de la semaine précis |
| **Mensuel** | Un jour du mois précis |
| **Ponctuel** | Une seule exécution programmée |

### Exemple : Nettoyage quotidien des leads

```deluge
// Exécution quotidienne à 2h du matin
// Désactiver les leads inactifs depuis 90 jours

criteria = "(Modified_Time:less_than:" + zoho.currentdate.subDay(90).toString("yyyy-MM-dd") + ")and(Lead_Status:not_equal:Converti)";
inactiveLeads = zoho.crm.searchRecords("Leads", criteria, 1, 200);

count = 0;
for each lead in inactiveLeads
{
    updateMap = Map();
    updateMap.put("Lead_Status", "Inactif");
    updateMap.put("$append_values", Map());
    
    tagList = List();
    tagList.add("Auto-Inactif");
    zoho.crm.addTags("Leads", lead.get("id"), tagList);
    
    zoho.crm.updateRecord("Leads", lead.get("id"), updateMap);
    count = count + 1;
}

info "Leads désactivés : " + count;
```

### Exemple : Rapport hebdomadaire

```deluge
// Tous les lundis à 8h : envoyer le résumé de la semaine

// Deals gagnés la semaine dernière
lastWeek = zoho.currentdate.subDay(7).toString("yyyy-MM-dd");
criteria = "(Stage:equals:Fermée gagnée)and(Closing_Date:greater_equal:" + lastWeek + ")";
wonDeals = zoho.crm.searchRecords("Deals", criteria, 1, 100);

totalAmount = 0;
dealList = "";
for each deal in wonDeals
{
    totalAmount = totalAmount + deal.get("Amount");
    dealList = dealList + "• " + deal.get("Deal_Name") + " : " + deal.get("Amount") + "€\n";
}

// Nouveaux leads cette semaine
criteria2 = "(Created_Time:greater_equal:" + lastWeek + ")";
newLeads = zoho.crm.searchRecords("Leads", criteria2, 1, 200);

// Email de rapport
body = "📊 Rapport hebdomadaire\n\n";
body = body + "🏆 Deals gagnés : " + wonDeals.size() + "\n";
body = body + "💰 Total : " + totalAmount + "€\n\n";
body = body + dealList + "\n";
body = body + "📥 Nouveaux leads : " + newLeads.size();

sendmail
[
    from: zoho.adminuserid
    to: "manager@entreprise.com"
    subject: "Rapport commercial - Semaine du " + lastWeek
    message: body
];
```

## 6. CommandCenter

Orchestration avancée multi-modules avec parcours visuels (Enterprise+).

### Concept

CommandCenter permet de créer des **journeys** (parcours) qui :
- Traversent plusieurs modules
- Incluent des attentes et conditions
- Gèrent des parcours parallèles
- Intègrent des services externes

### Exemple : Parcours client complet

```
Nouveau Lead
    │
    ├── [Email de bienvenue]
    │
    ├── [Attendre 2 jours]
    │
    ├── [Lead a ouvert l'email ?]
    │   ├── OUI → [Assigner au commercial + Tâche d'appel]
    │   └── NON → [Email de relance]
    │              │
    │              ├── [Attendre 3 jours]
    │              │
    │              └── [A répondu ?]
    │                  ├── OUI → [Assigner au commercial]
    │                  └── NON → [Mettre en nurturing]
    │
    ├── [Commercial qualifie le lead ?]
    │   ├── OUI → [Convertir en Contact + Deal]
    │   │          │
    │   │          └── [Deal gagné ?]
    │   │              ├── OUI → [Créer projet + Facture]
    │   │              └── NON → [Enregistrer raison de perte]
    │   │
    │   └── NON → [Lead_Status = "Non qualifié"]
```

## 7. Macros

Actions groupées exécutables en un clic par l'utilisateur.

```
Macro : "Qualifier et planifier"
Actions :
  1. Mise à jour : Lead_Status = "Qualifié"
  2. Créer tâche : "Premier rendez-vous" (J+3)
  3. Email : Template "Demande de RDV"
  4. Tag : "Qualifié manuellement"
```

## 8. Wizards

Formulaires guidés multi-étapes pour la saisie de données.

```
Wizard : "Nouveau client"
Étape 1 : Informations entreprise
  → Champs : Nom, SIRET, Industrie, Site web
Étape 2 : Contact principal
  → Champs : Nom, Prénom, Email, Téléphone, Fonction
Étape 3 : Besoin
  → Champs : Produits intéressés, Budget, Échéance
Étape 4 : Récapitulatif
  → Création : Compte + Contact + Deal
```

## Ordre d'exécution global

```
1. Règles de validation
2. Règles de layout
3. Règles d'attribution
4. Règles d'approbation (si applicable)
5. Workflows
6. Blueprints
7. CommandCenter
8. Webhooks
9. Actions planifiées (selon planning)
```

## Tableau récapitulatif

| Outil | Quand l'utiliser | Édition min |
|-------|-----------------|-------------|
| Workflow | Actions automatiques sur événement | Standard |
| Blueprint | Processus séquentiel obligatoire | Professionnel |
| Assignment | Attribution automatique des leads | Standard |
| Approval | Validation hiérarchique | Standard |
| Validation | Contrôle qualité des données | Standard |
| Escalade | Respect des SLA | Enterprise |
| Schedule | Actions périodiques | Professionnel |
| CommandCenter | Parcours multi-modules | Enterprise |
| Macros | Actions manuelles groupées | Standard |
| Wizards | Saisie guidée | Enterprise |

## Bonnes pratiques

1. **Documenter** : Chaque automatisation doit avoir un nom clair et une description
2. **Tester** : Environnement sandbox pour les tests
3. **Monitorer** : Vérifier les logs régulièrement
4. **Limites** : Respecter les quotas API et d'exécution
5. **Simplicité** : Préférer plusieurs règles simples à une règle complexe
6. **Nommage** : Convention type `MODULE_Trigger_Action` (ex: `DEAL_Won_CreateInvoice`)
7. **Versionning** : Noter les changements dans un fichier de changelog

---
*Voir aussi : [workflows.md](workflows.md), [blueprints.md](blueprints.md), [../zoho-deluge/](../zoho-deluge/) pour les fonctions.*
