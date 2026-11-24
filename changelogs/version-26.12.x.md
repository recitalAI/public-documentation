# Version 26.12.x

## Quoi de neuf dans cette version ?

La version 26.12 facilite la réutilisation et le transfert de configurations entre environnements. Les **Blueprints** font leur apparition : ils regroupent des ressources réutilisables, comme des Agents, des modèles et des Workflows. Vous pouvez aussi **exporter et importer un Workflow** avec ses dépendances.

Cette version améliore également le suivi des jobs de Workflow, la **Review avancée**, la configuration des modèles de langage et la portabilité des Agents et des files de révision. Plusieurs correctifs renforcent la fiabilité des éditions et des échanges de documents.

## Changements incompatibles et actions nécessaires

### Extract

Les chemins des tableaux Extract sont relatifs à `/extract/api/v1`.

| Route | Avant | Après | Action |
| --- | --- | --- | --- |
| `DELETE /system_2/extract_path/{ext_path_id}/` | Sans barre finale | Avec barre finale ; succès 204 | Mettez à jour l'URL, sans compter sur une redirection. |
| `POST /model/{model_id}/version/{version}/compatible/` | Sans barre finale | Avec barre finale ; `version` reste un entier dans le chemin et `labels` une liste facultative dans le corps | Mettez à jour l'URL, sans compter sur une redirection. |
| `GET /health/` | Sans paramètre ; 200, `status` textuel (`starting`, `running`, `error`) et booléen `sql_connection` | Même URL ; `status` et `services.sql` booléens ; 200 si SQL répond, 500 sinon | Adaptez la lecture du JSON et du statut HTTP de vos sondes. |
| `GET /system_2/document_type/prepare/{document_type_id}/` | Préparation du ZIP sans choix d'inclure les fichiers | `with_files` (booléen de requête) obligatoire ; `callback_url` (chaîne, facultative) remplace la notification si renseignée ; `send_notification` reste à `true` par défaut | Envoyez `with_files=true` pour inclure les fichiers ou `false` pour ne transférer que la configuration. La réponse 200 contient toujours `filename`. |
| `POST /files/{file_id}/copy_to_doctype/{document_type_id}/` | `section_to_copy` facultatif, défaut nul | En requête, défaut `full` ; valeurs `full`, `top`, `right`, `bottom`, `left` | Omettez le paramètre pour une page entière ou choisissez une valeur de la liste. |
| `POST /production/{file_id}/copy_to_doctype/{document_type_id}/` | Même défaut nul dans les données de formulaire | Même choix et défaut `full` dans le formulaire | N'envoyez pas de valeur nulle explicite. |
| `POST /system_2/document_type/import` | 202 sans résultat structuré ; ZIP multipart dans `zipfile` | 202 avec `id`, `name`, `created`, `updated`, `warnings` ; ajout de `existing_name` (nom de l'Agent cible) et `references` (chaîne JSON), tous deux facultatifs dans le formulaire | Gardez `zipfile` ; utilisez `id` pour suivre l'import. Le 202 ne signifie pas que le traitement est terminé. |

#### Extraction générative : sélection explicite du modèle

Auparavant, un modèle de repli pouvait être sélectionné implicitement. Choisissez maintenant un modèle dans l'Agent **avant** de lancer une extraction comportant un champ ou un groupe génératif. Sans modèle explicite, les appels suivants peuvent répondre **412** avec un message dans `detail`, avant la mise en file. Une extraction exclusivement classique ne nécessite pas de modèle de langage.

Tous ces appels utilisent `POST`, sous `/extract/api/v1` :

| Opération | Chemins concernés |
| --- | --- |
| Envoi d'un fichier ou de son contenu en base64 | `/files/`, `/files/base64/`, `/production/files/`, `/production/files/base64/` |
| Copie vers un Agent (configurer le modèle de destination) | `/files/{file_id}/copy_to_doctype/{document_type_id}/`, `/production/{file_id}/copy_to_doctype/{document_type_id}/` |
| Retraitement | `/files/reprocess/` (corps JSON : `file_ids`), `/files/{file_id}/rerun/system1/`, `/production/files/{file_id}/rerun/system1/` |
| Préannotation ou actualisation ciblée | `/dataset/annotation/{dataset_id}/prefill_agent/{document_type_id}/`, `/system_2/data_point/{document_type_id}/refresh/` (paramètre de requête facultatif `data_point_id` pour ne contrôler qu'un champ) |

### Authenticator : API des Blueprints

Ces changements concernent les intégrations qui utilisaient déjà l'API des dépôts.

| Route | Avant | Après | Action |
| --- | --- | --- | --- |
| `POST /auth/api/v1/repositories/{repository_id}/blueprints/` | Corps JSON avec `path` ; titre facultatif | Chemin généré, `path` rejeté (422) ; titre `org.opencontainers.image.title` obligatoire (1 à 64 caractères, sans séparateur de chemin ni caractère de contrôle). `latest_version_tag` et `objects` restent requis. Succès 202 ; dépôt en lecture seule : 403. | Retirez `path`, fournissez un titre et ciblez un dépôt accessible en écriture. |
| `GET /auth/api/v1/repositories/blueprints` | `object_types` : liste de chaînes | `object_types` : liste d'objets avec `type` (chaîne) et `count` (entier ou nul) ; ajout de `contents` | Lisez `type` dans chaque objet. |

### Review

Chemins relatifs à `/extract-review/api/v1`.

| Route | Avant | Après | Action |
| --- | --- | --- | --- |
| `POST /queues/{queue_id}/documents/` | Sans `filename` en requête, nom `document_{reference}` | Sans `filename`, nom du fichier source ; `reference` reste obligatoire | Indiquez `filename` si vous dépendez d'un nom précis. |
| `PATCH /queues/{queue_id}/documents/{document_id}/` | `PROCESSING` accepté dans `status` (corps JSON) | `PROCESSING` reste lisible, mais n'est plus accepté en écriture (422) ; succès 204 | Omettez `status` pour modifier un autre champ sans changer l'état. |

### Anciennes intégrations Extract

Si votre intégration utilise encore une URL historique sans le préfixe `/extract/api/v1`, passez au chemin canonique. Par exemple, `GET /files/{file_id}/` devient `GET /extract/api/v1/files/{file_id}/`. Cette action ne concerne pas les intégrations qui utilisent déjà les chemins canoniques, sauf pour les changements détaillés ci-dessus.

## Nouvelles fonctionnalités et améliorations

### Blueprints et ressources réutilisables

- Créez et réutilisez des Blueprints pour transférer des Agents et modèles d'extraction ou de classification, ainsi que des Workflows.
- Les Blueprints de Workflows peuvent inclure davantage de dépendances, afin de faciliter leur réutilisation dans d'autres environnements.
- La liste des Blueprints présente plus clairement le type et le contenu des ressources. Les dépôts en lecture seule sont identifiables et la sélection des Agents prend mieux en compte les filtres et la pagination.

### Workflows et suivi des jobs

- **Export et import avec dépendances :** un Workflow peut être exporté avec ses objets associés, puis importé dans un autre environnement. L'import prend en charge certains cas de mise à jour d'objets existants et affiche les références nécessaires aux fournisseurs. Les connexions et secrets spécifiques à la destination restent à vérifier après l'import.
- **Étapes Python :** prise en charge des tunnels SSH lorsque l'environnement est configuré pour les utiliser.
- **Suivi des jobs :** amélioration de la liste des jobs, de la consultation des sous-workflows et de l'affichage des détails d'un job dans un panneau latéral.
- **Notifications :** vous pouvez choisir quels événements déclenchent une notification webhook.

### Review et Review avancée

- **Portabilité :** export et import des configurations de files de révision.
- **Identification des documents :** ouverture d'un document de Review avancée à partir d'un identifiant externe transmis par un Workflow.
- **Entrée d'extraction :** possibilité de définir une entrée par défaut dans une étape de Review avancée.
- **Validation :** relance manuelle de la vérification des règles dans une file et affichage de messages personnalisés pour les champs nécessitant une correction.
- **Saisie et navigation :** raccourci de copie depuis le champ suivant et navigation plus fluide entre les pages.
- **Webhooks :** nouvelles tentatives de notification en cas d'échec. Les applications réceptrices doivent pouvoir reconnaître un événement déjà traité.
- **Entretien des files :** possibilité de demander la suppression en lot des documents anciens, selon la politique de conservation applicable.

### Extraction, classification et modèles de langage

- **Groupes d'extraction :** l'API expose le champ `group_labels` et prend en charge sa mise à jour.
- **Modèles LLM :** possibilité d'ajouter un fournisseur ou un modèle propre à l'organisation et de configurer le délai maximal de certains traitements génératifs.
- **Portabilité des Agents :** export et import des configurations d'Agents et de modèles ; l'export d'un Agent d'extraction peut exclure ses fichiers.
- **Classification :** amélioration de la configuration des Agents, de la validation et de l'affichage de l'état de correction des documents.
- **Traces de traitement :** ajout d'informations sur le coût des appels aux modèles de langage. Ces estimations ne correspondent pas nécessairement aux montants facturés.
- **Documents et OCR :** amélioration des opérations de copie, d'export et d'import, notamment pour les documents historiques et les annotations de position.

## Ajouts et autres évolutions API

### Extract : réponses et filtres

Chemins relatifs à `/extract/api/v1`.

| Route | Avant | Après | Description / action |
| --- | --- | --- | --- |
| `GET /files/{file_id}/`, `GET /production/files/{file_id}/` et `GET /production/files/next/{document_type_id}/` | `pages` obligatoire ; `image_path` obligatoire dans chaque page | `pages` est déprécié et peut être nul ; `image_path` est facultatif et nullable | Adaptez les lecteurs stricts. Pour récupérer une image, utilisez `GET /image/file/{file_id}/{page_nb}/`. |
| `GET /files/{file_id}/info/` | `image` obligatoire dans `FileRepresentationOut` | `image` facultatif et nullable | Ne supposez plus que le chemin de stockage est présent. |
| `GET /system_2/verify/{document_type_id}/` | `NextFilePageOut.image` obligatoire | `image` facultatif et nullable | Acceptez une page sans image dans la réponse. |
| `GET /files/{file_id}/text/` et `GET /production/files/{file_id}/text/` | Pas de GET sur ces chemins | 200 avec un objet JSON `text` | Récupérez le texte intégral sans envoyer de corps ; les POST existants restent disponibles. |
| `GET /dataset/entry/{entry_id}/` | Pas de filtre de libellé pour les compteurs | `annotated_label_filter` (chaîne de requête facultative) ; compteurs `nb_to_validate`, `nb_to_complete` | Omettez le filtre pour compter tous les libellés. |

### Extract et Classify : délai des traitements génératifs

| Route | Avant | Après | Description / paramètres |
| --- | --- | --- | --- |
| `POST /extract/api/v1/system_2/document_type/` et `PATCH /extract/api/v1/system_2/document_type/{document_type_id}/` | Pas de délai configurable dans l'Agent | `llm_timeout` dans le corps JSON | Entier entre 60 et 300 secondes, facultatif (`null` par défaut dans le schéma). |
| `POST /classify/api/v1/classification-agents/` et `PUT /classify/api/v1/classification-agents/{agent_id}` | Pas de délai configurable dans l'Agent | Même champ `llm_timeout` | Mêmes bornes ; vérifiez le délai voulu pour vos traitements génératifs. |

### Classify : transfert des Agents et modèles

| Route | Avant | Après | Description / paramètres |
| --- | --- | --- | --- |
| `POST /classify/api/v1/classification-agents/import-transfer/` | Pas de route | 200 avec le résultat du transfert | Multipart : `zipfile` obligatoire, `name` facultatif et `update` (booléen, défaut `false`). |
| `POST /classify/api/v1/classification-models/import-transfer/` | Pas de route | 200 avec le résultat du transfert | Mêmes champs de formulaire, appliqués au modèle. |

### Authenticator : liste des dépôts

| Route | Avant | Après | Description / paramètres |
| --- | --- | --- | --- |
| `GET /auth/api/v1/repositories/` | Pas de filtre en écriture ni de compteur par dépôt | `writable` (booléen de requête, défaut `false`) et `blueprint_count` (entier dans chaque résultat) | Utilisez `writable=true` pour limiter les résultats aux dépôts accessibles en écriture. |

### Review : texte OCR et transfert des files

Chemins relatifs à `/extract-review/api/v1`. Pour les trois routes OCR, `{page_number}` désigne le numéro de page. Le type d'OCR enregistré pour le document est utilisé s'il existe ; sinon, c'est celui de la file. Le texte et les positions peuvent changer, pas les chemins ni les formats.

| Route | Avant | Après | Description / paramètres |
| --- | --- | --- | --- |
| `GET /queues/{queue_id}/documents/{document_id}/page/{page_number}/text/` | OCR de la configuration de la file | OCR du document en priorité | Texte brut, sans filtre de zone. |
| `GET /queues/{queue_id}/documents/{document_id}/page/{page_number}/text/location/` | OCR de la configuration de la file | OCR du document en priorité | Texte brut dans la zone définie par `x_min`, `x_max`, `y_min`, `y_max` en requête (de 0 à 1). |
| `GET /queues/{queue_id}/documents/{document_id}/page/{page_number}/words/` | OCR de la configuration de la file | OCR du document en priorité | Liste JSON de mots (`word`, `coordinates`). |
| `GET /queues/{queue_id}/export/` | Pas de route | 200 avec la configuration JSON de la file | Utilisez cette réponse comme `data` lors de l'import. |
| `POST /queues/import/` | Pas de route | 200 avec le résultat du transfert | Corps JSON : `data` obligatoire, `name` facultatif et `update` (booléen, défaut `false`) pour mettre à jour une file existante. |
| `DELETE /queues/{queue_id}/documents/` | Pas de suppression en lot sur ce chemin | 204 si des documents sont mis en file pour suppression | `older_than_n_days` (entier de requête ≥ 0, défaut `0`). Précisez le seuil de conservation : `0` sélectionne tous les documents antérieurs à l'appel, sauf ceux verrouillés par un autre utilisateur. |

Dans les réponses documentaires, privilégiez `polyvore_reference` : `reference` est toujours fourni, mais déprécié.

### Workflows : transferts et liste des Workflows

Les routes sous `/workflows/api/v1/workflows/bundle` sont de nouveaux appels ; elles ne remplacent pas l'import et l'export existants.

| Route (sous `/workflows/api/v1`) | Avant | Après | Description / paramètres |
| --- | --- | --- | --- |
| `GET /workflows/bundle/resources/` | Pas de route | 200, liste de chemins | `name_filter` (chaîne facultative) et paramètres de pagination. |
| `GET /workflows/bundle/{workflow_id}/dependencies/` | Pas de route | 200, dépendances et avertissements | Consultez les objets, références et ressources avant l'export. |
| `POST /workflows/bundle/{workflow_id}/export/` | Pas de route | 202 avec `export_id` | Corps facultatif : `extra` (objets `type` et `id`) et `resources` (chemins), listes vides par défaut. `callback_url` (chaîne de requête) est facultatif. |
| `GET /workflows/bundle/exports/{export_id}/` | Pas de route | Archive ZIP si prête ; 404 sinon | Utilisez l'identifiant d'export (32 caractères hexadécimaux minuscules). |
| `POST /workflows/bundle/import/` | Pas de route | 201 avec le résultat de l'import ; 409 si le nom du Workflow principal existe et que `update` vaut `false` | Formulaire multipart : `bundle_file` (ZIP obligatoire), `name` (facultatif), `update` (booléen, défaut `false`). |
| `GET /workflows/` | Pas de filtre d'archivage | `archived` (booléen de requête facultatif) | `true` pour les Workflows archivés, `false` pour les autres ; sans filtre si omis. |

### Filtrage des webhooks par service

Ces appels acceptent désormais `webhook_events`, une chaîne de requête facultative. Séparez les événements par des virgules sans espaces ; une valeur absente ou vide ne filtre pas les notifications. Ce filtre ne change pas le corps de l'appel ni la reprise des envois en échec.

| Service | Routes concernées |
| --- | --- |
| Workflows (`/workflows/api/v1`) | `POST /jobs/` (par exemple `?webhook_events=success,failure`) |
| Review (`/extract-review/api/v1`) | `POST /queues/{queue_id}/documents/`, `POST /queues/{queue_id}/documents/upload/` |
| Classify (`/classify/api/v1`) | `POST /docs/predictions/`, `POST /docs/predictions/batch/`, `POST /texts/predictions/` |
| Classify : documents | `POST /documents/`, `POST /documents/classification-agent-prediction`, `PATCH /documents/{document_id}`, `PATCH /documents/{document_id}/pages` |
| Classify : e-mails | `POST /emails/attachments/predict/{model}/`, `POST /emails/predict/{model}/`, `PATCH /emails/{email_id}` |

## Principaux correctifs

### Review : fiabilité des corrections

- L'action **`CLEAR ALL`** efface également les valeurs des groupes en Review avancée.
- L'édition d'une cellule n'affiche plus simultanément l'ancienne valeur et la nouvelle.
- Le bouton d'envoi est correctement actualisé lorsque les conditions de validation sont réunies.

### Blueprints et transferts de données

- Corrections de la création, de la synchronisation et de l'affichage des dépôts de Blueprints.
- Amélioration de la conservation des annotations de position et des libellés lors de certains exports et imports.
- Corrections de copie ou d'export de documents lorsque leurs données sont stockées dans le stockage documentaire commun.

### Interface et opérations courantes

- L'application peut demander un rafraîchissement de la page après le déploiement d'une nouvelle version de l'interface.
- Les jetons API sont masqués visuellement dans les paramètres.
- Corrections de la suppression de boîtes de messagerie, du premier affichage d'un onglet Workflow dans Performance et de la gestion des colonnes ignorées dans Search.

Pour les changements déjà introduits au cours de la série 26.8.x, consultez les [notes de version 26.8.x](version-26.8.x-06-10-2026.md).
