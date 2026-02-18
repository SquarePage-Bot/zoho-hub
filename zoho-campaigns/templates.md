# Templates Email

## Présentation

Zoho Campaigns propose un éditeur de templates puissant avec des modèles pré-conçus et un éditeur drag & drop pour créer des emails professionnels sans coder.

## Types de templates

| Type | Description | Usage |
|------|-------------|-------|
| **Pré-conçus** | Modèles fournis par Zoho, classés par catégorie | Démarrage rapide |
| **Personnalisés** | Créés avec l'éditeur drag & drop | Templates récurrents |
| **HTML brut** | Code HTML importé | Designs complexes sur mesure |
| **Texte brut** | Email sans formatage | Emails personnels, délivrabilité |
| **Sauvegardés** | Campagnes précédentes réutilisées | Newsletters récurrentes |

## Éditeur Drag & Drop

### Interface

```
┌─────────────────────────────────────────────────────┐
│ Barre d'outils : [Sauvegarder] [Prévisualiser]     │
├──────────────┬──────────────────────────────────────┤
│  Composants  │         Zone d'édition               │
│              │  ┌────────────────────────────────┐   │
│  📝 Texte    │  │        LOGO ENTREPRISE         │   │
│  🖼️ Image    │  │        ━━━━━━━━━━━━━━          │   │
│  🔘 Bouton   │  │  Titre de la newsletter        │   │
│  ─── Sépar.  │  │                                │   │
│  📊 Tableau  │  │  Contenu principal ici...      │   │
│  📱 Social   │  │                                │   │
│  🎥 Vidéo    │  │  [BOUTON CTA]                  │   │
│  📦 Produit  │  │                                │   │
│  💻 Code     │  │  ──────────────────            │   │
│  📐 Layout   │  │  Pied de page / Désinscription │   │
│              │  └────────────────────────────────┘   │
└──────────────┴──────────────────────────────────────┘
```

### Composants disponibles

#### Texte
```
Éditeur riche : gras, italique, liens, listes, alignement
Balises de fusion : $[FNAME]$, $[COMPANY]$, etc.
Polices web-safe : Arial, Helvetica, Georgia, Verdana
Taille, couleur, espacement personnalisables
```

#### Image
```
Sources :
- Upload (max 1 Mo, formats : JPG, PNG, GIF)
- Bibliothèque Zoho
- URL externe
- Stock photos intégrées (Unsplash)

Options :
- Alt text (obligatoire pour l'accessibilité)
- Lien cliquable
- Dimensions responsive
- Bordures et marges
```

#### Bouton (CTA)
```
Propriétés :
- Texte : "Découvrir maintenant"
- URL : https://monsite.fr/offre
- Couleur de fond : #0066CC
- Couleur du texte : #FFFFFF
- Bordure arrondie : 4px
- Largeur : Auto / Pleine largeur
- Alignement : Centre
```

#### Layout (Colonnes)
```
Dispositions disponibles :
[──────────────────] 1 colonne (100%)
[────────][────────] 2 colonnes (50/50)
[──────][────────── ] 2 colonnes (33/67)
[────][────][────]   3 colonnes (33/33/33)
[───][───][───][───] 4 colonnes (25/25/25/25)
```

#### Réseaux sociaux
```
Icônes cliquables vers vos profils :
Facebook | Twitter | LinkedIn | Instagram | YouTube
Style : Couleur / Noir et blanc / Personnalisé
Forme : Rond / Carré / Carré arrondi
```

#### Produit (E-commerce)
```
Bloc produit avec :
- Image du produit
- Nom
- Prix (barré + nouveau prix)
- Description courte
- Bouton "Acheter"

Source : Manuel ou sync Zoho Commerce / Shopify
```

## Structure d'un template newsletter

### Exemple complet

```
┌─────────────────────────────────────────┐
│              [LOGO]                      │
│         TechCorp Newsletter             │
│    Mars 2026 • techcorp.fr              │
├─────────────────────────────────────────┤
│                                         │
│  Bonjour $[FNAME]$,                     │
│                                         │
│  Voici les actualités du mois...        │
│                                         │
├────────────────┬────────────────────────┤
│  [IMAGE]       │  Article 1             │
│                │  Description courte... │
│                │  [Lire la suite →]     │
├────────────────┼────────────────────────┤
│  Article 2     │  [IMAGE]               │
│  Description.. │                        │
│  [Lire →]      │                        │
├────────────────┴────────────────────────┤
│                                         │
│  ┌─────────────────────────────────┐    │
│  │    OFFRE SPÉCIALE -20%          │    │
│  │    Code : MARS2026              │    │
│  │    [EN PROFITER]                │    │
│  └─────────────────────────────────┘    │
│                                         │
├─────────────────────────────────────────┤
│  📘 📱 💼 🎥                             │
│  Facebook Twitter LinkedIn YouTube      │
│                                         │
│  © 2026 TechCorp - Tous droits réservés │
│  123 Rue Example, 75001 Paris           │
│  Se désinscrire | Préférences           │
└─────────────────────────────────────────┘
```

