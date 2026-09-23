---
description: Exécuter un traitement Python personnalisé dans un Workflow.
---

# Module Workflow : Code personnalisé

Le module **Code personnalisé** exécute un traitement Python fourni par l'utilisateur. La fonction d'entrée doit respecter cette signature :

```python
def execute_action(job, input):
    return StepActionType.done, {}
```

`job` expose les informations du job, notamment `job.data`. `input` contient la valeur sélectionnée par l'**Expression d'entrée**, ou un élément de cette valeur lorsque **Itérer sur l'entrée** est activé.

## Entrée

L'expression dépend du traitement :

* `data['final_result']` transmet un objet de données ;
* `files['file']` transmet la collection de fichiers ;
* `None` ne sélectionne aucune entrée.

## Paramètres

| Paramètre | Explication et exemple |
| --- | --- |
| Expression d'entrée | Sélectionne la valeur reçue dans `input`, par exemple `data['final_result']`. |
| Itérer sur l'entrée | Appelle la fonction une fois par élément lorsque l'expression renvoie une liste ou un `zip(...)`. |

## Résultat

Le dictionnaire renvoyé en deuxième position est fusionné dans les données du job. Par exemple, `return StepActionType.done, {"score": 0.95}` rend la valeur accessible avec `data['score']` dans une étape suivante.

Pour plus de détails, [contactez l'équipe reciTAL](../../contact/nous-contacter.md).
