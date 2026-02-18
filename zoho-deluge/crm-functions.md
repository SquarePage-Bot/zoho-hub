# 🔗 Zoho Deluge - Fonctions CRM

> Toutes les fonctions `zoho.crm.*` pour manipuler les données CRM depuis Deluge.

## CRUD - Opérations de base

### Créer un enregistrement

```deluge
// zoho.crm.createRecord(module, dataMap)
// Retourne : Map avec id et détails

leadMap = Map();
leadMap.put("Last_Name", "Dupont");
leadMap.put("First_Name", "Jean");
leadMap.put("Email", "jean@acme.com");
leadMap.put("Company", "Acme Corp");
leadMap.put("Phone", "+33612345678");
leadMap.put("Lead_Source", "Site Web");
leadMap.put("Industry", "Technology");

response = zoho.crm.createRecord("Leads", leadMap);
info response;
// {"id":"5234876000000123456","Created_Time":"2026-02-18T16:30:00+01:00",...}

newId = response.get("id");
info "Lead créé avec l'ID : " + newId;
```

### Créer avec des options

```deluge
// Avec trigger de workflow
leadMap = Map();
leadMap.put("Last_Name", "Martin");
leadMap.put("Company", "Beta Corp");

// Le 3ème paramètre contrôle les triggers
// optionalMap : {"trigger":["workflow"]} pour déclencher les workflows
optMap = Map();
triggers = List();
triggers.add("workflow");
triggers.add("blueprint");
optMap.put("trigger", triggers);

response = zoho.crm.createRecord("Leads", leadMap, optMap);
```

### Lire un enregistrement par ID

```deluge
// zoho.crm.getRecordById(module, recordId)
// Retourne : Map de l'enregistrement

contact = zoho.crm.getRecordById("Contacts", "5234876000000123456");

info contact.get("First_Name");     // "Jean"
info contact.get("Last_Name");      // "Dupont"
info contact.get("Email");          // "jean@acme.com"

// Champ lookup (retourne un objet)
account = contact.get("Account_Name");
if(account != null)
{
    info account.get("name");       // "Acme Corp"
    info account.get("id");         // "5234876000000789012"
}

// Propriétaire
owner = contact.get("Owner");
info owner.get("name");             // "Commercial 1"
info owner.get("id");               // "5234876000000111222"
```

### Mettre à jour un enregistrement

```deluge
// zoho.crm.updateRecord(module, recordId, dataMap)

updateMap = Map();
updateMap.put("Stage", "Négociation");
updateMap.put("Amount", 35000);
updateMap.put("Probability", 75);

response = zoho.crm.updateRecord("Deals", "5234876000000123456", updateMap);
info response;
```

### Supprimer un enregistrement

```deluge
// zoho.crm.deleteRecord(module, recordId)

response = zoho.crm.deleteRecord("Leads", "5234876000000123456");
info response;  // {"code":"SUCCESS","message":"record deleted"}
```

## Recherche et récupération

### Lister les enregistrements

```deluge
// zoho.crm.getRecords(module, page, perPage)
// page : numéro de page (commence à 1)
// perPage : nombre de records (max 200)

leads = zoho.crm.getRecords("Leads", 1, 50);
info "Nombre de leads : " + leads.size();

for each lead in leads
{
    info lead.get("Last_Name") + " - " + lead.get("Company");
}
```

### Rechercher avec critères

```deluge
// zoho.crm.searchRecords(module, criteria, page, perPage)

// Opérateurs : equals, not_equal, starts_with, contains, not_contains
// greater_than, less_than, greater_equal, less_equal, between

// Recherche simple
criteria = "(Stage:equals:Négociation)";
deals = zoho.crm.searchRecords("Deals", criteria, 1, 100);

// Recherche avec AND
criteria = "(Stage:equals:Négociation)and(Amount:greater_than:10000)";
deals = zoho.crm.searchRecords("Deals", criteria, 1, 100);

// Recherche avec OR
criteria = "(Lead_Source:equals:Site Web)or(Lead_Source:equals:LinkedIn)";
leads = zoho.crm.searchRecords("Leads", criteria, 1, 100);

// Recherche combinée
criteria = "((Stage:equals:Négociation)and(Amount:greater_than:10000))or(Stage:equals:Proposition/Devis)";
deals = zoho.crm.searchRecords("Deals", criteria, 1, 100);

// Recherche par date
today = zoho.currentdate.toString("yyyy-MM-dd");
criteria = "(Closing_Date:less_equal:" + today + ")and(Stage:not_equal:Fermée gagnée)";
overdue = zoho.crm.searchRecords("Deals", criteria, 1, 100);
```

### Récupérer tous les enregistrements (pagination)

