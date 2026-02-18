# ✅ Zoho Deluge - Bonnes Pratiques

> Patterns, anti-patterns, optimisation et debug.

## Structure du code

### Organisation

```deluge
// ===================================
// FONCTION : Qualification automatique des leads
// DÉCLENCHEUR : Workflow - Création de lead
// PARAMÈTRE : leadId (String)
// AUTEUR : SquareBot
// DATE : 2026-02-18
// ===================================

// --- 1. RÉCUPÉRATION DES DONNÉES ---
lead = zoho.crm.getRecordById("Leads", leadId);
if(lead == null)
{
    info "❌ Lead non trouvé : " + leadId;
    return;
}
info "📋 Lead : " + lead.get("Last_Name") + " - " + lead.get("Company");

// --- 2. LOGIQUE MÉTIER ---
score = 0;
// ... calcul ...

// --- 3. MISE À JOUR ---
updateMap = Map();
updateMap.put("Custom_Score", score);
zoho.crm.updateRecord("Leads", leadId, updateMap);

// --- 4. ACTIONS POST ---
// Notifications, tâches, etc.

info "✅ Terminé. Score : " + score;
```

### Nommage

| Élément | Convention | Exemple |
|---------|-----------|---------|
| Variables | camelCase | `dealAmount`, `contactEmail` |
| Constantes | UPPER_SNAKE | `MAX_RETRIES = 3` |
| Fonctions | PascalCase descriptif | `CalculerScoreLead` |
| Connexions | snake_case | `zoho_crm_conn` |

## Gestion des erreurs

### Pattern try/catch systématique

```deluge
// ❌ MAUVAIS : pas de gestion d'erreur
response = invokeurl[url: apiUrl type: GET];
data = response.toMap();

// ✅ BON : gestion complète
try
{
    response = invokeurl
    [
        url: apiUrl
        type: GET
        headers: {"Authorization": "Bearer " + token}
        detailed: true
    ];
    
    statusCode = response.get("statusCode");
    if(statusCode == 200)
    {
        data = response.get("responseText").toMap();
        // Traiter...
    }
    else
    {
        info "⚠️ API erreur " + statusCode + " : " + response.get("responseText");
    }
}
catch(e)
{
    info "❌ Exception : " + e.getMessage();
}
```

### Null safety

```deluge
// ❌ MAUVAIS : crash si null
email = contact.get("Email").toLowerCase();

// ✅ BON : vérification
email = contact.get("Email");
if(email != null && email != "")
{
    email = email.toLowerCase();
}
else
{
    email = "";
}

// ✅ ENCORE MIEUX : ifnull
email = ifnull(contact.get("Email"), "").toLowerCase();
```

### Lookups sécurisés

```deluge
// ❌ MAUVAIS : crash si le lookup est null
accountName = deal.get("Account_Name").get("name");

// ✅ BON
accountLookup = deal.get("Account_Name");
if(accountLookup != null)
{
    accountName = accountLookup.get("name");
    accountId = accountLookup.get("id");
}
else
{
    accountName = "N/A";
    accountId = null;
}
```

## Performance

### Minimiser les appels API

```deluge
// ❌ MAUVAIS : un appel par record dans la boucle (N+1)
leads = zoho.crm.getRecords("Leads", 1, 100);
for each lead in leads
{
    updateMap = Map();
    updateMap.put("Status", "Traité");
    zoho.crm.updateRecord("Leads", lead.get("id"), updateMap);
    // 100 appels API !
}

// ✅ BON : bulk update
leads = zoho.crm.getRecords("Leads", 1, 100);
updates = List();
for each lead in leads
{
    update = Map();
    update.put("id", lead.get("id"));
    update.put("Status", "Traité");
    updates.add(update);
}
zoho.crm.bulkUpdate("Leads", updates);
// 1 seul appel API !
```

### Recherche ciblée

```deluge
// ❌ MAUVAIS : récupérer tout et filtrer en Deluge
allDeals = zoho.crm.getRecords("Deals", 1, 200);
for each deal in allDeals
{
    if(deal.get("Stage") == "Négociation" && deal.get("Amount") > 10000)
    {
        // Traiter...
    }
}

// ✅ BON : filtrer côté CRM
criteria = "(Stage:equals:Négociation)and(Amount:greater_than:10000)";
filteredDeals = zoho.crm.searchRecords("Deals", criteria, 1, 200);
for each deal in filteredDeals
{
    // Traiter directement les bons records
}
```

### Limiter les données récupérées

```deluge
// ❌ MAUVAIS : récupérer tout l'enregistrement pour un seul champ
deal = zoho.crm.getRecordById("Deals", dealId);
amount = deal.get("Amount");

// ✅ ACCEPTABLE : (Deluge ne supporte pas la sélection de champs natifs)
// Mais via API/connexion :
response = invokeurl
[
    url: "https://www.zohoapis.eu/crm/v6/Deals/" + dealId + "?fields=Amount,Stage"
    type: GET
    connection: "zoho_crm_conn"
];
```

