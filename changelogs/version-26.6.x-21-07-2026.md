# Version 26.6.x (21/07/2026)

## Quoi de neuf dans cette version ?

### Nouvelle version majeure de la Review

Une nouvelle version majeure de la Review est disponible ! Bien plus qu'un écran de vidéo codage, cette nouvelle mouture vous permet de configurer intégralement les écrans de correction, de gérer des Files avec des délais et des SLA de réalisation, de gérer des utilisateurs correcteurs et experts avec des droits d'accès spécifiques.

### Fichiers de Ressources globaux

Les fichiers de Ressources deviennent globaux et sont désormais accessibles à tous les Services, et plus uniquement aux Workflows. La Review peut donc accéder aux fichiers Ressources et proposer par exemple des auto complétion dans les champs de correction.

### Champs State et Status

Les champs `State` et `Status` des Workflows sont clarifiés. Le champ `State` est un champ utilisateur, qui peut être exploité pour suivre l'avancement d'un workflow selon des étapes propres au cas d'usage. Le champ `Status` est un champ système, utilisé par la Suite uniquement, et qui prend désormais l'une des valeurs suivantes : `start`, `running`, `success` et `failure`.

## ![:rotating\_light:](https://a.slack-edge.com/production-standard-emoji-assets/16.0/google-medium/1f6a8@2x.png) Points d'attention

### API Ressources

Les Ressources sont désormais remontées au niveau de l'Organisation et plus du Workflow, ce qui implique un changement des API qui les exploitent.

Les anciennes API sont désormais marquées `deprecated` dans le Swagger ainsi que dans les en-têtes HTTP.

