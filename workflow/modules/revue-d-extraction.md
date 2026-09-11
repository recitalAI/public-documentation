---
description: Soumettre un résultat d'extraction à une revue humaine dans un Workflow.
---

# Module Workflow : Revue d'extraction

**Dépendance produit : Extract.** Le module **Revue d'extraction** suspend le Workflow pendant qu'un opérateur vérifie et corrige un résultat d'extraction.

<figure><img src="../../.gitbook/assets/image (122).png" alt="Écran de revue d'une extraction"><figcaption>Écran de revue d'une extraction.</figcaption></figure>

## Entrée

Le module attend l'objet de résultat d'une extraction, et non le fichier source. L'expression `data['extract']` sélectionne la sortie enregistrée par une étape **Extraction** sous la clé `extract`.

## Paramètres

| Paramètre | Explication et exemple |
| --- | --- |
| Expression d'entrée | `data['extract']` sélectionne le résultat à revoir. |
| Itérer sur l'entrée | Crée une revue pour chaque résultat lorsque l'expression renvoie une liste. |
| Expression de la date d'expiration de la revue | Par exemple `data.get('expiration_deadline', None)` pour lire une échéance facultative. |
| Action d'expiration | `validate` valide la revue ; `discard` l'écarte. |
| Clé de sortie | Par exemple `review` : le retour est accessible avec `data['review']`. |

## Résultat

Le retour de revue est enregistré sous `result` dans la clé de sortie. Il reprend la structure d'extraction, complétée par les informations de revue. Consultez la [structure des résultats d'Extraction](../../integration-api/extraction/structure-des-resultats-dextraction.md).
