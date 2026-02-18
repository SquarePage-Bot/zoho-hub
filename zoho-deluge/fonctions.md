# 📚 Zoho Deluge - Fonctions Intégrées

> Référence complète des fonctions built-in : texte, nombres, dates, fichiers, email.

## Fonctions de texte (String)

### Manipulation de base

```deluge
texte = "  Hello World  ";

// Longueur
info texte.length();           // 15

// Supprimer les espaces
info texte.trim();             // "Hello World"
info texte.leftTrim();         // "Hello World  "
info texte.rightTrim();        // "  Hello World"

// Casse
info "hello".toUpperCase();    // "HELLO"
info "HELLO".toLowerCase();    // "hello"

// Première lettre majuscule
info "hello world".toStartCase(); // "Hello World"
```

### Recherche et test

```deluge
texte = "Bonjour le monde";

// Contient
info texte.contains("monde");     // true

// Commence par / Finit par
info texte.startsWith("Bon");     // true
info texte.endsWith("monde");     // true

// Position
info texte.indexOf("le");         // 8
info texte.lastIndexOf("o");      // 13

// Test vide
info "".isEmpty();                // true
info " ".isEmpty();               // false (espace)
```

### Extraction

```deluge
texte = "Jean-Pierre Dupont";

// Sous-chaîne
info texte.subString(0, 4);       // "Jean"
info texte.subString(5);          // "Pierre Dupont"

// Premier/dernier caractère
info texte.charAt(0);             // "J"

// Avant/après un séparateur
info texte.getPrefix("-");        // "Jean"
info texte.getSuffix("-");        // "Pierre Dupont"
```

### Transformation

```deluge
// Remplacement
info "Hello World".replaceAll("World", "Zoho");  // "Hello Zoho"
info "aabaa".replaceFirst("a", "X");              // "Xabaa"

// Split (diviser en liste)
texte = "un,deux,trois,quatre";
parts = texte.toList(",");
info parts;                        // ["un","deux","trois","quatre"]
info parts.size();                 // 4

// Join (assembler une liste)
liste = {"a", "b", "c"};
info liste.toString(",");          // "a,b,c"
info liste.toString(" - ");        // "a - b - c"

// Padding
info "42".leftPad(5, "0");        // "00042"
info "Hi".rightPad(10, ".");      // "Hi........"

// Supprimer des caractères
info "Hello!".remove("!");         // "Hello"

// URL encode/decode
info encodeUrl("hello world");     // "hello+world"
info decodeUrl("hello+world");     // "hello world"
```

### Regex (Expressions régulières)

```deluge
email = "jean@acme.com";

// Test de pattern
info email.matches("[a-zA-Z0-9+_.-]+@[a-zA-Z0-9.-]+");  // true

// Extraction avec regex
texte = "Tel: 06 12 34 56 78";
phone = texte.replaceAll("[^0-9]", "");
info phone;  // "0612345678"

// Extraire un groupe
url = "https://www.example.com/page/123";
id = url.replaceAll(".*/page/([0-9]+).*", "$1");
info id;  // "123"
```

## Fonctions numériques

```deluge
// Arrondi
info round(3.7);           // 4
info round(3.14159, 2);    // 3.14
info ceil(3.2);            // 4
info floor(3.8);           // 3

// Valeur absolue
info abs(-42);             // 42

// Min / Max
info min(10, 20);          // 10  (Deluge natif n'a pas min/max, utiliser if)
// Alternative :
a = 10;
b = 20;
minimum = if(a < b, a, b);

// Puissance
info power(2, 10);         // 1024

// Racine carrée
info sqrt(144);            // 12

// Conversion
info "42".toLong();        // 42 (int)
info "3.14".toDecimal();   // 3.14 (float)
info 42.toString();        // "42"

// Random
info randomNumber(1, 100); // Nombre aléatoire entre 1 et 100
```

## Fonctions de date et heure

### Dates courantes

```deluge
// Date du jour
today = zoho.currentdate;
info today;  // 2026-02-18

// Date/heure actuelle
now = zoho.currenttime;
info now;  // 2026-02-18T16:30:00+01:00
```

### Arithmétique de dates

```deluge
date = zoho.currentdate;

// Ajouter
info date.addDay(7);       // +7 jours
info date.addMonth(1);     // +1 mois
info date.addYear(1);      // +1 an
info date.addHour(3);      // +3 heures (sur datetime)
info date.addMinutes(30);  // +30 minutes (sur datetime)

// Soustraire
info date.subDay(7);       // -7 jours
info date.subMonth(1);     // -1 mois
info date.subYear(1);      // -1 an
```

### Extraction de composants

```deluge
date = zoho.currentdate;

info date.getYear();       // 2026
info date.getMonth();      // 2
info date.getDay();        // 18
info date.getDayOfWeek();  // 4 (mercredi, 1=dimanche)

datetime = zoho.currenttime;
info datetime.getHour();   // 16
info datetime.getMinutes(); // 30
info datetime.getSeconds(); // 0
```

### Formatage

```deluge
date = zoho.currentdate;

// Formats courants
info date.toString("dd/MM/yyyy");        // "18/02/2026"
info date.toString("yyyy-MM-dd");        // "2026-02-18"
info date.toString("d MMMM yyyy");       // "18 février 2026"
info date.toString("EEEE d MMMM yyyy"); // "mercredi 18 février 2026"
info date.toString("dd-MMM-yy");         // "18-Feb-26"

datetime = zoho.currenttime;
info datetime.toString("dd/MM/yyyy HH:mm");     // "18/02/2026 16:30"
info datetime.toString("yyyy-MM-dd'T'HH:mm:ssZ"); // ISO 8601
```

