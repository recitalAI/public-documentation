# Connexion boîte mail

Connecter une boîte mail permet de récupérer automatiquement les emails entrants et de les envoyer dans un Workflow, sans passer par l'API. Le connecteur se configure depuis **Workflows → Input**.

{% hint style="info" %}
Le Workflow ciblé doit contenir un module **Ingest Email** pour récupérer le fichier `.eml` et commencer son traitement. Voir [Les modules de Workflow](les-modules-workflow.md).
{% endhint %}

## Configuration du connecteur IMAP

| Champ | Description |
|---|---|
| **Nom** | Le nom du connecteur vers la boîte mail. |
| **Configuration IMAP – Email** | L'adresse e-mail à configurer (par exemple l'adresse Gmail complète). |
| **Configuration IMAP – Hôte** | Pour Gmail, `imap.gmail.com`. |
| **Configuration IMAP – Port** | Pour IMAP avec SSL, `993`. |
| **Entrée des données – Dossier** | Le dossier où chercher les mails. En général `INBOX`. |
| **Authentification – Fournisseur** | Les fournisseurs Google et Microsoft sont gérés par le connecteur. |
| **Authentification – Client ID / Client Secret** | À obtenir en configurant un projet dans la Google Cloud Console et en activant l'API Gmail (section *Credentials* → *Create Credentials* → ID client OAuth 2.0). |
| **Workflow** | Le Workflow vers lequel sont envoyés les fichiers `.eml`. Il doit contenir un module **Ingest Email**. |

L'**URI de redirection autorisé** à renseigner côté Google est : `https://extract.recital.ai/suite/redirect`.

## Voir aussi

- [Les modules de Workflow](les-modules-workflow.md) — le module **Ingest Email**.
