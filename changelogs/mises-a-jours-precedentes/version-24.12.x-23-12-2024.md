---
description: >-
  Agents de classification, Amélioration de l'écran de review de classification,
  Amélioration du workflow
---

# Version 24.12.x (23/12/2024)

## Fonctionnalités

#### Dataset

* Possibilité d'importer des datasets d'email depuis un fichier ZIP

#### Extraction

* Une option a été ajoutée pour permettre à un extracteur de renvoyer le paragraphe entier plutôt que simplement la valeur extraite.
* Une barre de progression a été ajoutée lors de l'entrainement d'un modèle d'extraction.

#### Classification

* Une notification a été créée pour avertir que l'entrainement de son modèle de classification a échoué.
* **Les Agents de Classification** sont maintenant disponibles sur la plateforme. Ils permettent de configurer différents paramètres pour la classification :
  * Renommer les catégories du modèle de classification
  * Ajouter des catégories supplémentaires (sélectionnable lors de la review de classification)
  * Définir un seuil de classification. En dessous de ce seuil, la prédiction sera remplacée par la classe "Unknown".
  * Définir les raisons du rejet d'une page ou d'un document
  * Choisir son moteur d'OCR
* Une étape "Agents de classification" a été ajoutée dans les workflow. La classification directement  depuis un modèle est toujours possible. Le résultat JSON pour l'étape "Agents de classification" est la même que pour celle d'un modèle de classification.

#### Écran de review de classification

* Pour le déliassage ou pour la classification simple, il est désormais possible de supprimer des pages (pages blanches, pages indésirables, etc) et d'en indiquer la raison.
* Un champ "contexte" a été ajouté à l'écran, afin d'apporter des instructions à l'opérateur. Un texte explicatif y figure par défaut. Il est possible de le configurer dans l'étape de classification review du workflow.
* Les classes proposées à l'opérateur dans le dropdown sont maintenant triées par ordre de probabilité
* Il est possible de changer la classe de plusieurs pages en même temps avec le principe de "bundle".
* Des informations concernant la review ont été ajoutées dans le JSON de sortie :
  * id et email du reviewer
  * date et heure de début et de fin de la review
  * le nombre d'éléments modifiés&#x20;

#### Workflows

* L'interface générale a été retravaillée.
* Les noms des statuts des étapes ont été modifiés :
  * "done" -> "completed"
* Il est dorénavant possible d'ajouter des fichiers "ressources".
* Dans le code personnalisé, il est possible d'accéder aux fichiers ressources, mais également aux fichiers des différentes collections
  * ./resources/filename.doc
  * ./collection\_x/filename.doc
* Pour les sous-workflows, ou toutes les étapes sur lesquelles il est possible d'itérer, des logs ont été rajoutés pour visualiser les input data, et les fichiers dans le scope.
* Les transitions ont été ajoutées dans les jobs. Cet ajout permet de capturer toutes les erreurs liées aux transitions, mais également à la gestion des inputs dans une étape.
* Il est maintenant possible d'archiver un workflow.
* Étape "Webhook": ajout de tous les paramètres permettant de configurer le callback

#### OCR

* Un redimensionnement des images est effectué sur tous les documents passant par les OCR de Google ou d'Azure. L'objectif étant de s'assurer que les images envoyées respectent la taille recommandée par ces OCR.

#### Production

* Les fichiers de production en attente de review sont automatiquement supprimés après un certain temps (60 jours), s'il y a plus d'un certain nombre de documents en attente (100 documents).

#### Autres

* Les restrictions au niveau des noms des datasets et agents ont été revues.
* La redirection vers l'URL de connexion est désormais fonctionnelle lors de la création ou de la modification d'un mot de passe.