**Tokens de format :**

| Token | Description | Exemple |
|-------|-------------|---------|
| `yyyy` | Année 4 chiffres | 2026 |
| `yy` | Année 2 chiffres | 26 |
| `MM` | Mois (01-12) | 02 |
| `MMM` | Mois abrégé | Feb |
| `MMMM` | Mois complet | February |
| `dd` | Jour (01-31) | 18 |
| `d` | Jour (1-31) | 18 |
| `EEEE` | Jour de la semaine | Wednesday |
| `EEE` | Jour abrégé | Wed |
| `HH` | Heure 24h (00-23) | 16 |
| `hh` | Heure 12h (01-12) | 04 |
| `mm` | Minutes | 30 |
| `ss` | Secondes | 00 |
| `a` | AM/PM | PM |

### Parsing de dates

```deluge
// String → Date
dateStr = "18/02/2026";
date = dateStr.toDate("dd/MM/yyyy");

// String → DateTime
dtStr = "2026-02-18T16:30:00+01:00";
dt = dtStr.toDateTime();

// Timestamp → Date
timestamp = 1771340400000; // milliseconds
date = timestamp.toDate();
```

### Comparaison et différence

```deluge
date1 = "2026-01-01".toDate("yyyy-MM-dd");
date2 = "2026-03-15".toDate("yyyy-MM-dd");

// Différence en jours
jours = daysBetween(date1, date2);
info jours;  // 73

// Différence en heures
heures = hoursBetween(datetime1, datetime2);

// Comparaison
if(date1.before(date2))
{
    info "date1 est avant date2";
}

if(date2.after(date1))
{
    info "date2 est après date1";
}

// Égalité
if(date1.equals(date2))
{
    info "Mêmes dates";
}
```

## Fonctions d'email

### Envoyer un email

```deluge
sendmail
[
    from: zoho.adminuserid
    to: "client@example.com"
    cc: "manager@entreprise.com"
    bcc: "archive@entreprise.com"
    subject: "Votre proposition commerciale"
    message: "<h1>Bonjour,</h1><p>Veuillez trouver ci-joint notre proposition.</p>"
    content_type: "text/html"
];
```

### Email avec pièce jointe

```deluge
// Récupérer un fichier
file = invokeurl
[
    url: "https://example.com/document.pdf"
    type: GET
];

sendmail
[
    from: zoho.adminuserid
    to: "client@example.com"
    subject: "Document"
    message: "Veuillez trouver le document ci-joint."
    attachments: file
];
```

### Email avec template CRM

```deluge
// Utiliser un template d'email CRM
templateId = "5234876000000111222";
sendmail
[
    from: zoho.adminuserid
    to: "client@example.com"
    subject: "Bienvenue"
    message: zoho.crm.getEmailTemplate(templateId, "Contacts", contactId)
];
```

## Fonctions de fichier

```deluge
// Télécharger un fichier
file = invokeurl
[
    url: "https://example.com/data.csv"
    type: GET
];

// Obtenir les infos du fichier
info file.getName();        // "data.csv"
info file.getFileSize();    // Taille en bytes
info file.getContentType(); // "text/csv"

// Convertir un string en fichier
content = "Col1,Col2\nVal1,Val2";
file = content.toFile("export.csv");
```

## Fonctions JSON

```deluge
// String → JSON (Map ou List)
jsonStr = '{"name":"Jean","age":35}';
jsonMap = jsonStr.toMap();
info jsonMap.get("name");  // "Jean"

// JSON Array → List
jsonArrayStr = '[{"id":1},{"id":2}]';
jsonList = jsonArrayStr.toJSONList();

// Map → JSON String
myMap = Map();
myMap.put("name", "Jean");
myMap.put("age", 35);
jsonOutput = myMap.toString();
info jsonOutput;  // {"name":"Jean","age":35}

// Accès aux objets imbriqués
jsonStr = '{"user":{"name":"Jean","address":{"city":"Paris"}}}';
data = jsonStr.toMap();
city = data.get("user").get("address").get("city");
info city;  // "Paris"
```

## Fonctions XML

```deluge
// Parser du XML
xmlStr = "<root><name>Jean</name><age>35</age></root>";
xmlMap = xmlStr.toXmlMap();

// Accéder aux valeurs
info xmlMap.get("root").get("name");  // "Jean"
```

## Fonctions utilitaires

```deluge
// Identifiant unique
uniqueId = zoho.encryption.generateRandomUUID();
info uniqueId;  // "a1b2c3d4-e5f6-7890-abcd-ef1234567890"

// Encodage
base64 = zoho.encryption.base64Encode("Hello World");
decoded = zoho.encryption.base64Decode(base64);

// Hash
md5 = zoho.encryption.md5("password");
sha256 = zoho.encryption.sha256("password");

// HMAC
hmac = zoho.encryption.hmacsha256("message", "secret_key");

// URL encode
encoded = encodeUrl("hello world & more");

// Pause (sleep) - ATTENTION : consomme du temps d'exécution
sleep(5000); // 5 secondes
```

## Fonctions d'organisation

```deluge
// ID de l'organisation
orgId = zoho.adminuserid;

// Informations utilisateur courant
userId = zoho.loginuserid;
userEmail = zoho.loginuseremail;
userName = zoho.loginuser;
```

---
*Voir aussi : [collections.md](collections.md) pour List/Map, [api-calls.md](api-calls.md) pour invokeurl.*
