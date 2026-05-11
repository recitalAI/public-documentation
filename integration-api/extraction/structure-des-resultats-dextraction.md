# Structure des résultats d'extraction

Le JSON de réponse envoyé au callback a cette structure :

```json
{
    "id": int, // Identifiant de l'extraction
    "name": str, // Nom du fichier
    "review_details": { // Utile pour la revue d'extraction
        "verified_by_id": null,
        "verified_by": "unknown",
        "verified_at": null,
        "opened_at": null,
        "manual_corrections": 0,
        "reviewer_comment": null
    },
    "status": "pending", // ["valid", "pending", "invalid"] 
    "number_of_pages": 1,
    "values": [...],
    "document_type_id": int, // Identifiant de l'Agent
    "correction_external_link": null, // Si la correction manuelle est activée, génère un lien publique avec un token valid X heures (configurable)
    "custom_metadata": null, // Renvoie le paramètre "custom_medata" s'il a été utilisé lors de l'appel
    "is_ocrized": true,
    "groups": [...],
    "objects": [...]
}
```



## Values

Liste chaque champ (extrait ou non) ne faisant pas partie d'un groupe. Un champ aura la structure suivante :&#x20;

```json
{
    "data_point_id": ..., // Identifiant du champ extrait
    "data_point_name": "...", // Nom de l'extracteur
    "value": {
        "origin": "custom_entity", // ["custom_entity", "extraction"]
        "confidence": 0.9986167550086975, // Taux de confiance du modèle - utilisation non-recommandée
        "status": "pending", // ["valid", "pending", "invalid"]
        "page_nb": 1, // Numéro de la page
        "value": "...", // Valeur extraire brute
        "location": { // Localisation
            "x_min": 0.518235294117647,
            "x_max": 0.5823529411764706,
            "y_min": 0.20318181818181819,
            "y_max": 0.21045454545454545
        },
        "valid_page_nb": null, // Numéro de la page après correction
        "valid_value": null, // Valeur après correction
        "valid_location": null, // Localisation après correction
        "normalized_value_type": "string", 
        "normalized_value": "...", // Valeur extraite normalisée
        "prevalidated": null,
        "extra": null,
        "business_rule_strings": []
    },
    "position": null,
    "intermediate": false // Visible depuis l'écran de correction ou non
}
```

## Groups

Liste chaque groupe de champs. Un groupe aura la structure suivante :

```json
{
    "group_id": ..., // Identifiant du groupe
    "group_name": "...", // Nom du groupe
    "subgroups": [ // Liste chaque occurence d'un groupe
        {
            "page_nb": 1,
            "values": [ // Liste chaque champ présent dans le groupe
                {
                    "confidence": 0.9994925260543823,
                    "status": "pending",
                    "value": "...",
                    "location": {
                        "x_min": 0.05411764705882353,
                        "x_max": 0.1988235294117647,
                        "y_min": 0.35409090909090907,
                        "y_max": 0.3613636363636364
                    },
                    "label": "...", // Nom du label
                    "id": ..., // Identifiant du champ extrait
                    "page_nb": 1,
                    "valid_page_nb": null,
                    "valid_value": null,
                    "valid_location": null,
                    "normalized_value_type": "string",
                    "normalized_value": "..." // Str, Int ou Float
                    "prevalidated": null,
                    "business_rule_strings": [],
                    "label_display_name": "..." // Nom du champ
                },
                {
                    "confidence": 0.9993214011192322,
                    "status": "pending",
                    "value": "0.00",
                    "location": {
                        "x_min": 0.40352941176470586,
                        "x_max": 0.42470588235294116,
                        "y_min": 0.35454545454545455,
                        "y_max": 0.36
                    },
                    "label": "...",
                    "id": 8029081,
                    "page_nb": 1,
                    "valid_page_nb": null,
                    "valid_value": null,
                    "valid_location": null,
                    "normalized_value_type": "string",
                    "normalized_value": "...",
                    "prevalidated": null,
                    "business_rule_strings": [],
                    "label_display_name": "..."
                },
                ... # Répétition pour chaque champs ajouté au groupe
            ],
            "id": 3013063, // Identifiant du groupe
            "index": 0,
            "origin": "extraction"
        },
        ... # Répétition pour chaque occurence
    ]
}
```

## Objets

Liste chaque objet détecté (uniquement si les options "détecter signature" ou "détecter QR code" sont activés dans l'agent d'extraction. Un objet aura la structure suivante :&#x20;

```json
{
    "object_dp_name": "SIGNATURE", // "SIGNATURE", "QR_CODE"
    "object_dp_id": ...,
    "object_values": [
        {
             "object_id": ...,
             "object_dp_id": ...,
             "confidence": null,
             "status": "pending",
             "page_nb": 1,
             "extra": null,
             "location": {
                  "x_min": 0.7296875,
                  "x_max": 0.9015625,
                  "y_min": 0.88125,
                  "y_max": 0.9609375
             },
             "origin": "model"
         }
     ]
}
```