## Anti-patterns courants

### 1. Boucle infinie

```deluge
// ❌ DANGER : un workflow met à jour un record, ce qui déclenche le même workflow
// Workflow sur Deals, trigger = modification
updateMap = Map();
updateMap.put("Custom_Field", "calculé");
zoho.crm.updateRecord("Deals", dealId, updateMap);
// → Boucle infinie !

// ✅ SOLUTION : vérifier avant de mettre à jour
deal = zoho.crm.getRecordById("Deals", dealId);
if(deal.get("Custom_Field") != "calculé")
{
    updateMap = Map();
    updateMap.put("Custom_Field", "calculé");
    zoho.crm.updateRecord("Deals", dealId, updateMap);
}

// ✅ ALTERNATIVE : désactiver les triggers
optMap = Map();
optMap.put("trigger", List()); // Aucun trigger
zoho.crm.updateRecord("Deals", dealId, updateMap, optMap);
```

### 2. Hardcoder les IDs

```deluge
// ❌ MAUVAIS
ownerId = "5234876000000111222"; // Qui est-ce ?!

// ✅ BON : chercher dynamiquement
criteria = "(email:equals:alice@entreprise.com)";
users = zoho.crm.getUsers("ActiveUsers", 1, 1);
// Ou utiliser une variable de configuration
```

### 3. Ignorer la pagination

```deluge
// ❌ MAUVAIS : ne récupère que les 200 premiers
allLeads = zoho.crm.getRecords("Leads", 1, 200);

// ✅ BON : paginer
allLeads = List();
page = 1;
loop
{
    batch = zoho.crm.getRecords("Leads", page, 200);
    if(batch.isEmpty()) { break; }
    allLeads.addAll(batch);
    if(batch.size() < 200) { break; }
    page = page + 1;
    if(page > 25) { break; } // Sécurité
}
```

### 4. String pour les nombres

```deluge
// ❌ MAUVAIS : comparaison de strings
if(deal.get("Amount") > "10000") // Compare alphabétiquement !

// ✅ BON : conversion explicite
amount = ifnull(deal.get("Amount"), 0).toLong();
if(amount > 10000)
{
    // ...
}
```

## Debug

### Logging structuré

```deluge
info "========== DÉBUT : MaFonction ==========";
info "Paramètre : dealId = " + dealId;

deal = zoho.crm.getRecordById("Deals", dealId);
info "Deal récupéré : " + deal.get("Deal_Name");
info "Stage : " + deal.get("Stage");
info "Amount : " + deal.get("Amount");

// ... logique ...

info "Résultat : score = " + score;
info "========== FIN : MaFonction ==========";
```

### Inspecter les objets

```deluge
// Afficher tout le contenu d'un record
record = zoho.crm.getRecordById("Deals", dealId);
info "Record complet : " + record.toString();

// Afficher les clés disponibles
info "Clés : " + record.keys();

// Vérifier le type
info "Type de Amount : " + record.get("Amount").getClass();
```

### Tester en isolation

```deluge
// Fonction de test standalone
// Exécuter manuellement avec un ID connu

testDealId = "5234876000000123456"; // ID de test

deal = zoho.crm.getRecordById("Deals", testDealId);
if(deal == null)
{
    info "❌ Deal non trouvé";
    return;
}

info "✅ Deal trouvé : " + deal.get("Deal_Name");

// Tester la logique sans modifier les données
// (commenter les updateRecord pour le test)
```

## Sécurité

### Ne pas exposer les secrets

```deluge
// ❌ MAUVAIS
apiKey = "sk_live_1234567890abcdef";
response = invokeurl[url: apiUrl type: GET headers: {"Authorization": "Bearer " + apiKey}];

// ✅ BON : utiliser une connexion Zoho
response = invokeurl
[
    url: apiUrl
    type: GET
    connection: "stripe_connection" // Token géré par Zoho
];
```

### Valider les entrées

```deluge
// Si la fonction est appelée par un widget (input utilisateur)
if(input_email == null || !input_email.contains("@"))
{
    return {"status": "error", "message": "Email invalide"};
}

// Échapper les caractères spéciaux si utilisé dans des critères
searchTerm = input_text.replaceAll("'", "\\'");
```

## Checklist avant déploiement

- [ ] Gestion des erreurs (try/catch sur les appels externes)
- [ ] Null checks sur tous les `.get()`
- [ ] Pas de boucle infinie possible
- [ ] Pas de secrets en dur
- [ ] Logs suffisants pour le debug
- [ ] Pagination gérée si volumes importants
- [ ] Bulk operations quand possible
- [ ] Testé avec des données réelles
- [ ] Commentaires sur la logique métier
- [ ] Triggers contrôlés (pas de cascades)

---
*Voir aussi : [exemples.md](exemples.md) pour des scripts complets appliquant ces pratiques.*
