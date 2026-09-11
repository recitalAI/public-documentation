---
description: Classer des documents dans un Workflow avec un Agent de classification.
---

# Module Workflow : Classification

**Dépendance produit : Classify.** Le module **Classification** utilise un Agent de classification de documents configuré dans l'organisation. Les cartes portant le nom d'un Agent sont des raccourcis vers ce même module.

## Entrée

Le module attend un fichier de document. L'expression `files['file']` sélectionne la collection `file`. Si la collection contient plusieurs documents, activez **Itérer sur l'entrée** pour transmettre un fichier à chaque exécution du module.

## Paramètres

| Paramètre | Explication et exemple |
| --- | --- |
| Expression d'entrée | `files['file']` sélectionne les documents à classer. |
| Agent de classification | Agent configuré dans l'organisation et utilisé pour la prédiction. |
| Expression de l'agent de classification | Sélection dynamique, par exemple `data['routing']['classification_agent']`. Elle remplace l'Agent sélectionné lorsqu'elle est renseignée. |
| Par page | Classe séparément les pages et produit les labels et ruptures nécessaires à une division ultérieure du document. |
| Inclure le texte OCR | Ajoute le texte OCR au résultat renvoyé par l'Agent. |
| Itérer sur l'entrée | Traite séparément chaque fichier sélectionné. |
| Clé de sortie | Par exemple `classify` : le résultat est accessible avec `data['classify']`. |

## Résultat

Consultez la [structure des résultats de Classification](../../integration-api/classification/structure-des-resultats-de-classification.md). Pour diviser un document après une classification par page, le module **Diviser un document** lit notamment :

* `data['classify']['result']['prediction']['labels']` ;
* `data['classify']['result']['prediction']['breaks']`.
