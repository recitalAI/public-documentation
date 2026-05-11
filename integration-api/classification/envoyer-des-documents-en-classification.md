# Envoyer des documents en classification

## Envoyer des documents en classification

Pour charger un document dans un Agent de classification, vous avez besoin de :

* `headers` pour vous authentifier (voir [Authentification](../authentification.md))
* `classification_agent_id` (int): ID de l'Agent de classification dans lequel vous souhaitez envoyer les documents. Il se trouve à droite du nom de l'Agent (#).
* `file_path`⁣ : Le chemin vers votre fichier.
* `callback_url`⁣ : L’adresse à laquelle seront renvoyées les valeurs extraites des documents. Voir [Récupérer les résultats de classification](envoyer-des-documents-en-classification.md#recuperer-les-resultats-de-la-classification)

Si l'extrait de code ci-dessous s'exécute correctement, un identifiant de classification vous sera envoyé, confirmant que le fichier a bien été reçu et est en cours de traitement.

<pre class="language-python"><code class="lang-python"><strong>import os
</strong>import requests

URL_SERVER = 'https://extract.classify.recital.ai/classify/api/v1'
classification_agent_id = ...
file_path = "..."
callback_url = "..."

filename = os.path.basename(file_path)
with open(file_path, "rb") as file:
    response = requests.post(
        url=f'{URL_SERVER}/classification_agent_prediction/',
        files={"file_in": (filename, file, "application/pdf")},
        data={
            "classification_agent_id": classification_agent_id,
            "is_bundle": False # True pour faire du déliassage (classification page à page)
            "webhook_url":callback_url
        },
        headers=headers
    )
    if response.status_code == 201:
        print(f"File {filename} is processing : {response.json()}")
    else:
        raise Exception(f"{filename} hasn't been uploaded - {response.status_code} - {response.reason} - {response.content}")
</code></pre>

## Récupérer les résultats de la classification

Le traitement des documents étant asynchrone, les résultats de la classification ne seront pas envoyés directement en réponse à l'appel.

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
