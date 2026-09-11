---
description: Classer un e-mail dans un Workflow avec un modèle de classification d'e-mail.
---

# Module Workflow : Classification E-mail

**Dépendance produit : Classify.** Le module **Classification E-mail** classe un e-mail avec un modèle configuré dans l'organisation. Il peut transmettre au modèle les pièces jointes issues de l'étape **Ingérer des e-mails**.

## Entrée

Le module attend les données structurées d'un e-mail. L'expression `data['email']` sélectionne l'objet produit sous la clé `email` ; elle ne sélectionne pas un fichier. Les pièces jointes sont sélectionnées séparément avec `files['attachments']`, qui désigne une collection de fichiers.

## Paramètres

| Paramètre | Explication et exemple |
| --- | --- |
| Expression d'entrée | `data['email']` sélectionne les données du message à classer. |
| Expression des pièces-jointes | `files['attachments']` sélectionne les fichiers joints au message. |
| Modèle | Modèle de classification d'e-mail configuré dans l'organisation. |
| Expression du modèle | Sélectionne dynamiquement un modèle à partir des données du job, par exemple `data['routing']['email_model']`. Elle remplace le modèle sélectionné lorsqu'elle est renseignée. |
| Itérer sur l'entrée | Traite séparément chaque e-mail lorsque l'expression renvoie une liste. |
| Clé de sortie | Par exemple `classify` : le résultat est accessible avec `data['classify']`. |

## Résultat

La sortie contient la prédiction renvoyée par Classify :

```json
{
  "classify": {
    "model": "classification-e-mails",
    "email_id": 42,
    "attachments": [101],
    "prediction": {
      "label": "facture",
      "probabilities": {
        "facture": 0.98,
        "autre": 0.02
      }
    }
  }
}
```

Cette sortie peut être transmise à **Revue de classification d'e-mail** avec `data['classify']`.
