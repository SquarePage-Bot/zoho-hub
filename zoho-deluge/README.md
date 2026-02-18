# 🔧 Zoho Deluge - Guide Complet

> Documentation exhaustive du langage Deluge (Data Enriched Language for the Universal Grid Environment).

## Vue d'ensemble

Deluge est le langage de scripting propriétaire de Zoho, utilisé dans :
- **Zoho CRM** : Fonctions custom, workflows, blueprints, boutons, widgets
- **Zoho Creator** : Logique métier des applications
- **Zoho Books/Inventory** : Automatisations comptables
- **Zoho Desk** : Automatisation du support
- **Zoho Flow** : Fonctions personnalisées dans les flux

## Caractéristiques

| Aspect | Description |
|--------|-------------|
| Paradigme | Impératif, procédural |
| Typage | Dynamique, faible |
| Exécution | Côté serveur (cloud Zoho) |
| Limites | 10 000 instructions/exécution, 5 min timeout |
| Débogage | `info` pour le logging, pas de debugger pas-à-pas |

## Fichiers de cette section

| Fichier | Contenu |
|---------|---------|
| [syntaxe.md](syntaxe.md) | Variables, opérateurs, structures de contrôle |
| [fonctions.md](fonctions.md) | Fonctions intégrées (string, date, math, etc.) |
| [collections.md](collections.md) | List, Map, manipulation de données |
| [api-calls.md](api-calls.md) | invokeurl, appels REST, intégrations |
| [crm-functions.md](crm-functions.md) | Fonctions spécifiques Zoho CRM |
| [bonnes-pratiques.md](bonnes-pratiques.md) | Patterns, erreurs courantes, optimisation |
| [exemples.md](exemples.md) | Scripts complets prêts à l'emploi |

## Environnements d'exécution

| Contexte | Déclencheur | Limites |
|----------|------------|---------|
| Workflow Function | Événement CRM | 5 000 instructions |
| Custom Function | Bouton, API, schedule | 10 000 instructions |
| Blueprint | Transition d'état | 5 000 instructions |
| Standalone | Actions planifiées | 10 000 instructions |
| Widget Function | Appel depuis widget JS | 10 000 instructions |
| Creator | Événement Creator | 25 000 instructions |

## Hello World

```deluge
// Mon premier script Deluge
name = "SquarePage";
info "Bonjour " + name + " ! 🚀";

// Créer un lead dans le CRM
leadMap = Map();
leadMap.put("Last_Name", "Test");
leadMap.put("Company", "Test Corp");
leadMap.put("Email", "test@example.com");

response = zoho.crm.createRecord("Leads", leadMap);
info response;
```

---
*Point d'entrée : commencer par [syntaxe.md](syntaxe.md), puis [fonctions.md](fonctions.md).*
