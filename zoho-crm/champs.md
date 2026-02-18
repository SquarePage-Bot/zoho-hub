# 🏷️ Zoho CRM - Champs (Fields)

> Référence complète des types de champs, validation, formules et personnalisation.

## Types de champs disponibles

### Champs texte

| Type | Nom API | Max caractères | Description |
|------|---------|----------------|-------------|
| Ligne unique | `single_line` | 255 | Texte court |
| Multi-lignes | `multi_line` | 32 000 | Texte long, zone de texte |
| Email | `email` | 100 | Validation format email |
| Téléphone | `phone` | 30 | Numéro de téléphone |
| URL | `website` | 255 | Lien web avec validation |
| Ligne auto | `autonumber` | - | Numéro auto-incrémenté |

### Champs numériques

| Type | Nom API | Description |
|------|---------|-------------|
| Nombre | `integer` | Entier (pas de décimales) |
| Décimal | `double` | Nombre à virgule |
| Devise | `currency` | Montant avec symbole monétaire |
| Pourcentage | `percent` | Valeur en % |

### Champs de sélection

| Type | Nom API | Description |
|------|---------|-------------|
| Liste déroulante | `picklist` | Choix unique parmi une liste |
| Multi-sélection | `multiselectpicklist` | Choix multiples |
| Case à cocher | `boolean` | Vrai/Faux |

### Champs date/heure

| Type | Nom API | Format | Description |
|------|---------|--------|-------------|
| Date | `date` | `yyyy-MM-dd` | Date seule |
| Date/Heure | `datetime` | `yyyy-MM-dd'T'HH:mm:ssZ` | Date et heure |

### Champs relationnels

| Type | Nom API | Description |
|------|---------|-------------|
| Lookup | `lookup` | Référence vers un autre module |
| Multi-select lookup | `multiselectlookup` | Références multiples |
| Utilisateur | `ownerlookup` | Référence vers un utilisateur |

### Champs spéciaux

| Type | Nom API | Description |
|------|---------|-------------|
| Formule | `formula` | Calculé automatiquement |
| Rollup | `rollup_summary` | Agrégation depuis sous-formulaire |
| Image | `profileimage` | Photo (contacts, leads) |
| Fichier upload | `fileupload` | Pièce jointe directe |
| Sous-formulaire | `subform` | Tableau de lignes enfant |

## Champs de formule

### Syntaxe

Les formules utilisent une syntaxe proche d'Excel.

**Types de retour possibles :**
- Texte (String)
- Nombre (Number)
- Date (Date)
- Date/Heure (DateTime)
- Booléen (Boolean)

### Fonctions disponibles dans les formules

#### Texte
```
concat(string1, string2, ...)     → Concaténation
contains(string, substring)       → Contient (booléen)
startswith(string, prefix)        → Commence par
endswith(string, suffix)          → Finit par
len(string)                       → Longueur
lower(string)                     → Minuscules
upper(string)                     → Majuscules
trim(string)                      → Supprime espaces
substring(string, start, end)     → Sous-chaîne
replace(string, old, new)         → Remplacement
lpad(string, length, pad_char)    → Padding gauche
rpad(string, length, pad_char)    → Padding droit
```

#### Numérique
```
abs(number)                       → Valeur absolue
ceil(number)                      → Arrondi supérieur
floor(number)                     → Arrondi inférieur
round(number, decimals)           → Arrondi
sqrt(number)                      → Racine carrée
power(base, exponent)             → Puissance
max(num1, num2, ...)              → Maximum
min(num1, num2, ...)              → Minimum
```

#### Date
```
now()                             → Date/heure actuelle
today()                           → Date du jour
dateValue(datetime)               → Extrait la date
year(date)                        → Année
month(date)                       → Mois (1-12)
day(date)                         → Jour (1-31)
dayofweek(date)                   → Jour de la semaine (1=dim, 7=sam)
hour(datetime)                    → Heure
minute(datetime)                  → Minute
adddays(date, n)                  → Ajoute n jours
addmonths(date, n)                → Ajoute n mois
subdays(date, n)                  → Soustrait n jours
datediff(date1, date2)            → Différence en jours
```

#### Logique
```
if(condition, valeur_vrai, valeur_faux)
and(cond1, cond2, ...)
or(cond1, cond2, ...)
not(condition)
isblank(field)
isnull(field)
nullvalue(field, default)
```

### Exemples de formules

**Nom complet :**
```
concat(${Salutation}, " ", ${First_Name}, " ", ${Last_Name})
```

**Jours depuis création :**
```
datediff(${Created_Time}, now())
```

**Montant TTC (TVA 20%) :**
```
${Amount} * 1.20
```

**Catégorie de deal par montant :**
```
if(${Amount} > 50000, "Grand compte",
  if(${Amount} > 10000, "Moyen compte", "Petit compte"))
```

