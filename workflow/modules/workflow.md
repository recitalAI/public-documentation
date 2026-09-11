---
description: Lancer un Workflow enfant en lui transmettant des fichiers et des données associés.
---

# Module Workflow : Workflow

Le module **Workflow** lance un Workflow enfant. Il permet de réutiliser un Workflow ou de traiter séparément les sous-documents produits par une étape précédente.

## Entrée : associer chaque fichier à ses données

Le module accepte des fichiers, des objets de données, ou une séquence combinant les deux. Pour transmettre à chaque exécution un fichier et les données qui le décrivent, utilisez **Itérer sur l'entrée** avec :

```python
zip(files['split-file'], data['split-pdf']['files'])
```

`files['split-file']` sélectionne les fichiers créés par **Diviser un document**. `data['split-pdf']['files']` sélectionne, dans le même ordre, leurs métadonnées (`name`, `label`, `pages` et `file_id`). `zip(...)` associe les éléments de même position et produit une suite de couples `(fichier, données)`. Avec **Itérer sur l'entrée**, chaque lancement du Workflow enfant reçoit un couple.

{% hint style="warning" %}
Les deux séquences doivent représenter les mêmes documents dans le même ordre. `zip(...)` s'arrête à la fin de la séquence la plus courte.
{% endhint %}

Pour transmettre uniquement les données du job, utilisez par exemple `data['final_result']`. Pour transmettre uniquement une collection de fichiers, utilisez par exemple `files['file']`.

## Paramètres

| Paramètre | Explication et exemple |
| --- | --- |
| Workflow | Workflow enfant à lancer. |
| Expression du Workflow | Sélection dynamique d'un Workflow, par exemple `data['routing']['workflow']`. Elle remplace le Workflow sélectionné lorsqu'elle est renseignée. |
| Ignorer les erreurs | Permet à l'étape parente de se terminer sans erreur si le Workflow enfant échoue. |
| Ignorer les résultats | N'attend pas le résultat du Workflow enfant ; la sortie conserve seulement l'identifiant du job créé. |
| Expression d'entrée | Par exemple `zip(files['split-file'], data['split-pdf']['files'])` pour construire les couples fichier/données attendus. |
| Itérer sur l'entrée | Doit être activé pour lancer un Workflow enfant par couple produit par `zip(...)`. |
| Clé de sortie | Par exemple `workflow` : le retour est accessible avec `data['workflow']`. |

## Exemple de résultat

Lorsque les résultats ne sont pas ignorés, la sortie contient l'identifiant du job enfant et les données renvoyées à son succès :

```json
{
  "workflow": {
    "job": 314,
    "data": {
      "final_result": {
        "document_type": "facture"
      }
    }
  }
}
```

Consultez aussi la [structure des résultats du Workflow](../../integration-api/workflow/structure-des-resultats-du-workflow.md).
