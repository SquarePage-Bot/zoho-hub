# 📊 Zoho Desk - Rapports

> Tableaux de bord, métriques clés et analyse de la performance du support.

## Table des matières

- [Tableau de bord principal](#tableau-de-bord-principal)
- [Métriques clés (KPIs)](#métriques-clés)
- [Rapports prédéfinis](#rapports-prédéfinis)
- [Rapports personnalisés](#rapports-personnalisés)
- [Rapports SLA](#rapports-sla)
- [Satisfaction client (CSAT)](#satisfaction-client)
- [Planification et export](#planification-et-export)

---

## Tableau de bord principal

### Vue d'ensemble (HQ Dashboard)

```
Zoho Desk HQ → Vue en temps réel

Widgets :
┌─────────────────────────────────────────────┐
│  Trafic en direct                           │
│  - Tickets reçus aujourd'hui : 42           │
│  - Tickets résolus aujourd'hui : 38         │
│  - Tickets en attente : 15                  │
│  - Agents connectés : 8/10                  │
├─────────────────────────────────────────────┤
│  Répartition par statut (camembert)         │
│  Ouvert: 23 | En cours: 12 | En attente: 8 │
│  Escaladé: 3 | Résolu: 156 | Fermé: 2450   │
├─────────────────────────────────────────────┤
│  Volume par canal (barres)                  │
│  Email: 65% | Chat: 20% | Tel: 10% | Web: 5%│
├─────────────────────────────────────────────┤
│  SLA (jauge)                                │
│  Conformité 1ère réponse : 92%              │
│  Conformité résolution : 87%                │
└─────────────────────────────────────────────┘
```

---

## Métriques clés

### Volume

| Métrique | Description |
|----------|-------------|
| **Tickets créés** | Nombre de nouveaux tickets par période |
| **Tickets résolus** | Nombre de tickets résolus |
| **Tickets en cours** | Tickets actuellement ouverts |
| **Backlog** | Tickets non résolus accumulés |
| **Ratio créés/résolus** | > 1 = backlog en augmentation |

### Temps

| Métrique | Description | Cible type |
|----------|-------------|------------|
| **Temps de première réponse** | Délai entre création et 1ère réponse agent | < 4h |
| **Temps de résolution** | Délai entre création et résolution | < 24h |
| **Temps moyen de traitement** | Durée effective de travail sur le ticket | < 2h |
| **Temps d'attente client** | Temps passé en statut "En attente" | Minimiser |

### Qualité

| Métrique | Description | Cible type |
|----------|-------------|------------|
| **CSAT (Satisfaction)** | % de clients satisfaits | > 90% |
| **FCR (First Contact Resolution)** | % résolus au premier contact | > 70% |
| **Taux de réouverture** | % tickets réouverts après fermeture | < 5% |
| **Taux d'escalade** | % tickets escaladés | < 10% |

### Productivité

| Métrique | Description |
|----------|-------------|
| **Tickets par agent/jour** | Volume traité par agent |
| **Temps de réponse moyen** | Rapidité des agents |
| **Taux d'utilisation** | % du temps passé sur les tickets |

---

## Rapports prédéfinis

### Rapports de tickets

```
Rapports → Tickets

- Tickets par statut
- Tickets par priorité
- Tickets par département
- Tickets par canal
- Tickets par agent
- Tickets par catégorie
- Tendance du volume (courbe sur le temps)
- Tickets créés vs résolus (comparaison)
```

### Rapports d'agents

```
Rapports → Agents

- Performance par agent (tickets résolus, temps moyen)
- Classement des agents
- Charge de travail actuelle
- Temps de connexion / disponibilité
- CSAT par agent
```

### Rapports par client

```
Rapports → Clients

- Tickets par client (top 10 demandeurs)
- Tickets par entreprise
- Temps de résolution par type de client
- Clients les plus satisfaits/insatisfaits
```

---

## Rapports personnalisés

### Création

```
Rapports → + Nouveau rapport

Étapes :
1. Module : Tickets, Contacts, Agents, Articles KB...
2. Type : Tableau, Résumé, Matrice
3. Colonnes et lignes
4. Filtres (période, département, agent...)
5. Graphique (barres, camembert, courbe, jauge)
6. Planification (optionnelle)
```

### Exemples

**Rapport : Temps de résolution par priorité et département**
```
Type : Matrice
Lignes : Département
Colonnes : Priorité
Valeurs : AVG(Temps de résolution)
Filtre : Période = Ce mois

Résultat :
┌──────────────┬──────────┬──────────┬──────────┬──────────┐
│              │ Basse    │ Moyenne  │ Haute    │ Urgente  │
├──────────────┼──────────┼──────────┼──────────┼──────────┤
│ Support      │ 48h      │ 24h      │ 6h       │ 2h       │
│ Technique    │ 72h      │ 36h      │ 12h      │ 4h       │
│ Facturation  │ 24h      │ 12h      │ 4h       │ 1h       │
└──────────────┴──────────┴──────────┴──────────┴──────────┘
```

**Rapport : Évolution hebdomadaire**
```
Type : Courbe
Axe X : Semaine
Séries : Tickets créés, Tickets résolus
Filtre : 12 dernières semaines
```

---

## Rapports SLA

### Conformité SLA

```
Rapports → SLA

Métriques :
- % de tickets avec 1ère réponse dans les temps
- % de tickets résolus dans les temps
- Nombre de violations par période
- Violations par département / agent / priorité
- Tendance de la conformité SLA
```

### Détail des violations

```
Rapport "Violations SLA - Ce mois" :

┌────────┬──────────────────┬───────────┬──────────┬──────────┐
│ Ticket │ Sujet            │ Priorité  │ SLA cible│ Réel     │
├────────┼──────────────────┼───────────┼──────────┼──────────┤
│ #1234  │ Erreur connexion │ Urgente   │ 1h       │ 2h15     │
│ #1245  │ Bug export PDF   │ Haute     │ 4h       │ 6h30     │
│ #1267  │ Lenteur système  │ Moyenne   │ 8h       │ 12h      │
└────────┴──────────────────┴───────────┴──────────┴──────────┘
```

---

## Satisfaction client

### Rapport CSAT

```
Rapports → Satisfaction client

Métriques :
- Score CSAT global : 87%
- Répartition : 😊 72% | 😐 15% | 😞 13%
- CSAT par agent
- CSAT par département
- CSAT par canal
- Évolution dans le temps
- Commentaires clients (texte libre)
```

### Analyse des commentaires

```
Commentaires négatifs fréquents :
- "Temps de réponse trop long" → Revoir les SLA
- "Je n'ai pas compris la solution" → Former les agents
- "Problème non résolu" → Vérifier les processus d'escalade
- "Trop de transferts" → Améliorer le routing
```

---

## Planification et export

### Planification automatique

```
Rapport → Planifier

Configuration :
- Fréquence : quotidienne, hebdomadaire, mensuelle
- Destinataires (emails)
- Format : PDF, Excel, CSV
- Heure d'envoi

Exemple :
  "Rapport hebdomadaire support" envoyé chaque lundi à 8h
  au manager et au directeur, format PDF
```

### Export

```
Formats :
- PDF (avec graphiques)
- Excel (données brutes)
- CSV

Options :
- Période sélectionnée
- Filtres appliqués
```

### Intégration Zoho Analytics

```
Pour des analyses plus poussées :
Paramètres → Intégrations → Zoho Analytics

Synchronise les données Desk vers Analytics pour :
- Tableaux de bord avancés
- Analyses croisées (Desk + CRM + Books)
- Visualisations complexes
- Partage avec des non-utilisateurs Desk
```

---

## Bonnes pratiques

1. **Quotidien** : Consulter le tableau de bord HQ pour le suivi en temps réel
2. **Hebdomadaire** : Analyser les tendances de volume et les violations SLA
3. **Mensuel** : Revue de la satisfaction client et des performances agents
4. **CSAT** : Réagir à chaque commentaire négatif
5. **Alertes** : Configurer des alertes sur les métriques critiques
6. **Benchmark** : Comparer les performances mois par mois

---

*Voir aussi : [tickets.md](tickets.md) | [configuration.md](configuration.md) | [automatisations.md](automatisations.md)*
