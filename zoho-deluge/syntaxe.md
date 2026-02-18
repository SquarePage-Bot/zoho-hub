# 📝 Zoho Deluge - Syntaxe

> Variables, types, opérateurs, structures de contrôle, gestion d'erreurs.

## Variables

### Déclaration

Pas de mot-clé de déclaration. Le type est inféré automatiquement.

```deluge
// Chaîne de caractères
nom = "Jean Dupont";

// Nombre entier
age = 35;

// Nombre décimal
prix = 99.99;

// Booléen
estClient = true;

// Null
valeur = null;

// Date
aujourd_hui = zoho.currentdate;

// Date/Heure
maintenant = zoho.currenttime;
```

### Types de données

| Type | Exemple | Description |
|------|---------|-------------|
| `STRING` | `"Hello"` | Chaîne de caractères |
| `INT` | `42` | Entier |
| `FLOAT/DECIMAL` | `3.14` | Nombre décimal |
| `BOOLEAN` | `true` / `false` | Booléen |
| `DATE` | `'2026-02-18'` | Date (format yyyy-MM-dd) |
| `DATETIME` | `'2026-02-18T16:30:00'` | Date et heure |
| `LIST` | `{1, 2, 3}` | Liste ordonnée |
| `MAP` | `{"key": "value"}` | Dictionnaire clé-valeur |
| `NULL` | `null` | Absence de valeur |
| `FILE` | (objet fichier) | Fichier uploadé |

### Conversion de types

```deluge
// String → Number
nombre = "42".toLong();
decimal = "3.14".toDecimal();

// Number → String
texte = 42.toString();

// String → Date
date = "2026-02-18".toDate("yyyy-MM-dd");

// Date → String
texte = zoho.currentdate.toString("dd/MM/yyyy");
// Résultat : "18/02/2026"

// String → Boolean
bool = "true".toBoolean();

// Vérifier le type
val = "hello";
info val.getClass(); // STRING
```

## Opérateurs

### Arithmétiques

```deluge
a = 10;
b = 3;

info a + b;   // 13
info a - b;   // 7
info a * b;   // 30
info a / b;   // 3 (division entière)
info a % b;   // 1 (modulo)
```

### Comparaison

```deluge
a = 10;
b = 20;

info a == b;   // false
info a != b;   // true
info a < b;    // true
info a > b;    // false
info a <= b;   // true
info a >= b;   // false
```

### Logiques

```deluge
a = true;
b = false;

info a && b;   // false (ET)
info a || b;   // true (OU)
info !a;       // false (NON)
```

### Concaténation de chaînes

```deluge
prenom = "Jean";
nom = "Dupont";

// Avec +
complet = prenom + " " + nom;
info complet; // "Jean Dupont"

// Avec concaténation de types mixtes
info "Montant : " + 1500 + "€"; // "Montant : 1500€"
```

## Structures de contrôle

### if / else if / else

```deluge
score = 75;

if(score >= 80)
{
    categorie = "Très chaud";
}
else if(score >= 50)
{
    categorie = "Chaud";
}
else if(score >= 20)
{
    categorie = "Tiède";
}
else
{
    categorie = "Froid";
}

info categorie; // "Chaud"
```

### Opérateur ternaire (ifnull)

```deluge
// ifnull : retourne la deuxième valeur si la première est null
email = ifnull(contact.get("Email"), "non-renseigné");

// Pas d'opérateur ternaire ? : classique en Deluge
// Utiliser if/else ou ifnull
```

### for each (boucle sur liste)

```deluge
fruits = {"pomme", "banane", "orange"};

for each fruit in fruits
{
    info "Fruit : " + fruit;
}
// Fruit : pomme
// Fruit : banane
// Fruit : orange
```

### for each avec index

```deluge
noms = {"Alice", "Bob", "Claire"};
index = 0;

for each nom in noms
{
    info index + " : " + nom;
    index = index + 1;
}
```

### for (boucle classique)

```deluge
// Boucle de 1 à 10
for i = 1 to 10
{
    info "Itération " + i;
}

// Attention : pas de boucle while en Deluge !
// Utiliser for avec un compteur
```

### for each sur Map

```deluge
config = Map();
config.put("nom", "SquarePage");
config.put("ville", "Paris");
config.put("pays", "France");

for each key in config.keys()
{
    info key + " = " + config.get(key);
}
```

### break et continue

