# Zoho Sign - Documents

## Envoyer un Document à Signer

### Étapes
```
1. Uploader le document (PDF, Word, Excel)
2. Ajouter les destinataires (signataires, en copie)
3. Placer les champs de signature
4. Configurer les options (délai, rappels)
5. Envoyer
```

### Champs Disponibles
| Champ | Description | Obligatoire |
|-------|-------------|-------------|
| Signature | Zone de signature | Configurable |
| Initiales | Paraphe | Configurable |
| Date | Date de signature (auto) | Non |
| Nom | Nom complet du signataire | Non |
| Email | Email du signataire | Non |
| Société | Nom de l'entreprise | Non |
| Titre | Fonction / poste | Non |
| Texte | Champ de saisie libre | Configurable |
| Case à cocher | Acceptation de conditions | Configurable |
| Menu déroulant | Choix parmi des options | Configurable |
| Pièce jointe | Upload par le signataire | Non |

### Exemple : Contrat de Prestation
```
Document : contrat-prestation-2026.pdf

Destinataires :
  1. Client (signataire) : jean@client.fr
     → Champs : Signature, Date, Nom, Société
  2. Votre entreprise (signataire) : direction@entreprise.fr
     → Champs : Signature, Date
  3. Comptable (en copie) : compta@entreprise.fr

Options :
  Délai de signature : 7 jours
  Rappels : Tous les 2 jours
  Langue : Français
  Message : "Merci de signer le contrat ci-joint."
```

## Statuts d'un Document

| Statut | Description |
|--------|-------------|
| Brouillon | Document en préparation |
| Envoyé | En attente de signature |
| Vu | Le signataire a ouvert le document |
| Signé partiellement | Certains ont signé, pas tous |
| Complété | Tous les signataires ont signé ✅ |
| Refusé | Un signataire a refusé ❌ |
| Expiré | Délai dépassé sans signature |
| Rappelé | Document annulé par l'expéditeur |

## Piste d'Audit

```
Document : contrat-prestation-2026.pdf
ID : DOC-2026-0042

Historique :
  18/02/2026 10:00 — Créé par admin@entreprise.fr
  18/02/2026 10:05 — Envoyé à jean@client.fr
  18/02/2026 14:22 — Ouvert par jean@client.fr (IP: 82.x.x.x)
  18/02/2026 14:35 — Signé par jean@client.fr
  18/02/2026 14:35 — Envoyé à direction@entreprise.fr
  19/02/2026 09:10 — Ouvert par direction@entreprise.fr
  19/02/2026 09:15 — Signé par direction@entreprise.fr
  19/02/2026 09:15 — Document complété ✅
  19/02/2026 09:15 — Copies envoyées à tous les participants

Certificat de complétion généré : audit-DOC-2026-0042.pdf
```

## Gestion des Documents

### Dossiers
```
Organisation :
  📁 Contrats
    📁 Clients
    📁 Fournisseurs
    📁 Employés
  📁 RH
    📁 Avenants
    📁 NDA
  📁 Finance
    📁 Bons de commande
```

### Recherche
```
Filtres disponibles :
  - Par statut (envoyé, signé, expiré...)
  - Par destinataire
  - Par date
  - Par dossier
  - Par tags
  - Recherche plein texte
```

## Bonnes Pratiques

1. **Utiliser des templates** pour les documents récurrents
2. **Configurer les rappels automatiques** pour éviter les oublis
3. **Organiser en dossiers** dès le départ
4. **Conserver les pistes d'audit** — valeur juridique importante
5. **Vérifier la conformité** eIDAS pour les documents à forte valeur
