# Envoyer des documents dans un Workflow

## Créer un nouveau Job

Pour charger un document dans un Workflow donné, vous avez besoin au minimum de:

* `headers` pour vous authentifier (voir [Authentification](../authentification.md)).
* `workflow_uuid` (str) ou `workflow_id` (int) :  Le UUID du Worflow reste fixe quelque soit sa  version. Le `workflow_id` est propre à la version du workflow.
* `file_path`⁣: Le chemin vers votre fichier.

Si l'extrait de code ci-dessous s'exécute correctement, un identifiant de job vous sera envoyé, confirmant que le fichier a bien été reçu et est en cours de traitement.

```python
import os
import requests

URL_SERVER = 'https://extract.workflows.recital.ai/workflows/api/v1'
workflow_uuid = "..."
file_path = "..."

filename = os.path.basename(file_path)
with open(file_path, "rb") as file:
    response = requests.post(
        url=f'{URL_SERVER}/jobs/',
        files={"file": (filename, file, "application/pdf")}, # Ici "file" désigne la collection dans laquelle le document sera envoyé
        params={"workflow_uuid": workflow_uuid},
        headers=headers
    )
    if response.status_code == 200:
        print(f"File {filename} is processing : {response.json()}")
    else:
        raise Exception(f"{filename} hasn't been uploaded - {response.status_code} - {response.reason} - {response.content}")
```

Si l'envoie du document s'est bien passé, vous pourrez suivre l'évolution de votre document dans l'onglet "[Jobs](../../workflow/jobs.md)" du Workflow.



