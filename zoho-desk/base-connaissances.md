# 📚 Zoho Desk - Base de connaissances

> Articles, catégories, portail client et self-service.

## Table des matières

- [Vue d'ensemble](#vue-densemble)
- [Structure](#structure)
- [Création d'articles](#création-darticles)
- [Portail client](#portail-client)
- [SEO et visibilité](#seo-et-visibilité)
- [Maintenance](#maintenance)

---

## Vue d'ensemble

La base de connaissances (Knowledge Base / KB) permet aux clients de trouver des réponses sans contacter le support.

**Bénéfices :**
- Réduction du volume de tickets (20-40% typiquement)
- Support 24/7 sans intervention humaine
- Cohérence des réponses
- Référence pour les agents

---

## Structure

### Organisation hiérarchique

```
Base de connaissances
├── Catégorie : "Démarrage"
│   ├── Section : "Premiers pas"
│   │   ├── Article : "Créer un compte"
│   │   ├── Article : "Configurer son profil"
│   │   └── Article : "Premiers pas avec le produit"
│   └── Section : "Installation"
│       ├── Article : "Installation Windows"
│       ├── Article : "Installation Mac"
│       └── Article : "Installation Linux"
├── Catégorie : "Guides d'utilisation"
│   ├── Section : "Module A"
│   └── Section : "Module B"
├── Catégorie : "FAQ"
│   ├── Article : "Comment réinitialiser mon mot de passe ?"
│   ├── Article : "Quels navigateurs sont supportés ?"
│   └── Article : "Comment contacter le support ?"
├── Catégorie : "Dépannage"
│   ├── Section : "Erreurs courantes"
│   └── Section : "Problèmes de connexion"
└── Catégorie : "Notes de version"
    ├── Article : "Version 3.2 - Février 2026"
    └── Article : "Version 3.1 - Janvier 2026"
```

### Visibilité des catégories

| Visibilité | Description |
|------------|-------------|
| **Publique** | Accessible à tous (clients, visiteurs) |
| **Connecté** | Visible uniquement aux utilisateurs connectés |
| **Agents uniquement** | Documentation interne pour les agents |

---

## Création d'articles

### Champs d'un article

```
Titre                  → Clair et descriptif (optimisé SEO)
Catégorie / Section    → Classification
Contenu                → Éditeur WYSIWYG riche
Pièces jointes         → Images, PDF, fichiers
Tags / Mots-clés       → Pour la recherche
Statut                 → Brouillon / Publié
Auteur                 → Agent qui a rédigé
Visibilité             → Publique / Connecté / Agents
Date d'expiration      → Optionnelle (rappel de mise à jour)
```

### Éditeur de contenu

```
Fonctionnalités de l'éditeur :
- Mise en forme (titres, gras, italique, listes)
- Images (upload ou URL)
- Vidéos intégrées (YouTube, Vimeo)
- Tableaux
- Blocs de code
- Liens internes vers d'autres articles
- Ancres (table des matières)
- HTML brut
```

### Structure recommandée d'un article

```markdown
# Titre de l'article

## Problème / Contexte
Description claire du problème ou de la fonctionnalité.

## Prérequis
- Condition 1
- Condition 2

## Solution / Étapes
1. Première étape avec capture d'écran
2. Deuxième étape
3. Troisième étape

## Résultat attendu
Ce que l'utilisateur devrait voir/obtenir.

## Articles liés
- [Article connexe 1](lien)
- [Article connexe 2](lien)

## Besoin d'aide ?
Si le problème persiste, [contactez notre support](lien).
```

### Processus de publication

```
1. Agent rédige un brouillon
2. Revue par un pair ou le manager
3. Publication
4. Feedback des clients (utile/pas utile)
5. Mise à jour si nécessaire
```

---

## Portail client

### Configuration

```
Configuration → Centre d'aide → Personnaliser

Éléments personnalisables :
- Logo et couleurs (thème)
- Nom du portail
- URL personnalisée (help.entreprise.com)
- Page d'accueil (catégories en vedette)
- Formulaire de soumission de ticket
- Barre de recherche
- Communauté (forum optionnel)
```

### Fonctionnalités du portail

```
Pour les clients :
├── Rechercher dans la base de connaissances
├── Parcourir les catégories
├── Lire les articles
├── Voter : "Cet article était-il utile ?" (👍/👎)
├── Commenter les articles
├── Soumettre un ticket
├── Suivre ses tickets existants
├── Consulter l'historique de ses demandes
└── Communauté / Forum (optionnel)
```

### Domaine personnalisé

```
Configuration → Centre d'aide → Domaine

Par défaut : desk.zoho.eu/portal/votreentreprise
Personnalisé : help.votreentreprise.com

Configuration DNS :
  CNAME : help.votreentreprise.com → desk.zoho.eu
  + Certificat SSL automatique
```

---

## SEO et visibilité

### Optimisation SEO

```
Pour chaque article :
- Titre optimisé (contient les mots-clés)
- Meta description (résumé de 160 caractères)
- URL propre (slug lisible)
- Titres H1/H2/H3 structurés
- Images avec alt text
- Liens internes entre articles

Paramètres globaux :
- Sitemap XML automatique
- Indexation par Google autorisée/bloquée
- Balise canonical
```

### Recherche interne

```
La barre de recherche du portail utilise :
- Recherche full-text dans le titre et le contenu
- Correspondance avec les tags
- Suggestions pendant la frappe
- Résultats classés par pertinence

Zia (IA) :
- Suggestion automatique d'articles aux clients qui tapent un ticket
- "Avant de soumettre, avez-vous consulté cet article ?"
```

---

## Maintenance

### Métriques à suivre

```
Rapports → Base de connaissances

Métriques clés :
- Articles les plus consultés
- Articles les moins consultés (à supprimer ou améliorer ?)
- Taux de "utile" vs "pas utile"
- Recherches sans résultat (= articles à créer)
- Tickets créés après consultation KB (= article insuffisant)
```

### Cycle de mise à jour

```
Revue mensuelle :
1. Vérifier les articles signalés "pas utile"
2. Analyser les recherches sans résultat
3. Mettre à jour les articles obsolètes
4. Créer des articles pour les questions fréquentes

Revue à chaque release :
1. Mettre à jour les guides impactés
2. Créer un article "Notes de version"
3. Archiver les articles de versions obsolètes
```

### Workflow de contribution

```
Agent résout un ticket avec une solution réutilisable :
  1. L'agent rédige un brouillon d'article depuis le ticket
  2. Le manager revoit et publie
  3. L'article est lié au ticket pour référence

Automatisation possible :
  Workflow : Si un ticket est résolu ET tag "à documenter"
  → Créer une tâche pour rédiger un article KB
```

---

## Bonnes pratiques

1. **Titre clair** : Le client doit comprendre le sujet en lisant le titre
2. **Captures d'écran** : Illustrer chaque étape avec des images annotées
3. **Simplicité** : Écrire pour un public non technique
4. **Liens** : Relier les articles entre eux pour la navigation
5. **Feedback** : Réagir aux votes négatifs en améliorant l'article
6. **Mise à jour** : Définir un responsable et un calendrier de revue

---

*Voir aussi : [tickets.md](tickets.md) | [configuration.md](configuration.md) | [rapports.md](rapports.md)*
