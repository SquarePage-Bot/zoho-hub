# Rapports et Statistiques

## Présentation

Zoho Campaigns offre des rapports détaillés pour analyser les performances des campagnes, comprendre le comportement des contacts et optimiser les futures actions.

## Rapports de campagne

### Métriques principales

| Métrique | Formule | Bon benchmark |
|----------|---------|---------------|
| **Taux de livraison** | Livrés / Envoyés × 100 | > 95% |
| **Taux d'ouverture** | Ouverts / Livrés × 100 | 20-25% (B2B) |
| **Taux de clic (CTR)** | Clics / Livrés × 100 | 2-5% |
| **Taux de clic/ouverture (CTOR)** | Clics / Ouverts × 100 | 10-15% |
| **Taux de rebond** | Bounces / Envoyés × 100 | < 2% |
| **Taux de désinscription** | Désinscriptions / Livrés × 100 | < 0.5% |
| **Taux de plainte spam** | Plaintes / Livrés × 100 | < 0.1% |
| **Taux de conversion** | Conversions / Clics × 100 | Variable |

### Tableau de bord d'une campagne

```
Campagne : Newsletter Mars 2026
Envoyée le : 10/03/2026 à 10h00

┌────────────────────────────────────────────────────────────┐
│  📤 Envoyés    📬 Livrés    📭 Bounces    🚫 Désinscrit.  │
│  10 000        9 750        250           45               │
│                (97.5%)      (2.5%)        (0.46%)          │
├────────────────────────────────────────────────────────────┤
│  👁️ Ouverts    🖱️ Clics     📊 CTOR      ⚠️ Spam          │
│  2 438         487          19.97%       3                 │
│  (25.0%)       (5.0%)                    (0.03%)           │
└────────────────────────────────────────────────────────────┘
```

### Évolution dans le temps

```
Ouvertures par heure (premières 48h) :

Heure 0-1   : ████████████████████ 35%
Heure 1-2   : ████████████ 20%
Heure 2-4   : ████████ 15%
Heure 4-8   : ██████ 12%
Heure 8-24  : ████ 10%
Heure 24-48 : ███ 8%
```

## Heatmap des clics

### Visualisation

```
La heatmap montre où les contacts cliquent dans l'email :

┌─────────────────────────────────────┐
│  [Logo] 🔵 2 clics                  │
│                                     │
│  Titre article 1                    │
│  "Lire la suite" 🔴 145 clics       │
│                                     │
│  Titre article 2                    │
│  "Lire la suite" 🟡 87 clics        │
│                                     │
│  [BOUTON CTA]   🔴 203 clics        │
│                                     │
│  [Facebook] 🔵 12  [Twitter] 🔵 8   │
│  [Se désinscrire]  🟢 45            │
└─────────────────────────────────────┘

🔴 = Forte activité  🟡 = Moyenne  🔵 = Faible  🟢 = Désinscription
```

### Rapport de liens détaillé

```
┌──────────────────────────────────┬────────┬────────┐
│ Lien                             │ Clics  │ % total│
├──────────────────────────────────┼────────┼────────┤
│ Bouton "Découvrir l'offre"       │ 203    │ 41.7%  │
│ "Lire la suite" article 1       │ 145    │ 29.8%  │
│ "Lire la suite" article 2       │ 87     │ 17.9%  │
│ Se désinscrire                   │ 45     │ 9.2%   │
│ Icône Facebook                   │ 12     │ 2.5%   │
│ Icône Twitter                    │ 8      │ 1.6%   │
├──────────────────────────────────┼────────┼────────┤
│ Total clics uniques              │ 487    │ 100%   │
└──────────────────────────────────┴────────┴────────┘
```

## Rapports par client email

```
┌──────────────────┬──────────┬──────────┬──────────┐
│ Client email     │ Ouverts  │ % liste  │ CTR      │
├──────────────────┼──────────┼──────────┼──────────┤
│ Gmail            │ 890      │ 36.5%    │ 5.2%     │
│ Outlook          │ 612      │ 25.1%    │ 4.8%     │
│ Apple Mail       │ 487      │ 20.0%    │ 5.5%     │
│ Yahoo Mail       │ 195      │ 8.0%     │ 3.1%     │
│ Thunderbird      │ 98       │ 4.0%     │ 6.2%     │
│ Autres           │ 156      │ 6.4%     │ 4.0%     │
└──────────────────┴──────────┴──────────┴──────────┘
```

## Rapports géographiques

```
Ouvertures par pays :

🇫🇷 France      : ████████████████████ 62%
🇧🇪 Belgique    : ██████ 15%
🇨🇭 Suisse      : ████ 10%
🇨🇦 Canada      : ███ 7%
🇲🇦 Maroc       : ██ 4%
🌍 Autres       : █ 2%
```

## Rapports d'appareil

