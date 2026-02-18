# Partage et Collaboration

## Présentation

Zoho Analytics offre des options de partage flexibles pour collaborer en interne, partager avec des clients ou intégrer des rapports dans des applications.

## Niveaux de partage

### Permissions

| Permission | Peut voir | Peut filtrer | Peut modifier | Peut supprimer |
|------------|-----------|-------------|---------------|----------------|
| **Lecture seule** | ✅ | ✅ | ❌ | ❌ |
| **Lecture + Export** | ✅ | ✅ | ❌ | ❌ |
| **Écriture** | ✅ | ✅ | ✅ | ❌ |
| **Contrôle total** | ✅ | ✅ | ✅ | ✅ |

### Partage par entité

| Entité | Ce qui est partagé |
|--------|-------------------|
| **Rapport** | Un seul rapport |
| **Tableau de bord** | Un dashboard complet |
| **Workspace** | Tout l'espace de travail |
| **Dossier** | Un groupe de rapports |

## Partager avec des utilisateurs

### Utilisateurs internes (même organisation)

```
Rapport/Dashboard → Partager → Utilisateurs

1. Email(s) : marie@entreprise.fr, paul@entreprise.fr
2. Permission : Lecture seule
3. Options :
   ☑ Notifier par email
   ☐ Autoriser le re-partage
   ☐ Autoriser l'export
```

### Utilisateurs externes (clients, partenaires)

```
Rapport/Dashboard → Partager → Utilisateurs externes

1. Email(s) : client@partenaire.fr
2. Permission : Lecture seule
3. Options :
   ☑ Le destinataire doit créer un compte Zoho (gratuit)
   ☐ Accès sans compte (lien privé)
   Expiration : 30 jours (optionnel)
```

## Liens de partage

### Lien privé

```
Rapport → Partager → Obtenir le lien

Type : Privé (authentification requise)
URL : https://analytics.zoho.eu/open-view/xxxxx

Seules les personnes avec qui le rapport est partagé
peuvent y accéder via ce lien.
```

### Lien public

```
Rapport → Partager → Rendre public

Type : Public (accessible à tous avec le lien)
URL : https://analytics.zoho.eu/open-view/yyyyy

⚠️ Attention : Toute personne ayant le lien peut voir les données.
Utiliser pour : Rapports non sensibles, dashboards de suivi public.

Options :
☑ Permettre le filtrage
☐ Permettre l'export
☐ Afficher la barre d'outils
Mot de passe : (optionnel)
```

## Intégration (Embed)

### Intégrer dans un site web

```html
<!-- iFrame pour intégrer un rapport -->
<iframe 
  src="https://analytics.zoho.eu/open-view/xxxxx?EMBED=true"
  width="100%"
  height="600"
  frameborder="0"
  allowfullscreen>
</iframe>
```

### Options d'intégration

```
Paramètres d'embed :
?EMBED=true            → Mode intégré (sans navigation Zoho)
&TOOLBAR=false         → Masquer la barre d'outils
&FILTERS=true          → Afficher les filtres
&TITLE=false           → Masquer le titre
&THEME=dark            → Thème sombre
&CRITERIA="Region"='IDF' → Filtre pré-appliqué
```

### Intégrer dans une application (SDK)

```javascript
// JavaScript SDK pour intégration avancée
ZohoAnalytics.init({
  serverUrl: "https://analytics.zoho.eu",
  reportUrl: "/open-view/xxxxx",
  width: "100%",
  height: "600px",
  theme: "light",
  toolbar: false,
  filters: {
    "Region": "Île-de-France",
    "Periode": "Ce mois"
  },
  onLoad: function() {
    console.log("Rapport chargé");
  },
  onDrillDown: function(data) {
    console.log("Drill-down:", data);
  }
});
```

## White-labeling (Plan Enterprise)

### Personnalisation de la marque

```
Paramètres → White Label

Options :
- Domaine personnalisé : analytics.monentreprise.fr
- Logo : Remplacer le logo Zoho par le vôtre
- Couleurs : Thème aux couleurs de l'entreprise
- Favicon : Icône personnalisée
- Email de notification : noreply@monentreprise.fr
```

