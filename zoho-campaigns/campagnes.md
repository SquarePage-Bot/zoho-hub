# Campagnes Email

## Créer une campagne

### Étapes de création

```
Campagnes → + Nouvelle campagne

Étape 1 : Informations de base
  - Nom : "Newsletter Mars 2026"
  - Type : Email classique
  - Objet : "Les tendances tech du mois 🚀"
  - Nom d'expéditeur : "Marie - TechCorp"
  - Email d'expéditeur : newsletter@techcorp.fr
  - Email de réponse : contact@techcorp.fr

Étape 2 : Destinataires
  - Liste(s) : Newsletter principale
  - Segment(s) : Contacts actifs (optionnel)
  - Exclure : Liste des désabonnés

Étape 3 : Contenu
  - Choisir un template ou créer depuis zéro
  - Éditeur drag & drop ou HTML

Étape 4 : Révision et envoi
  - Prévisualiser (desktop + mobile)
  - Envoyer un test
  - Programmer ou envoyer immédiatement
```

### Objet de l'email (Subject Line)

#### Bonnes pratiques

| Pratique | Exemple |
|----------|---------|
| **Court et percutant** | "3 outils qui changent tout" (< 50 caractères) |
| **Personnalisation** | "Marie, votre rapport est prêt" |
| **Émojis (avec modération)** | "Nouvelle fonctionnalité 🎉" |
| **Urgence** | "Dernières heures : -30% sur tout" |
| **Question** | "Prêt pour votre transformation digitale ?" |
| **Chiffres** | "5 erreurs à éviter en email marketing" |

#### Balises de personnalisation

```
Balises disponibles dans l'objet et le corps :

$[FNAME]$        → Prénom du contact
$[LNAME]$        → Nom du contact
$[COMPANY]$      → Entreprise
$[CUSTOM:champ]$ → Champ personnalisé
$[DATE]$         → Date du jour
$[CITY]$         → Ville

Exemple d'objet :
"$[FNAME]$, découvrez les nouveautés de mars"
→ "Marie, découvrez les nouveautés de mars"
```

### Pré-header

```
Le pré-header est le texte affiché après l'objet dans la boîte de réception.

Objet : "Les tendances tech du mois 🚀"
Pré-header : "IA, cybersécurité, cloud : notre sélection d'articles"

Conseil : Compléter l'objet, ne pas le répéter
```

## A/B Testing

### Concept

L'A/B testing permet de tester 2 variantes (ou plus) sur un échantillon avant d'envoyer la version gagnante au reste de la liste.

### Configurer un A/B test

```
Campagnes → + Nouvelle campagne → A/B Testing

Étape 1 : Choisir l'élément à tester
  - Objet de l'email
  - Nom de l'expéditeur
  - Contenu de l'email
  - Heure d'envoi

Étape 2 : Créer les variantes
  Variante A : "5 astuces pour booster vos ventes"
  Variante B : "$[FNAME]$, boostez vos ventes avec ces 5 astuces"

Étape 3 : Paramètres du test
  - Taille de l'échantillon : 20% de la liste (10% par variante)
  - Critère de victoire : Taux d'ouverture / Taux de clic / Taux de conversion
  - Durée du test : 4 heures
  - Action après le test : Envoyer automatiquement la variante gagnante

Étape 4 : Destinataires
  - Liste : Newsletter (10 000 contacts)
  - Échantillon : 2 000 (1 000 par variante)
  - Reste : 8 000 (recevront la variante gagnante)
```

### Exemple d'A/B test complet

```
Test : Objet de la newsletter de mars

Variante A : "Newsletter Mars : IA, Cloud et Cybersécurité"
Variante B : "Marie, 3 tendances tech à ne pas manquer en mars 🚀"

Liste : 20 000 contacts
Échantillon : 4 000 (2 000 par variante)
Durée : 6 heures
Critère : Taux d'ouverture

Résultats après 6h :
┌────────────┬──────────────┬──────────────┬──────────────┐
│            │ Taux ouvert. │ Taux clic    │ Désinscript. │
├────────────┼──────────────┼──────────────┼──────────────┤
│ Variante A │ 22.3%        │ 3.1%         │ 0.2%         │
│ Variante B │ 28.7% ✅     │ 4.5%         │ 0.1%         │
└────────────┴──────────────┴──────────────┴──────────────┘

→ Variante B gagnante → Envoyée aux 16 000 contacts restants
```

