---
description: Comparer une extraction existante au résultat d'un second Agent d'extraction.
---

# Module Workflow : Validation d'ensemble

**Dépendance produit : Extract.** Le module **Validation d'ensemble** envoie le document à un Agent d'extraction utilisé comme second avis, puis compare ses valeurs au résultat d'extraction déjà présent. Les valeurs concordantes sont confirmées dans l'extraction d'origine.

## Entrées

Le module associe deux entrées configurées séparément :

* `files['file']` sélectionne le fichier à analyser par l'Agent de validation ;
* `data['extract']` sélectionne le résultat de la première extraction à comparer.

Il ne faut pas utiliser `zip(...)` ici : le module possède une **Expression d'entrée** pour le fichier et une **Expression de l'extraction** pour les données correspondantes.

## Paramètres

| Paramètre | Explication et exemple |
| --- | --- |
| Agent d'extraction | Agent d'extraction configuré qui fournit le second avis. |
| Expression de l'agent d'extraction | Sélection dynamique d'un Agent, par exemple `data['routing']['validator_agent']`. Elle remplace l'Agent sélectionné lorsqu'elle est renseignée. |
| Expression d'entrée | `files['file']` sélectionne le document à valider. |
| Extract Expr | `data['extract']` sélectionne la première extraction à comparer. |
| Itérer sur l'entrée | Traite séparément chaque fichier sélectionné ; chaque itération doit correspondre au résultat d'extraction attendu. |
| Clé de sortie | Par exemple `ensemble_validation` : le résultat est accessible avec `data['ensemble_validation']`. |

## Résultat

La réponse de la seconde extraction est enregistrée sous `result`. Après comparaison, elle contient aussi `data_point_values`, les valeurs simples de l'extraction d'origine après validation, et `extraction_group_values`, ses groupes après validation.

```json
{
  "ensemble_validation": {
    "result": {
      "id": 502,
      "name": "facture.pdf",
      "review_details": {
        "verified_by_id": null,
        "verified_by": "unknown",
        "verified_at": null,
        "opened_at": null,
        "manual_corrections": 0,
        "reviewer_comment": null
      },
      "status": "automated",
      "number_of_pages": 1,
      "pages_rotated_by": [0],
      "values": {},
      "document_type_id": 17,
      "correction_external_link": null,
      "custom_metadata": {},
      "is_ocrized": true,
      "user_correction": false,
      "groups": {},
      "objects": [],
      "polyvore_reference": null,
      "data_point_values": [],
      "extraction_group_values": []
    }
  }
}
```

Le contenu détaillé des valeurs et groupes suit la [structure des résultats d'Extraction](../../integration-api/extraction/structure-des-resultats-dextraction.md). Si la seconde extraction échoue, `result` contient le payload d'erreur reçu.