## Design responsive

### Bonnes pratiques mobile

```
Largeur recommandée : 600px (max)
Taille de police : 16px minimum pour le corps
Boutons : 44px de hauteur minimum (zone tactile)
Images : Largeur max 100%, redimensionnement auto
Colonnes : Empilées sur mobile (2 col → 1 col)

Prévisualisation :
Éditeur → Prévisualiser → 📱 Mobile / 💻 Desktop
```

### Exemple de CSS responsive intégré

```html
<style>
@media only screen and (max-width: 600px) {
  .container { width: 100% !important; }
  .column { display: block !important; width: 100% !important; }
  .button { width: 100% !important; }
  .hide-mobile { display: none !important; }
}
</style>
```

## Templates HTML personnalisés

### Importer un template HTML

```
Templates → + Nouveau → Importer HTML

Règles :
- HTML inline styles (pas de <style> externe)
- Tables pour la mise en page (compatibilité email)
- Images hébergées (pas de base64)
- Largeur max : 600px
- Tester sur : Gmail, Outlook, Apple Mail, Yahoo
```

### Structure HTML type

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
</head>
<body style="margin:0; padding:0; background-color:#f4f4f4;">
  <table width="100%" cellpadding="0" cellspacing="0">
    <tr>
      <td align="center">
        <table width="600" cellpadding="0" cellspacing="0" 
               style="background-color:#ffffff;">
          
          <!-- Header -->
          <tr>
            <td style="padding:20px; text-align:center;">
              <img src="https://monsite.fr/logo.png" alt="Logo" width="200">
            </td>
          </tr>
          
          <!-- Contenu -->
          <tr>
            <td style="padding:30px; font-family:Arial,sans-serif; 
                        font-size:16px; line-height:1.6; color:#333333;">
              <h1 style="color:#0066CC;">Bonjour $[FNAME|there]$ !</h1>
              <p>Votre contenu ici...</p>
              
              <!-- CTA -->
              <table cellpadding="0" cellspacing="0" style="margin:20px auto;">
                <tr>
                  <td style="background-color:#0066CC; border-radius:4px; 
                             padding:12px 30px;">
                    <a href="https://monsite.fr/offre" 
                       style="color:#ffffff; text-decoration:none; 
                              font-weight:bold;">
                      Découvrir →
                    </a>
                  </td>
                </tr>
              </table>
            </td>
          </tr>
          
          <!-- Footer -->
          <tr>
            <td style="padding:20px; text-align:center; font-size:12px; 
                        color:#999999; background-color:#f8f8f8;">
              © 2026 TechCorp<br>
              <a href="$[UNSUBSCRIBE]$">Se désinscrire</a> | 
              <a href="$[PREFERENCES]$">Préférences</a>
            </td>
          </tr>
          
        </table>
      </td>
    </tr>
  </table>
</body>
</html>
```

## Bibliothèque de templates

### Catégories pré-conçues

| Catégorie | Templates | Usage |
|-----------|-----------|-------|
| **Newsletter** | 15+ | Actualités récurrentes |
| **Promotion** | 20+ | Offres, soldes, événements |
| **E-commerce** | 10+ | Produits, abandon de panier |
| **Bienvenue** | 8+ | Onboarding, première interaction |
| **Événement** | 10+ | Invitations, rappels, suivi |
| **Transactionnel** | 5+ | Confirmation, facture, expédition |
| **Fêtes** | 12+ | Noël, Nouvel An, Saint-Valentin |

## Gestion des templates

```
Templates → Bibliothèque

Actions :
- 📋 Dupliquer : Copier un template comme base
- ✏️ Modifier : Éditer le template
- 👁️ Prévisualiser : Aperçu desktop + mobile
- 📤 Exporter : Télécharger en HTML
- 🗑️ Supprimer : Retirer de la bibliothèque
- 📁 Organiser : Classer par dossier/tag
```

## Bonnes pratiques

1. **Ratio texte/image** : 60% texte, 40% images (délivrabilité)
2. **Alt text** : Toujours renseigner le texte alternatif des images
3. **Prévisualiser** : Tester sur desktop ET mobile avant envoi
4. **Un CTA clair** : Un seul objectif principal par email
5. **Poids** : Garder l'email < 100 Ko (sans images)
6. **Polices web-safe** : Utiliser Arial, Helvetica, Georgia
7. **Dark mode** : Tester l'affichage en mode sombre
8. **Footer complet** : Adresse physique + lien désinscription obligatoire
