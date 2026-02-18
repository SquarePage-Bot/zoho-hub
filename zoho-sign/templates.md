# Zoho Sign - Templates (Modèles)

## Créer un Template

### Étapes
```
1. Documents → Templates → Nouveau
2. Uploader le document modèle (PDF)
3. Définir les rôles (ex: "Client", "Employeur")
4. Placer les champs pour chaque rôle
5. Configurer les options par défaut
6. Sauvegarder le template
```

### Exemple : Template NDA (Accord de Confidentialité)
```
Template : NDA Standard
Document : nda-template.pdf

Rôles :
  Rôle 1 : "Partie Divulgante" (votre entreprise)
  Rôle 2 : "Partie Réceptrice" (le tiers)

Champs Rôle 1 :
  Page 3 : Signature, Nom, Date, Titre

Champs Rôle 2 :
  Page 3 : Signature, Nom, Date, Société, Titre

Champs pré-remplis :
  Nom entreprise : "VotreSociété SAS"
  Adresse : "10 rue du Commerce, 75001 Paris"

Options :
  Délai : 14 jours
  Rappels : Tous les 3 jours
  Ordre : Rôle 2 d'abord, puis Rôle 1
```

## Variables dans les Templates

### Champs Dynamiques
```
Utiliser des variables pour personnaliser chaque envoi :

{{nom_client}}        → Rempli à l'envoi
{{date_debut}}        → Rempli à l'envoi
{{montant}}           → Rempli à l'envoi
{{duree_contrat}}     → Rempli à l'envoi

Exemple dans le document :
"Le présent contrat est conclu entre VotreSociété SAS
et {{nom_client}} pour une durée de {{duree_contrat}} mois
à compter du {{date_debut}}, pour un montant de {{montant}}€ HT."
```

## Templates depuis Zoho CRM

```
Envoyer un template depuis une fiche CRM :

1. Ouvrir un Deal dans Zoho CRM
2. Actions → Envoyer pour signature (Zoho Sign)
3. Sélectionner le template "Contrat client"
4. Les champs CRM pré-remplissent les variables :
   {{nom_client}} ← Deal.Account_Name
   {{montant}} ← Deal.Amount
   {{email_client}} ← Deal.Contact.Email
5. Vérifier et envoyer
```

## Bonnes Pratiques

1. **Un template par type de document** : NDA, contrat, avenant, devis
2. **Utiliser des variables** pour la personnalisation
3. **Tester chaque template** avant utilisation en production
4. **Versionner les templates** — archiver les anciennes versions
5. **Lier aux modules CRM** pour automatiser le pré-remplissage
