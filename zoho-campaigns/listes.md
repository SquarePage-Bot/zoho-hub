# Listes de Contacts et Segments

## Listes de diffusion

### Créer une liste

```
Contacts → Listes de diffusion → + Nouvelle liste

Champs :
- Nom : Newsletter mensuelle
- Description : Abonnés à la newsletter corporate
- Signupform : Associer un formulaire d'inscription
- Confirmation : Double opt-in activé
```

### Types de listes

| Type | Description | Exemple |
|------|-------------|---------|
| **Opt-in** | Contacts ayant donné leur consentement | Newsletter, blog digest |
| **Transactionnelle** | Clients existants (relation commerciale) | Notifications de commande |
| **Interne** | Équipe interne | Communications internes |

### Importer des contacts

#### Sources d'import

| Source | Méthode |
|--------|---------|
| **Fichier CSV/XLS** | Upload direct avec mapping des colonnes |
| **Zoho CRM** | Synchronisation automatique |
| **Zoho Forms** | Formulaire → Liste automatique |
| **Copier-coller** | Coller des emails directement |
| **API** | Import programmatique |

#### Processus d'import CSV

```
1. Contacts → Importer → Fichier CSV
2. Mapper les colonnes :
   Email        → Adresse email
   Prénom       → First Name
   Nom          → Last Name
   Entreprise   → Company
   Ville        → City
   
3. Options :
   ☑ Mettre à jour les contacts existants
   ☑ Ignorer les doublons
   ☐ Envoyer un email de confirmation

4. Validation :
   - Vérification syntaxe email
   - Détection des doublons
   - Suppression des bounces connus
```

#### Exemple de fichier CSV

```csv
email,prenom,nom,entreprise,ville,interet
marie@example.fr,Marie,Dupont,TechCorp,Paris,SaaS
paul@example.fr,Paul,Martin,DesignLab,Lyon,Design
sophie@example.fr,Sophie,Bernard,DataFlow,Marseille,Analytics
```

### Champs de contact

#### Champs par défaut

| Champ | Type | Description |
|-------|------|-------------|
| Email | Email | Adresse email (identifiant unique) |
| First Name | Texte | Prénom |
| Last Name | Texte | Nom de famille |
| Company | Texte | Entreprise |
| Phone | Téléphone | Numéro de téléphone |
| Country | Texte | Pays |
| City | Texte | Ville |

#### Champs personnalisés

```
Paramètres → Champs personnalisés → + Nouveau champ

Types disponibles :
- Texte (ligne, paragraphe)
- Nombre (entier, décimal)
- Date
- Liste déroulante
- Case à cocher
- URL

Exemple :
Nom : "Secteur d'activité"
Type : Liste déroulante
Valeurs : Tech | Finance | Santé | Éducation | Commerce | Autre
```

### Formulaires d'inscription

#### Créer un formulaire

```
Contacts → Formulaires → + Nouveau formulaire

Types :
- Embedded (intégré dans une page web)
- Popup (fenêtre modale)
- Landing page (page autonome hébergée par Zoho)
```

#### Exemple de formulaire intégrable

```html
<!-- Code à copier sur votre site -->
<div id="zoho-campaigns-form">
  <form action="https://campaigns.zoho.eu/..." method="POST">
    <label>Email *</label>
    <input type="email" name="CONTACT_EMAIL" required>
    
    <label>Prénom</label>
    <input type="text" name="FIRSTNAME">
    
    <label>Centre d'intérêt</label>
    <select name="CUSTOM_INTEREST">
      <option>SaaS</option>
      <option>E-commerce</option>
      <option>Marketing</option>
    </select>
    
    <label>
      <input type="checkbox" required>
      J'accepte de recevoir des emails marketing
    </label>
    
    <button type="submit">S'inscrire</button>
  </form>
</div>
```

#### Double opt-in

```
Workflow double opt-in :
1. L'utilisateur remplit le formulaire
2. → Email de confirmation envoyé automatiquement
3. L'utilisateur clique sur le lien de confirmation
4. → Contact ajouté à la liste avec statut "Confirmé"

Avantages :
- Meilleure qualité de liste
- Conformité RGPD
- Meilleure délivrabilité
```

## Segments

### Concept

Les segments sont des filtres dynamiques appliqués à une liste. Les contacts entrent et sortent automatiquement du segment en fonction des critères.

