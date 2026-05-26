# Structure des résultats du workflow

> 🚧 **À reprendre** — JB a signalé que cette page doit être révisée (couvrir le mécanisme de callback en plus de la structure JSON).

## Structure du workflow

Les données du workflows vont être constituées de l'ensemble des étapes successives de ce dernier. Prenons par exemple le workflow suivant :

<figure><img src="../../.gitbook/assets/image (127).png" alt=""><figcaption></figcaption></figure>

Chaque étape que l'on rajoute dans le workflow possède un paramètre "Clé de sortie", qui correspond à la clé JSON des données du workflow.

<figure><img src="../../.gitbook/assets/image (128).png" alt=""><figcaption></figcaption></figure>

La sortie du JSON aura donc la structure suivante :

```json
{
  "classify": {...}, // Les résultats de l'étape de classification
  "split-pdf": {...} // Les résultats de l'étape de déliassage,
  "custom_metadata": null // Renvoie le paramètre "custom_metadata" s'il est utilisé
}
```

## Structure des étapes du workflow

Voir [Les modules Workflow](../../workflow/les-modules-workflow.md)