### Éléments testables

| Élément | Quoi tester | Impact principal |
|---------|-------------|-----------------|
| **Objet** | Ton, longueur, émojis, personnalisation | Taux d'ouverture |
| **Expéditeur** | Nom de personne vs marque | Taux d'ouverture |
| **Contenu** | Layout, images, texte, CTA | Taux de clic |
| **Heure d'envoi** | Matin vs après-midi, jour de semaine | Taux d'ouverture |
| **CTA** | Texte du bouton, couleur, position | Taux de clic |

## Programmation d'envoi

### Options d'envoi

```
1. Envoi immédiat
   → Envoyé dès validation

2. Programmé
   → Date et heure spécifiques
   → Exemple : 10/03/2026 à 10h00 (Europe/Paris)

3. Envoi selon le fuseau horaire du destinataire
   → Chaque contact reçoit l'email à la même heure locale
   → Exemple : 10h00 heure locale pour chaque destinataire

4. Envoi optimal (Smart Send)
   → Zoho analyse l'historique d'ouverture de chaque contact
   → Envoie à l'heure où le contact est le plus susceptible d'ouvrir
```

### Meilleurs moments d'envoi (benchmarks)

```
B2B :
  Meilleurs jours : Mardi, Mercredi, Jeudi
  Meilleures heures : 10h-11h, 14h-15h
  Éviter : Lundi matin, Vendredi après-midi

B2C :
  Meilleurs jours : Mardi, Jeudi, Samedi
  Meilleures heures : 8h-9h, 12h-13h, 20h-21h
  Éviter : Dimanche soir
```

## Contenu dynamique

### Blocs conditionnels

Afficher du contenu différent selon le profil du contact :

```html
<!-- Contenu conditionnel dans l'éditeur -->

SI [Secteur] = "Tech" :
  Afficher → "Découvrez nos solutions Cloud"
  
SI [Secteur] = "Finance" :
  Afficher → "Conformité et sécurité des données"
  
SINON :
  Afficher → "Nos solutions pour votre entreprise"
```

### Exemple de contenu dynamique

```
Email : Promotion de rentrée

Pour les clients Premium :
  → "-30% sur le renouvellement annuel"
  → CTA : "Renouveler maintenant"

Pour les prospects :
  → "Essai gratuit 30 jours"
  → CTA : "Commencer l'essai"

Pour les clients inactifs :
  → "Vous nous manquez ! -50% pour revenir"
  → CTA : "Réactiver mon compte"
```

## Emails transactionnels vs marketing

| Caractéristique | Marketing | Transactionnel |
|----------------|-----------|----------------|
| **Consentement** | Opt-in requis | Relation commerciale suffit |
| **Désinscription** | Lien obligatoire | Non requis |
| **Contenu** | Promotionnel | Informatif (commande, facture) |
| **Fréquence** | Planifiée | Déclenché par événement |
| **Exemple** | Newsletter, promo | Confirmation de commande |

## Envoi multicanal

### Email + SMS

```
Campagne multicanal :
1. Envoyer l'email
2. Attendre 24h
3. SI pas ouvert → Envoyer un SMS de rappel
4. SI ouvert mais pas cliqué → Envoyer email de relance
```

### Email + Réseaux sociaux

```
Publier simultanément :
☑ Email → Liste principale
☑ Facebook → Page entreprise
☑ Twitter → Compte @techcorp
☑ LinkedIn → Page entreprise

Le contenu est automatiquement adapté à chaque plateforme.
```

## Bonnes pratiques

1. **Segmenter** : Ne jamais envoyer le même email à toute la base
2. **Tester** : Toujours faire un A/B test sur les campagnes importantes
3. **Personnaliser** : Utiliser le prénom et des contenus dynamiques
4. **Optimiser pour mobile** : 60%+ des emails sont lus sur mobile
5. **Un seul CTA principal** : Ne pas disperser l'attention
6. **Envoyer un test** : Vérifier sur plusieurs clients email avant l'envoi
7. **Respecter la fréquence** : 1-2 emails/semaine maximum pour la plupart des listes
8. **Nettoyer régulièrement** : Supprimer les bounces et inactifs
