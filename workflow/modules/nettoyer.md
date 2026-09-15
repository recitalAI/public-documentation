---
description: Supprimer des données, fichiers ou historiques devenus inutiles dans un Workflow.
---

# Module Workflow : Nettoyer

Le module **Nettoyer** supprime les données ou fichiers devenus inutiles, généralement en fin de Workflow. Il ne possède pas d'**Expression d'entrée** : ses options s'appliquent directement au job.

## Paramètres

| Paramètre | Explication et exemple |
| --- | --- |
| Keep data | Préserve l'objet `data` du job. Activez cette option si une étape ou un callback doit encore lire les résultats. |
| Keep files | Préserve les collections de fichiers du job. |
| Conserver l'historique | Préserve l'historique détaillé des étapes. |
| Conserver préliminaire | Préserve les données et fichiers initiaux du job. |

Une option activée conserve la catégorie correspondante ; une option désactivée autorise sa suppression. Le module ne crée aucune donnée de sortie.