```deluge
// Pas de break ni continue en Deluge standard !
// Alternatives :

// Au lieu de break : utiliser un flag
found = false;
for each item in myList
{
    if(!found && item.get("name") == "cible")
    {
        info "Trouvé !";
        found = true;
    }
}

// Au lieu de continue : utiliser if
for each item in myList
{
    if(item.get("status") != "ignoré")
    {
        // Traiter l'item
        info item;
    }
}
```

## Gestion d'erreurs

### try / catch

```deluge
try
{
    // Code qui peut échouer
    response = invokeurl
    [
        url: "https://api.externe.com/data"
        type: GET
        headers: {"Authorization": "Bearer xxx"}
    ];
    info response;
}
catch(e)
{
    // Gestion de l'erreur
    info "Erreur : " + e.getMessage();
    
    // Notifier par email en cas d'erreur
    sendmail
    [
        from: zoho.adminuserid
        to: "admin@squarepage.fr"
        subject: "Erreur API"
        message: "L'appel API a échoué : " + e.getMessage()
    ];
}
```

### Vérifications préventives

```deluge
// Vérifier null avant d'utiliser
contact = zoho.crm.getRecordById("Contacts", contactId);
if(contact != null)
{
    email = contact.get("Email");
    if(email != null && email != "")
    {
        // Utiliser l'email en toute sécurité
        info "Email : " + email;
    }
}

// ifnull pour les valeurs par défaut
montant = ifnull(deal.get("Amount"), 0);
nom = ifnull(contact.get("First_Name"), "") + " " + ifnull(contact.get("Last_Name"), "");
```

## Commentaires

```deluge
// Commentaire sur une ligne

/* 
   Commentaire
   sur plusieurs
   lignes 
*/

// IMPORTANT : Les commentaires comptent dans la limite d'instructions !
// Garder les commentaires concis en production
```

## Portée des variables

```deluge
// Les variables sont globales dans la fonction
x = 10;

if(true)
{
    x = 20; // Modifie la variable existante
    y = 30; // Crée une nouvelle variable accessible partout
}

info x; // 20
info y; // 30 (accessible hors du if)
```

## Fonctions personnalisées

### Déclaration (dans Zoho CRM)

```
Setup → Personnalisation → Fonctions → Nouvelle fonction
  Nom : calculerScore
  Module : Leads
  Catégorie : Button / Workflow / Standalone
  Paramètres : leadId (String)
```

### Corps de la fonction

```deluge
// Paramètre reçu : leadId

lead = zoho.crm.getRecordById("Leads", leadId);
if(lead == null)
{
    return "Lead non trouvé";
}

score = 0;
// ... calcul du score ...

return score.toString();
```

### Retour de valeur

```deluge
// return pour retourner une valeur
// Fonctionne avec tous les types

return "succès";
return 42;
return true;
return myMap;
return myList;

// Return map pour les widgets
returnMap = Map();
returnMap.put("status", "success");
returnMap.put("score", 85);
return returnMap.toString();
```

## Patterns courants

### Vérification de chaîne vide

```deluge
// En Deluge, tester null ET vide
value = record.get("Field_Name");
if(value != null && value != "")
{
    // Le champ a une valeur
}

// Raccourci avec ifnull
if(ifnull(value, "") != "")
{
    // Le champ a une valeur
}
```

### Manipulation de dates

```deluge
// Date du jour
today = zoho.currentdate;

// Date/heure actuelle
now = zoho.currenttime;

// Ajouter des jours
dans7jours = today.addDay(7);

// Soustraire des mois
il_y_a_3_mois = today.subMonth(3);

// Formater
formatted = today.toString("dd/MM/yyyy");
isoFormat = today.toString("yyyy-MM-dd");

// Parser une date
dateStr = "18/02/2026";
date = dateStr.toDate("dd/MM/yyyy");

// Différence entre dates
jours = daysBetween(date1, date2);
heures = hoursBetween(datetime1, datetime2);
```

### Logging et debug

```deluge
// info : affiche dans les logs (visible dans les logs de fonction)
info "Début du script";
info "Variable x = " + x;
info "Record : " + record.toString();

// Pour le debug structuré
info "=== ÉTAPE 1 : Récupération du lead ===";
lead = zoho.crm.getRecordById("Leads", leadId);
info "Lead récupéré : " + lead.get("Last_Name");

info "=== ÉTAPE 2 : Calcul du score ===";
score = 42;
info "Score calculé : " + score;

// Les logs sont visibles dans :
// Setup → Personnalisation → Fonctions → Logs
```

---
*Voir aussi : [fonctions.md](fonctions.md) pour les fonctions intégrées, [collections.md](collections.md) pour List et Map.*
