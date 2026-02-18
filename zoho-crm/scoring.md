# 📈 Zoho CRM - Scoring

> Système de notation des leads et contacts pour prioriser les efforts commerciaux.

## Concept

Le scoring attribue un **score numérique** à chaque lead/contact basé sur :
- **Données démographiques** (profil) : industrie, taille, localisation
- **Données comportementales** (engagement) : emails ouverts, pages visitées, formulaires remplis

Score élevé = prospect chaud, à contacter en priorité.

## Types de scoring

### 1. Scoring par règles (Rule-based)

Définition manuelle des critères et points.

**Exemples de règles :**

| Critère | Points | Type |
|---------|--------|------|
| Industrie = "Tech" | +15 | Profil |
| Employés > 50 | +10 | Profil |
| Pays = "France" | +5 | Profil |
| Email ouvert | +5 | Comportement |
| Lien cliqué dans email | +10 | Comportement |
| Page pricing visitée | +20 | Comportement |
| Formulaire soumis | +25 | Comportement |
| Pas d'activité depuis 30j | -15 | Comportement |
| Email bounced | -20 | Négatif |
| Désinscription | -50 | Négatif |

### 2. Scoring prédictif (Zia AI)

Disponible en Enterprise+ : Zia analyse automatiquement les patterns des deals gagnés pour scorer les leads.

**Zia prend en compte :**
- Historique des conversions
- Interactions (emails, appels, réunions)
- Similitude avec les clients existants
- Données comportementales

## Configuration du scoring

### Création d'une règle de scoring

```
Setup → Automatisation → Scoring Rules → Nouvelle règle

Module : Leads
Nom : "Score Lead Qualification"
```

### Règles de scoring positif

```
Critère 1 : Lead_Source = "Site Web"
  → +20 points

Critère 2 : Industry IN ("Technology", "SaaS", "E-commerce")
  → +15 points

Critère 3 : Annual_Revenue > 1000000
  → +10 points

Critère 4 : Country = "France" OR Country = "Belgique"
  → +5 points

Critère 5 : No_of_Employees > 20
  → +5 points
```

### Règles de scoring négatif

```
Critère 1 : Email = vide
  → -10 points

Critère 2 : Phone = vide AND Mobile = vide
  → -5 points

Critère 3 : Lead_Status = "Non intéressé"
  → -30 points

Critère 4 : Days_since_last_activity > 60
  → -20 points
```

### Scoring comportemental (SalesSignals)

| Signal | Points |
|--------|--------|
| Email ouvert | +3 |
| Email cliqué | +7 |
| Réponse email reçue | +15 |
| Appel complété > 2min | +10 |
| Formulaire web soumis | +20 |
| Chat en direct initié | +15 |
| Document consulté (Zoho Writer) | +10 |
| Survey complété | +5 |
| Visite page (SalesIQ) | +2/page |
| Visite page pricing | +15 |

## Utilisation du score

### Seuils et actions recommandées

| Score | Catégorie | Action |
|-------|-----------|--------|
| 0-20 | Froid | Nurturing par email automatique |
| 21-50 | Tiède | Suivi commercial sous 7 jours |
| 51-80 | Chaud | Contact sous 48h |
| 81+ | Très chaud | Contact immédiat, priorité maximale |

### Automatiser avec les workflows

```
Workflow : "Lead chaud - Alerte commerciale"
Déclencheur : Score (quand le score atteint 80)
Module : Leads
Actions :
  1. Email au propriétaire : "Lead chaud à contacter !"
  2. Tâche : "Appeler immédiatement" (priorité Haute)
  3. Mise à jour : Lead_Status = "Contacté"
  4. Tag : "Hot Lead"
```

### Vues basées sur le score

Créer des vues filtrées :
- "Mes leads chauds" : `Score >= 80 AND Owner = current_user`
- "Leads à nurture" : `Score < 20 AND Created_Time > last 30 days`
- "Leads en déclin" : `Score decreased in last 7 days`

## Scoring via Deluge

### Lire le score

```deluge
lead = zoho.crm.getRecordById("Leads", leadId);
score = lead.get("Score");
info "Score du lead : " + score;
```

### Scoring personnalisé par fonction

