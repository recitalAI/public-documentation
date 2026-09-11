---
description: Fusionner plusieurs documents en un seul fichier dans un Workflow.
---

# Module Workflow : Fusionner des documents

Le module **Fusionner des documents** fusionne, dans leur ordre d'entrée, les pages de plusieurs documents en un seul fichier PDF.

## Entrée

Le module attend une collection de fichiers complète. L'expression `files['file']` sélectionne tous les fichiers de la collection `file`. N'activez pas **Itérer sur l'entrée** lorsque ces fichiers doivent être fusionnés ensemble : l'itération transmettrait un fichier à la fois.

Pour fusionner les sous-documents d'une division, utilisez par exemple `files['split-file']`.

## Paramètres

| Paramètre | Explication et exemple |
| --- | --- |
| Collection de sortie | Par exemple `merged-file` : le PDF créé est accessible avec `files['merged-file']`. |
| Nom du fichier de sortie | Par exemple `dossier-complet.pdf`. |
| Expression d'entrée | `files['file']` ou `files['split-file']` sélectionne les documents à fusionner. |
| Itérer sur l'entrée | Laissez cette option désactivée pour fusionner la collection en un seul document. |
| Clé de sortie | Par exemple `merge-pdf` : les informations du fichier sont accessibles avec `data['merge-pdf']`. |

## Résultat

```json
{
  "merge-pdf": {
    "file_id": 103,
    "name": "dossier-complet.pdf"
  }
}
```
