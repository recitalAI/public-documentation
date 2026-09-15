---
description: Extraire les fichiers d'une archive ou d'un e-mail dans une collection Workflow.
---

# Module Workflow : Décompresser

**Dépendance produit : Classify.** Le module **Décompresser** extrait le contenu d'une archive ou d'un e-mail dans une collection de fichiers.

## Entrée

Le module attend un seul fichier d'archive ou d'e-mail. `files['file']` sélectionne la collection `file`. Si plusieurs archives doivent être traitées, activez **Itérer sur l'entrée** pour transmettre un fichier à chaque exécution.

## Paramètres

| Paramètre | Explication et exemple |
| --- | --- |
| Collection de sortie | Par exemple `unpacked` : les fichiers extraits sont accessibles avec `files['unpacked']`. |
| Expression d'entrée | `files['file']` sélectionne l'archive ou l'e-mail à décompresser. |
| Extensions | Liste séparée par des virgules, par exemple `pdf,png,jpg`, pour ne conserver que ces formats. Une valeur vide n'applique pas de restriction. |
| Max unpack depth | `1` extrait seulement le premier niveau ; les archives ou e-mails imbriqués restent des fichiers. Une valeur non renseignée n'impose pas de limite. |
| Itérer sur l'entrée | Traite séparément chaque fichier sélectionné. |
| Clé de sortie | Par exemple `unpack` : le bilan est accessible avec `data['unpack']`. |

## Résultat

```json
{
  "unpack": {
    "unpacked": [
      "facture.pdf",
      "justificatif.png"
    ],
    "skipped": [
      "signature.gif"
    ]
  }
}
```

`unpacked` et `skipped` contiennent les noms des fichiers respectivement conservés et écartés.
