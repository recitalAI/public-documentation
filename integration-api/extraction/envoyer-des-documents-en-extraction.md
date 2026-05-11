# Envoyer des documents en extraction

## Envoyer des documents en extraction

Pour charger un document dans un Agent d'extraction, vous avez besoin de :

* `headers` pour vous authentifier (voir [Authentification](../authentification.md))
* `doc_type_id` (int): ID de l'Agent d'extraction dans lequel vous souhaitez envoyer les documents. Il se trouve à droite du nom de l'Agent (#).
* `file_path`⁣ : Le chemin vers votre fichier.
* `callback_url`⁣ : L’adresse à laquelle seront renvoyées les valeurs extraites des documents. Voir [Récupérer les résultats de l'extraction](envoyer-des-documents-en-extraction.md#recuperer-les-resultats-de-lextraction).

Si l'extrait de code ci-dessous s'exécute correctement, un identifiant d'extraction vous sera envoyé, confirmant que le fichier a bien été reçu et est en cours de traitement.

<pre class="language-python"><code class="lang-python"><strong>import os
</strong>import requests

URL_SERVER = 'https://extract.api.recital.ai/extract/api/v1'
doc_type_id = ...
file_path = "..."
callback_url = "..."

filename = os.path.basename(file_path)
with open(file_path, "rb") as file:
    response = requests.post(
        url=f'{URL_SERVER}/production/files/',
        files={"file_in": (filename, file, "application/pdf")},
        data={"doctype_id": doc_type_id, "callback_url":callback_url},
        headers=headers
    )
    if response.status_code == 201:
        print(f"File {filename} is processing : {response.json()}")
    else:
        raise Exception(f"{filename} hasn't been uploaded - {response.status_code} - {response.reason} - {response.content}")
</code></pre>

## Récupérer les résultats de l’extraction

Le traitement des documents étant asynchrone, les résultats de l'extraction ne seront pas envoyés directement en réponse à l'appel.

### Via un callback

```python
from fastapi import FastAPI, Request
import json

app = FastAPI()

@app.post("/callback")
async def receive_callback(request: Request):
    data = await request.json()  # Récupérer les données envoyées avec la requête
    # Sauvegarder les données dans un fichier
    with open("response.json", "w") as f:
        json.dump(data, f, indent=4)  # Ecrire les données dans un fichier JSON formaté
    return {"message": "Données reçues et sauvegardées"}
```