Nous vous invitons donc à mettre à jour les parties de votre code qui réalisent les appels aux API Ressources ([https://extract.workflow.recital.ai/workflows/api/v1/resources/files/](https://extract.workflow.recital.ai/workflows/api/v1/resources/files/)).&#x20;

Les nouveaux appels API auront la forme [https://extract.auth.recital.ai/auth/api/v1/resources/](https://extract.auth.recital.ai/auth/api/v1/resources/).

Pour information, les appels vers l'ancienne URL seront fonctionnels jusqu'au 31/12/2026 mais n'existeront plus après.

### Champs State et Status

Comme expliqué plus haut, les champs `State` et `Status` changent de sémantique. Assurez-vous notamment que vous n'utilisez pas le champ `Status` dans vos workflows car ce dernier étant un champ système utilisé par la plateforme, il pourra être modifié durant l'exécution du workflow.

Si vous utilisez `Status`, remplacez-le par `State`.

## End points API

### Fichiers ressources

<mark style="color:$danger;">**Comme indiqué plus haut il faut remplacer les route API Ressources**</mark>

**AVANT :** [https://extract.workflow.recital.ai/workflows/api/v1/resources/files/](https://extract.workflow.recital.ai/workflows/api/v1/resources/files/)&#x20;

**APRÈS :** [https://extract.auth.recital.ai/auth/api/v1/resources/files/](https://extract.auth.recital.ai/auth/api/v1/resources/files/)&#x20;

### Authenticator

<table><thead><tr><th width="275">Endpoints de https://extract.auth.recital.ai</th><th width="116">Avant (v26.4.19)</th><th width="119">Après (v26.6.X)</th><th>Description / Paramètres</th></tr></thead><tbody><tr><td>/auth/api/v1/resources/extensions/ <strong>(GET)</strong></td><td>Absente</td><td><strong>Disponible</strong></td><td>Récupération de la liste des extensions de fichiers supportées.</td></tr><tr><td>/auth/api/v1/resources/folders/ <strong>(GET, POST, DELETE)</strong></td><td>Absente</td><td><strong>Disponible</strong></td><td><p>Gestion des répertoires de stockage.</p><p>- Query (GET) : folder (string, optionnel)</p><p>- Body (POST) : FolderIn (path requis)</p><p>- Query (DELETE) : path (string, requis)</p></td></tr><tr><td>/auth/api/v1/resources/folders/rename/ <strong>(POST)</strong></td><td>Absente</td><td><strong>Disponible</strong></td><td><p>Renommage d'un dossier.</p><p>- Query : path (string, requis)</p><p>- Body : ResourceRename (name requis)</p></td></tr><tr><td>/auth/api/v1/resources/folders/bulk/ <strong>(DELETE)</strong></td><td>Absente</td><td><strong>Disponible</strong></td><td><p>Suppression groupée de dossiers.</p><p>- Body : ResourcesToDelete (paths requis)</p></td></tr><tr><td>/auth/api/v1/resources/files/ <strong>(GET, POST, PUT, DELETE)</strong></td><td>Absente</td><td><strong>Disponible</strong></td><td><p>Gestion des fichiers (liste, upload, modification, suppression).</p><p>- Query (GET/POST) : folder (string, optionnel)</p><p>- Body (POST) : file_in (binary, requis)</p><p>- Query (PUT/DELETE) : path (string, requis)</p></td></tr><tr><td>/auth/api/v1/resources/files/content/ <strong>(GET)</strong></td><td>Absente</td><td><strong>Disponible</strong></td><td><p>Récupération du contenu binaire d'un fichier.</p><p>- Query : path (string, requis)</p></td></tr><tr><td>/auth/api/v1/resources/files/blank/ <strong>(POST)</strong></td><td>Absente</td><td><strong>Disponible</strong></td><td><p>Génération d'un fichier vide.</p><p>- Query : path (string, requis)</p></td></tr><tr><td>/auth/api/v1/resources/files/rename/ <strong>(POST)</strong></td><td>Absente</td><td><strong>Disponible</strong></td><td><p>Renommage d'un fichier.</p><p>- Query : path (string, requis)</p><p>- Body : ResourceRename (name requis)</p></td></tr><tr><td>/auth/api/v1/resources/files/move/ <strong>(POST)</strong></td><td>Absente</td><td><strong>Disponible</strong></td><td><p>Déplacement d'un fichier vers un autre dossier.</p><p>- Query : path (string, requis)</p><p>- Body : ResourceMove (target_folder optionnel)</p></td></tr><tr><td>/auth/api/v1/resources/files/bulk/ <strong>(DELETE)</strong></td><td>Absente</td><td><strong>Disponible</strong></td><td><p>Suppression groupée de fichiers.</p><p>- Body : ResourcesToDelete (paths requis)</p></td></tr><tr><td>Schémas d'utilisateurs (UserCreate, UserOut, GroupUserOut, etc.)</td><td>Langue par défaut : "en" (Anglais)</td><td>Langue par défaut : "fr" (Français)</td><td>La langue par défaut de l'utilisateur (UserLanguage) est passée de l'anglais au français dans les différents schémas de l'API.</td></tr></tbody></table>

#### 3) Tableau comparatif des routes et paramètres (Workflows)

_<mark style="color:$danger;">**Attention: si vous utilisiez ces attributs d’étape dans vos blocs de code des Workflow, alors il faudra les modifier dans vos codes avec la nouvelle version :**</mark>_&#x20;

* <mark style="color:$danger;">Les attributs d’étape</mark> <mark style="color:$danger;"></mark>_<mark style="color:$danger;">**status**</mark>_ _<mark style="color:$danger;">**initial**</mark>_ <mark style="color:$danger;"></mark><mark style="color:$danger;">et</mark> <mark style="color:$danger;"></mark>_<mark style="color:$danger;">**final**</mark>_ <mark style="color:$danger;"></mark><mark style="color:$danger;">ont été supprimés.</mark>
* <mark style="color:$danger;">Exemple:</mark>
* <mark style="color:$danger;">AVANT:</mark>

```
    "step": {
        "id": 69620,
        "workflow_id": 10353,
        "created_at": "2026-07-09T14:52:25.777838",
        "updated_at": null,
        "name": "Start",
        "action": "started",
        "action_type": "state",
        "config": null,
        "initial": true,
        "final": false,
        "deprecated": false,
        "enabled": true
    },
```

* <mark style="color:$danger;">APRES:</mark>
* ```
   "step": {
          "id": 69620,
          "workflow_id": 10353,
          "created_at": "2026-07-09T14:52:25.777838",
          "updated_at": null,
          "name": "Start",
          "action": "started",
          "action_type": "state",
          "config": null,
          "deprecated": false,
          "enabled": true
      },
  ```
* <mark style="color:$danger;">Le cycle de vie du job est désormais explicite dans l’attribut status, avec un nombre très limité de statuts possibles:</mark>
  * <mark style="color:$danger;">La gestion des erreurs, signalée par l'échec (</mark>_<mark style="color:$danger;">**failure**</mark>_<mark style="color:$danger;">), est désormais explicite.</mark>
  * <mark style="color:$danger;">Des événements dédiés de démarrage (</mark>_<mark style="color:$danger;">**start**</mark>_<mark style="color:$danger;">), de succès (</mark>_<mark style="color:$danger;">**success**</mark>_<mark style="color:$danger;">) et d’échec (</mark>_<mark style="color:$danger;">**failure**</mark>_<mark style="color:$danger;">) sont désormais déclenchés à un niveau inférieur.</mark>
* <mark style="color:$danger;">Les pseudo-états (auparavant utilisés pour signaler des erreurs) ont été supprimés.</mark>
* <mark style="color:$danger;">Les étapes d’état (</mark>_<mark style="color:$danger;">**state steps**</mark>_<mark style="color:$danger;">) se contentent désormais de modifier le champ state et peuvent provoquer l’échec du travail si elles sont configurées à cet effet.</mark>
  * <mark style="color:$danger;">Le champ state est une valeur technique utilisée pour le signalement et le requêtage des travaux.</mark>

| Endpoints de https://extract.workflows.recital.ai                                                                                            | Avant (v26.4.19)                                                                            | Après (v26.6.X)                                                                                                                 | Description / Paramètres                                                                                                                                                                                                                                                                                                                                                          |
| -------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **GET** /workflows/api/v1/jobs/                                                                                                              | Paramètre status absent.                                                                    | **Disponible**                                                                                                                  | <p>Ajout d'un filtre sur l'état général du travail. </p><p>- Query : status (string, optionnel)</p>                                                                                                                                                                                                                                                                               |
| **POST** /workflows/api/v1/jobs/                                                                                                             | Paramètre references absent.                                                                | **Disponible**                                                                                                                  | <p>Ajout d'une option de liaison ou de référencement externe pour le travail. </p><p>- Query : references (string, optionnel)</p>                                                                                                                                                                                                                                                 |
| **GET** /workflows/api/v1/workflows/by-uuid                                                                                                  | Existant                                                                                    | **Existant (avec paramètre query additionnel)**                                                                                 | <p>Ajout de paramètre : Ajout du paramètre optionnel archived<br>(type : boolean<br>, par défaut false<br>) pour filtrer ou inclure les architectures de workflows archivées.</p>                                                                                                                                                                                                 |
| Schéma de réponse : JobResponse & JobResponseWithData                                                                                        | Schéma standard sans suivi de durée ni horodatages fins.                                    | **Modélisation enrichie**                                                                                                       | <p>Ajout d'indicateurs de performance et de cycle de vie. </p><p>- Property : started_at (string, format date-time) </p><p>- Property : success_at (string, format date-time) </p><p>- Property : error_at (string, format date-time) </p><p>- Property : duration (number, durée en secondes) </p><p>- Property : status (string, enum : created, running, success, failure)</p> |
| Schéma de réponse : JobEntry                                                                                                                 | Propriété duration absente.                                                                 | **Disponible**                                                                                                                  | Ajout du suivi de performance pour chaque entrée historique de tâche. - Property : duration (number ou null)                                                                                                                                                                                                                                                                      |
| Schéma de réponse : JobStatus                                                                                                                | Propriété existante .                                                                       | **Disponible**                                                                                                                  | Nouveau schéma de type énumération contenant les valeurs : \["created", "running", "success", "failure"].                                                                                                                                                                                                                                                                         |
| Schémas d'étapes : StepRequest, StepPartialRequest & StepResponse                                                                            | Présence des drapeaux de début et de fin de cycle d'étapes.                                 | **Flags retirés**                                                                                                               | <p>Suppression complète des propriétés définissant le caractère d'entrée ou de sortie des étapes. </p><p>- Property : initial (boolean) </p><p>- Property : final (boolean)</p>                                                                                                                                                                                                   |
| Déclencheurs : **Webhooks**                                                                                                                  | Seul l'événement générique state:change était défini via le schéma WorkflowsWebhookPayload. | **Spécification typée et nouveaux événements**                                                                                  | <p>Nouveaux déclencheurs Webhooks : Intégration de quatre webhooks natifs pour notifier le système externe des événements majeurs :<br>• Workflow Job / start<br>• Workflow Job / success<br>• Workflow Job / failure<br>• Workflow Job / custom<br>(pour les étapes de script webhook personnalisées).</p>                                                                       |
| <p><strong>Routes d'API de ressources /workflows/api/v1/resources/...</strong><br><strong>(Gestion de fichiers &#x26; dossiers)</strong></p> | <p>Actives et classées sous les tags Resource Folders<br>et Resource Files<br></p>          | <p><strong>Marquées comme obsolètes (deprecated: true</strong><br><strong>) sous le tag Resources (Deprecated)</strong><br></p> | **Dépréciation globale : L'ensemble des endpoints de stockage et de manipulation de fichiers de ressources internes est déprécié.**                                                                                                                                                                                                                                               |

#### Refonte des payloads de Webhooks

* Les anciens schémas complexes préfixés (ex. `WorkflowsWebhookPayload`, `WorkflowsWebhookPayloadJob`, etc.) ont été simplifiés et remplacés par des schémas standardisés :
  * `JobStartWebhook`, `JobSuccessWebhook`, `JobFailureWebhook`, `JobCustomWebhook`, `JobStateChangeWebhook`.
  * `WebhookPayloadEvent`, `WebhookPayloadJob`, `WebhookPayloadStep`, `WebhookPayloadWorkflow`.

#### 4) Tableau comparatif des routes et paramètres (Extract)

| Endpoints de https://extract.api.recital.ai                      | Avant (v26.4.19)                                                    | Après (v26.6.X)                                            | Description / Paramètres                                                                                                                                                                                       |
| ---------------------------------------------------------------- | ------------------------------------------------------------------- | ---------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **DELETE** `/extract/api/v1/system_2/extract_path/{ext_path_id}` | Existant                                                            | ❌ Supprimé                                                 | **Endpoint retiré** : L'opération de suppression d'un chemin d'extraction pour System 2 n'est plus disponible sur cette route.                                                                                 |
| **POST** `/extract/api/v1/files/`                                | Existant (sans paramètre query)                                     | Existant (avec paramètre query)                            | **Modification de paramètres** : Ajout du paramètre de requête optionnel `reference` (type : `string`) permettant de spécifier une référence à la création du fichier.                                         |
| **POST** `/extract/api/v1/production/files/`                     | <p>Existant (avec query params workflow_id<br>et workflow_uuid)</p> | <p>Existant (avec query param reference<br>uniquement)</p> | <p>Modification de paramètres : Les paramètres query workflow_id<br>(integer) et workflow_uuid<br>(string) ont été retirés au profit d'un paramètre query unique et optionnel nommé reference<br>(string).</p> |

#### 5) Tableau comparatif des routes et paramètres (Classify)

| Endpoints de https://extract.classify.recital.ai | Avant (v26.4.19)                                                                                  | Après (v26.6.X)                                                                                                      | Description / Paramètres                                                                                                                                                                 |
| ------------------------------------------------ | ------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Enum / Schéma** : ValueType                    | Uniquement 6 types de données supportés : unspecified, date, integer, float, regex, string.       | **Extension du support**                                                                                             | <p>Élargissement des types de valeurs exploitables pour les modèles de classification. </p><p>- Property : datetime - Property : time</p>                                                |
| **Enum / Schéma** : NormalizedValueType          | Uniquement 4 types de normalisation disponibles : string, datetime, integer, float.               | **Extension du support**                                                                                             | <p>Ajout de formats de dates et de temps fins pour les valeurs normalisées. </p><p>- Property : date </p><p>- Property : time</p>                                                        |
| **Schéma de requête** : DocumentUpdate           | La propriété breaks (servant à la découpe des pages d'un bundle) n'acceptait pas de valeur nulle. | **Typage nullable**                                                                                                  | <p>La propriété breaks est assouplie pour accepter une valeur nulle, ce qui permet de réinitialiser ou d'annuler les découpes appliquées. </p><p>- Property : breaks (array ou null)</p> |
| **POST** /classify/api/v1/documents/             | <p>Contient les query params workflow_id<br>et workflow_uuid</p>                                  | ![❌](data:image/gif;base64,R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw==) **Inexistants (Supprimés)** | <p>Suppression de paramètres : Les query params workflow_id<br>(integer) et workflow_uuid<br>(string) ont été retirés de l'endpoint d'importation de documents.</p>                      |

## Changelog

### Version 26.6.X (2026-07-21)

### NOUVELLES FEATURES&#x20;

#### Infra

* Ajout de Scaleway comme fournisseur de LLM

#### Annotation

* Meilleure organisation des étiquettes dans l'interface utilisateur d'annotation

#### Extraction &#x20;

* Amélioration du processus de mise à jour des agents d'extraction
* Possibilité de relancer les extractions génératives
* Mise en place de workers dédiés aux tâches de LLM dans le service d'extraction
* Ajout d'un raccourci pour passer à la page suivante du fichier en cours dans l'écran de validation et de révision de l'agent d'extraction
* Unification du processus de création des agents d'extraction
* Possibilité de créer un agent d'extraction à partir d'agents d'extraction existants
* Ajout de la prise en charge des expressions régulières de groupe (group regex) dans l'interface utilisateur

#### Review

* Autocomplete dans la review
* Prise en charge de la suppression de pages dans la liseuse de documents
* Configuration de la review :
  * Ajout d'un onglet d'importation de mises en page ODS dans l'éditeur de file d'attente
  * Ajout d'une liseuse et d'un éditeur de feuilles de calcul prenant en charge les ressources ODS, XLSX, XLS et CSV

#### Tableaux

* Nouvelle méthode d'agrégation de lignes
* Scission des entités lorsqu'elles chevauchent plusieurs cellules de tableau
* Possibilité de corriger une colonne entière d'un seul coup ? Possibilité de trier les colonnes du tableau de distribution des labels

#### Ressources

* Ajout d'une section « Ressources » autonome adossée au système de stockage d'authenticator
* Ajout de la carte « Ressources » dans la section Studio de l'accueil

#### Workflows

* Ajout de la prise en charge des PDF préfixés par des métadonnées IMSP dans le moteur de traitement automatique
* Introduction des types « Date » et « Heure » pour les extracteurs
* Possibilité d'importer plusieurs fichiers simultanément lors des tests de workflow
* Gestion des pièces jointes en double dans l'action d'importation d'e-mails
* Autorisation du passage des variables d'environnement de proxy dans les étapes de code de workflow ? Mise à jour des politiques de rétention des tâches de workflow ? Amélioration de la gestion du cycle de vie des jobs

#### API

* Suppression du champ d'orientation paysage (landscape) d'EntryPage et de FileRepresentation
* Encapsulation du payload HTTP de l'étape Webhook dans une enveloppe webhook dédiée et mise à jour de sa documentation
* API backend pour créer des agents d'extraction à partir d'agents d'extraction existants
