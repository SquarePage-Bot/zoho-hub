# 🧪 Zoho Deluge - Exemples Complets

> Scripts prêts à l'emploi pour les cas d'usage courants.

## 1. Attribution automatique des leads (Round Robin)

```deluge
// Trigger : Workflow sur création de Lead
// Paramètre : leadId

// Liste des commerciaux (IDs)
commerciaux = List();
commerciaux.add("5234876000000111001"); // Alice
commerciaux.add("5234876000000111002"); // Bob
commerciaux.add("5234876000000111003"); // Claire

// Récupérer le compteur actuel (stocké dans un module custom ou variable org)
// Alternative simple : basé sur l'ID du lead (modulo)
lead = zoho.crm.getRecordById("Leads", leadId);
if(lead == null) { return; }

// Round robin basé sur le dernier chiffre de l'ID
lastDigit = leadId.subString(leadId.length() - 1).toLong();
index = lastDigit % commerciaux.size();
assignTo = commerciaux.get(index.toNumber());

// Assigner
updateMap = Map();
updateMap.put("Owner", assignTo);
zoho.crm.updateRecord("Leads", leadId, updateMap);

info "Lead " + leadId + " assigné à l'index " + index;
```

## 2. Enrichissement de lead via API externe

```deluge
// Enrichir un lead avec des données d'entreprise
// Trigger : Workflow sur création de Lead
// Paramètre : leadId

lead = zoho.crm.getRecordById("Leads", leadId);
if(lead == null) { return; }

email = ifnull(lead.get("Email"), "");
if(email == "") { return; }

// Extraire le domaine
domain = email.getSuffix("@");
if(domain.contains("gmail.com") || domain.contains("yahoo.com") || domain.contains("hotmail.com"))
{
    info "Email personnel, pas d'enrichissement";
    return;
}

try
{
    // Appel API Clearbit (exemple)
    response = invokeurl
    [
        url: "https://company.clearbit.com/v2/companies/find?domain=" + domain
        type: GET
        headers: {"Authorization": "Bearer sk_clearbit_xxx"}
        detailed: true
    ];
    
    if(response.get("statusCode") == 200)
    {
        data = response.get("responseText").toMap();
        
        updateMap = Map();
        
        companyName = data.get("name");
        if(companyName != null)
        {
            updateMap.put("Company", companyName);
        }
        
        industry = data.get("category").get("industry");
        if(industry != null)
        {
            updateMap.put("Industry", industry);
        }
        
        employees = data.get("metrics").get("employees");
        if(employees != null)
        {
            updateMap.put("No_of_Employees", employees);
        }
        
        if(updateMap.size() > 0)
        {
            zoho.crm.updateRecord("Leads", leadId, updateMap);
            info "Lead enrichi avec " + updateMap.size() + " champs";
        }
    }
}
catch(e)
{
    info "Erreur enrichissement : " + e.getMessage();
}
```

## 3. Création automatique de devis depuis un deal

```deluge
// Trigger : Bouton custom sur le deal
// Paramètre : dealId

deal = zoho.crm.getRecordById("Deals", dealId);
if(deal == null)
{
    return {"status": "error", "message": "Deal non trouvé"};
}

// Récupérer les infos nécessaires
accountId = deal.get("Account_Name").get("id");
contactId = deal.get("Contact_Name").get("id");

// Créer le devis
quoteMap = Map();
quoteMap.put("Subject", "Devis - " + deal.get("Deal_Name"));
quoteMap.put("Deal_Name", dealId);
quoteMap.put("Account_Name", accountId);
quoteMap.put("Contact_Name", contactId);
quoteMap.put("Valid_Till", zoho.currentdate.addDay(30).toString("yyyy-MM-dd"));
quoteMap.put("Terms_and_Conditions", "Validité 30 jours. Paiement à 30 jours.");

// Copier les lignes de produits du deal
productDetails = deal.get("Product_Details");
if(productDetails != null && productDetails.size() > 0)
{
    quoteMap.put("Product_Details", productDetails);
}

response = zoho.crm.createRecord("Quotes", quoteMap);
quoteId = response.get("id");

// Mettre à jour le deal avec le lien vers le devis
updateDeal = Map();
updateDeal.put("Quote_Id", quoteId); // Si champ custom existant
zoho.crm.updateRecord("Deals", dealId, updateDeal);

info "Devis créé : " + quoteId;
return {"status": "success", "quote_id": quoteId};
```

