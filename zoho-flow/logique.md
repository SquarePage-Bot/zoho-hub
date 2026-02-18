# Zoho Flow - Logique et Conditions

## Blocs de Décision

### If / Else (Condition Simple)
Exécuter des actions différentes selon une condition.

```
SI ${trigger.data.country} == "France"
  → Créer dans CRM (Organisation FR)
SINON
  → Créer dans CRM (Organisation International)
```

### Conditions Multiples (AND / OR)
```
SI (${trigger.data.score} > 50 ET ${trigger.data.source} == "Website")
  → Assigner au commercial senior
SINON SI (${trigger.data.score} > 20)
  → Assigner au commercial junior
SINON
  → Ajouter à la séquence de nurturing
```

### Opérateurs Disponibles
| Opérateur | Description | Exemple |
|-----------|-------------|---------|
| == | Égal | `status == "Active"` |
| != | Différent | `type != "Test"` |
| > | Supérieur | `score > 50` |
| < | Inférieur | `amount < 1000` |
| >= | Supérieur ou égal | `age >= 18` |
| <= | Inférieur ou égal | `priority <= 3` |
| contains | Contient | `email contains "@zoho"` |
| starts_with | Commence par | `name starts_with "Dr"` |
| ends_with | Finit par | `file ends_with ".pdf"` |
| is_empty | Est vide | `phone is_empty` |
| is_not_empty | N'est pas vide | `email is_not_empty` |

## Boucles (For Each)

### Itérer sur une Liste
```
POUR CHAQUE ligne DANS ${google_sheets.rows}
  → Créer un contact dans Zoho CRM
    Nom : ${ligne.name}
    Email : ${ligne.email}
    Téléphone : ${ligne.phone}
FIN POUR
```

### Limites des Boucles
- Maximum **500 itérations** par exécution
- Chaque itération consomme **1 tâche**
- Utiliser des filtres pour réduire le nombre d'itérations

## Variables et Expressions

### Variables Système
```
${system.current_date}      → 2026-02-18
${system.current_time}      → 14:30:00
${system.current_datetime}  → 2026-02-18T14:30:00Z
${system.flow_id}           → ID du flow en cours
${system.execution_id}      → ID de l'exécution
```

### Variables de Contexte
```
${trigger.data.*}           → Données du déclencheur
${step1.data.*}             → Résultat de l'étape 1
${step2.data.*}             → Résultat de l'étape 2
${connection.token}         → Token de la connexion
```

### Expressions Conditionnelles (Ternaire)
```
${trigger.data.type == "VIP" ? "Priorité haute" : "Priorité normale"}
```

### Fonctions de Transformation
```
# Listes
${filter(list, "status", "active")}     → Filtrer les éléments actifs
${map(list, "email")}                    → Extraire les emails
${count(list)}                           → Nombre d'éléments
${first(list)}                           → Premier élément
${last(list)}                            → Dernier élément
${join(list, ", ")}                      → Concaténer avec séparateur

# Texte
${split(text, ",")}                      → Découper en liste
${substring(text, 0, 10)}               → 10 premiers caractères
${length(text)}                          → Longueur du texte
```

## Chemins Parallèles

Exécuter plusieurs branches simultanément :

```
TRIGGER : Nouveau deal gagné
  ├── Branche 1 : Créer une facture (Zoho Books)
  ├── Branche 2 : Notifier l'équipe (Slack)
  └── Branche 3 : Mettre à jour le tableau de bord (Sheets)
```

## Gestion d'Erreurs dans la Logique

### Try / Catch
```
ESSAYER :
  → Créer un contact dans CRM
EN CAS D'ERREUR :
  → Logger l'erreur dans Google Sheets
  → Envoyer une notification Slack
  → ${error.message} contient le détail
```

### Vérification Préalable
```
SI ${step_recherche.data.count} > 0
  → Le contact existe → Mettre à jour
SINON
  → Le contact n'existe pas → Créer
```

## Bonnes Pratiques

1. **Toujours prévoir le cas "SINON"** pour éviter les flux silencieux
2. **Limiter la profondeur des conditions** — maximum 3 niveaux d'imbrication
3. **Utiliser des chemins parallèles** quand les actions sont indépendantes
4. **Tester chaque branche** avec des données de test spécifiques
5. **Documenter la logique** avec des commentaires dans le nom des étapes
