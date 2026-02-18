# 📦 Zoho Deluge - Collections (List & Map)

> Manipulation des listes et dictionnaires en Deluge.

## List (Listes)

### Création

```deluge
// Liste vide
maListe = List();

// Liste avec valeurs initiales
nombres = {1, 2, 3, 4, 5};
noms = {"Alice", "Bob", "Claire"};
mixte = {"texte", 42, true, null};

// Liste depuis une chaîne
csv = "un,deux,trois";
liste = csv.toList(",");
info liste;  // ["un", "deux", "trois"]
```

### Opérations de base

```deluge
liste = {"pomme", "banane", "orange"};

// Taille
info liste.size();        // 3

// Ajouter
liste.add("kiwi");        // Ajoute à la fin
liste.add(0, "fraise");   // Ajoute à l'index 0

// Accéder
info liste.get(0);        // "fraise"
info liste.get(2);        // "banane"

// Modifier
liste.set(1, "mangue");   // Remplace l'index 1

// Supprimer
liste.remove(0);          // Supprime par index
liste.remove("orange");   // Supprime par valeur
liste.removeAll({"kiwi", "mangue"}); // Supprime plusieurs

// Vider
liste.clear();

// Contient
info {"a","b","c"}.contains("b");  // true

// Est vide
info liste.isEmpty();     // true (après clear)
```

### Itération

```deluge
noms = {"Alice", "Bob", "Claire"};

// for each
for each nom in noms
{
    info nom;
}

// Avec index manuel
i = 0;
for each nom in noms
{
    info i + ": " + nom;
    i = i + 1;
}
```

### Tri et recherche

```deluge
nombres = {5, 2, 8, 1, 9, 3};

// Tri croissant
nombres.sort(true);
info nombres;  // [1, 2, 3, 5, 8, 9]

// Tri décroissant
nombres.sort(false);
info nombres;  // [9, 8, 5, 3, 2, 1]

// Index d'un élément
info {"a","b","c","b"}.indexOf("b");     // 1
info {"a","b","c","b"}.lastIndexOf("b"); // 3

// Distinct (supprimer les doublons)
listeAvecDoublons = {1, 2, 2, 3, 3, 3};
listeUnique = listeAvecDoublons.distinct();
info listeUnique;  // [1, 2, 3]
```

### Sous-listes et fusion

```deluge
liste = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};

// Sous-liste (de l'index X à Y exclus)
sousListe = liste.subList(2, 5);
info sousListe;  // [3, 4, 5]

// Fusionner deux listes
liste1 = {1, 2, 3};
liste2 = {4, 5, 6};
liste1.addAll(liste2);
info liste1;  // [1, 2, 3, 4, 5, 6]

// Intersection
a = {1, 2, 3, 4, 5};
b = {3, 4, 5, 6, 7};
a.retainAll(b);
info a;  // [3, 4, 5]
```

### Conversion

```deluge
// Liste → String
liste = {"un", "deux", "trois"};
texte = liste.toString(",");
info texte;  // "un,deux,trois"

texte2 = liste.toString(" | ");
info texte2;  // "un | deux | trois"

// String → Liste
csv = "a;b;c;d";
liste = csv.toList(";");
```

## Map (Dictionnaires)

### Création

```deluge
// Map vide
maMap = Map();

// Ajouter des entrées
maMap.put("nom", "Dupont");
maMap.put("prenom", "Jean");
maMap.put("age", 35);
maMap.put("actif", true);

// Map depuis JSON
jsonStr = '{"nom":"Dupont","prenom":"Jean","age":35}';
maMap = jsonStr.toMap();
```

### Opérations de base

```deluge
config = Map();
config.put("host", "api.example.com");
config.put("port", 443);
config.put("ssl", true);

// Accéder
info config.get("host");      // "api.example.com"
info config.get("inexistant"); // null

// Taille
info config.size();            // 3

// Contient une clé
info config.containsKey("host");   // true
info config.containsKey("user");   // false

// Contient une valeur
info config.containsValue(443);    // true

// Supprimer
config.remove("ssl");

// Toutes les clés
info config.keys();            // ["host", "port"]

// Toutes les valeurs
info config.values();          // ["api.example.com", 443]
```

### Itération

```deluge
user = Map();
user.put("nom", "Dupont");
user.put("prenom", "Jean");
user.put("email", "jean@acme.com");

// Itérer sur les clés
for each key in user.keys()
{
    info key + " = " + user.get(key);
}
// nom = Dupont
// prenom = Jean
// email = jean@acme.com
```

### Maps imbriquées