## 4. Rapport email hebdomadaire

```deluge
// Trigger : Action planifiée (Schedule) - Chaque lundi à 8h
// Pas de paramètre

lastMonday = zoho.currentdate.subDay(7).toString("yyyy-MM-dd");
today = zoho.currentdate.toString("yyyy-MM-dd");

// --- Deals gagnés ---
criteria = "(Stage:equals:Fermée gagnée)and(Closing_Date:greater_equal:" + lastMonday + ")and(Closing_Date:less_equal:" + today + ")";
wonDeals = zoho.crm.searchRecords("Deals", criteria, 1, 200);
wonTotal = 0;
wonDetails = "";
for each d in wonDeals
{
    amount = ifnull(d.get("Amount"), 0);
    wonTotal = wonTotal + amount;
    wonDetails = wonDetails + "<tr><td>" + d.get("Deal_Name") + "</td><td>" + amount + "€</td><td>" + ifnull(d.get("Owner").get("name"), "") + "</td></tr>";
}

// --- Deals perdus ---
criteria = "(Stage:equals:Fermée perdue)and(Closing_Date:greater_equal:" + lastMonday + ")";
lostDeals = zoho.crm.searchRecords("Deals", criteria, 1, 200);
lostTotal = 0;
for each d in lostDeals
{
    lostTotal = lostTotal + ifnull(d.get("Amount"), 0);
}

// --- Nouveaux leads ---
criteria = "(Created_Time:greater_equal:" + lastMonday + "T00:00:00+01:00)";
newLeads = zoho.crm.searchRecords("Leads", criteria, 1, 200);

// --- Pipeline ouvert ---
criteria = "(Stage:not_equal:Fermée gagnée)and(Stage:not_equal:Fermée perdue)";
pipeline = zoho.crm.searchRecords("Deals", criteria, 1, 200);
pipelineTotal = 0;
for each d in pipeline
{
    pipelineTotal = pipelineTotal + ifnull(d.get("Amount"), 0);
}

// --- Email HTML ---
html = "<html><body style='font-family:Arial,sans-serif;'>";
html = html + "<h1>📊 Rapport Commercial Hebdomadaire</h1>";
html = html + "<p>Semaine du " + lastMonday + " au " + today + "</p>";

html = html + "<h2>📈 Résumé</h2>";
html = html + "<table border='1' cellpadding='8' style='border-collapse:collapse;'>";
html = html + "<tr><td><strong>🏆 Deals gagnés</strong></td><td>" + wonDeals.size() + " deals</td><td><strong>" + wonTotal + "€</strong></td></tr>";
html = html + "<tr><td><strong>❌ Deals perdus</strong></td><td>" + lostDeals.size() + " deals</td><td>" + lostTotal + "€</td></tr>";
html = html + "<tr><td><strong>📥 Nouveaux leads</strong></td><td colspan='2'>" + newLeads.size() + " leads</td></tr>";
html = html + "<tr><td><strong>💼 Pipeline ouvert</strong></td><td>" + pipeline.size() + " deals</td><td>" + pipelineTotal + "€</td></tr>";
html = html + "</table>";

if(wonDeals.size() > 0)
{
    html = html + "<h2>🏆 Deals Gagnés</h2>";
    html = html + "<table border='1' cellpadding='8' style='border-collapse:collapse;'>";
    html = html + "<tr><th>Deal</th><th>Montant</th><th>Commercial</th></tr>";
    html = html + wonDetails;
    html = html + "</table>";
}

html = html + "</body></html>";

sendmail
[
    from: zoho.adminuserid
    to: "direction@squarepage.fr"
    cc: "commercial@squarepage.fr"
    subject: "📊 Rapport commercial - Semaine du " + lastMonday
    message: html
    content_type: "text/html"
];

info "Rapport envoyé";
```

