---
description: Rendez-vous dans la page "Workflow", puis l'onglet "Input"
---

# Inputs

Des connecteurs peuvent être configurés afin de récupérer en temps réel les documents et emails, plutôt que de devoir les envoyer par API.

{% hint style="info" %}
Pour le moment, seul le connecteur pour les boîtes mails est disponible. Les connecteurs pour un bucket S3 ou une connexion FTP arriveront dans les prochaines mises à jour.
{% endhint %}

## Boites e-mail

<figure><img src="../.gitbook/assets/image (117).png" alt=""><figcaption></figcaption></figure>

**Nom** : Le nom du connecteur vers la boîte mail.

**Configuration IMAP - Email** : L'adresse e-mail que vous souhaitez configurer (par exemple, votre adresse Gmail complète).

**Configuration IMAP - Hôte** : Pour Gmail, l'hôte est imap.gmail.com.

**Configuration IMAP - Port** : Pour IMAP avec SSL, le port est 993.

**Entrée des données - Dossier** : Le dossier dans lequel aller chercher les mails. En général, cela reste INBOX à moins que vous ayez des besoins spécifiques.

**Authentification - Fournisseur** : Pour le moment, seules les adresses Google sont gérées par le connecteur. D'autres fournisseurs seront ajoutés dans les prochaines mises à jour.

**Authentification - Client ID et Client Secret** : Pour obtenir ces informations, vous devez configurer un projet dans la Google Cloud Console et activer l'API Gmail. Allez ensuite dans Credentials (Identifiants) et cliquez sur "Create Credentials" (ID client OAuth 2.0).

**Workflow** : Sélectionnez le workflow vers lequel seront envoyés les fichiers .eml. Ce workflow devra avoir un module "Ingest Email" pour récupérer ce fichier et commencer à le traiter.



**L'URI de redirection autorisé** est la suivante : [https://extract.recital.ai/suite/redirect](https://extract.recital.ai/suite/redirect).