### Créer un segment

```
Contacts → Segments → + Nouveau segment

Nom : "Clients actifs premium"
Liste source : Tous les contacts

Critères (AND/OR) :
```

### Types de critères

#### Données de profil
```
Exemples :
- Ville = "Paris"
- Secteur d'activité = "Tech"
- Date d'inscription < 6 mois
- Champ personnalisé "Plan" = "Premium"
```

#### Comportement email
```
Exemples :
- A ouvert un email dans les 30 derniers jours
- A cliqué sur un lien dans la dernière campagne
- N'a pas ouvert d'email depuis 90 jours
- A ouvert plus de 5 emails ce mois
```

#### Activité e-commerce (si intégré)
```
Exemples :
- A acheté dans les 30 derniers jours
- Panier moyen > 100€
- Catégorie préférée = "Électronique"
- N'a pas acheté depuis 60 jours
```

### Exemples de segments courants

#### Segment : Contacts engagés
```
Critères (AND) :
- A ouvert au moins 1 email dans les 30 derniers jours
- N'est pas désabonné
- Email valide (pas de bounce)

Usage : Cibler les contacts réceptifs pour les campagnes importantes
```

#### Segment : Contacts inactifs
```
Critères (AND) :
- N'a ouvert aucun email depuis 90 jours
- Inscrit depuis plus de 30 jours
- N'est pas désabonné

Usage : Campagne de réengagement ou nettoyage de liste
```

#### Segment : Leads chauds
```
Critères (AND) :
- A cliqué sur un lien dans les 7 derniers jours
- A visité la page tarifs (si tracking web actif)
- Secteur = "Tech" OU "Finance"

Usage : Transmettre au commercial via Zoho CRM
```

#### Segment géographique
```
Critères (OR) :
- Pays = "France"
- Pays = "Belgique"
- Pays = "Suisse"

Usage : Campagne francophone avec contenu localisé
```

## Nettoyage et hygiène de liste

### Gestion des bounces

| Type de bounce | Action |
|----------------|--------|
| **Hard bounce** | Email invalide → Supprimé automatiquement |
| **Soft bounce** | Boîte pleine, serveur down → 3 tentatives puis suppression |

### Processus de nettoyage recommandé

```
Fréquence : Mensuelle

1. Supprimer les hard bounces (automatique)
2. Identifier les contacts inactifs (segment > 90 jours sans ouverture)
3. Envoyer une campagne de réengagement aux inactifs
4. Supprimer les contacts qui ne réagissent pas après 2 relances
5. Vérifier et fusionner les doublons
6. Valider les emails avec un outil de vérification
```

### Désabonnement

```
Chaque email contient obligatoirement un lien de désinscription.

Options de désinscription :
- Se désabonner de cette liste uniquement
- Se désabonner de toutes les listes
- Modifier les préférences (fréquence, sujets)

Page de désinscription personnalisable :
Paramètres → Désinscription → Personnaliser la page
```

## Synchronisation Zoho CRM

### Configurer la synchronisation

```
Paramètres → Intégrations → Zoho CRM → Configurer

Options :
- Direction : CRM → Campaigns / Campaigns → CRM / Bidirectionnelle
- Fréquence : Temps réel / Horaire / Quotidienne
- Mapping des champs : Associer les champs CRM aux champs Campaigns

Filtres CRM :
- Module : Leads / Contacts / Les deux
- Vue : Tous / Vue personnalisée
- Critère : Statut du lead = "Qualifié"
```

### Sync bidirectionnelle

```
CRM → Campaigns :
- Nouveau lead qualifié → Ajouté à la liste "Prospects"
- Lead converti en client → Déplacé vers liste "Clients"

Campaigns → CRM :
- Contact ouvre un email → Activité enregistrée dans CRM
- Contact clique sur "Demande de démo" → Tâche créée pour le commercial
- Contact se désabonne → Champ CRM mis à jour
```

## Tags et organisation

### Utiliser les tags

```
Les tags permettent une catégorisation flexible en plus des listes :

Tags exemples :
- Source : "salon-2026", "webinar-mars", "site-web"
- Intérêt : "produit-A", "produit-B", "service-premium"
- Statut : "VIP", "prospect-chaud", "partenaire"

Avantage : Un contact peut avoir plusieurs tags
contrairement aux listes qui sont plus rigides
```