## 5. Détection et fusion de doublons

```deluge
// Trigger : Action planifiée quotidienne
// Objectif : Identifier les leads avec le même email

info "=== Détection de doublons ===";

leads = zoho.crm.searchRecords("Leads", "(Email:is not:null)", 1, 200);
emailMap = Map(); // email → liste d'IDs
doublons = List();

for each lead in leads
{
    email = lead.get("Email").toLowerCase().trim();
    
    if(emailMap.containsKey(email))
    {
        emailMap.get(email).add(lead.get("id"));
        if(!doublons.contains(email))
        {
            doublons.add(email);
        }
    }
    else
    {
        ids = List();
        ids.add(lead.get("id"));
        emailMap.put(email, ids);
    }
}

info "Doublons trouvés : " + doublons.size();

// Créer un rapport
rapport = "";
for each email in doublons
{
    ids = emailMap.get(email);
    rapport = rapport + "📧 " + email + " : " + ids.size() + " leads (IDs: " + ids.toString(", ") + ")\n";
    
    // Optionnel : tagger les doublons
    for each id in ids
    {
        tagList = List();
        tagList.add("Doublon potentiel");
        zoho.crm.addTags("Leads", id, tagList);
    }
}

if(doublons.size() > 0)
{
    sendmail
    [
        from: zoho.adminuserid
        to: "admin@squarepage.fr"
        subject: "⚠️ " + doublons.size() + " doublons détectés dans les leads"
        message: rapport
    ];
}
```

## 6. Synchronisation CRM → Google Sheets

```deluge
// Exporter les deals du mois en cours vers Google Sheets
// Trigger : Action planifiée mensuelle

sheetId = "1ABC_SHEET_ID_XYZ";
sheetName = "Deals " + zoho.currentdate.toString("yyyy-MM");

// Récupérer les deals
firstDay = zoho.currentdate.toString("yyyy-MM") + "-01";
criteria = "(Created_Time:greater_equal:" + firstDay + "T00:00:00+01:00)";
deals = zoho.crm.searchRecords("Deals", criteria, 1, 200);

// Préparer les données
values = List();

// Header
header = List();
header.add("Nom");
header.add("Montant");
header.add("Étape");
header.add("Compte");
header.add("Date de clôture");
header.add("Commercial");
values.add(header);

// Données
for each deal in deals
{
    row = List();
    row.add(ifnull(deal.get("Deal_Name"), ""));
    row.add(ifnull(deal.get("Amount"), 0).toString());
    row.add(ifnull(deal.get("Stage"), ""));
    
    accountName = "";
    if(deal.get("Account_Name") != null)
    {
        accountName = deal.get("Account_Name").get("name");
    }
    row.add(accountName);
    
    row.add(ifnull(deal.get("Closing_Date"), ""));
    
    ownerName = "";
    if(deal.get("Owner") != null)
    {
        ownerName = deal.get("Owner").get("name");
    }
    row.add(ownerName);
    
    values.add(row);
}

// Envoyer à Google Sheets
body = Map();
body.put("values", values);

response = invokeurl
[
    url: "https://sheets.googleapis.com/v4/spreadsheets/" + sheetId + "/values/" + encodeUrl(sheetName + "!A1") + "?valueInputOption=USER_ENTERED"
    type: PUT
    headers: {"Content-Type": "application/json"}
    parameters: body.toString()
    connection: "google_sheets_conn"
];

info "Export terminé : " + deals.size() + " deals exportés";
```

## 7. Chatbot Webhook (réception)

