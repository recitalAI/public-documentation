# Version 26.12.x

## En bref

Cette version facilite le transfert de vos configurations d'un environnement à l'autre et le travail des personnes chargées de vérifier les documents.

- **Réutilisez vos configurations** grâce aux nouveaux Blueprints et à l'import/export des Workflows avec leurs dépendances, sans avoir à reconstruire manuellement chaque élément associé.
- **Guidez la vérification des documents** dans Review avancée : attirez l'attention sur une valeur avec une couleur et indiquez précisément ce qui doit être vérifié.
- **Maîtrisez vos traitements génératifs** en choisissant vos fournisseurs de modèles, en ajustant les délais de traitement et en consultant les estimations de coût.

## Changements incompatibles : actions à prévoir

Les adaptations ci-dessous concernent les Agents et les intégrations qui utilisent les fonctionnalités ou les appels mentionnés.

- **Agents d'extraction générative :** si des champs ou groupes génératifs utilisent un modèle de repli implicite, sélectionnez explicitement un modèle dans l'Agent. Sans modèle renseigné, les appels concernés peuvent renvoyer une erreur `412` avant la mise en file. Les traitements exclusivement classiques ne sont pas concernés par cette exigence.
- **Intégrations API Extract :** si vous préparez l'export d'un Agent par API, renseignez désormais le paramètre obligatoire `with_files` (`true` ou `false`). Deux routes canoniques nécessitent également une barre oblique finale.
- **Intégrations API Review :** si vous modifiez le statut d'un document par API, n'envoyez plus la valeur `PROCESSING`, qui reste un état géré par le service.

D'autres adaptations ciblées concernent les anciennes URL Extract, les sondes de supervision, la copie et le transfert d'Agents, ainsi que les clients qui utilisaient déjà l'API des dépôts de Blueprints. Consultez le [tableau des migrations API](#migrations-api) si votre intégration utilise ces appels.

Si votre intégration valide strictement les réponses JSON, vérifiez aussi les [évolutions de paramètres et de réponses](#autres-evolutions-de-parametres-et-de-reponses), notamment les champs de fichiers désormais facultatifs ou susceptibles de contenir une valeur nulle.

## Nouvelles fonctionnalités et améliorations

### Blueprints et transferts de configuration

- **Blueprints :** créez des ensembles de ressources réutilisables pour partager vos configurations entre environnements. Ils peuvent regrouper des Agents et des modèles d'extraction ou de classification, ainsi que des Workflows. Vous pouvez les modifier ou les supprimer, consulter leur contenu et identifier les dépôts en lecture seule.
- **Transfert de Workflows :** exportez un Workflow avec ses dépendances, puis importez-le dans un autre environnement pour éviter de recréer manuellement chaque objet associé. Avant de mettre à jour un Workflow existant, vérifiez les objets ciblés, les références aux fournisseurs, les connexions et les secrets propres à l'environnement de destination.
- **Transfert d'Agents, de modèles et de files de Review :** réutilisez leurs configurations. Pour un Agent d'extraction, vous pouvez choisir de ne pas inclure les fichiers ; pour une file de Review, l'export contient la configuration, sans les documents ni leurs valeurs.

### Workflows et suivi des jobs

- **Notifications par job :** si votre intégration ne doit recevoir que les notifications de réussite ou d'échec, filtrez les événements lors de la création d'un job via l'API avec le paramètre facultatif `webhook_events`. Ce réglage s'applique au job, pas à la configuration du Workflow dans l'interface. Voir [comment filtrer les notifications à la création d'un job](#filtrer-les-notifications-a-la-creation-dun-job).
- **Suivi des jobs :** retrouvez les jobs enfants et leurs détails dans le panneau latéral de la liste des jobs, pour identifier plus facilement le traitement à examiner.
- **Étapes Python :** utilisez des tunnels SSH lorsque l'environnement est configuré pour les prendre en charge.

### Review et Review avancée

- **Consignes de vérification par champ :** depuis le Workflow, associez une couleur et un message personnalisé à une valeur. Par exemple, demandez à la personne qui vérifie le document de comparer la date extraite avec celle de la pièce d'identité. Voir [comment transmettre une couleur et un message](#transmettre-une-couleur-et-un-message-a-la-review-avancee).
- **Préparation de la vérification :** l'étape de Review avancée utilise par défaut le résultat d'Extract. Vous pouvez transmettre un identifiant externe pour retrouver le document.
- **Vérification et navigation :** relancez manuellement le contrôle des règles ou un webhook en échec. Un raccourci permet de copier la valeur du champ suivant, et le défilement de la visionneuse permet de passer d'une page à l'autre.
- **Entretien des files :** demandez la suppression en lot des documents anciens, selon votre politique de conservation.

### Extraction, classification et modèles de langage

- **Fournisseurs de modèles :** ajoutez un fournisseur de modèles de langage propre à votre organisation.
- **Délais de traitement :** choisissez un délai de 60 à 300 secondes pour les traitements génératifs des Agents d'extraction ou de classification.
- **Suivi des coûts :** consultez dans les traces les estimations de coût des appels aux modèles de langage, pour mieux suivre leur utilisation. Ces estimations ne correspondent pas nécessairement aux montants facturés.
- **Classification :** retrouvez l'état de correction dans la liste des documents des Agents de classification.
- **Groupes d'extraction :** l'API expose le champ `group_labels` et prend en charge sa mise à jour.

### Interface et utilisation quotidienne

- L'application peut vous inviter à actualiser la page après une mise à jour de l'interface.
- Les jetons API sont masqués visuellement dans les paramètres.

## Principaux correctifs

- **Review avancée :** l'action **`CLEAR ALL`** efface aussi les valeurs des groupes. L'édition d'une cellule n'affiche plus simultanément l'ancienne valeur et la nouvelle, et le bouton d'envoi s'actualise lorsque les conditions de validation sont réunies.
- **Export de jeux de données :** la récupération des images de pages a été corrigée, y compris pour les documents utilisant l'ancien stockage.
- **Copie et transfert de configurations :** plusieurs opérations conservent mieux les annotations de position et les libellés. La création et la synchronisation des dépôts de Blueprints ont également été corrigées.

Pour les changements déjà introduits dans la série précédente, consultez les [notes de version 26.8.x](version-26.8.x-06-10-2026.md).

## Détails avancés pour les intégrations API

Cette section s'adresse aux intégrateurs et aux personnes qui configurent des Workflows. Retrouvez les exemples d'utilisation et les contrats d'API détaillés selon votre besoin : [filtrage des notifications](#filtrer-les-notifications-a-la-creation-dun-job), [consignes de Review avancée](#transmettre-une-couleur-et-un-message-a-la-review-avancee), [migrations API](#migrations-api), [transferts et ressources](#transferts-ressources-et-entretien) ou [autres évolutions API](#autres-evolutions-de-parametres-et-de-reponses).

### Filtrer les notifications à la création d'un job

Sur `POST /workflows/api/v1/jobs/`, indiquez l'adresse de réception dans `webhook_url`, puis les événements souhaités dans `webhook_events`. Les événements émis sont `start`, `state:change` (lorsqu'une étape d'état change l'état du job), `success` et `failure`. Séparez les valeurs par des virgules sans espaces. Si le paramètre est absent ou vide, les notifications ne sont pas filtrées.