```deluge
// Créer une structure complexe
client = Map();
client.put("nom", "Acme Corp");

adresse = Map();
adresse.put("rue", "123 rue de Paris");
adresse.put("ville", "Paris");
adresse.put("cp", "75001");
client.put("adresse", adresse);

contacts = List();
contact1 = Map();
contact1.put("nom", "Dupont");
contact1.put("role", "CEO");
contacts.add(contact1);

contact2 = Map();
contact2.put("nom", "Martin");
contact2.put("role", "CTO");
contacts.add(contact2);

client.put("contacts", contacts);

// Accéder
info client.get("adresse").get("ville");  // "Paris"
info client.get("contacts").get(0).get("nom");  // "Dupont"

// En JSON
info client.toString();
// {"nom":"Acme Corp","adresse":{"rue":"123 rue de Paris","ville":"Paris","cp":"75001"},"contacts":[{"nom":"Dupont","role":"CEO"},{"nom":"Martin","role":"CTO"}]}
```

### JSON ↔ Map

```deluge
// JSON String → Map
json = '{"user":{"name":"Jean","roles":["admin","editor"],"settings":{"theme":"dark"}}}';
data = json.toMap();

// Accès
info data.get("user").get("name");               // "Jean"
info data.get("user").get("roles").get(0);        // "admin"
info data.get("user").get("settings").get("theme"); // "dark"

// Map → JSON String
output = data.toString();
info output;
```

## Patterns courants

### Construire un body pour API

```deluge
// Construire un payload JSON structuré
payload = Map();
payload.put("data", List());

record = Map();
record.put("Last_Name", "Dupont");
record.put("Email", "jean@acme.com");
record.put("Company", "Acme");
payload.get("data").add(record);

info payload.toString();
// {"data":[{"Last_Name":"Dupont","Email":"jean@acme.com","Company":"Acme"}]}
```

### Transformer une liste de records

```deluge
// Récupérer des deals et extraire les noms
deals = zoho.crm.getRecords("Deals", 1, 50);
dealNames = List();

for each deal in deals
{
    dealNames.add(deal.get("Deal_Name"));
}

info "Deals : " + dealNames.toString(", ");
```

### Grouper par valeur

```deluge
// Grouper des deals par stage
deals = zoho.crm.getRecords("Deals", 1, 100);
grouped = Map();

for each deal in deals
{
    stage = ifnull(deal.get("Stage"), "Sans étape");
    if(!grouped.containsKey(stage))
    {
        grouped.put(stage, List());
    }
    grouped.get(stage).add(deal.get("Deal_Name"));
}

// Afficher
for each stage in grouped.keys()
{
    info stage + " : " + grouped.get(stage).size() + " deals";
}
```

### Dédupliquer par champ

```deluge
// Dédupliquer une liste de records par email
records = zoho.crm.searchRecords("Contacts", "(Email:is not:null)");
seen = Map();
unique = List();

for each record in records
{
    email = record.get("Email").toLowerCase();
    if(!seen.containsKey(email))
    {
        seen.put(email, true);
        unique.add(record);
    }
}

info "Contacts uniques : " + unique.size();
```

### Construire un CSV

```deluge
// Générer un CSV à partir de records
deals = zoho.crm.getRecords("Deals", 1, 50);
csv = "Nom,Montant,Étape,Date de clôture\n";

for each deal in deals
{
    nom = ifnull(deal.get("Deal_Name"), "");
    montant = ifnull(deal.get("Amount"), "0");
    stage = ifnull(deal.get("Stage"), "");
    date = ifnull(deal.get("Closing_Date"), "");
    
    csv = csv + "\"" + nom + "\"," + montant + ",\"" + stage + "\",\"" + date + "\"\n";
}

// Convertir en fichier
csvFile = csv.toFile("export_deals.csv");

// Envoyer par email
sendmail
[
    from: zoho.adminuserid
    to: "manager@entreprise.com"
    subject: "Export deals"
    message: "Veuillez trouver l'export ci-joint."
    attachments: csvFile
];
```

## Bonnes pratiques

1. **Initialisation** : Toujours initialiser les listes/maps avant de les utiliser
2. **Null check** : Vérifier `!= null` avant `.get()`
3. **ifnull** : Utiliser `ifnull(value, default)` pour les valeurs par défaut
4. **Size check** : Vérifier `.size() > 0` avant d'itérer
5. **Performance** : Éviter les boucles imbriquées sur de gros volumes (>500 records)
6. **Mémoire** : Les collections sont en mémoire, attention aux gros volumes

---
*Voir aussi : [syntaxe.md](syntaxe.md) pour les bases, [api-calls.md](api-calls.md) pour les appels HTTP.*