## Export et distribution

### Export manuel

```
Rapport → Exporter

Formats :
- PDF : Avec mise en page, logo, date
- Excel : Données brutes + graphiques
- CSV : Données uniquement
- Image : PNG ou JPG du graphique
- HTML : Page web autonome
```

### Distribution programmée par email

```
Rapport/Dashboard → Planifier → + Nouveau

Configuration :
- Destinataires : direction@entreprise.fr
- Fréquence : Tous les lundis à 8h00
- Format : PDF
- Objet : "Rapport hebdomadaire - Semaine {week}"
- Message personnalisé : "Bonjour, voici le rapport..."
- Filtres appliqués : Ce mois, Toutes régions

Options avancées :
☑ Envoyer même si les données n'ont pas changé
☐ Envoyer uniquement si une alerte est déclenchée
☑ Inclure un résumé texte dans le corps de l'email
```

### Distribution conditionnelle

```
Envoyer le rapport uniquement si :
- CA du mois < Objectif → Alerte au directeur
- Taux de satisfaction < 80% → Alerte au responsable support
- Stock produit < seuil → Alerte au responsable logistique
```

## Collaboration

### Commentaires

```
Rapport → Commentaires (💬)

Fonctionnalités :
- Ajouter un commentaire sur un rapport
- Mentionner un utilisateur @marie
- Joindre une capture d'écran
- Répondre à un commentaire
- Résoudre/fermer un fil de discussion
```

### Annotations

```
Sur un graphique, annoter un point de données :

Exemple :
Point : Mars 2026, CA = 180 000€ (baisse)
Annotation : "Impact grève transport - 2 semaines d'activité réduite"

Les annotations sont visibles par tous les utilisateurs partagés.
```

### Favoris et organisation

```
Organisation :
- ⭐ Favoris : Épingler les rapports les plus utilisés
- 📁 Dossiers : Classer les rapports par thème
- 🏷️ Tags : Étiqueter pour recherche rapide
- 🔍 Recherche : Trouver un rapport par nom ou contenu
```

## Sécurité des données

### Row-Level Security (RLS)

Filtrer les données visibles selon l'utilisateur :

```
Paramètres → Sécurité → Row-Level Security

Règle : 
SI utilisateur = "marie@entreprise.fr"
  → Afficher uniquement les lignes WHERE Region = "IDF"

SI utilisateur = "paul@entreprise.fr"
  → Afficher uniquement les lignes WHERE Region = "AURA"

SI rôle = "Directeur"
  → Afficher toutes les lignes

Résultat : Marie ne voit que les données IDF,
même sur un dashboard partagé avec toute l'équipe.
```

### Audit trail

```
Paramètres → Audit → Journal d'activité

Événements tracés :
- Qui a consulté quel rapport
- Qui a modifié quelles données
- Qui a partagé avec qui
- Exports effectués
- Connexions et tentatives échouées

Rétention : 90 jours (configurable)
```

## Portail de reporting client

### Créer un portail

```
Paramètres → Portails → + Nouveau portail

Configuration :
- Nom : "Espace client TechCorp"
- URL : https://reporting.techcorp.fr (white-label)
- Logo et couleurs personnalisés
- Dashboards exposés : Sélection spécifique
- Utilisateurs : Inviter par email

Chaque client voit uniquement SES données (via RLS).
```

## Bonnes pratiques

1. **Principe du moindre privilège** : Donner les permissions minimales nécessaires
2. **Row-Level Security** : Indispensable quand plusieurs clients/équipes partagent le même dashboard
3. **Liens publics avec prudence** : Jamais pour des données sensibles
4. **Planifier les distributions** : Automatiser plutôt qu'envoyer manuellement
5. **Auditer régulièrement** : Vérifier qui a accès à quoi
6. **Favoriser l'embed** : Intégrer dans les outils existants plutôt que forcer un changement d'outil
7. **Annoter** : Documenter les anomalies directement sur les graphiques
