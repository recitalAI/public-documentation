---
description: Créer une revue avancée dans une file d'attente Extract Review depuis un Workflow.
---

# Module Workflow : Revue avancée

**Dépendance produit : Extract Review.** Le module **Revue avancée** crée un document dans une file d'attente de revue puis suspend le Workflow jusqu'à sa validation ou son recyclage. Pour le fonctionnement des files, consultez [Revue avancée](../../autres/review-avancee.md).

{% hint style="info" %}
Cette page décrit la version **26.8.38**. Dans cette version, les données d'Extraction doivent être converties au format de revue. Les champs **Résultat d'extraction** et **Résultats personnalisés** ne font pas partie de cette configuration.
{% endhint %}

## Format attendu en entrée

**Expression d'entrée** doit renvoyer un objet décrivant la revue, et non une liste de valeurs ou le résultat natif d'Extraction. L'expression par défaut est `data['advanced-review']`.

| Propriété | Format et nécessité |
| --- | --- |
| `polyvore_reference` | Chaîne identifiant un document déjà stocké et accessible dans votre organisation. Obligatoire, sauf si **Polyvore reference expression** la fournit. Ne pas utiliser un chemin local ou une URL de téléchargement. |
| `filename` | Chaîne facultative. Sans nom, Extract Review utilise `document_<référence>`. |
| `values` | Liste facultative de valeurs **au format de revue** décrit ci-dessous. Omise, `null` ou vide, elle laisse les champs s'initialiser selon le layout de la file. |
| `due_date` | Échéance facultative au format ISO 8601, par exemple `2026-11-10T16:00:00+00:00`. Omise ou `null`, aucune échéance n'est fournie. |

Objet minimal, une fois la référence d'exemple remplacée par une référence réelle :

```json
{
  "polyvore_reference": "REFERENCE_DOCUMENT_EXISTANT"
}
```

Cet objet est la **valeur évaluée** de l'expression. Pour utiliser l'expression par défaut, placez-le sous la clé `advanced-review` des données du Workflow.

### Valeurs simples et groupes

Chaque élément de `values` désigne un champ par `data_point_config_name`, qui doit correspondre au nom configuré dans le layout de la file. Pour un champ simple, `value` est une chaîne, un nombre, un booléen ou `null` ; une date peut être fournie sous forme de chaîne ISO 8601. Sans `value`, la valeur initiale est `null`.

Les propriétés facultatives sont :

- `page_nb` : numéro de page entier à partir de **1** ;
- `x_min`, `x_max`, `y_min`, `y_max` : coordonnées entre **0 et 1**, avec chaque minimum inférieur ou égal au maximum ; elles sont directement dans l'élément, pas sous `location` ;
- `index` : indice d'occurrence, **0** par défaut ;
- `require_review` : booléen, **false** par défaut ;
- `callback_metadata` et `options` : objets de métadonnées facultatifs.

Pour représenter une occurrence de groupe, fournissez un élément parent dont `value` est une liste de champs enfants, avec le même format. Utilisez un parent par occurrence, avec des indices distincts :

```json
{
  "polyvore_reference": "REFERENCE_DOCUMENT_EXISTANT",
  "values": [
    {
      "data_point_config_name": "lignes",
      "index": 0,
      "value": [
        {"data_point_config_name": "designation", "value": "Abonnement"},
        {"data_point_config_name": "montant", "value": 120}
      ]
    }
  ]
}
```

Les noms de cet exemple doivent être adaptés au layout. Les noms de champs simples inconnus sont ignorés à l'enregistrement ; une requête acceptée ne garantit donc pas que toutes ses valeurs seront affichées. Les champs manquants sont initialisés selon le layout. Une référence inexistante, un type de valeur incompatible ou des coordonnées invalides peuvent empêcher la création ou l'initialisation de la revue.

## Paramètres

| Paramètre | Explication et exemple |
| --- | --- |
| File d'attente | File Extract Review dont le layout définit les champs à revoir. |
| Expression d'entrée | Sélectionne ou construit l'objet de revue. Par défaut : `data['advanced-review']`. |
| Itérer sur l'entrée | Crée une revue pour chaque objet lorsque l'expression renvoie une liste. |
| Polyvore reference expression | Remplace la référence de l'objet, par exemple `data['extract']['result']['polyvore_reference']`. |
| Values expression | Remplace entièrement la liste `values` de l'objet par une liste au format de revue. |
| Expression de la date d'échéance | Remplace l'échéance de l'objet, par exemple `data['review_due_date']`. |
| Clé de sortie | Par défaut `advanced-review` : le retour est accessible avec `data['advanced-review']`. |

