# Version 25.2.x (05/02/2025)

### Version 25.2.1 (05/02/2025)

#### **Extraction**

* Ajout d'un paramètre permettant d'étendre une extraction à une cellule de tableau. Avec cette option d'activé, une extraction s'étend jusqu'à la fin de la cellule de tableau, et deux extractions différentes ne peuvent pas coexister au sein d'une même cellule de tableau.&#x20;
* &#x20;Dans la configuration d'un agent d'extraction, il est maintenant possible de choisir entre une sortie JSON en format liste (format historique) ou format dictionnaire.

#### Écran de review d'extraction

* Pour les tableaux, les lignes sont triées par ordre d'apparition dans le document.
* Possibilité d'ajouter un commentaire lors de la review.

#### Classification

* Dans la matrice de confusion, les documents en erreur sont ouverts dans un nouvel onglet.
* Correction d'un bug lié à la suppression d'un modèle de classification.
* Nouvel route API permettant de changer le label d'une classification en post-traitement avant la review.
* Les seuils relatifs de classification (label "Unknown") ont été ajoutés. Un seuil relatif va regarder la différence de score entre le premier et le deuxième meilleur label d'une prédiction.

#### Écran de review de classification

* En mode délissage, ajout d'un bouton pour sélectionner toutes les pages d'un coup.
* Possibilité d'ajouter un commentaire lors de la review.
* En mode déliassage, les listes déroulantes pour changer le label des pages (tri par probabilité) avaient le même ordre pour toutes les pages (indexé sur la page n°1). Ce problème est désormais résolu.
* Les documents dans la bannière de review sont dorénavant groupés par agent de classification.
* Possibilité de faire un feedback (ajouter une page ou le document entier dans un dataset).

#### Workflows

* L'étape "Split Document" accepte une liste de labels à ignorer. Les pages labellisées avec un label à ignorer ne seront pas prises en compte dans le découpage du document.
* Ajout d'une section "Ressources" permettant d'uploader des fichiers, et de pouvoir y accéder dans les workflows.
* Ajout d'une étape "Fusionner les documents".
* La sélection d'un sous-workflow dans l'étape "Workflow" est maintenant sauvegardée.
* L'étape "Lecture de Mail" possède désormais des paramètres pour filtrer les pièces jointes (extensions, tailles, noms)

#### OCR

* Rotation automatique des pages avec l'OCR Azure.

#### Autres

* Mise en place d'un système de priorisation équitable des documents de différentes organisations (Round-robin). Concrètement, cela signifie que sur le Saas mutualisé, si un client envoie un très grand nombre de documents à la suite, celà ne devrait pas impacter (ou très peu) les autres organisations.
* Les options "Pivoter automatiquement les pages" et "Redresser les documents de travers" sont activées par défaut.
* Correction de quelques problèmes d'heure/date.
* Les bugs liés aux exports / import d'agent ou de dataset ont été corrigés.

### Version 25.2.2 (24/02/2025)

#### Classification

* Dans la configuration des agents de classification, on peut activer une option permettant de générer un lien public vers l'écran de revue de classification. On peut également gérer la durée de vie de ce lien.

#### Écran de review de classification

* Résolution d'un bug empêchant des utilisateurs "Reviewer" d'accéder à la review des agents de classification.
* :exclamation: L'output JSON de "review\_details" a été homogénéisé avec celui de l'extraction.&#x20;
  * `validated_` → `verified_`

#### Écran de review d'extraction

* L'ordre des datapoints et groupe peut être configuré dans l'agent d'extraction.

#### Workflows

* :exclamation: L'output JSON de l'étape "Décompresser" a été modifié pour lister les fichiers non décompressés. Voir [Décompresser](../../workflow/modules/decompresser.md#resultat)
* Ajout d'une option dans l'étape "Webhook" pour activer ou non le Retry automatique.
* Ajout du paramètre "Agent Expression" dans l'étape de "Classification" pour utiliser une expression dynamique plutôt qu'un agent fixe.
* Ajout d'un bouton pour désélectionner un agent ou workflow dans la liste déroulante pour les étapes "Workflow", "Classification" et "Extraction".
* Ajout de la librairie python "xlrd" dans l'étape "Code Personnalisé".
* La suppression des workflow dans la corbeille a été corrigée.

#### Autres

* Homogénéisation des outputs JSON de classification et review de classification. Voir [Structure des résultats de Classification ](../../integration-api/classification/structure-des-resultats-de-classification.md)
