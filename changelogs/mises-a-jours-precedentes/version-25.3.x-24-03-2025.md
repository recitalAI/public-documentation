# Version 25.3.x (24/03/2025)

### Version 25.3.1 (24/03/2025)

#### **Extraction**

* Les groupes d'extracteurs sont maintenant triés par ordre d'apparition dans le document. Cela concerne également les extracteurs de type "Cluster".
* Correction de plusieurs problèmes avec les extracteurs avec la méthode d'extraction "Groupe".
* Lorsqu'un agent d'extraction était "locked" certaines fonctionnalités étaient encore éditables, ce n'est plus le cas.

#### Écran de review d'extraction

* L'ordre des datapoints et groupes peut être configuré dans l'onglet "Extracteur" d'un agent d'extraction. Cet ordre sera conservé lors du vidéo-codage et de la validation.
* Correction des règles de gestion qui affichaient "0" pour les sommes d'éléments dans un groupe.

#### Classification

* Homogénéisation des sorties JSON de Classification et Classification Review _(aucune modification ou suppression de clé)_. Voir [structure-des-resultats-de-classification.md](../../integration-api/classification/structure-des-resultats-de-classification.md "mention")
* Possibilité d'exporter et importer un agent de classification.
* Correction de l'entrainement des modèles de classification de mails.
* Correction dans un agent de classification lors du changement d'un modèle vers un autre.&#x20;

#### Écran de review de classification

* Les utilisateurs "Reviewer" ont accès aux review d'agents de classification.
* Correctif de redirection lors de la soumission d'un document en review.
* L'écran de review a été amélioré pour les mails.
* Nouvelle fonctionnalité de déliassage permettant de séparer 2 pages avec la même catégorie (Exemple : Facture n°1 | Facture n°2)

#### Workflows

* Possibilité de créer un job à partir du nom d'un workflow plutôt que par son uuid ou son id. La version active du Worfklow est automatiquement récupérée.
* Ajout d'une description éditable pour chaque Workflow.
* Une correction a été appliquée pour la suppression des workflows dans la corbeille.
* Dans la sortie JSON de l'étape "Webhook", ajout d'une clé "retries" qui liste les tentatives effectuées. Voir [#webhook](../../workflow/les-modules-workflow.md#webhook "mention")
* Correctif appliqué sur le paramètre **Ignorer pièce jointe si le nom contient** dans l'étape "Ingérer des e-mails".

#### OCR

* Le choix de l'OCR peut se faire au niveau d'un agent (extraction ou classification) ou d'un dataset.

#### Datasets

* Les fichiers .msg sont bien pris en compte dans les datasets de mails.
* Lors d'un export puis import d'un dataset, on ne pouvait pas entrainer de modèle d'extraction même si on avait un nombre de documents supérieur au seuil minimum. Il fallait ajouter puis supprimer un document pour que le statut se mette à jour. C'est maintenant corrigé.
