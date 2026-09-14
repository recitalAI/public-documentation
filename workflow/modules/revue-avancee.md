---
description: Créer une revue avancée dans une file d'attente Extract Review depuis un Workflow.
---

# Module Workflow : Revue avancée

**Dépendance produit : Extract Review.** Le module **Revue avancée** crée un document dans une file d'attente de revue puis suspend le Workflow jusqu'à sa validation ou son recyclage. Pour le fonctionnement des files, consultez [Revue avancée](../../autres/review-avancee.md).

## Entrée

Le module attend un objet de données décrivant le document à revoir. L'expression `data['advanced-review']` sélectionne cet objet. Celui-ci peut fournir `polyvore_reference`, `filename`, `values` et `due_date`. Les expressions dédiées peuvent remplacer la référence, les valeurs ou l'échéance lues dans cet objet.

Exemple de données d'entrée :

```json
{
  "advanced-review": {
    "polyvore_reference": "documents/facture-42.pdf",
    "filename": "facture-42.pdf",
    "values": [],
    "due_date": "2026-07-24T16:00:00+00:00"
  }
}
```

## Paramètres

| Paramètre | Explication et exemple |
| --- | --- |
| File d'attente | File Extract Review dans laquelle créer le document. |
| Expression d'entrée | `data['advanced-review']` sélectionne l'objet décrivant la revue. |
| Itérer sur l'entrée | Crée une revue pour chaque objet lorsque l'expression renvoie une liste. |
| Polyvore reference expression | Sélectionne une référence de fichier, par exemple `data['document']['polyvore_reference']`. |
| Values expression | Sélectionne les valeurs initiales, par exemple `data['extract']['result']['values']`. |
| Expression de la date d'échéance | Sélectionne une échéance, par exemple `data['review_due_date']`. |
| Clé de sortie | Par exemple `advanced-review` : le retour est accessible avec `data['advanced-review']`. |

## Résultat

Pendant l'attente, la sortie conserve l'entrée dans `input` et utilise `null` pour `result`. À la fin de la revue, `result` contient exactement le payload renvoyé par Extract Review. Pour un document validé, sa structure est la suivante :

```json
{
  "advanced-review": {
    "result": {
      "message": "Document validated in Extract Review",
      "timestamp": "2026-07-24T15:42:10.123456",
      "document": {
        "id": 42,
        "filename": "facture-42.pdf",
        "status": "VALIDATED",
        "queue_id": 7,
        "external_id": null,
        "worker_id": 12
      },
      "review_details": {
        "opened_at": "2026-07-24T15:30:00+00:00",
        "verified_at": "2026-07-24T15:42:10+00:00"
      },
      "values": []
    }
  }
}
```

Pour un document recyclé, `document.status` vaut `RECYCLED` et le payload contient `recycle_data` à la place de `values`.
