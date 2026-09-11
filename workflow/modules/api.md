---
description: Le module source API d'un Workflow.
---

# Module Workflow : API

Le module **API** est la source d'un job créé par l'API Workflow. Il identifie l'origine du job et ne possède ni expression d'entrée ni paramètre propre dans l'étape.

Les fichiers et données transmis lors de la création du job deviennent les collections `files` et l'objet `data` que les étapes suivantes peuvent sélectionner dans leurs expressions d'entrée.