```deluge
// Itérer sur toutes les pages
page = 1;
perPage = 200;
allRecords = List();

loop
{
    records = zoho.crm.getRecords("Deals", page, perPage);
    if(records.size() == 0)
    {
        break;
    }
    allRecords.addAll(records);
    page = page + 1;
    
    if(records.size() < perPage)
    {
        break; // Dernière page
    }
    
    // Sécurité : max 10 pages (2000 records)
    if(page > 10)
    {
        break;
    }
}

info "Total : " + allRecords.size() + " records";
```

## Enregistrements liés (Related Records)

### Obtenir les enregistrements liés

```deluge
// zoho.crm.getRelatedRecords(relatedModule, parentModule, parentId)

// Contacts d'un compte
contacts = zoho.crm.getRelatedRecords("Contacts", "Accounts", accountId);
for each contact in contacts
{
    info contact.get("Full_Name") + " - " + contact.get("Email");
}

// Deals d'un contact
deals = zoho.crm.getRelatedRecords("Deals", "Contacts", contactId);
for each deal in deals
{
    info deal.get("Deal_Name") + " : " + deal.get("Amount") + "€";
}

// Notes d'un deal
notes = zoho.crm.getRelatedRecords("Notes", "Deals", dealId);

// Activités (tâches + événements)
tasks = zoho.crm.getRelatedRecords("Tasks", "Deals", dealId);
events = zoho.crm.getRelatedRecords("Events", "Contacts", contactId);
```

## Conversion de leads

```deluge
// zoho.crm.convertLead(leadId, convertData)

convertData = Map();
convertData.put("overwrite", true);
convertData.put("notify_lead_owner", true);
convertData.put("notify_new_entity_owner", true);

// Optionnel : données du deal à créer
dealData = Map();
dealData.put("Deal_Name", "Projet " + lead.get("Company"));
dealData.put("Closing_Date", zoho.currentdate.addMonth(3).toString("yyyy-MM-dd"));
dealData.put("Stage", "Qualification");
dealData.put("Amount", 10000);
convertData.put("Deals", dealData);

// Optionnel : assigner à un utilisateur différent
// convertData.put("assign_to", "5234876000000111222");

response = zoho.crm.convertLead(leadId, convertData);
info response;
// {"Contacts":"5234876000000888999","Accounts":"5234876000000777888","Deals":"5234876000000666777"}

contactId = response.get("Contacts");
accountId = response.get("Accounts");
dealId = response.get("Deals");
```

## Tags

```deluge
// Ajouter des tags
tagList = List();
tagList.add("VIP");
tagList.add("Priorité haute");
response = zoho.crm.addTags("Deals", dealId, tagList);

// Supprimer des tags
removeList = List();
removeList.add("À relancer");
response = zoho.crm.removeTags("Deals", dealId, removeList);
```

## Notes

```deluge
// Créer une note
noteMap = Map();
noteMap.put("Note_Title", "Compte-rendu appel");
noteMap.put("Note_Content", "Discussion avec le client concernant le projet CRM.\n\nPoints abordés :\n- Budget confirmé\n- Timeline : 3 mois\n- Décideur : M. Dupont");
noteMap.put("Parent_Id", dealId);
noteMap.put("se_module", "Deals");

response = zoho.crm.createRecord("Notes", noteMap);
```

## Tâches et événements

### Créer une tâche

```deluge
taskMap = Map();
taskMap.put("Subject", "Préparer la proposition commerciale");
taskMap.put("Due_Date", zoho.currentdate.addDay(3).toString("yyyy-MM-dd"));
taskMap.put("Status", "Not Started");
taskMap.put("Priority", "High");
taskMap.put("Owner", ownerId);
taskMap.put("What_Id", dealId);     // Lié à un deal
taskMap.put("se_module", "Deals");
taskMap.put("Who_Id", contactId);   // Lié à un contact

response = zoho.crm.createRecord("Tasks", taskMap);
```

### Créer un événement

```deluge
eventMap = Map();
eventMap.put("Event_Title", "Rendez-vous client - " + dealName);
eventMap.put("Start_DateTime", zoho.currentdate.addDay(2).toString("yyyy-MM-dd'T'10:00:00+01:00"));
eventMap.put("End_DateTime", zoho.currentdate.addDay(2).toString("yyyy-MM-dd'T'11:00:00+01:00"));
eventMap.put("What_Id", dealId);
eventMap.put("se_module", "Deals");
eventMap.put("Location", "Bureau client - Paris");
eventMap.put("Description", "Présentation de la proposition");

// Ajouter des participants
participants = List();
participant1 = Map();
participant1.put("participant", contactId);
participant1.put("type", "contact");
participants.add(participant1);
eventMap.put("Participants", participants);

response = zoho.crm.createRecord("Events", eventMap);
```

## Opérations en masse (Bulk)

### Créer en masse

```deluge
// Créer plusieurs enregistrements d'un coup (max 100)
records = List();

for i = 1 to 10
{
    record = Map();
    record.put("Last_Name", "Lead " + i);
    record.put("Company", "Company " + i);
    record.put("Email", "lead" + i + "@example.com");
    records.add(record);
}

response = zoho.crm.bulkCreate("Leads", records);
info response;
```

