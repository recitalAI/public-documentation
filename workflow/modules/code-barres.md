---
description: Détecter les codes-barres présents sur chaque page d'un document Workflow.
---

# Module Workflow : Code-barres

Le module **Code-barres** détecte et lit les codes-barres présents dans un document.

## Entrée

Le module attend un seul fichier de document. `files['file']` sélectionne la collection `file`. Si plusieurs documents doivent être analysés, activez **Itérer sur l'entrée** pour transmettre un fichier à chaque exécution.

## Paramètres

| Paramètre | Explication et exemple |
| --- | --- |
| Expression d'entrée | `files['file']` sélectionne le document à analyser. |
| Itérer sur l'entrée | Analyse séparément chaque fichier sélectionné. |
| Clé de sortie | Par exemple `barcodes` : le résultat est accessible avec `data['barcodes']`. |

## Résultat

La sortie est une liste par page. Chaque page contient zéro, un ou plusieurs objets avec le texte décodé et les coordonnées normalisées du rectangle englobant, sous la forme `[[x_min, y_min], [x_max, y_max]]`.

```json
{
  "barcodes": [
    [
      {
        "text": "3760123456789",
        "coords": [
          [0.125, 0.42],
          [0.534, 0.61]
        ]
      }
    ],
    []
  ]
}
```

Dans cet exemple, un code-barres est détecté sur la première page et aucun sur la deuxième.