```deluge
// Fonction appelée par un webhook (ex: chatbot site web)
// Paramètre : requestBody (String - JSON du webhook)

data = requestBody.toMap();

visitorName = ifnull(data.get("name"), "Inconnu");
visitorEmail = ifnull(data.get("email"), "");
visitorMessage = ifnull(data.get("message"), "");
visitorPage = ifnull(data.get("page"), "");

info "Chat reçu de : " + visitorName + " (" + visitorEmail + ")";

// Chercher si le contact/lead existe
contactId = null;
if(visitorEmail != "")
{
    contacts = zoho.crm.searchRecords("Contacts", "(Email:equals:" + visitorEmail + ")", 1, 1);
    if(contacts.size() > 0)
    {
        contactId = contacts.get(0).get("id");
        info "Contact existant trouvé : " + contactId;
    }
    else
    {
        // Chercher dans les leads
        leads = zoho.crm.searchRecords("Leads", "(Email:equals:" + visitorEmail + ")", 1, 1);
        if(leads.size() > 0)
        {
            info "Lead existant trouvé : " + leads.get(0).get("id");
        }
        else
        {
            // Créer un nouveau lead
            leadMap = Map();
            leadMap.put("Last_Name", visitorName);
            leadMap.put("Email", visitorEmail);
            leadMap.put("Lead_Source", "Chat en ligne");
            leadMap.put("Description", "Premier message : " + visitorMessage);
            
            response = zoho.crm.createRecord("Leads", leadMap);
            info "Nouveau lead créé : " + response.get("id");
        }
    }
}

// Si contact existant, ajouter une note
if(contactId != null)
{
    noteMap = Map();
    noteMap.put("Note_Title", "Chat - " + zoho.currentdate.toString("dd/MM/yyyy"));
    noteMap.put("Note_Content", "Message du chat :\n" + visitorMessage + "\n\nPage : " + visitorPage);
    noteMap.put("Parent_Id", contactId);
    noteMap.put("se_module", "Contacts");
    zoho.crm.createRecord("Notes", noteMap);
}

// Retourner une réponse au chatbot
returnMap = Map();
returnMap.put("status", "success");
returnMap.put("existing_contact", contactId != null);
return returnMap.toString();
```

## 8. Pipeline health check

```deluge
// Vérification quotidienne de la santé du pipeline
// Trigger : Action planifiée quotidienne à 9h

alertes = List();
today = zoho.currentdate;

// 1. Deals sans activité depuis 14 jours
twoWeeksAgo = today.subDay(14).toString("yyyy-MM-dd");
criteria = "(Modified_Time:less_than:" + twoWeeksAgo + "T00:00:00+01:00)and(Stage:not_equal:Fermée gagnée)and(Stage:not_equal:Fermée perdue)";
staleDeals = zoho.crm.searchRecords("Deals", criteria, 1, 100);

if(staleDeals.size() > 0)
{
    msg = "⚠️ " + staleDeals.size() + " deals inactifs depuis 14+ jours :";
    for each d in staleDeals
    {
        msg = msg + "\n  • " + d.get("Deal_Name") + " (" + ifnull(d.get("Amount"), 0) + "€) - " + ifnull(d.get("Owner").get("name"), "?");
    }
    alertes.add(msg);
}

// 2. Deals avec date de clôture dépassée
criteria = "(Closing_Date:less_than:" + today.toString("yyyy-MM-dd") + ")and(Stage:not_equal:Fermée gagnée)and(Stage:not_equal:Fermée perdue)";
overdueDeals = zoho.crm.searchRecords("Deals", criteria, 1, 100);

if(overdueDeals.size() > 0)
{
    msg = "🔴 " + overdueDeals.size() + " deals avec date de clôture dépassée :";
    for each d in overdueDeals
    {
        msg = msg + "\n  • " + d.get("Deal_Name") + " (clôture prévue : " + d.get("Closing_Date") + ")";
    }
    alertes.add(msg);
}

// 3. Leads non contactés depuis 48h
twoDaysAgo = today.subDay(2).toString("yyyy-MM-dd");
criteria = "(Lead_Status:equals:Non contacté)and(Created_Time:less_than:" + twoDaysAgo + "T00:00:00+01:00)";
oldLeads = zoho.crm.searchRecords("Leads", criteria, 1, 100);

if(oldLeads.size() > 0)
{
    msg = "📥 " + oldLeads.size() + " leads non contactés depuis 48h+";
    alertes.add(msg);
}

// Envoyer le rapport si des alertes existent
if(alertes.size() > 0)
{
    rapport = "🏥 Health Check Pipeline - " + today.toString("dd/MM/yyyy") + "\n\n";
    for each alerte in alertes
    {
        rapport = rapport + alerte + "\n\n";
    }
    
    sendmail
    [
        from: zoho.adminuserid
        to: "manager@squarepage.fr"
        subject: "🏥 Pipeline Health Check - " + alertes.size() + " alerte(s)"
        message: rapport
    ];
    
    info "Health check envoyé avec " + alertes.size() + " alertes";
}
else
{
    info "✅ Pipeline sain, aucune alerte";
}
```

