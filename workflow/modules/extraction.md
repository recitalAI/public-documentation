---
description: Extraire les données d'un document dans un Workflow avec un Agent d'extraction.
---

# Module Workflow : Extraction

**Dépendance produit : Extract.** Le module **Extraction** utilise un Agent d'extraction configuré dans l'organisation. Les cartes portant le nom d'un Agent sont des raccourcis vers ce même module.

## Entrée

Le module attend un fichier de document. L'expression `files['file']` sélectionne la collection `file`. Si elle contient plusieurs documents, activez **Itérer sur l'entrée** pour transmettre un fichier à chaque exécution.

## Paramètres

| Paramètre | Explication et exemple |
| --- | --- |
| Expression d'entrée | `files['file']` sélectionne les documents à traiter. |
| Agent d'extraction | Agent configuré dans l'organisation et utilisé pour l'extraction. |
| Expression de l'agent d'extraction | Sélection dynamique, par exemple `data['routing']['extraction_agent']`. Elle remplace l'Agent sélectionné lorsqu'elle est renseignée. |
| Inclure le texte OCR | Ajoute le texte OCR au résultat d'extraction. |
| Itérer sur l'entrée | Traite séparément chaque fichier sélectionné. |
| Clé de sortie | Par exemple `extract` : le résultat est accessible avec `data['extract']`. |

## Résultat

Consultez la [structure des résultats d'Extraction](../../integration-api/extraction/structure-des-resultats-dextraction.md). Cette sortie peut être transmise à **Revue d'extraction** avec `data['extract']`, ou comparée par **Validation d'ensemble** avec l'expression d'extraction `data['extract']`.