```http
POST /workflows/api/v1/jobs/?workflow_id=42&webhook_url=https%3A%2F%2Fintegration.example%2Fcallbacks&webhook_events=success,failure
Authorization: Bearer <jeton-api>
Content-Type: application/json

{}
```

Remplacez `42` et l'URL de réception par les valeurs de votre intégration. Cet appel ne notifie que la réussite ou l'échec du job. `webhook_extra_headers` et `webhook_extra_params` acceptent aussi des objets JSON sérialisés et encodés dans l'URL, respectivement pour ajouter des en-têtes HTTP et des paramètres de requête à la notification.

L'envoi séparé de fichiers reste possible via `POST /workflows/api/v1/jobs/{job_id}/files/` : envoyez les fichiers en `multipart/form-data`. Le nom de chaque partie désigne la collection de destination ; l'envoi ne configure pas le filtre de notifications.

### Transmettre une couleur et un message à la Review avancée

Dans l'étape de Review avancée du Workflow, saisissez dans **Résultats personnalisés** une expression qui renvoie les valeurs à transmettre. Le nom `data_point_config_name` doit correspondre au champ de la file. Par exemple :

```python
{
    'values': [
        {
            'data_point_config_name': 'date_naissance',
            'value': '1990-01-15',
            'require_review': True,
            'review_message': 'Vérifier la date sur la pièce d’identité.',
            'color': '#3366CC',
        }
    ]
}
```

La liste `values` fournie remplace les valeurs déduites automatiquement du résultat Extract : incluez les autres champs à transmettre. `color` est facultatif, au format `#RRGGBB` ; un état d'erreur ou d'avertissement peut prendre le dessus sur la couleur de la cellule. Pour afficher la consigne comme message de vérification, associez `review_message` à `require_review: true` dans les données JSON. Sans message personnalisé, Review utilise la consigne standard.

Si vous alimentez la file directement par API, les mêmes propriétés sont disponibles dans les éléments `DataPointValueCreate` de `POST /extract-review/api/v1/queues/{queue_id}/documents/` et `POST /extract-review/api/v1/queues/{queue_id}/documents/{document_id}/values/`. Dans les réponses de valeurs, `color` se trouve sur la valeur de champ et `review_message` dans l'objet de valeur avec coordonnées.

