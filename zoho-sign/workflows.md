# Zoho Sign - Workflows de Signature

## Ordre de Signature

### Séquentiel
```
Ordre strict : chaque signataire reçoit le document après le précédent.

1. Employé signe → 2. Manager signe → 3. DRH signe → 4. Direction signe

Avantage : Chaque niveau valide avant le suivant
Inconvénient : Plus lent (dépend du plus lent)
```

### Parallèle
```
Tous les signataires reçoivent le document en même temps.

  ┌→ Signataire A
  ├→ Signataire B  (tous en parallèle)
  └→ Signataire C

Avantage : Plus rapide
Inconvénient : Pas de validation hiérarchique
```

### Mixte
```
Combiner séquentiel et parallèle :

Étape 1 (séquentiel) : Employé signe
Étape 2 (parallèle) : Manager + DRH signent en même temps
Étape 3 (séquentiel) : Direction signe en dernier
```

## Approbations (sans signature)

```
Ajouter des approbateurs qui valident sans signer :

1. Rédacteur prépare le contrat
2. → Approbateur juridique vérifie le contenu (Approuve / Refuse)
3. → Si approuvé : envoi aux signataires
4. → Si refusé : retour au rédacteur avec commentaires
```

## Rappels Automatiques

```
Configuration :
  Premier rappel : 2 jours après l'envoi
  Rappels suivants : Tous les 3 jours
  Maximum de rappels : 5
  Action après expiration : Notifier l'expéditeur

Personnalisation du message de rappel :
  Objet : "Rappel : Document en attente de votre signature"
  Corps : "Bonjour {{nom}}, le document {{titre}} attend
           votre signature depuis {{jours}} jours..."
```

## Automatisation avec Zoho Flow

```
Exemple : Onboarding RH automatisé

TRIGGER : Nouveau employé dans Zoho People

ACTIONS :
  1. Générer le contrat depuis le template "CDI Standard"
     Variables : nom, poste, salaire, date_debut
  2. Envoyer pour signature via Zoho Sign
     Ordre : Employé → Manager → DRH
  3. Quand signé → Sauvegarder dans le dossier employé
  4. Notifier les RH par email
  5. Mettre à jour le statut dans Zoho People
```

## Délégation

```
Si un signataire est absent :
  → Possibilité de déléguer à un remplaçant
  → Le remplaçant signe au nom du signataire original
  → La piste d'audit trace la délégation

Configuration :
  "Autoriser la délégation" : Oui
  "Délégation automatique après" : 5 jours sans action
  "Délégué par défaut" : manager@entreprise.fr
```

## Bonnes Pratiques

1. **Utiliser l'ordre séquentiel** pour les documents hiérarchiques
2. **Paralléliser** quand les signataires sont indépendants
3. **Configurer les rappels** — les gens oublient
4. **Ajouter des approbateurs** pour les documents sensibles
5. **Automatiser via Zoho Flow** les processus récurrents
