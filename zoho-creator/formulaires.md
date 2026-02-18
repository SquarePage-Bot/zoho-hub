# 📝 Zoho Creator - Formulaires

> Guide complet sur les formulaires Creator : types de champs, validation, actions on submit.

## Table des matières

- [Types de champs](#types-de-champs)
- [Propriétés des champs](#propriétés-des-champs)
- [Règles de validation](#règles-de-validation)
- [Actions on Submit](#actions-on-submit)
- [Sous-formulaires (Subforms)](#sous-formulaires-subforms)
- [Mise en page et sections](#mise-en-page-et-sections)
- [Permissions et visibilité](#permissions-et-visibilité)

---

## Types de champs

### Champs texte

| Type | Nom API | Description | Limite |
|------|---------|-------------|--------|
| **Single Line** | `text` | Texte court sur une ligne | 255 caractères |
| **Multi Line** | `textarea` | Zone de texte multiligne | 32 000 caractères |
| **Rich Text** | `rich_text` | Éditeur WYSIWYG HTML | 32 000 caractères |
| **Name** | `name` | Préfixe + Prénom + Nom | Composé de 3 sous-champs |
| **Address** | `address` | Adresse complète structurée | Composé de sous-champs |

### Champs numériques

| Type | Nom API | Description | Précision |
|------|---------|-------------|-----------|
| **Number** | `number` | Nombre entier | Jusqu'à 19 chiffres |
| **Decimal** | `decimal` | Nombre décimal | Jusqu'à 16 chiffres, 10 décimales |
| **Currency** | `currency` | Montant avec devise | Précision configurable |
| **Percent** | `percent` | Pourcentage | Affichage avec % |
| **Auto Number** | `autonumber` | Numérotation automatique | Préfixe + suffixe configurable |
| **Formula** | `formula` | Champ calculé en Deluge | Résultat dynamique |

### Champs de sélection

| Type | Nom API | Description |
|------|---------|-------------|
| **Drop Down** | `dropdown` | Liste déroulante à choix unique |
| **Radio** | `radio` | Boutons radio (choix unique visible) |
| **Checkbox** | `checkbox` | Cases à cocher (choix multiples) |
| **Multi Select** | `multiselect` | Liste à sélection multiple |
| **Decision Box** | `decision` | Oui/Non (booléen) |

### Champs date et heure

| Type | Nom API | Format par défaut |
|------|---------|-------------------|
| **Date** | `date` | dd-MMM-yyyy |
| **Date-Time** | `datetime` | dd-MMM-yyyy HH:mm |
| **Time** | `time` | HH:mm |

### Champs relationnels

| Type | Nom API | Description |
|------|---------|-------------|
| **Lookup** | `lookup` | Relation vers un autre formulaire (clé étrangère) |
| **Subform** | `subform` | Sous-formulaire intégré (relation 1-N) |
| **Add Notes** | `notes` | Champ notes avec historique |
| **Users** | `users` | Sélection d'utilisateurs Zoho |

### Champs médias

| Type | Nom API | Limites |
|------|---------|---------|
| **File Upload** | `file` | Max 50 Mo par fichier, 5 fichiers |
| **Image** | `image` | JPG, PNG, GIF, max 50 Mo |
| **Audio** | `audio` | Enregistrement audio intégré |
| **Video** | `video` | Lien vidéo |
| **Signature** | `signature` | Champ signature manuscrite |

### Champs spéciaux

| Type | Description |
|------|-------------|
| **URL** | Lien web validé |
| **Email** | Adresse email validée |
| **Phone** | Numéro de téléphone |
| **Integration** | Champ connecté à une API externe |
| **Object** | Données JSON structurées |

---

## Propriétés des champs

### Propriétés communes

```
Nom du champ (Field Name)     → Identifiant interne (non modifiable après création)
Libellé (Display Name)        → Texte affiché à l'utilisateur
Obligatoire (Mandatory)       → Oui/Non
Valeur par défaut (Default)   → Expression statique ou Deluge
Info-bulle (Tooltip)          → Texte d'aide au survol
Texte d'aide (Help Text)      → Texte affiché sous le champ
Unique                        → Empêche les doublons
Masqué (Hidden)               → Visible uniquement via Deluge
```

### Valeur par défaut dynamique

```deluge
// Date du jour
zoho.currentdate

// Utilisateur connecté
zoho.loginuser

// Numéro séquentiel
"DEM-" + Demandes.count() + 1

// Valeur basée sur un paramètre URL
input.param_client
```

### Propriétés spécifiques au Lookup

```
Formulaire lié          → Formulaire cible de la relation
Champ affiché           → Champ visible dans le dropdown
Filtrage dynamique      → Critères pour limiter les choix
Fetch other fields      → Remplir d'autres champs automatiquement
```

**Exemple de Lookup avec filtrage :**
```deluge
// Filtrer les produits par catégorie sélectionnée
// Dans les propriétés du lookup "Produit" :
Critère : Produits.Categorie == input.Categorie
```

**Exemple de Fetch Other Fields :**
```
Quand le lookup "Client" est sélectionné :
  → Remplir "Email_Client" avec Client.Email
  → Remplir "Telephone" avec Client.Phone
  → Remplir "Adresse" avec Client.Adresse
```

---

## Règles de validation

### Validation intégrée

Les champs ont des validations natives :
- **Email** : Format email valide
- **URL** : Format URL valide
- **Phone** : Caractères numériques et +/-
- **Number** : Valeurs numériques uniquement
- **Date** : Format date valide

### Validation personnalisée (Deluge)

Les validations s'écrivent dans le script **On Validate** du formulaire :

```deluge
// Validation simple
if (input.Montant <= 0)
{
    cancel submit;
    alert "Le montant doit être supérieur à zéro.";
}

// Validation avec regex
emailPattern = "^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,}$";
if (!input.Email.matches(emailPattern))
{
    cancel submit;
    alert "Format d'email invalide.";
}

// Validation croisée entre champs
if (input.Date_fin < input.Date_debut)
{
    cancel submit;
    alert "La date de fin doit être postérieure à la date de début.";
}

// Vérification d'unicité personnalisée
existant = Clients[SIRET == input.SIRET];
if (existant.count() > 0)
{
    cancel submit;
    alert "Un client avec ce SIRET existe déjà.";
}

// Validation conditionnelle
if (input.Type == "Entreprise" && input.SIRET == "")
{
    cancel submit;
    alert "Le SIRET est obligatoire pour une entreprise.";
}
```

### Validation avec messages multiples

```deluge
erreurs = List();

if (input.Nom == "")
{
    erreurs.add("Le nom est obligatoire");
}
if (input.Email == "")
{
    erreurs.add("L'email est obligatoire");
}
if (input.Montant < 100)
{
    erreurs.add("Le montant minimum est de 100€");
}

if (erreurs.size() > 0)
{
    cancel submit;
    alert erreurs.toString("\n");
}
```

---

## Actions on Submit

### Événements disponibles

| Événement | Déclencheur |
|-----------|-------------|
| **On Load** | Chargement du formulaire |
| **On Validate** | Avant l'enregistrement (peut annuler) |
| **On Success** | Après enregistrement réussi |
| **On User Input** | Quand un champ change de valeur |

### On Load

```deluge
// Pré-remplir des champs
input.Date_creation = zoho.currentdate;
input.Createur = zoho.loginuser;
input.Statut = "Brouillon";

// Masquer/afficher des champs selon le contexte
if (zoho.loginuser != "admin@entreprise.com")
{
    hide Montant_HT;
    hide Marge;
}

// Charger des données externes
clientInfo = zoho.crm.getRecordById("Contacts", input.CRM_ID);
if (clientInfo != null)
{
    input.Nom_Client = clientInfo.get("Full_Name");
    input.Email_Client = clientInfo.get("Email");
}
```

### On User Input

```deluge
// Calcul dynamique quand un champ change
// Trigger : champ "Quantite" ou "Prix_Unitaire"
input.Total_HT = input.Quantite * input.Prix_Unitaire;
input.TVA = input.Total_HT * 0.20;
input.Total_TTC = input.Total_HT + input.TVA;

// Affichage conditionnel
// Trigger : champ "Type_Client"
if (input.Type_Client == "Entreprise")
{
    show SIRET;
    show Raison_Sociale;
    hide Prenom;
}
else
{
    hide SIRET;
    hide Raison_Sociale;
    show Prenom;
}

// Lookup dynamique
// Trigger : champ "Departement"
if (input.Departement == "Technique")
{
    input.Responsable = "tech-lead@entreprise.com";
}
```

### On Success (après enregistrement)

```deluge
// Envoyer un email de confirmation
sendmail
[
    from: zoho.adminuserid
    to: input.Email
    subject: "Votre demande " + input.Numero + " a été enregistrée"
    message: "<h2>Confirmation</h2><p>Bonjour " + input.Nom + ",</p><p>Votre demande a bien été prise en compte. Numéro de suivi : <strong>" + input.Numero + "</strong></p>"
];

// Créer un enregistrement lié
taskMap = Map();
taskMap.put("Titre", "Traiter demande " + input.Numero);
taskMap.put("Assignee", input.Responsable);
taskMap.put("Date_Echeance", zoho.currentdate.addDay(5));
taskMap.put("Demande_Liee", input.ID);
insert into Taches values taskMap;

// Appeler une API externe
payload = Map();
payload.put("reference", input.Numero);
payload.put("client", input.Nom);
payload.put("montant", input.Montant);
response = invokeurl
[
    url: "https://api.entreprise.com/webhook/nouvelle-demande"
    type: POST
    headers: {"Content-Type": "application/json"}
    parameters: payload.toString()
];

// Mettre à jour le CRM
if (input.CRM_Deal_ID != null)
{
    updateMap = Map();
    updateMap.put("Stage", "Qualification");
    zoho.crm.updateRecord("Deals", input.CRM_Deal_ID, updateMap);
}

// Redirection après soumission
openUrl("https://app.entreprise.com/confirmation?ref=" + input.Numero, "same window");
```

---

## Sous-formulaires (Subforms)

### Création

Un sous-formulaire est un formulaire enfant intégré dans un formulaire parent. Il permet de gérer des lignes multiples (ex : lignes de facture).

```
Formulaire "Commande"
├── Champ : Numero_Commande
├── Champ : Client (Lookup)
├── Champ : Date
└── Sous-formulaire "Lignes_Commande"
    ├── Champ : Produit (Lookup)
    ├── Champ : Quantite (Number)
    ├── Champ : Prix_Unitaire (Currency)
    └── Champ : Total_Ligne (Formula)
```

### Manipulation en Deluge

```deluge
// Lire les lignes du sous-formulaire
lignes = input.Lignes_Commande;
totalCommande = 0;
for each ligne in lignes
{
    totalCommande = totalCommande + (ligne.Quantite * ligne.Prix_Unitaire);
}
input.Total_HT = totalCommande;

// Ajouter une ligne programmatiquement
nouvelleLigne = Lignes_Commande();
nouvelleLigne.Produit = "Produit A";
nouvelleLigne.Quantite = 1;
nouvelleLigne.Prix_Unitaire = 100.00;
input.Lignes_Commande.add(nouvelleLigne);

// Supprimer les lignes avec quantité 0
for each ligne in input.Lignes_Commande
{
    if (ligne.Quantite == 0)
    {
        delete ligne;
    }
}
```

---

## Mise en page et sections

### Sections

```
Section "Informations générales"
├── Colonne 1 : Nom, Prénom, Email
└── Colonne 2 : Téléphone, Entreprise

Section "Détails de la demande" (repliable)
├── Type, Priorité
└── Description

Section "Données internes" (masquée par défaut)
├── Statut, Assignee
└── Date_Traitement
```

### Contrôle dynamique des sections

```deluge
// Masquer/afficher une section
hide Section_Donnees_Internes;
show Section_Details;

// Rendre une section en lecture seule
disable Section_Informations;
```

### Mise en page multi-colonnes

- **1 colonne** : Formulaires simples
- **2 colonnes** : Formulaires standards
- **3 colonnes** : Formulaires complexes avec beaucoup de champs

---

## Permissions et visibilité

### Niveaux de permission

| Permission | Description |
|------------|-------------|
| **Tous les utilisateurs** | Formulaire accessible publiquement |
| **Utilisateurs connectés** | Nécessite une connexion Zoho |
| **Rôles spécifiques** | Limité à certains rôles |
| **Administrateur** | Uniquement les admins de l'app |

### Permissions par champ

```deluge
// Rendre un champ en lecture seule selon le rôle
if (zoho.loginuserrole != "Admin" && zoho.loginuserrole != "Manager")
{
    disable Montant;
    disable Remise;
}

// Masquer des champs sensibles
if (zoho.loginuserrole == "Agent")
{
    hide Marge;
    hide Cout_Revient;
}
```

### Formulaire public (sans connexion)

```
Paramètres du formulaire → Accès → Lien public
URL générée : https://creatorapp.zoho.eu/owner/app/#Form:Formulaire
```

Options du lien public :
- Activer/désactiver le CAPTCHA
- Message de confirmation personnalisé
- Redirection après soumission
- Limite de soumissions
- Date d'expiration du lien

---

## Bonnes pratiques

1. **Nommage cohérent** : Utiliser `Snake_Case` pour les noms de champs internes
2. **Validation robuste** : Combiner validations natives et Deluge
3. **Performance** : Limiter les appels API dans On User Input (exécuté à chaque changement)
4. **UX** : Utiliser des sections et l'affichage conditionnel pour simplifier les formulaires longs
5. **Sécurité** : Ne jamais exposer de données sensibles dans les formulaires publics
6. **Mobile** : Tester systématiquement le rendu mobile

---

*Voir aussi : [workflows.md](workflows.md) | [deluge-creator.md](deluge-creator.md) | [rapports.md](rapports.md)*