## 9. Widget : Formulaire de saisie rapide

```deluge
// Fonction appelée par un widget JS côté client
// Paramètres : contact_name, contact_email, deal_name, deal_amount

// Validation
if(contact_name == null || contact_name == "")
{
    return '{"status":"error","message":"Le nom du contact est obligatoire"}';
}
if(contact_email == null || !contact_email.contains("@"))
{
    return '{"status":"error","message":"Email invalide"}';
}

try
{
    // Chercher ou créer le compte (basé sur le domaine email)
    domain = contact_email.getSuffix("@");
    criteria = "(Website:contains:" + domain + ")";
    accounts = zoho.crm.searchRecords("Accounts", criteria, 1, 1);
    
    accountId = null;
    if(accounts.size() > 0)
    {
        accountId = accounts.get(0).get("id");
    }
    else
    {
        accountMap = Map();
        accountMap.put("Account_Name", domain.getPrefix(".").toStartCase());
        accountMap.put("Website", "https://" + domain);
        resp = zoho.crm.createRecord("Accounts", accountMap);
        accountId = resp.get("id");
    }
    
    // Créer le contact
    contactMap = Map();
    nameParts = contact_name.toList(" ");
    if(nameParts.size() > 1)
    {
        contactMap.put("First_Name", nameParts.get(0));
        contactMap.put("Last_Name", nameParts.subList(1, nameParts.size()).toString(" "));
    }
    else
    {
        contactMap.put("Last_Name", contact_name);
    }
    contactMap.put("Email", contact_email);
    contactMap.put("Account_Name", accountId);
    
    contactResp = zoho.crm.createRecord("Contacts", contactMap);
    contactId = contactResp.get("id");
    
    // Créer le deal si demandé
    dealId = null;
    if(deal_name != null && deal_name != "")
    {
        dealMap = Map();
        dealMap.put("Deal_Name", deal_name);
        dealMap.put("Amount", ifnull(deal_amount, "0").toDecimal());
        dealMap.put("Stage", "Qualification");
        dealMap.put("Closing_Date", zoho.currentdate.addMonth(3).toString("yyyy-MM-dd"));
        dealMap.put("Account_Name", accountId);
        dealMap.put("Contact_Name", contactId);
        
        dealResp = zoho.crm.createRecord("Deals", dealMap);
        dealId = dealResp.get("id");
    }
    
    result = Map();
    result.put("status", "success");
    result.put("contact_id", contactId);
    result.put("account_id", accountId);
    result.put("deal_id", dealId);
    return result.toString();
}
catch(e)
{
    return '{"status":"error","message":"' + e.getMessage() + '"}';
}
```

---
*Ces exemples appliquent les patterns de [bonnes-pratiques.md](bonnes-pratiques.md). Adapter les IDs et noms de champs à votre configuration.*