**Prochain anniversaire :**
```
if(month(${Date_of_Birth}) > month(today()) ||
   (month(${Date_of_Birth}) == month(today()) && day(${Date_of_Birth}) >= day(today())),
   dateValue(concat(year(today()), "-", lpad(month(${Date_of_Birth}), 2, "0"), "-", lpad(day(${Date_of_Birth}), 2, "0"))),
   dateValue(concat(year(today()) + 1, "-", lpad(month(${Date_of_Birth}), 2, "0"), "-", lpad(day(${Date_of_Birth}), 2, "0")))
)
```

## Champs Rollup (Récapitulatifs)

Agrègent les données des enregistrements enfants (sous-formulaires ou modules liés).

**Fonctions d'agrégation :**
- `SUM` : Somme
- `AVG` : Moyenne
- `MAX` : Maximum
- `MIN` : Minimum
- `COUNT` : Nombre d'enregistrements

**Exemple :**
- Sur un Compte : `SUM` du montant de toutes les Transactions associées
- Sur un Deal : `COUNT` des Tâches liées

## Validation de champs

### Règles de validation

Permettent de contrôler les données saisies avant enregistrement.

**Syntaxe :**
```
// Le montant doit être positif
if(${Amount} <= 0)
{
    alert "Le montant doit être supérieur à 0";
}

// L'email doit être professionnel
if(contains(${Email}, "gmail.com") || contains(${Email}, "yahoo.com"))
{
    alert "Veuillez utiliser une adresse email professionnelle";
}

// Date de clôture dans le futur
if(${Closing_Date} < today())
{
    alert "La date de clôture doit être dans le futur";
}
```

### Contraintes de champ

| Contrainte | Description |
|-----------|-------------|
| Obligatoire | Le champ doit être rempli |
| Unique | Pas de doublons |
| Longueur max | Limite de caractères |
| Expression régulière | Pattern personnalisé |

## Layouts (Mises en page)

### Concept

Un layout définit quels champs sont visibles et dans quel ordre pour un module donné. Différents layouts peuvent être assignés à différents profils.

### Structure

```
Layout
├── Section 1 (ex: "Informations principales")
│   ├── Champ 1 (1 ou 2 colonnes)
│   ├── Champ 2
│   └── ...
├── Section 2 (ex: "Adresse")
│   ├── Champ 3
│   └── ...
└── Sous-formulaire (si applicable)
```

### Multi-layouts

- Chaque module peut avoir **plusieurs layouts**
- Utile pour différents types d'enregistrements (ex: Lead B2B vs Lead B2C)
- Le layout détermine les champs visibles, les picklists disponibles, et les actions

## Sous-formulaires (Subforms)

### Utilisation

Permettent d'intégrer une liste de lignes dans un enregistrement (relation parent-enfant).

**Cas d'usage :**
- Lignes de produits dans un devis
- Historique des interactions
- Liste de participants à un événement

### Accès en Deluge

```deluge
// Lire un sous-formulaire
record = zoho.crm.getRecordById("Deals", dealId);
lignes = record.get("Product_Details"); // Nom du sous-formulaire

for each ligne in lignes
{
    produit = ligne.get("Product_Name");
    quantite = ligne.get("Quantity");
    prix = ligne.get("Unit_Price");
    info produit + " x " + quantite + " = " + (quantite * prix);
}

// Écrire dans un sous-formulaire
subformList = List();

ligne1 = Map();
ligne1.put("Product_Name", "5234876000000111222"); // ID produit
ligne1.put("Quantity", 5);
ligne1.put("Unit_Price", 100);
subformList.add(ligne1);

ligne2 = Map();
ligne2.put("Product_Name", "5234876000000333444");
ligne2.put("Quantity", 2);
ligne2.put("Unit_Price", 250);
subformList.add(ligne2);

updateMap = Map();
updateMap.put("Product_Details", subformList);
response = zoho.crm.updateRecord("Deals", dealId, updateMap);
```

## Champs système (non modifiables)

| Champ | Nom API | Description |
|-------|---------|-------------|
| ID | `id` | Identifiant unique |
| Créé par | `Created_By` | Utilisateur créateur |
| Date de création | `Created_Time` | Timestamp de création |
| Modifié par | `Modified_By` | Dernier modificateur |
| Date de modification | `Modified_Time` | Timestamp dernière modif |
| Propriétaire | `Owner` | Utilisateur responsable |

## Bonnes pratiques

1. **Nommage** : Utiliser des noms API en anglais cohérents (`Revenue_Annual` plutôt que `CA`)
2. **Obligatoire** : Marquer obligatoires uniquement les champs vraiment essentiels
3. **Picklists** : Limiter à 50 valeurs max, utiliser des valeurs standardisées
4. **Formules** : Préférer les formules aux calculs manuels
5. **Layouts** : Un layout par profil d'utilisation
6. **Documentation** : Documenter chaque champ personnalisé avec sa description

---
*Voir aussi : [modules.md](modules.md) pour les modules, [workflows.md](workflows.md) pour automatiser selon les champs.*
