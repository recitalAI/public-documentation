---
description: >-
  Catalogue des modules disponibles dans l'éditeur de Workflow et accès à leur
  documentation de référence.
---

# Les modules Workflow

Dans l'éditeur de Workflow, le bouton **Ajouter une étape** ouvre la bibliothèque **Étapes**. Le filtre **Tout** permet de rechercher un module par nom. Dix-sept autres filtres regroupent les modules par usage : **Action, AI, Archive, Automation, Classification, Code, Document, Email, Extraction, Génération, Input, Output, Post-processing, Review, Sources, État** et **Validation**.

{% hint style="info" %}
Les cartes générées à partir des Agents d'extraction, des Agents de classification et des anciens modèles de classification d'e-mail sont propres à votre organisation. Elles renvoient vers les modules canoniques **Extraction**, **Classification** ou **Classification E-mail** ; ce ne sont pas des types de module supplémentaires.
{% endhint %}

<figure><img src="../.gitbook/assets/workflow_steps_library.png" alt="Bibliothèque Étapes des modules de Workflow"><figcaption>La bibliothèque <em>Étapes</em>, accessible avec <strong>Ajouter une étape</strong>.</figcaption></figure>

## Modules par filtre

Un module peut apparaître sous plusieurs filtres. Par exemple, **Code-barres** relève des filtres **Action**, **Extraction** et **Document**.

| Filtre | Modules concernés |
| --- | --- |
| Sources | API, CRON, IMAP |
| Action | Workflow, Diviser un document, Fusionner des documents, Décompresser, Code-barres |
| AI | Classification E-mail, Classification, Extraction, LLM, Validation d'ensemble |
| Archive | Décompresser |
| Automation | Workflow |
| Classification | Classification E-mail, Classification, Revue de classification, Revue de classification d'e-mail |
| Code | Code personnalisé |
| Document | Classification, Extraction, Revue de classification, Revue d'extraction, Revue avancée, Diviser un document, Fusionner des documents, Code-barres, Validation d'ensemble |
| Email | Ingérer des e-mails, Classification E-mail, Revue de classification d'e-mail, Décompresser |
| Extraction | Extraction, Revue d'extraction, Revue avancée, Décompresser, Code-barres |
| Génération | LLM |
| Input | Ingérer des e-mails |
| Output | Nettoyer, Webhook |
| Post-processing | Diviser un document, Fusionner des documents |
| Review | Revue de classification, Revue de classification d'e-mail, Revue d'extraction, Revue avancée |
| État | Start, Done, État personnalisé |
| Validation | Validation d'ensemble |

## Choisir et relier les modules

Une **Expression d'entrée** sélectionne ce que l'étape reçoit dans le job :

* `files['file']` sélectionne tous les fichiers de la collection `file` ;
* `data['final_result']` sélectionne la valeur stockée sous la clé `final_result` dans les données du job ;
* `zip(files['split-file'], data['split-pdf']['files'])` associe chaque fichier de la collection `split-file` aux données décrivant ce même fichier. Avec **Itérer sur l'entrée**, l'étape traite successivement chaque couple `(fichier, données)`.

Les pages de référence indiquent le type attendu et une expression adaptée à chaque module. Lorsqu'une étape produit un résultat, sa **Clé de sortie** détermine sous quelle clé de `data` il est enregistré.

## Référence par module

Chaque module disponible possède une page de référence unique.

### Sources

* [API](modules/api.md)
* [CRON](modules/cron.md)
* [IMAP](modules/imap.md)

### Input

* [Ingérer des e-mails](modules/ingerer-des-e-mails.md)

### AI

* [Classification E-mail](modules/classification-e-mail.md)
* [Classification](modules/classification.md)
* [Extraction](modules/extraction.md)
* [LLM](modules/llm.md)

### Review

* [Revue de classification](modules/revue-de-classification.md)
* [Revue de classification d'e-mail](modules/revue-de-classification-d-e-mail.md)
* [Revue d'extraction](modules/revue-d-extraction.md)
* [Revue avancée](modules/revue-avancee.md)

### Action

* [Workflow](modules/workflow.md)
* [Diviser un document](modules/diviser-un-document.md)
* [Décompresser](modules/decompresser.md)
* [Fusionner des documents](modules/fusionner-des-documents.md)
* [Code-barres](modules/code-barres.md)

{% hint style="warning" %}
**Transfert d'e-mail** et **Envoi d'e-mail** apparaissent comme des cartes à venir dans la bibliothèque **Étapes**. Elles ne sont pas activables dans cette version.
{% endhint %}

### Validation

* [Validation d'ensemble](modules/validation-d-ensemble.md)

### Code

* [Code personnalisé](modules/code-personnalise.md)

### État

* [Start](modules/start.md)
* [Done](modules/done.md)
* [État personnalisé](modules/etat-personnalise.md)

### Output

* [Nettoyer](modules/nettoyer.md)
* [Webhook](modules/webhook.md)
