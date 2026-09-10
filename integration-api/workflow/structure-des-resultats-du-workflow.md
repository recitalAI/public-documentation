# Structure des résultats du Workflow

## Structure du Workflow

Les données du Workflows vont être constituées de l'ensemble des étapes successives de ce dernier. Prenons par exemple le Workflow suivant :

<figure><img src="../../.gitbook/assets/image (127).png" alt=""><figcaption></figcaption></figure>

Chaque étape que l'on rajoute dans le Workflow possède un paramètre "Clé de sortie", qui correspond à la clé JSON des données du Workflow.

<figure><img src="../../.gitbook/assets/image (128).png" alt=""><figcaption></figcaption></figure>

La sortie du JSON aura donc la structure suivante :

```json
{
  "classify": {...}, // Les résultats de l'étape de classification
  "split-pdf": {...}, // Les résultats de l'étape de déliassage
  "extract": {...}, // Les résultats de l'étape d'extraction
  "custom_metadata": null // Renvoie le paramètre "custom_metadata" s'il est utilisé
}
```

## Structure d'une étape

Le contenu d'une clé de sortie n'est pas directement le résultat du module : celui-ci est encapsulé, à côté des identifiants de l'Agent et du job qui l'ont produit. Pour une étape d'extraction :

```json
{
  "extract": {          // La clé de sortie de l'étape
    "doctype": 2028,    // Identifiant de l'Agent d'extraction
    "job": 2840990,     // Identifiant du job d'extraction
    "result": {         // Le résultat d'extraction lui-même
      "id": 2840990,
      "name": "document.pdf",
      "status": "in_workflow",
      "number_of_pages": 42,
      "values": [...],
      "groups": [...],
      "objects": [...]
    }
  }
}
```

{% hint style="warning" %}
Lors de l'intégration, pensez à descendre jusqu'à `result` pour retrouver les structures décrites dans [Structure des résultats d'extraction](../extraction/structure-des-resultats-dextraction.md) et [Structure des résultats de classification](../classification/structure-des-resultats-de-classification.md).
{% endhint %}

## Structure des étapes du Workflow

Voir [Les modules Workflow](../../workflow/les-modules-workflow.md)

