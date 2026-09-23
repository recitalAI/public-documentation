---
description: Les modules Sources API, CRON et IMAP disponibles dans un Workflow.
---

# Modules Workflow : Sources

Les modules **Sources** représentent l'origine d'un job Workflow. **API**, **CRON** et **IMAP** ne possèdent ni expression d'entrée ni paramètre de configuration propre dans l'étape.

## API

Le module **API** identifie un job créé par l'API Workflow.

Les fichiers et données transmis lors de la création du job deviennent les collections `files` et l'objet `data` que les étapes suivantes peuvent sélectionner dans leurs expressions d'entrée.

## CRON

Le module **CRON** identifie un job créé par une planification.

Le job est traité périodiquement selon la configuration du service. Le module **CRON** ne garantit pas à lui seul une fréquence fixe.

## IMAP

Le module **IMAP** identifie un job créé à partir d'une boîte mail configurée.

Pour exploiter le message et ses pièces jointes dans les étapes suivantes, utilisez le module [Ingérer des e-mails](ingerer-des-e-mails.md). Consultez aussi [Connexion boîte mail](../connexion-boite-mail.md).