### Migrations API

Les chemins abrégés ci-dessous sont relatifs au préfixe indiqué dans la colonne **API**.

| API | Route canonique | Avant | Après et action |
| --- | --- | --- | --- |
| `/extract/api/v1` | `POST /model/{model_id}/version/{version}/compatible/` et `DELETE /system_2/extract_path/{ext_path_id}/` | Sans barre finale | Ajoutez la barre finale, sans compter sur une redirection. |
| `/extract/api/v1` | `GET /health/` | `status` textuel et `sql_connection` booléen | Lisez `status` et `services.sql` (booléens). Attendez `200` si SQL répond, `500` sinon. |
| `/extract/api/v1` | `GET /system_2/document_type/prepare/{document_type_id}/` | Pas de choix d'inclure les fichiers | Envoyez `with_files=true` pour inclure les fichiers, `false` pour n'exporter que la configuration. Le paramètre facultatif `callback_url` remplace la notification s'il est renseigné ; `send_notification` reste à `true` par défaut. |
| `/extract/api/v1` | `POST /files/{file_id}/copy_to_doctype/{document_type_id}/` et `POST /production/{file_id}/copy_to_doctype/{document_type_id}/` | `section_to_copy` facultatif, défaut nul | Omettez le paramètre pour utiliser `full` ou envoyez `full`, `top`, `right`, `bottom` ou `left` (en requête pour `/files`, dans le formulaire pour `/production`). N'envoyez pas de valeur nulle explicite. |
| `/extract/api/v1` | `POST /system_2/document_type/import` | `202` sans résultat structuré | Le multipart conserve `zipfile` et accepte `existing_name` (nom de l'Agent cible) et `references` (chaîne JSON), facultatifs. La réponse `202` contient `id`, `name`, `created`, `updated`, `warnings` ; elle ne garantit pas la fin de l'import. |
| `/extract-review/api/v1` | `PATCH /queues/{queue_id}/documents/{document_id}/` | `PROCESSING` accepté dans `status` | `PROCESSING` reste lisible mais n'est plus accepté en écriture (`422`). Omettez `status` pour modifier un autre champ sans changer l'état. |
| `/extract-review/api/v1` | `POST /queues/{queue_id}/documents/` | Sans `filename`, nom `document_{reference}` | Le nom du fichier source est utilisé par défaut. Transmettez `filename` en requête si vous dépendez d'un nom précis ; `reference` reste obligatoire en entrée. |
| `/auth/api/v1` | `POST /repositories/{repository_id}/blueprints/` | `path` fourni par le client ; titre facultatif | Retirez `path`. Le chemin est généré et `org.opencontainers.image.title` est obligatoire (1 à 64 caractères, sans séparateur de chemin ni caractère de contrôle). `latest_version_tag` et `objects` restent requis. |
| `/auth/api/v1` | `GET /repositories/blueprints` | `object_types` contient des chaînes | Lisez `type` dans chaque objet de la liste ; `count` peut être nul. `contents` est ajouté. |

Les anciens alias Extract sans `/extract/api/v1` ne sont plus maintenus. Par exemple, remplacez `GET /files/{file_id}/` par `GET /extract/api/v1/files/{file_id}/`.

Pour les champs ou groupes génératifs, vérifiez le modèle de l'Agent avant les envois (y compris en base64), copies, retraitements, préannotations et actualisations de valeurs. Pour les copies, c'est le modèle de l'Agent **de destination** qui s'applique. Sur `POST /extract/api/v1/system_2/data_point/{document_type_id}/refresh/`, `data_point_id` permet de cibler un seul champ.

### Transferts, ressources et entretien

Ces routes s'ajoutent aux routes existantes. Les imports d'Agents et de modèles Classify et de files Review renvoient un résultat contenant `id`, `name`, `created`, `updated` et `warnings`.

| Route | Utilisation |
| --- | --- |
| `GET /workflows/api/v1/workflows/bundle/resources/` | Liste les chemins des ressources pouvant être exportées. Paramètres : `name_filter` (facultatif), `limit` (100 par défaut), `offset` (0 par défaut). |
| `GET /workflows/api/v1/workflows/bundle/{workflow_id}/dependencies/` | Consultez les dépendances, références, ressources et avertissements avant l'export. |
| `POST /workflows/api/v1/workflows/bundle/{workflow_id}/export/` | Renvoie `202` et `export_id`. Corps JSON facultatif : `extra` (objets `type` et `id`) et `resources` (chemins). Le paramètre de requête `callback_url` est facultatif. |
| `GET /workflows/api/v1/workflows/bundle/exports/{export_id}/` | Télécharge l'archive ZIP lorsqu'elle est prête (`404` sinon). |
| `POST /workflows/api/v1/workflows/bundle/import/` | Multipart : `bundle_file` (ZIP), `name` (facultatif), `update` (`false` par défaut). Renvoie `201`, ou `409` si le Workflow principal existe et que `update=false`. Vérifiez les dépendances ciblées avant d'autoriser une mise à jour. |
| `GET /extract-review/api/v1/queues/{queue_id}/export/` et `POST /extract-review/api/v1/queues/import/` | L'export accepte `queue_version_id` en requête. À l'import, passez la configuration exportée dans `data`, avec `name` facultatif et `update` (`false` par défaut) pour mettre à jour une file existante. |
| `DELETE /extract-review/api/v1/queues/{queue_id}/documents/` | La mise en file de la suppression renvoie `204`. Indiquez `older_than_n_days` (entier de requête ≥ 0) : la valeur par défaut `0` sélectionne tous les documents de la file antérieurs à l'appel, sauf ceux verrouillés par un autre utilisateur. `queue_version_id` ne limite pas la suppression à une version. |
| `POST /classify/api/v1/classification-agents/import-transfer/` et `POST /classify/api/v1/classification-models/import-transfer/` | Multipart : `zipfile` (obligatoire), `name` (facultatif), `update` (`false` par défaut). Une mise à jour peut modifier un Agent du même nom. Un modèle existant n'est pas écrasé ; avec `update=true`, si ses poids diffèrent, un nouveau modèle est importé. Les fournisseurs d'OCR et de LLM référencés par un Agent sont résolus par nom. |
| `PUT /auth/api/v1/repositories/{repository_id}/blueprints/{blueprint_id}` et `DELETE /auth/api/v1/repositories/{repository_id}/blueprints/` | Le `PUT` publie une nouvelle version avec les `objects` du corps JSON. Le `DELETE` prend un objet JSON avec la liste `ids`. Les deux appels renvoient `202`. |

### Autres évolutions de paramètres et de réponses

| Surface | Évolution et action |
| --- | --- |
| Review : réponses documentaires et valeurs | Utilisez `polyvore_reference` ; `reference` reste fourni mais est déprécié. Les clients à liste fermée doivent prévoir `DELETED` dans `DocumentStatus`, sans l'envoyer dans le `PATCH` de document. Acceptez les champs de couleur et de message décrits plus haut. |
| Review : OCR | Les `GET` `page/{page_number}/text/`, `page/{page_number}/text/location/` et `page/{page_number}/words/` sous `/extract-review/api/v1/queues/{queue_id}/documents/{document_id}/` utilisent l'OCR enregistré pour le document en priorité, sinon celui de la file. Texte et positions peuvent différer ; chemins et formats restent identiques. |
| Extract : réponses File et texte | `FileOut.pages` est déprécié et peut être nul ; `image_path` dans ses pages ainsi que `FileRepresentationOut.image` et `NextFilePageOut.image` sont facultatifs et nullables. `GET /extract/api/v1/files/{file_id}/text/` et `GET /extract/api/v1/production/files/{file_id}/text/` renvoient un objet JSON `text` sans corps de requête. |
| Extract : annotations | Les schémas `DatasetOut`, `EntryOut` et `EntryExtendedOut` ajoutent `nb_to_validate` et `nb_to_complete`. `GET /extract/api/v1/dataset/entry/` accepte `pending_annotation_filter` (booléen) et `annotated_label_filter` (chaîne) ; `GET /extract/api/v1/dataset/entry/{entry_id}/` accepte aussi `annotated_label_filter` pour les compteurs. |
| Extract et Classify : Agents | `llm_timeout` est facultatif et nullable, de 60 à 300 secondes. Classify ajoute aussi `validated_at` (date et heure ou `null`) aux réponses `DocumentOut`. |
| Review et Classify : webhooks | Les deux POST de création de documents Review (`/documents/` et `/documents/upload/` sous `/extract-review/api/v1/queues/{queue_id}`) et les appels Classify de prédiction et de mise à jour qui utilisent la configuration de webhook acceptent `webhook_extra_headers`, `webhook_extra_params` et `webhook_events` en requête. Les événements dépendent de l'opération appelée. |
| Authenticator : fournisseurs LLM et métriques | Les routes sous `/auth/api/v1/llm-providers/` permettent de gérer les fournisseurs et de tester une connexion avec `POST /auth/api/v1/llm-providers/test/`. `GET /auth/api/v1/metrics/` ajoute `total_cost` (nombre ou `null`). |
| Listes de Workflows et de dépôts | `GET /workflows/api/v1/workflows/` accepte `archived` (booléen facultatif). `GET /auth/api/v1/repositories/` accepte `writable=true` et ajoute `blueprint_count` dans les résultats. |