### Mettre à jour en masse

```deluge
// Mettre à jour plusieurs records
updates = List();

for each deal in dealsToUpdate
{
    update = Map();
    update.put("id", deal.get("id"));
    update.put("Stage", "Fermée perdue");
    update.put("Reason_for_Loss", "Inactif depuis 6 mois");
    updates.add(update);
}

response = zoho.crm.bulkUpdate("Deals", updates);
```

## Upsert (Créer ou mettre à jour)

```deluge
// Si l'enregistrement existe (basé sur un champ unique), met à jour
// Sinon, crée un nouvel enregistrement

data = Map();
data.put("Email", "jean@acme.com");  // Champ unique pour le matching
data.put("Last_Name", "Dupont");
data.put("First_Name", "Jean-Pierre"); // Mise à jour du prénom
data.put("Company", "Acme Corp");

// Le 3ème paramètre spécifie le(s) champ(s) de déduplication
response = zoho.crm.upsert("Leads", data, {"Email"});
info response;
```

## Métadonnées

```deluge
// Obtenir les champs d'un module
fields = zoho.crm.getFields("Deals");
for each field in fields
{
    info field.get("api_name") + " (" + field.get("data_type") + ")";
}

// Obtenir les utilisateurs
users = zoho.crm.getUsers("ActiveUsers", 1, 50);
for each user in users
{
    info user.get("full_name") + " - " + user.get("email");
}

// Obtenir l'organisation
org = zoho.crm.getOrgDetails();
info org.get("company_name");
```

## Patterns utiles

### Trouver ou créer un compte

```deluge
// Chercher un compte par nom, créer s'il n'existe pas
accountName = "Acme Corp";

criteria = "(Account_Name:equals:" + accountName + ")";
accounts = zoho.crm.searchRecords("Accounts", criteria, 1, 1);

if(accounts.size() > 0)
{
    accountId = accounts.get(0).get("id");
    info "Compte trouvé : " + accountId;
}
else
{
    newAccount = Map();
    newAccount.put("Account_Name", accountName);
    newAccount.put("Industry", "Technology");
    
    response = zoho.crm.createRecord("Accounts", newAccount);
    accountId = response.get("id");
    info "Compte créé : " + accountId;
}
```

### Dupliquer un enregistrement

```deluge
// Copier un deal
source = zoho.crm.getRecordById("Deals", sourceDealId);

newDeal = Map();
newDeal.put("Deal_Name", source.get("Deal_Name") + " (copie)");
newDeal.put("Stage", "Qualification");
newDeal.put("Amount", source.get("Amount"));
newDeal.put("Account_Name", source.get("Account_Name").get("id"));
newDeal.put("Contact_Name", source.get("Contact_Name").get("id"));
newDeal.put("Closing_Date", zoho.currentdate.addMonth(3).toString("yyyy-MM-dd"));

response = zoho.crm.createRecord("Deals", newDeal);
info "Deal dupliqué : " + response.get("id");
```

### Calculer des agrégations

```deluge
// Somme des deals d'un compte
deals = zoho.crm.getRelatedRecords("Deals", "Accounts", accountId);
totalOuvert = 0;
totalGagne = 0;
count = 0;

for each deal in deals
{
    amount = ifnull(deal.get("Amount"), 0);
    stage = ifnull(deal.get("Stage"), "");
    
    if(stage == "Fermée gagnée")
    {
        totalGagne = totalGagne + amount;
    }
    else if(stage != "Fermée perdue")
    {
        totalOuvert = totalOuvert + amount;
        count = count + 1;
    }
}

info "Deals ouverts : " + count + " pour " + totalOuvert + "€";
info "Total gagné : " + totalGagne + "€";

// Mettre à jour le compte avec les totaux
updateMap = Map();
updateMap.put("Pipeline_ouvert", totalOuvert);
updateMap.put("CA_total", totalGagne);
zoho.crm.updateRecord("Accounts", accountId, updateMap);
```

## Limites

| Limite | Valeur |
|--------|--------|
| getRecords per_page max | 200 |
| searchRecords per_page max | 200 |
| bulkCreate max records | 100 |
| bulkUpdate max records | 100 |
| API calls par exécution | ~25 (dépend du contexte) |
| Recherche : critères max | 10 |

## Bonnes pratiques

1. **Batch** : Utiliser `bulkCreate`/`bulkUpdate` plutôt que des boucles de `createRecord`
2. **Pagination** : Toujours paginer avec `getRecords`
3. **Null checks** : Vérifier les retours null (`getRecordById` peut retourner null)
4. **Triggers** : Contrôler les triggers pour éviter les boucles infinies
5. **Logs** : Logger les opérations critiques (création, suppression)
6. **ID** : Stocker les IDs comme String (trop grands pour des entiers)

---
*Voir aussi : [api-calls.md](api-calls.md) pour invokeurl, [exemples.md](exemples.md) pour des scripts complets.*
