---
description: Rendez-vous dans la page "Workflow", puis sur l'onglet "Jobs".
---

# Jobs

## Qu'est-ce qu'un Job ?

Un Job représente le parcours d'un document au travers du workflow vers lequel il a été envoyé. Chaque Module, Etat ou Transition d'un workflow est retranscrit dans les jobs.

{% hint style="info" %}
Un job permet de:

* Suivre la progression d'un document.
* Identifier l'état d'un document : started, waiting, done, error, custom.
* Consulter les données à chaque étape du Workflow.
* Voir les logs à chaque étape du Workflow, et donc déboguer si besoin.
* Télécharger les documents originaux et les documents générés.
{% endhint %}

## Consulter les jobs

Si vous avez réussi à envoyer un premier document - que ce soit par API (voir [Envoyer des documents dans un workflow](../integration-api/workflow/envoyer-des-documents-dans-un-workflow.md)) ou via l'upload d'un document pour tester le workflow - vous devriez voir apparaître un premier job.

<figure><img src="../.gitbook/assets/image (112).png" alt=""><figcaption><p>Tableau des jobs</p></figcaption></figure>

Pour consulter les détails d'un job, cliquez sur l'ID du Job.

### Historique

Le premier onglet "Historique", permet de voir le statut et les données étape par étape. On y retrouve également les logs, permettant entre autres de débugger les différents paramètres d'un module, ou de débugger un module custom (code Python).

<figure><img src="../.gitbook/assets/image (113).png" alt=""><figcaption><p>Jobs - Historique</p></figcaption></figure>

### Données

Cet écran permet d'afficher **en direct** les données finales (ajout successif des données à chaque étape).

<figure><img src="../.gitbook/assets/image (114).png" alt=""><figcaption><p>Jobs - Données</p></figcaption></figure>

### Fichiers

Cet écran permet de retrouver tous les documents dans les différentes collections et de les télécharger.

{% hint style="info" %}
Le module de cleanup permet actuellement de supprimer les data et tous les fichiers.
{% endhint %}

<figure><img src="../.gitbook/assets/image (115).png" alt=""><figcaption><p>Jobs - Fichier</p></figcaption></figure>