```deluge
// Fonction custom de scoring avancé
// Appelée par workflow à la création/modification du lead

lead = zoho.crm.getRecordById("Leads", leadId);
score = 0;

// --- Scoring Profil ---

// Source
source = ifnull(lead.get("Lead_Source"), "");
sourceScores = Map();
sourceScores.put("Site Web", 20);
sourceScores.put("Référence", 25);
sourceScores.put("LinkedIn", 15);
sourceScores.put("Salon", 10);
sourceScores.put("Achat de liste", -5);
if(sourceScores.containsKey(source))
{
    score = score + sourceScores.get(source);
}

// Industrie
industry = ifnull(lead.get("Industry"), "");
hotIndustries = {"Technology", "SaaS", "Financial Services", "Healthcare"};
if(hotIndustries.contains(industry))
{
    score = score + 15;
}

// Taille entreprise
employees = ifnull(lead.get("No_of_Employees"), 0);
if(employees > 200)
{
    score = score + 15;
}
else if(employees > 50)
{
    score = score + 10;
}
else if(employees > 10)
{
    score = score + 5;
}

// Pays cible
country = ifnull(lead.get("Country"), "");
targetCountries = {"France", "Belgique", "Suisse", "Luxembourg", "Canada"};
if(targetCountries.contains(country))
{
    score = score + 10;
}

// --- Scoring Complétude ---

// Pénalité si infos manquantes
if(lead.get("Email") == null || lead.get("Email") == "")
{
    score = score - 10;
}
if(lead.get("Phone") == null || lead.get("Phone") == "")
{
    score = score - 5;
}
if(lead.get("Company") == null || lead.get("Company") == "")
{
    score = score - 10;
}

// --- Scoring Engagement ---

// Vérifier les activités récentes
activities = zoho.crm.getRelatedRecords("Activities", "Leads", leadId);
if(activities.size() > 0)
{
    score = score + 10;
    // Bonus si activité dans les 7 derniers jours
    lastActivity = activities.get(0);
    actDate = lastActivity.get("Activity_Date_Time");
    if(actDate != null)
    {
        daysSince = daysBetween(actDate.toDate(), zoho.currentdate);
        if(daysSince <= 7)
        {
            score = score + 10;
        }
    }
}

// --- Enregistrer le score ---
// Utiliser un champ personnalisé "Custom_Score" (Number)
updateMap = Map();
updateMap.put("Custom_Score", score);

// Catégoriser
if(score >= 80)
{
    updateMap.put("Lead_Category", "Très chaud");
}
else if(score >= 50)
{
    updateMap.put("Lead_Category", "Chaud");
}
else if(score >= 20)
{
    updateMap.put("Lead_Category", "Tiède");
}
else
{
    updateMap.put("Lead_Category", "Froid");
}

zoho.crm.updateRecord("Leads", leadId, updateMap);
info "Score calculé : " + score;
```

## Scoring prédictif (Zia)

### Fonctionnement

1. Zia analyse les deals des 12 derniers mois
2. Identifie les patterns des leads convertis vs non convertis
3. Attribue un score de 0 à 100
4. Affiche les facteurs positifs et négatifs

### Interprétation

```
Lead : Jean Dupont - Acme Corp
Score Zia : 85/100

Facteurs positifs :
  ✅ Industrie similaire aux clients existants (+18)
  ✅ Taille d'entreprise dans la cible (+12)
  ✅ Engagement email élevé (+15)
  ✅ Temps de réponse rapide (+10)

Facteurs négatifs :
  ❌ Pas d'appel téléphonique (-8)
  ❌ Pas de visite de page pricing (-5)
```

## Bonnes pratiques

1. **Commencer simple** : 5-10 règles max au départ, affiner avec le temps
2. **Scores négatifs** : Indispensables pour déprioriser les leads inactifs
3. **Révision régulière** : Revoir les poids tous les trimestres
4. **Alignement vente/marketing** : Définir ensemble les seuils MQL/SQL
5. **Decay** : Implémenter un déclin du score dans le temps (ex: -5/mois sans activité)
6. **A/B testing** : Comparer les taux de conversion selon les seuils

---
*Voir aussi : [workflows.md](workflows.md) pour automatiser selon le score, [vues-filtres.md](vues-filtres.md) pour filtrer par score.*
