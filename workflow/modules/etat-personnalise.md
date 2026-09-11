---
description: Définir un état personnalisé pour un job Workflow.
---

# Module Workflow : État personnalisé

Le module **État personnalisé** définit une valeur d'état choisie pour le job. La valeur initiale proposée par cette carte est `custom-state`. Si une URL de callback a été fournie lors de la création du job, le changement d'état déclenche une notification.

## Entrée

Le module ne requiert pas d'entrée pour changer l'état. L'**Expression d'entrée** `None` laisse la sélection vide ; les données du job restent disponibles pour la notification de changement d'état.

## Paramètres

| Paramètre | Explication et exemple |
| --- | --- |
| Valeur de l'état | Valeur transmise dans le changement d'état, par exemple `waiting-for-approval`. |
| Expression d'entrée | `None` : aucune valeur particulière n'est nécessaire. |
| État d'erreur | Si cette option est activée, l'étape se termine en erreur après le changement d'état. |

Le module ne crée aucune donnée de sortie.
