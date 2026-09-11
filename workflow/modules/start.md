---
description: Définir l'état initial started d'un job Workflow.
---

# Module Workflow : Start

Le module **Start** définit l'état `started`, qui représente l'état initial du job. Si une URL de callback a été fournie lors de la création du job, ce changement d'état déclenche une notification.

## Entrée

Le module ne requiert pas d'entrée pour changer l'état. L'**Expression d'entrée** `None` laisse la sélection vide ; les données du job restent disponibles pour la notification de changement d'état.

## Paramètres

| Paramètre | Explication et exemple |
| --- | --- |
| Expression d'entrée | `None` : aucune valeur particulière n'est nécessaire. |
| État d'erreur | Si cette option est activée, l'étape se termine en erreur après le changement d'état. |

Le module ne crée aucune donnée de sortie.
