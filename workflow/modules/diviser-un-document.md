---
description: Diviser un document en sous-documents à partir des labels d'une classification par page.
---

# Module Workflow : Diviser un document

Le module **Diviser un document** sépare un document en sous-documents à partir des labels et ruptures produits, par exemple, par une **Classification** exécutée **Par page**.

## Entrées

Le module combine trois sélections :

* `files['file']` sélectionne le fichier à diviser ;
* `data['classify']['result']['prediction']['labels']` sélectionne un label par page ;
* `data['classify']['result']['prediction']['breaks']` sélectionne les ruptures entre sous-documents.

L'expression de fichier sélectionne une collection, tandis que les deux autres expressions sélectionnent des données. Le module utilise ces valeurs ensemble ; elles ne doivent pas être regroupées avec `zip(...)`.

## Paramètres

| Paramètre | Explication et exemple |
| --- | --- |
| Collection de sortie | Par exemple `split-file` : les sous-documents sont accessibles avec `files['split-file']`. |
| Expression d'entrée | `files['file']` sélectionne le document source. |
| Expression des labels | `data['classify']['result']['prediction']['labels']` sélectionne les labels par page. |
| Breaks Expr | `data['classify']['result']['prediction']['breaks']` sélectionne les débuts de sous-documents. |
| Labels à ignorer | Par exemple `["discarded"]` exclut du résultat les pages portant ce label. |
| Itérer sur l'entrée | Traite séparément chaque fichier si l'expression renvoie plusieurs documents accompagnés de données compatibles. |
| Clé de sortie | Par exemple `split-pdf` : les métadonnées sont accessibles avec `data['split-pdf']`. |

## Résultat

```json
{
  "split-pdf": {
    "files": [
      {
        "name": "document-split-1.pdf",
        "label": "Carte grise",
        "pages": [1, 2],
        "file_id": 101
      },
      {
        "name": "document-split-2.pdf",
        "label": "CNI",
        "pages": [3],
        "file_id": 102
      }
    ]
  }
}
```

Les fichiers correspondants sont placés dans la collection configurée. Pour lancer un Workflow enfant par sous-document, associez fichiers et données avec `zip(files['split-file'], data['split-pdf']['files'])` et activez **Itérer sur l'entrée**.
