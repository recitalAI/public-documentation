# Structure des résultats de classification

## Classification simple

```json
{
    "correction_external_link": null // URL public vers la revue de classification si activée
    "token": null,
    "status": "finished",
    "prediction": {
        "filename": "....",
        "model": "...", // Le nom du modèle utilisé
        "discarded": false, // Utile pour la review de classification
        "discard_reason": null, // Utile pour la review de classification
        "probabilities": { // Les probabilités pour chaque catégorie
            "Label1": 0.08173,
            "Label2": 0.01386,
            "Label3": 0.90145,
        },
        "pages": [...], // Ne pas utiliser
        "label": "Label1" // La catégorie prédite par le modèle
    },
    "document": {
        "id": int, // the ID of classification
        "type_": "doc",
        "org_id": int, // Identifiant de l'organisation
        "agent_id": int // Identifiant de l'agent de classification
    },
    "review_details": { // Utile pour la revue de classification
        "verified_by_id": null,
        "verified_by": "unknown",
        "verified_at": null,
        "opened_at": null,
        "manual_corrections": 0,
        "reviewer_comment": null
    }
}
```

## Déliassage (Classification page à page)

```json
{
    "correction_external_link": null // URL public vers la revue de classification si activée
    "token": null,
    "status": "finished",
    "prediction": {
        "filename": "....",
        "model": "...", // Le nom du modèle utilisé
        "discarded": false, // Utile pour la review de classification
        "discard_reason": null, // Utile pour la review de classification
        "probabilities": {}, // Ne pas utiliser
        "pages": [
        {
            "page_number": 1,
            "discarded": false, // Utile pour la review de classification
            "discard_reason": null, // Utile pour la review de classification
            "label": "Label3", // La catégorie de la page prédite par le modèle de classification
            "probabilities": { // Les probabilités pour chaque catégorie
                "Label1": 0.08173,
                "Label2": 0.01386,
                "Label3": 0.90145,
            }
        },
        {
             "page_number": 2,
             ....
        }
        ],
        "labels": ["Label3", "Label1", "Label2"] // Les catégories de chaque page prédites par le modèle
    },
    "document": {
        "id": int, // the ID of classification
        "type_": "doc",
        "org_id": int, // Identifiant de l'organisation
        "agent_id": int // Identifiant de l'agent de classification
    },
    "review_details": { // Utile pour la revue de classification
        "verified_by_id": null,
        "verified_by": "unknown",
        "verified_at": null,
        "opened_at": null,
        "manual_corrections": 0,
        "reviewer_comment": null
    }
}
```



