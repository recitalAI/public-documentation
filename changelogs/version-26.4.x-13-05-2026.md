# Version 26.4.x (13/05/2026)

#### Extraction

* Migration de la table des ressources vers le nouveau composant DataTable
*

#### Workflows

*

#### Autre

* Correctifs et améliorations des pages Performance & Activité.



#### Routes API

Voici les routes qui ont été modifiées :&#x20;

#### 1) Route datapoint:

PUT /extract/api/v1/system\_2/extraction\_groups/{group\_id}/ PUT /extract/api/v1/system\_2/data\_point/{data\_point\_id}/&#x20;

**Nouveaux paramètres:**  &#x20;

`"extraction_regexes": [     "string"   ],`  &#x20;

`"labels_descriptions": [     "string"   ],`  &#x20;

`"label_regex": [     {       "pattern": "",       "substitute": ""     }   ],`



#### 2) Route Workflow Jobs

GET /workflows/api/v1/jobs/{job\_id} GET /workflows/api/v1/jobs/{job\_id}/history/

* **Nouveaux paramètres:**  &#x20;
* "parent\_id": 0,  &#x20;
* "related\_job\_id": 0



* **Paramètres supprimés:**&#x20;
* "children": null,  &#x20;
* "data": "string",  &#x20;
* "preliminary\_data": {},  &#x20;
* "logs": "string"



#### 3) Nouvelles routes pour récupérer les données json: &#x20;

* GET /workflows/api/v1/jobs/{job\_id}/data &#x20;
* GET /workflows/api/v1/jobs/{job\_id}/preliminary\_data &#x20;
* GET /workflows/api/v1/jobs/{job\_id}/history/{entry\_id}/data &#x20;
* GET /workflows/api/v1/jobs/{job\_id}/history/{entry\_id}/logs



#### 4) Le schéma de réponse json envoyé au Webhook url a aussi été modifié :

* **Avant :**    &#x20;

&#x20;    "job": {        &#x20;

&#x20;          "id": "number",        &#x20;

&#x20;           "state": "done",        &#x20;

&#x20;           "data": {...}        &#x20;

&#x20;     },    &#x20;

&#x20;    "data": null



*   **Après  :**  &#x20;

    "job": {        &#x20;

    &#x20;     "id": "number",        &#x20;

    &#x20;     "state": "done",        &#x20;

    &#x20;     "is\_test": true,        &#x20;

    &#x20;     "custom\_metadata": null    &#x20;

    },    &#x20;

&#x20;     data": {...}



#### 5) Routes modifiées :&#x20;

* POST /auth/api/v1/users/password/reset/{id}/&#x20;
* GET /auth/api/v1/users/{id}/&#x20;
* PUT /auth/api/v1/users/{id}/&#x20;
* DELETE /auth/api/v1/users/{id}/&#x20;
* POST /auth/api/v1/users/{id}/check-password/&#x20;
* PUT /auth/api/v1/users/{id}/toggle-datascientist
* **Paramètre avant:** user\_id and org\_id
* **Paramètre après :** id



#### 6) Nouvelles routes :&#x20;

* GET /extract/api/v1/dataset/label/{dataset\_id}/distribution/&#x20;
* POST /extract/api/v1/system\_2/document\_type/{document\_type\_id}/add\_extractors/&#x20;
* GET /auth/api/v1/health/request
* GET /auth/api/v1/ocr-config/&#x20;
* PATCH /auth/api/v1/ocr-config/&#x20;
* GET /auth/api/v1/ocr-providers/&#x20;
* POST /auth/api/v1/ocr-providers/&#x20;
* GET /auth/api/v1/ocr-providers/{provider\_id}/&#x20;
* PATCH /auth/api/v1/ocr-providers/{provider\_id}/&#x20;
* DELETE /auth/api/v1/ocr-providers/{provider\_id}/
* GET /auth/api/v1/repositories/&#x20;
* POST /auth/api/v1/repositories/&#x20;
* GET /auth/api/v1/repositories/blueprints&#x20;
* GET /auth/api/v1/repositories/{repository\_id}&#x20;
* PATCH /auth/api/v1/repositories/{repository\_id}&#x20;
* DELETE /auth/api/v1/repositories/{repository\_id}&#x20;
* POST /auth/api/v1/repositories/{repository\_id}
* PATCH /classify/api/v1/classification-models/{model\_id}/



#### 7) Routes supprimées :&#x20;

* POST /extract/api/v1/dataset/annotation/prefill\_entries/{model\_id}/&#x20;
* DELETE /extract/api/v1/dataset/annotation/prefill\_entries/{entry\_id}&#x20;
* GET /extract/api/v1/production/files/next/{document\_type\_id}/ POST /classify/api/v1/documents/
