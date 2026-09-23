---
description: Les modules d'état Start, Done et État personnalisé d'un Workflow.
---

# Modules Workflow : États

Les modules **Start**, **Done** et **État personnalisé** mettent à jour l'état général du job. Si une URL de callback a été fournie lors de la création du job, chaque changement d'état déclenche une notification.

Les modules d'état ne requièrent pas d'entrée pour changer l'état. L'**Expression d'entrée** `None` laisse la sélection vide ; les données du job restent disponibles pour la notification de changement d'état.

## Paramètres communs

| Paramètre | Explication et exemple |
| --- | --- |
| Expression d'entrée | `None` : aucune valeur particulière n'est nécessaire. |
| État d'erreur | Si cette option est activée, l'étape se termine en erreur après le changement d'état. |

Les modules d'état ne créent aucune donnée de sortie.

## Start

Le module **Start** définit l'état `started`, qui représente l'état initial du job.

## Done

Le module **Done** définit l'état `done`, qui représente l'état final du job.

## État personnalisé

Le module **État personnalisé** définit une valeur d'état choisie pour le job. La valeur initiale proposée par cette carte est `custom-state`.

| Paramètre | Explication et exemple |
| --- | --- |
| Valeur de l'état | Valeur transmise dans le changement d'état, par exemple `waiting-for-approval`. |
