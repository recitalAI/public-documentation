---
description: Soumettre une classification d'e-mail à une revue humaine dans un Workflow.
---

# Module Workflow : Revue de classification d'e-mail

**Dépendance produit : Classify.** Le module **Revue de classification d'e-mail** suspend le Workflow pendant qu'un opérateur confirme, corrige ou écarte la classification d'un e-mail.

## Entrée

Le module attend l'objet produit par **Classification E-mail**. L'expression `data['classify']` sélectionne ces données ; elle ne sélectionne ni le fichier d'e-mail ni ses pièces jointes. L'objet doit notamment contenir les références `email_id` et `model` produites pendant la classification.

## Paramètres

| Paramètre | Explication et exemple |
| --- | --- |
| Expression d'entrée | `data['classify']` sélectionne la classification d'e-mail à revoir. |
| Itérer sur l'entrée | Crée une revue par élément lorsque l'expression renvoie une liste. |
| Clé de sortie | Par exemple `classify` : le retour de revue est accessible avec `data['classify']`. |

## Résultat

Après la revue, la sortie contient le retour transmis par Classify sous `result` :

```json
{
  "classify": {
    "model": "classification-e-mails",
    "email_id": 42,
    "result": {
      "feedback": {
        "feedback": "facture"
      },
      "reviewer_comment": "Catégorie confirmée",
      "email": {
        "id": 42,
        "type_": "email"
      }
    }
  }
}
```

`feedback.feedback` contient la classification retenue, `reviewer_comment` le commentaire éventuel et `email` l'identifiant de l'e-mail revu. Si l'e-mail est écarté, l'objet `feedback` contient `discard_reason` à la place de `feedback`.
