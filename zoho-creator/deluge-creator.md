# 🔧 Zoho Creator - Deluge spécifique

> Fonctions et syntaxes Deluge propres à Zoho Creator.

## Table des matières

- [Variables intégrées](#variables-intégrées)
- [Opérations CRUD](#opérations-crud)
- [Sous-formulaires](#sous-formulaires)
- [Manipulation de fichiers](#manipulation-de-fichiers)
- [Intégrations inter-apps](#intégrations-inter-apps)
- [Fonctions personnalisées](#fonctions-personnalisées)
- [Gestion d'erreurs](#gestion-derreurs)

---

## Variables intégrées

### Variables système

| Variable | Description | Exemple |
|----------|-------------|---------|
| `zoho.currentdate` | Date du jour | 18-Feb-2026 |
| `zoho.currenttime` | Date et heure actuelles | 18-Feb-2026 16:30:00 |
| `zoho.loginuser` | Email de l'utilisateur connecté | user@entreprise.com |
| `zoho.loginuserid` | ID de l'utilisateur connecté | 3860000000012345 |
| `zoho.loginuserrole` | Rôle de l'utilisateur | Admin |
| `zoho.adminuserid` | Email de l'admin de l'app | admin@entreprise.com |
| `zoho.appname` | Nom de l'application | mon-app |
| `zoho.appuri` | URI de l'application | /owner/mon-app |
| `zoho.ipaddress` | IP de l'utilisateur | 192.168.1.1 |

### Variables de formulaire

| Variable | Contexte | Description |
|----------|----------|-------------|
| `input.Champ` | On Add/Edit/Validate | Valeur du champ en cours de saisie |
| `input.Champ_old` | On Edit | Ancienne valeur avant modification |
| `input.ID` | Tous | ID de l'enregistrement |

---

## Opérations CRUD

### Create (Insertion)

```deluge
// Méthode 1 : insert into
newRecord = Map();
newRecord.put("Nom", "Jean Dupont");
newRecord.put("Email", "jean@acme.com");
newRecord.put("Statut", "Actif");
newRecord.put("Date_Creation", zoho.currentdate);
result = insert into Clients values newRecord;
newId = result.ID;  // Récupérer l'ID créé

// Méthode 2 : create record (syntaxe API interne)
response = zoho.creator.createRecord(
    "owner",       // propriétaire
    "mon-app",     // application
    "Clients",     // formulaire
    newRecord      // données
);
```

### Read (Lecture)

```deluge
// Tous les enregistrements
tous = Clients[Statut == "Actif"];

// Avec critères multiples
filtres = Clients[Statut == "Actif" && Ville == "Paris" && CA > 10000];

// Par ID
client = Clients[ID == 3860000000012345];

// Premier enregistrement
premier = Clients[Statut == "Nouveau"].first();

// Dernier enregistrement
dernier = Clients[Date_Creation != null].last();

// Trier les résultats
tries = Clients[Statut == "Actif"].sort(Nom, true);  // true = ascendant

// Limiter les résultats
topDix = Clients[Statut == "Actif"].sort(CA, false).limit(10);

// Recherche textuelle (contient)
resultats = Clients[Nom.contains("Dup")];

// Recherche avec startsWith
resultats = Clients[Code_Postal.startsWith("75")];
```

### Update (Mise à jour)

```deluge
// Mise à jour d'un enregistrement
client = Clients[ID == input.Client_ID];
client.Statut = "Premium";
client.Date_Upgrade = zoho.currentdate;
client.Responsable = zoho.loginuser;

// Mise à jour en masse
inactifs = Clients[Derniere_Connexion < zoho.currentdate.subMonth(12)];
for each client in inactifs
{
    client.Statut = "Inactif";
}

// Mise à jour conditionnelle
record = Commandes[ID == input.ID];
if (record.Statut == "En attente")
{
    record.Statut = "Validée";
    record.Date_Validation = zoho.currentdate;
}
```

### Delete (Suppression)

```deluge
// Supprimer un enregistrement
record = Archives[ID == input.Archive_ID];
delete record;

// Supprimer en masse
anciens = Logs[Date < zoho.currentdate.subYear(1)];
for each log in anciens
{
    delete log;
}
```

---

## Sous-formulaires

### Lecture

```deluge
// Accéder aux lignes d'un sous-formulaire
commande = Commandes[ID == commandeId];
lignes = commande.Lignes_Commande;  // Collection de sous-enregistrements

totalHT = 0;
for each ligne in lignes
{
    totalHT = totalHT + (ligne.Quantite * ligne.Prix_Unitaire);
    info ligne.Produit + " : " + ligne.Quantite + " x " + ligne.Prix_Unitaire;
}
```

### Ajout de lignes

```deluge
// Ajouter une ligne à un sous-formulaire existant
row = Lignes_Commande();
row.Produit = "Widget Pro";
row.Quantite = 5;
row.Prix_Unitaire = 29.99;
row.Remise = 10;

commande = Commandes[ID == commandeId];
commande.Lignes_Commande.add(row);
```

### Modification de lignes

```deluge
lignes = input.Lignes_Commande;
for each ligne in lignes
{
    // Appliquer une remise de 10% sur toutes les lignes
    ligne.Prix_Unitaire = ligne.Prix_Unitaire * 0.90;
    ligne.Total_Ligne = ligne.Quantite * ligne.Prix_Unitaire;
}
```

---

## Manipulation de fichiers

### Upload et lecture

```deluge
// Récupérer un fichier uploadé
fichier = input.Document;
nomFichier = fichier.getFileName();
tailleFichier = fichier.getFileSize();

// Télécharger un fichier depuis une URL
contenu = invokeurl
[
    url: "https://example.com/fichier.pdf"
    type: GET
];

// Attacher un fichier à un enregistrement
record = Documents[ID == docId];
record.Fichier = contenu;
```

### Génération de PDF

```deluge
// Générer un PDF à partir d'un template
pdfContent = zoho.creator.generateDocument(
    "mon-app",
    "Template_Facture",
    input.ID
);

// Envoyer le PDF par email
sendmail
[
    from: zoho.adminuserid
    to: input.Email_Client
    subject: "Votre facture " + input.Numero
    message: "Veuillez trouver ci-joint votre facture."
    attachments: pdfContent
];
```

---

## Intégrations inter-apps

### Creator → CRM

```deluge
// Rechercher un contact CRM
contacts = zoho.crm.searchRecords("Contacts", "(Email:equals:" + input.Email + ")");
if (contacts.size() > 0)
{
    contact = contacts.get(0);
    input.CRM_Contact_ID = contact.get("id");
    input.Entreprise = contact.get("Account_Name").get("name");
}

// Créer un deal CRM
dealMap = Map();
dealMap.put("Deal_Name", input.Nom_Projet);
dealMap.put("Stage", "Qualification");
dealMap.put("Amount", input.Budget);
dealMap.put("Contact_Name", input.CRM_Contact_ID);
dealMap.put("Closing_Date", input.Date_Prevue.toString("yyyy-MM-dd"));
response = zoho.crm.createRecord("Deals", dealMap);
dealId = response.get("id");
```

### Creator → Books

```deluge
// Créer une facture Zoho Books
invoiceMap = Map();
invoiceMap.put("customer_id", input.Books_Customer_ID);
invoiceMap.put("date", zoho.currentdate.toString("yyyy-MM-dd"));
invoiceMap.put("due_date", zoho.currentdate.addDay(30).toString("yyyy-MM-dd"));
invoiceMap.put("reference_number", input.Numero);

lineItems = List();
for each ligne in input.Lignes
{
    item = Map();
    item.put("name", ligne.Designation);
    item.put("rate", ligne.Prix_Unitaire);
    item.put("quantity", ligne.Quantite);
    item.put("tax_id", "TVA_20");
    lineItems.add(item);
}
invoiceMap.put("line_items", lineItems);

response = invokeurl
[
    url: "https://www.zohoapis.eu/books/v3/invoices?organization_id=" + orgId
    type: POST
    headers: {"Content-Type": "application/json"}
    parameters: invoiceMap.toString()
    connection: "zoho_books"
];
```

### Creator → Desk

```deluge
// Créer un ticket support
ticketMap = Map();
ticketMap.put("subject", input.Sujet);
ticketMap.put("description", input.Description);
ticketMap.put("email", input.Email_Client);
ticketMap.put("departmentId", "DEPT_ID");
ticketMap.put("priority", input.Priorite);
ticketMap.put("channel", "Web");

response = invokeurl
[
    url: "https://desk.zoho.eu/api/v1/tickets"
    type: POST
    headers: {"Content-Type": "application/json"}
    parameters: ticketMap.toString()
    connection: "zoho_desk"
];
ticketId = response.get("id");
input.Ticket_Desk_ID = ticketId;
```

---

## Fonctions personnalisées

### Déclaration

```deluge
// Fonction réutilisable dans l'application
// Paramètres : clientId (bigint), montant (decimal)
// Retour : string

string calculerRemise(bigint clientId, decimal montant)
{
    client = Clients[ID == clientId];
    remise = 0;

    if (client.Type == "Premium")
    {
        if (montant > 10000)
        {
            remise = 15;
        }
        else if (montant > 5000)
        {
            remise = 10;
        }
        else
        {
            remise = 5;
        }
    }
    else if (client.Type == "Standard" && montant > 10000)
    {
        remise = 5;
    }

    montantFinal = montant * (1 - remise / 100);
    return "Remise: " + remise + "% - Montant final: " + montantFinal + "€";
}
```

### Appel de la fonction

```deluge
resultat = calculerRemise(input.Client_ID, input.Montant_HT);
info resultat;
```

---

## Gestion d'erreurs

### Try-Catch

```deluge
try
{
    response = invokeurl
    [
        url: "https://api.externe.com/data"
        type: GET
        connection: "api_externe"
    ];

    if (response.get("responseCode") == 200)
    {
        // Traitement OK
        input.Donnee_Externe = response.get("data");
    }
    else
    {
        // Erreur API
        info "Erreur API : " + response.toString();
        input.Statut_Sync = "Erreur";
    }
}
catch (e)
{
    info "Exception : " + e.toString();
    input.Statut_Sync = "Erreur";

    // Logger l'erreur
    logMap = Map();
    logMap.put("Date", zoho.currenttime);
    logMap.put("Source", "Sync API externe");
    logMap.put("Message", e.toString());
    logMap.put("Record_ID", input.ID);
    insert into Logs_Erreurs values logMap;
}
```

### Vérifications courantes

```deluge
// Vérifier qu'un record existe avant d'y accéder
record = Clients[ID == clientId];
if (record != null && record.count() > 0)
{
    // Traitement
}
else
{
    alert "Client introuvable";
}

// Vérifier la réponse d'une API
if (response != null && response.containsKey("id"))
{
    info "Succès : " + response.get("id");
}
else
{
    info "Échec : " + response.toString();
}
```

---

## Bonnes pratiques

1. **Nommer les variables** de manière explicite (`totalFactureHT` > `t`)
2. **Commenter le code** pour la maintenance future
3. **Logger les erreurs** dans un formulaire dédié
4. **Limiter les boucles** : éviter les boucles imbriquées sur de gros volumes
5. **Utiliser des fonctions** pour le code réutilisable
6. **Tester unitairement** chaque fonction dans l'éditeur Deluge

---

*Voir aussi : [../../zoho-deluge/](../../zoho-deluge/) pour la syntaxe Deluge complète | [workflows.md](workflows.md) | [api.md](api.md)*