Les expressions dédiées, lorsqu'elles sont renseignées, remplacent les propriétés correspondantes : elles ne fusionnent pas les listes de valeurs. Pour personnaliser les résultats dans cette version, construisez l'objet dans **Expression d'entrée**, ou fournissez la liste complète dans **Values expression**.

## Relier Extraction à Revue avancée

### Distinguer les deux formats

Après la fin d'une étape **Extraction** dont la **Clé de sortie** est `extract` :

- `data['extract']` contient l'enveloppe Workflow : `doctype`, `job` et `result` ;
- `data['extract']['result']` contient le résultat d'Extraction ;
- `data['extract']['result']['values']` contient les champs simples d'Extraction, **pas** des valeurs déjà au format de revue. Les occurrences de groupes sont dans `groups`.

Consultez la [structure des résultats d'Extraction](../../integration-api/extraction/structure-des-resultats-dextraction.md) pour le détail du format natif.

Dans la version 26.8.38, utilisez **`data`**, pas `result`, comme variable d'expression : `result['extract']` n'est pas défini dans ce contexte. Le mot `result` dans `data['extract']['result']` est une **clé de l'enveloppe**, pas une variable.

### Exemple de conversion d'un champ extrait

Voici un extrait des données disponibles après Extraction ; la référence doit provenir d'une extraction réelle :

```json
{
  "extract": {
    "doctype": 2028,
    "job": 2840990,
    "result": {
      "name": "facture-42.pdf",
      "polyvore_reference": "REFERENCE_DOCUMENT_EXISTANT",
      "values": [
        {
          "data_point_name": "numero_facture",
          "value": {
            "value": "FA 0042",
            "normalized_value": "FA-0042",
            "page_nb": 1,
            "location": {
              "x_min": 0.1,
              "x_max": 0.4,
              "y_min": 0.1,
              "y_max": 0.2
            }
          }
        }
      ],
      "groups": []
    }
  }
}
```

Choisissez une **File d'attente** dont le layout contient le champ `numero_facture`. Dans **Expression d'entrée**, construisez l'objet de revue avec cette expression :

```python
{
    'polyvore_reference': data['extract']['result']['polyvore_reference'],
    'filename': data['extract']['result']['name'],
    'values': [{
        'data_point_config_name': v['data_point_name'],
        'value': v['value']['normalized_value'] if v['value'].get('normalized_value') is not None else v['value'].get('value'),
        'page_nb': v['value'].get('page_nb'),
        'x_min': v['value']['location'].get('x_min') if v['value'].get('location') is not None else None,
        'x_max': v['value']['location'].get('x_max') if v['value'].get('location') is not None else None,
        'y_min': v['value']['location'].get('y_min') if v['value'].get('location') is not None else None,
        'y_max': v['value']['location'].get('y_max') if v['value'].get('location') is not None else None
    } for v in data['extract']['result']['values']]
}
```

Laissez les expressions dédiées vides pour ne pas remplacer ces propriétés. L'objet transmis à la revue est alors :

```json
{
  "polyvore_reference": "REFERENCE_DOCUMENT_EXISTANT",
  "filename": "facture-42.pdf",
  "values": [
    {
      "data_point_config_name": "numero_facture",
      "value": "FA-0042",
      "page_nb": 1,
      "x_min": 0.1,
      "x_max": 0.4,
      "y_min": 0.1,
      "y_max": 0.2
    }
  ]
}
```

Cet exemple suppose que `values` est une liste de champs simples avec un objet `value`, et non une sortie d'Extraction configurée sous forme de dictionnaire. Il utilise la valeur normalisée lorsqu'elle n'est pas `null`, sinon la valeur brute. Il ne transfère ni les corrections `valid_value` ni les groupes : convertissez séparément chaque occurrence de `groups` au format parent/enfants décrit plus haut. Adaptez `extract` si la clé de sortie de l'étape Extraction est différente.

Ne transmettez pas directement `data['extract']`, `data['extract']['result']` ou sa liste `values` comme objet de revue : la version 26.8.38 ne convertit pas ces structures. Attendez la réussite de l'étape Extraction avant d'accéder à son résultat.

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
