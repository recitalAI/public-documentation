---
description: Soumettre un résultat de classification de document à une revue humaine dans un Workflow.
---

# Module Workflow : Revue de classification

**Dépendance produit : Classify.** Le module **Revue de classification** suspend le Workflow pendant qu'un opérateur vérifie une classification de document.

<figure><img src="../../.gitbook/assets/image (1).png" alt="Écran de revue d'une classification"><figcaption>Écran de revue d'une classification.</figcaption></figure>

{% hint style="info" %}
Une étape de Review reste bloquante jusqu'à son traitement par un opérateur. Lorsqu'une date d'expiration est configurée, son dépassement est traité périodiquement ; aucune fréquence fixe n'est garantie.
{% endhint %}

## Entrée

Le module attend l'objet de résultat d'une classification, et non le fichier classé. L'expression `data['classify']` sélectionne la sortie enregistrée par une étape **Classification** sous la clé `classify`.

## Paramètres

| Paramètre | Explication et exemple |
| --- | --- |
| Contexte | Instructions affichées pendant la revue, par exemple `Vérifier les pages classées comme justificatif.` |
| Expression d'entrée | `data['classify']` sélectionne le résultat à revoir. |
| Expression du contexte | Contexte dynamique, par exemple `data['review_instructions']`. |
| Itérer sur l'entrée | Crée une revue pour chaque résultat lorsque l'expression renvoie une liste. |
| Expiration deadline expression | Par exemple `data.get('expiration_deadline', None)` pour lire une échéance facultative dans les données. |
| Action d'expiration | `validate` valide la revue ; `discard` l'écarte. |
| Clé de sortie | Par exemple `classify` : le résultat revu remplace ou complète `data['classify']`. |

## Résultat

Le module conserve la structure de Classification sous la clé choisie et place le retour de revue dans `result`. Consultez la [structure des résultats de Classification](../../integration-api/classification/structure-des-resultats-de-classification.md).