```
┌─────────────┬──────────┬──────────┐
│ Appareil    │ Ouverts  │ %        │
├─────────────┼──────────┼──────────┤
│ 📱 Mobile   │ 1 463    │ 60%      │
│ 💻 Desktop  │ 731      │ 30%      │
│ 📱 Tablette │ 244      │ 10%      │
└─────────────┴──────────┴──────────┘
```

## Rapports de liste

### Croissance de la liste

```
Liste : Newsletter principale

Mois        Inscrits  Désinscr.  Bounces  Net    Total
Janvier     +320      -45        -12      +263   8 750
Février     +410      -38        -8       +364   9 114
Mars        +380      -52        -15      +313   9 427

Taux de croissance mensuel moyen : +3.4%
```

### Engagement par segment

```
┌─────────────────────┬──────────┬──────────┬──────────┐
│ Segment             │ Contacts │ Tx ouv.  │ CTR      │
├─────────────────────┼──────────┼──────────┼──────────┤
│ Clients actifs      │ 3 200    │ 35.2%    │ 8.1%     │
│ Prospects chauds    │ 1 500    │ 28.7%    │ 5.3%     │
│ Inscrits récents    │ 800      │ 42.1%    │ 6.5%     │
│ Contacts inactifs   │ 2 500    │ 5.3%     │ 0.8%     │
│ VIP                 │ 200      │ 51.0%    │ 12.4%    │
└─────────────────────┴──────────┴──────────┴──────────┘
```

## Rapports de workflow

### Performance d'un workflow

```
Workflow : Onboarding SaaS

┌─────────────────────────┬──────────┬──────────┐
│ Étape                   │ Entrées  │ Tx conv. │
├─────────────────────────┼──────────┼──────────┤
│ Email bienvenue         │ 1 000    │ 65% ouv. │
│ ↓                       │          │          │
│ Email tutoriel (J+2)    │ 850      │ 45% ouv. │
│ ↓                       │          │          │
│ Email étude de cas (J+5)│ 720      │ 38% ouv. │
│ ↓                       │          │          │
│ Email offre démo (J+10) │ 600      │ 22% ouv. │
│ ↓                       │          │          │
│ Conversion (demande     │ 85       │ 8.5%     │
│ démo ou achat)          │          │          │
└─────────────────────────┴──────────┴──────────┘

Taux de conversion global : 8.5% (85/1000)
Revenu généré : 12 750€
ROI du workflow : 340%
```

## Rapports comparatifs

### Comparer des campagnes

```
Rapports → Comparer des campagnes → Sélectionner 2-5 campagnes

┌──────────────────┬──────────┬──────────┬──────────┐
│ Campagne         │ Tx ouv.  │ CTR      │ Désinsc. │
├──────────────────┼──────────┼──────────┼──────────┤
│ Newsletter Jan   │ 22.1%    │ 3.8%     │ 0.5%     │
│ Newsletter Fév   │ 24.5%    │ 4.2%     │ 0.4%     │
│ Newsletter Mars  │ 25.0%    │ 5.0%     │ 0.46%    │
│ Promo Soldes     │ 31.2%    │ 8.7%     │ 0.8%     │
│ Webinar Invite   │ 28.8%    │ 6.1%     │ 0.2%     │
└──────────────────┴──────────┴──────────┴──────────┘

Tendance : Amélioration constante des newsletters (+13% taux ouv.)
```

## Export des rapports

```
Formats d'export :
- PDF (pour partage et archivage)
- CSV (pour analyse dans un tableur)
- Zoho Analytics (pour rapports avancés cross-canal)

Rapports programmés :
- Fréquence : Quotidien / Hebdomadaire / Mensuel
- Destinataires : Email du manager marketing
- Contenu : Résumé des KPI + top/flop campagnes
```

## Intégration Zoho Analytics

```
Pour des rapports avancés :
Paramètres → Intégrations → Zoho Analytics

Données synchronisées :
- Toutes les campagnes et leurs métriques
- Données des contacts et segments
- Historique des workflows
- Données e-commerce (si connecté)

Avantages :
- Tableaux de bord interactifs
- Croisement avec données CRM
- Rapports prédictifs
- KPI personnalisés
```

## Bonnes pratiques

1. **Analyser après chaque envoi** : Revoir les métriques dans les 48h
2. **Comparer dans le temps** : Suivre l'évolution mois par mois
3. **Segmenter l'analyse** : Les moyennes cachent les disparités
4. **Surveiller la délivrabilité** : Un taux de bounce > 5% est un signal d'alarme
5. **Agir sur les inactifs** : Campagne de réengagement ou nettoyage
6. **Optimiser les CTA** : Analyser la heatmap pour améliorer le placement
7. **Tester systématiquement** : A/B tester et mesurer l'impact
8. **Automatiser les rapports** : Programmer un export hebdomadaire aux stakeholders
