# Version 26.6.8 (xx/0x/2026)

## NOUVEAUTES

-Un nouveau service est dorénavant accessible sur la Suite reciTAL ! La "_**Review App**_" vous permet de gérer des Files dans lesquelles les documents à valider sont déposés et présentés aux utilisateurs via des écrans entièrement paramétrables.\
\
-De plus, les _**fichiers de Ressources**_ deviennent "globaux" et sont désormais accessibles à tous les Services, et plus uniquement aux Workflows. La Review App peut donc accéder aux fichiers Ressources et proposer par exemple des auto complétion dans les champs de correction.<br>

**-Simplification du fallback OCR de l'organisation.** Au lieu d'une configuration paramètre par paramètre, nous appliquons désormais toute la configuration de l'organisation si aucun moteur OCR n'est défini sur l'agent. C'est plus simple et plus cohérent.

\
-Les champs \`_**State**_\` et \`_**Status**_\` des Workflows ont également été clarifiés. Le champ \`_**State**_\` est un champ utilisateur, qui peut être exploité pour suivre l'avancement d'un workflow selon des étapes propres au cas d'usage. Le champ \`_**Status**_\` est un champ système, utilisé par la Suite uniquement, et qui prend désormais l'une des valeurs suivantes : \`start\`, \`running\`, \`pending\`, \`success\` et \`failure\`.\
\
![:rotating\_light:](https://a.slack-edge.com/production-standard-emoji-assets/16.0/google-medium/1f6a8@2x.png) Points d'attention ![:rotating\_light:](https://a.slack-edge.com/production-standard-emoji-assets/16.0/google-medium/1f6a8@2x.png)\
\- Les appels API aux Ressources [https://extract.workflow.recital.ai/workflows/api/v1/resources/files/](https://extract.workflow.recital.ai/workflows/api/v1/resources/files/) doivent être remplacés (cf. plus bas).\
\- Vérifiez la nouvelle sémantique des champs **State** et **Status** si vous les utilisez dans des workflows ou dans votre code.

## ENDPOINTS API

#### 1) Endpoint fichiers ressources:

_<mark style="color:$danger;">**Attention: si vous utilisiez ces routes pour accéder aux fichiers ressources dans vos blocs de code des Workflow, alors il faudra les modifier dans vos codes avec la nouvelle version :**</mark>_&#x20;

_**AVANT :**_&#x20;

[https://extract.workflow.recital.ai/workflows/api/v1/resources/files/](https://extract.workflow.recital.ai/workflows/api/v1/resources/files/)&#x20;

_**APRES :**_&#x20;

[https://extract.auth.recital.ai/auth/api/v1/resources/files/](https://extract.auth.recital.ai/auth/api/v1/resources/files/)&#x20;

#### 2) Tableau comparatif des routes et paramètres (Authentification)

<table><thead><tr><th width="275">Endpoints de https://extract.auth.recital.ai</th><th width="116">Avant (v26.4.19)</th><th width="119">Après (v26.6.8)</th><th>Description / Paramètres</th></tr></thead><tbody><tr><td>/auth/api/v1/repositories/{repository_id}/blueprints/sync <strong>(POST)</strong></td><td>Absente</td><td><strong>Disponible</strong></td><td><p>Synchronisation des blueprints d'un dépôt.</p><p>- Path : repository_id (integer)</p></td></tr><tr><td>/auth/api/v1/repositories/{repository_id}/blueprints/{blueprint_id}/install <strong>(POST)</strong></td><td>Absente</td><td><strong>Disponible</strong></td><td><p>Installation d'un blueprint.</p><p>- Path : repository_id (integer), blueprint_id (integer)</p><p>- Query : new_name (string, optionnel)</p></td></tr><tr><td>/auth/api/v1/resources/extensions/ <strong>(GET)</strong></td><td>Absente</td><td><strong>Disponible</strong></td><td>Récupération de la liste des extensions de fichiers supportées.</td></tr><tr><td>/auth/api/v1/resources/folders/ <strong>(GET, POST, DELETE)</strong></td><td>Absente</td><td><strong>Disponible</strong></td><td><p>Gestion des répertoires de stockage.</p><p>- Query (GET) : folder (string, optionnel)</p><p>- Body (POST) : FolderIn (path requis)</p><p>- Query (DELETE) : path (string, requis)</p></td></tr><tr><td>/auth/api/v1/resources/folders/rename/ <strong>(POST)</strong></td><td>Absente</td><td><strong>Disponible</strong></td><td><p>Renommage d'un dossier.</p><p>- Query : path (string, requis)</p><p>- Body : ResourceRename (name requis)</p></td></tr><tr><td>/auth/api/v1/resources/folders/bulk/ <strong>(DELETE)</strong></td><td>Absente</td><td><strong>Disponible</strong></td><td><p>Suppression groupée de dossiers.</p><p>- Body : ResourcesToDelete (paths requis)</p></td></tr><tr><td>/auth/api/v1/resources/files/ <strong>(GET, POST, PUT, DELETE)</strong></td><td>Absente</td><td><strong>Disponible</strong></td><td><p>Gestion des fichiers (liste, upload, modification, suppression).</p><p>- Query (GET/POST) : folder (string, optionnel)</p><p>- Body (POST) : file_in (binary, requis)</p><p>- Query (PUT/DELETE) : path (string, requis)</p></td></tr><tr><td>/auth/api/v1/resources/files/content/ <strong>(GET)</strong></td><td>Absente</td><td><strong>Disponible</strong></td><td><p>Récupération du contenu binaire d'un fichier.</p><p>- Query : path (string, requis)</p></td></tr><tr><td>/auth/api/v1/resources/files/blank/ <strong>(POST)</strong></td><td>Absente</td><td><strong>Disponible</strong></td><td><p>Génération d'un fichier vide.</p><p>- Query : path (string, requis)</p></td></tr><tr><td>/auth/api/v1/resources/files/rename/ <strong>(POST)</strong></td><td>Absente</td><td><strong>Disponible</strong></td><td><p>Renommage d'un fichier.</p><p>- Query : path (string, requis)</p><p>- Body : ResourceRename (name requis)</p></td></tr><tr><td>/auth/api/v1/resources/files/move/ <strong>(POST)</strong></td><td>Absente</td><td><strong>Disponible</strong></td><td><p>Déplacement d'un fichier vers un autre dossier.</p><p>- Query : path (string, requis)</p><p>- Body : ResourceMove (target_folder optionnel)</p></td></tr><tr><td>/auth/api/v1/resources/files/bulk/ <strong>(DELETE)</strong></td><td>Absente</td><td><strong>Disponible</strong></td><td><p>Suppression groupée de fichiers.</p><p>- Body : ResourcesToDelete (paths requis)</p></td></tr><tr><td>/auth/api/v1/spreadsheet/convert <strong>(POST)</strong></td><td>Absente</td><td><strong>Disponible</strong></td><td><p>Conversion d'un fichier tableur.</p><p>- Body : file (binary, requis)</p></td></tr><tr><td>/auth/api/v1/spreadsheet/save <strong>(POST)</strong></td><td>Absente</td><td><strong>Disponible</strong></td><td><p>Sauvegarde d'un tableur.</p><p>- Query : filename (string, requis)</p></td></tr><tr><td>/auth/api/v1/repositories/{repository_id}/blueprints/ <strong>(POST)</strong></td><td>Schéma BlueprintCreate classique</td><td>Modèle de blueprint restructuré (Spécification OCI)</td><td><p><strong>Avant :</strong> Métadonnées standards (meta_display_name, meta_description, meta_maintainer_name) et BlueprintObjectReference nécessitant service, type, export_path (9 types autorisés).</p><p></p><p><strong>Après :</strong> Réorganisé autour du standard OCI (org.opencontainers.image.* et vnd.blueprint.size). Objets restreints à 2 valeurs (emails_dataset et documents_dataset) ne requérant que product et type.</p></td></tr><tr><td>/auth/api/v1/repositories/ <strong>(POST)</strong></td><td>Port obligatoire dans RepositoryCreate</td><td>Port optionnel dans RepositoryCreate</td><td>Le champ port (type integer) était obligatoire pour la création. Il accepte désormais la valeur nulle (integer ou null) et n'est plus requis dans le schéma.</td></tr><tr><td>Schémas d'utilisateurs (UserCreate, UserOut, GroupUserOut, etc.)</td><td>Langue par défaut : "en" (Anglais)</td><td>Langue par défaut : "fr" (Français)</td><td>La langue par défaut de l'utilisateur (UserLanguage) est passée de l'anglais au français dans les différents schémas de l'API.</td></tr><tr><td><strong>GET</strong> /auth/api/v1/repositories/</td><td>Paramètres : name_filter<br>, sort_desc<br>, limit<br>, offset<br>.</td><td>Paramètres : name_filter<br>, sort_desc<br>, inactive<br>(Nouveau), limit<br>, offset<br>.</td><td>Ajout de paramètre : Ajout du paramètre query optionnel inactive<br>(type : boolean<br>) pour filtrer les dépôts inactifs.</td></tr></tbody></table>

#### 3) Tableau comparatif des routes et paramètres (Workflows)

_<mark style="color:$danger;">**Attention: si vous utilisiez ces attributs d’étape dans vos blocs de code des Workflow, alors il faudra les modifier dans vos codes avec la nouvelle version :**</mark>_&#x20;

* <mark style="color:$danger;">Les attributs d’étape</mark> <mark style="color:$danger;"></mark>_<mark style="color:$danger;">**status**</mark>_ _<mark style="color:$danger;">**initial**</mark>_ <mark style="color:$danger;"></mark><mark style="color:$danger;">et</mark> <mark style="color:$danger;"></mark>_<mark style="color:$danger;">**final**</mark>_ <mark style="color:$danger;"></mark><mark style="color:$danger;">ont été supprimés.</mark>
* <mark style="color:$danger;">Le cycle de vie du travail (job) est désormais explicite dans l’attribut status, avec un nombre très limité de statuts possibles:</mark>
  * <mark style="color:$danger;">La gestion des erreurs, signalée par l'échec (</mark>_<mark style="color:$danger;">**failure**</mark>_<mark style="color:$danger;">), est désormais explicite.</mark>
  * <mark style="color:$danger;">Des événements dédiés de démarrage (</mark>_<mark style="color:$danger;">**start**</mark>_<mark style="color:$danger;">), de succès (</mark>_<mark style="color:$danger;">**success**</mark>_<mark style="color:$danger;">) et d’échec (</mark>_<mark style="color:$danger;">**failure**</mark>_<mark style="color:$danger;">) sont désormais déclenchés à un niveau inférieur.</mark>
* <mark style="color:$danger;">Les pseudo-états (auparavant utilisés pour signaler des erreurs) ont été supprimés.</mark>
* <mark style="color:$danger;">Les étapes d’état (</mark>_<mark style="color:$danger;">**state steps**</mark>_<mark style="color:$danger;">) se contentent désormais de modifier le champ state et peuvent provoquer l’échec du travail si elles sont configurées à cet effet.</mark>
  * <mark style="color:$danger;">Le champ state est une valeur technique utilisée pour le signalement et le requêtage des travaux.</mark>

| Endpoints de https://extract.workflows.recital.ai                                                                                            | Avant (v26.4.19)                                                                            | Après (v26.6.8)                                                                                                                 | Description / Paramètres                                                                                                                                                                                                                                                                                                                                                          |
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

| Endpoints de https://extract.api.recital.ai                      | Avant (v26.4.19)                                                    | Après (v26.6.8)                                            | Description / Paramètres                                                                                                                                                                                       |
| ---------------------------------------------------------------- | ------------------------------------------------------------------- | ---------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **GET** `/extract/api/v1/dataset/entry/{entry_id}/reference`     | ❌ Inexistant                                                        | Existant                                                   | **Nouvel endpoint** : Permet de récupérer la référence d'une entrée de dataset par son identifiant (`entry_id`).                                                                                               |
| **GET** `/extract/api/v1/files/{file_id}/reference`              | ❌ Inexistant                                                        | Existant                                                   | **Nouvel endpoint** : Permet de récupérer la référence d'un fichier par son identifiant (`file_id`). Supporte le query paramètre optionnel `is_external_id`.                                                   |
| **GET** `/extract/api/v1/production/{file_id}/reference`         | ❌ Inexistant                                                        | Existant                                                   | **Nouvel endpoint** : Permet de récupérer la référence d'un fichier de production par son identifiant (`file_id`). Supporte le query paramètre optionnel `is_external_id`.                                     |
| **DELETE** `/extract/api/v1/system_2/extract_path/{ext_path_id}` | Existant                                                            | ❌ Supprimé                                                 | **Endpoint retiré** : L'opération de suppression d'un chemin d'extraction pour System 2 n'est plus disponible sur cette route.                                                                                 |
| **POST** `/extract/api/v1/files/`                                | Existant (sans paramètre query)                                     | Existant (avec paramètre query)                            | **Modification de paramètres** : Ajout du paramètre de requête optionnel `reference` (type : `string`) permettant de spécifier une référence à la création du fichier.                                         |
| **POST** `/extract/api/v1/production/files/`                     | <p>Existant (avec query params workflow_id<br>et workflow_uuid)</p> | <p>Existant (avec query param reference<br>uniquement)</p> | <p>Modification de paramètres : Les paramètres query workflow_id<br>(integer) et workflow_uuid<br>(string) ont été retirés au profit d'un paramètre query unique et optionnel nommé reference<br>(string).</p> |

#### 5) Tableau comparatif des routes et paramètres (Classify)

| Endpoints de https://extract.classify.recital.ai | Avant (v26.4.19)                                                                                  | Après (v26.6.8)                                                                                                      | Description / Paramètres                                                                                                                                                                 |
| ------------------------------------------------ | ------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Enum / Schéma** : ValueType                    | Uniquement 6 types de données supportés : unspecified, date, integer, float, regex, string.       | **Extension du support**                                                                                             | <p>Élargissement des types de valeurs exploitables pour les modèles de classification. </p><p>- Property : datetime - Property : time</p>                                                |
| **Enum / Schéma** : NormalizedValueType          | Uniquement 4 types de normalisation disponibles : string, datetime, integer, float.               | **Extension du support**                                                                                             | <p>Ajout de formats de dates et de temps fins pour les valeurs normalisées. </p><p>- Property : date </p><p>- Property : time</p>                                                        |
| **Schéma de requête** : DocumentUpdate           | La propriété breaks (servant à la découpe des pages d'un bundle) n'acceptait pas de valeur nulle. | **Typage nullable**                                                                                                  | <p>La propriété breaks est assouplie pour accepter une valeur nulle, ce qui permet de réinitialiser ou d'annuler les découpes appliquées. </p><p>- Property : breaks (array ou null)</p> |
| **POST** /classify/api/v1/documents/             | <p>Contient les query params workflow_id<br>et workflow_uuid</p>                                  | ![❌](data:image/gif;base64,R0lGODlhAQABAIAAAP///wAAACH5BAEAAAAALAAAAAABAAEAAAICRAEAOw==) **Inexistants (Supprimés)** | <p>Suppression de paramètres : Les query params workflow_id<br>(integer) et workflow_uuid<br>(string) ont été retirés de l'endpoint d'importation de documents.</p>                      |

### 6) Nouveau : routes Extract Review&#x20;

### https://staging.review.recital.ai/extract-review/docs

##

## 7) Journal des modifications (Changelog)

### Version 26.6.1 (2026-06-18)

#### Nouvelles fonctionnalités

* Possibilité de réessayer les extractions génératives
* Ajout de Scaleway comme fournisseur de LLM
* Activation du pré-remplissage des brouillons (_draft seeding_) à partir de n'importe quelle mise en page (_layout_) et correction des problèmes de liaison (_binding_) des tableaux de données
* Consolidation des fichiers de test par méthode dans des suites CRUD par domaine de ressources
* Ajout de suggestions basées sur les ressources aux champs de saisie de la file d'attente de révision, avec corrections de bugs
* Activation de la validation des fichiers de workflow depuis l'étape de code (_code step_)
* Conversion des importations de composants de routage en importations dynamiques chargées à la demande (_lazy-loading_) dans l'ensemble des modules applicatifs
* Ajout de la prise en charge des PDF préfixés par des métadonnées IMSP dans le moteur de traitement automatique (_magic_)
* Ajout de la génération de mises en page (_layouts_) de file d'attente pilotée par l'agent, avec corrections pour le format ODS
* Nouvelle méthode d'agrégation de lignes optimales (_optimal lines_)
* Ajout de la carte « Ressources » dans la section Studio de l'accueil
* Chargement différé (_lazy-loading_) des dépendances d'interface utilisateur lourdes pour réduire la taille initiale du bundle de production
* Suppression des fichiers de localisation (traductions) inutilisés et ajout de ceux manquants
* Prise en charge de la suppression de pages dans la liseuse de documents intégrable (_PluggableDocViewer_)
* Ajout d'un onglet d'importation de mises en page (_layouts_) ODS dans l'éditeur de file d'attente (_QueueEditor_)
* Correction du dépassement de temps (_timeout_) lors du build de _x-data-spreadsheet_ dans les builds Docker de production
* Meilleure organisation des étiquettes (_labels_) dans l'interface utilisateur d'annotation
* Correction du build de _x-data-spreadsheet_
* Mise en place de workers dédiés aux tâches de LLM dans le service d'extraction (_extract_)
* Ajout d'une liseuse et d'un éditeur de feuilles de calcul prenant en charge les ressources ODS, XLSX, XLS et CSV
* Suppression des ressources obsolètes (_legacy_) et consolidation dans l'onglet Studio
* Correction d'un problème de build mineur
* Ajout d'une section « Ressources » autonome adossée au système de stockage de l'authentificateur
* Utilisation des vignettes (_thumbnails_) et des images à partir du stockage partagé et amélioration de la sécurité globale
* Approuvé par : Nicolas Cadet
* Ajout de modes de dessin et de sélection d'annotations pour la révision des documents dans les files d'attente
* Ajout des directives d'espace de travail Cursor et des compétences de l'agent (_agent skills_)
* Ajout de tests pour le code cryptographique dans handyman
* Ajout de la gestion des fichiers de ressources dans Authenticator
* Consolidation des fichiers de test d'extraction dans des suites CRUD par domaine de ressources
* Amélioration de la projection des boîtes englobantes (_bounding boxes_) de l'OCR pour l'extraction générative
* Affichage des superpositions structurelles (_overlays_) dans l'écran d'annotation (_AnnotationView_)
* Utilisation du stockage partagé pour les fichiers de tâches de workflow (_jobfiles_)
* Ajout d'une ombre de colonne (_column shadow_) aux entrées de jeux de données
* Prise en charge de la recherche textuelle dans la liseuse de documents intégrable (_PluggableDocViewer_)
* Correction d'une erreur de sélection de texte lors de la validation
* Introduction des types « Date » et « Heure » pour les extracteurs
* Suppression du champ d'orientation paysage (_landscape_) d'EntryPage et de FileRepresentation
* Migration de l'écran d'annotation (_AnnotationView_) vers la liseuse de documents intégrable (_PluggableDocViewer_)
* Encapsulation du payload HTTP de l'étape Webhook dans une enveloppe webhook dédiée et mise à jour de sa documentation
* Développement de l'interface utilisateur (front-end) pour tester l'installation des modèles de jeux de données (_dataset blueprints_)
* Ajout de la page de méthode de création de mise en page (_LayoutCreationMethod_) et affinage du flux de mise en page des brouillons de file d'attente
* Mise à jour des politiques de rétention des tâches de workflow (_Workflows jobs_)
* Amélioration de la gestion du cycle de vie des tâches (_Jobs_)
* Ajout d'une logique de secours (_fallback_) d'OCR dans l'agent d'extraction et dans l'importation de jeux de données
* Mise en place d'un système multi-revendications (_multi claims_) rétrocompatible
* Architecture de la liseuse de documents intégrable (_PluggableDocViewer_) rendue pleinement modulaire (_pluggable_)
* Prise en charge du dessin et de l'affichage d'annotations dans la liseuse de documents intégrable (_PluggableDocViewer_)
* Activation de la création de files d'attente et de l'édition de mises en page directement depuis le flux de l'agent d'extraction
* Possibilité de trier les colonnes du tableau de distribution des étiquettes (_labels_)
* Obligation d'effectuer des sauvegardes atomiques de points de données (_datapoints_) avant la soumission définitive des documents dans les files de révision
* Implémentation du backend d'installation des blueprints (modèles), prenant actuellement en charge les jeux de données de documents
* Scission des entités lorsqu'elles chevauchent plusieurs cellules de tableau
* Amélioration du processus de mise à jour des agents d'extraction
* Affichage du texte détecté dans la liseuse de documents intégrable (_PluggableDocViewer_)
* \[SER-765] \[Sélection de la langue] Utilisation du français comme langue par défaut pour les nouveaux utilisateurs
* \[SER-408] Ajout d'un raccourci pour passer à la page suivante du fichier en cours dans l'écran de validation et de révision de l'agent d'extraction
* \[SER-1056] \[Workflow] Possibilité d'importer plusieurs fichiers simultanément lors des tests de workflow
* Correction d'une colonne entière d'un seul coup
* Diverses améliorations apportées aux workflows
* Amélioration du sélecteur de fournisseur d'OCR avec une recherche fiable et obligation de sélection d'un moteur
* Unification du processus de création des agents d'extraction
* Nouveau flux de création de jeux de données et meilleure gestion de la configuration OCR
* Synchronisation et récupération (_pull_) des blueprints (modèles) dans la base de données
* API backend pour créer des agents d'extraction à partir d'agents d'extraction existants
* Rend l'action de lecture de codes-barres moins verbeuse dans les workflows
* Évitement maximal de la manipulation des données brutes, des données préliminaires et des journaux d'exécution dans les workflows
* Gestion des pièces jointes en double dans l'action d'importation d'e-mails (_Ingest email_)
* Autorisation du passage des variables d'environnement de proxy dans les étapes de code (_code steps_) de workflow
* Amélioration du processus de mise à jour des agents d'extraction et de l'expérience utilisateur (UX)
* Création d'un agent d'extraction à partir d'agents d'extraction existants
* Ajout de la prise en charge des expressions régulières de groupe (_group regex_) dans l'interface utilisateur (front)
* Nouveau processus de création d'agent et interface modernisée pour le tableau des agents

#### Corrections de bugs

* Correction du pilote du système de fichiers qui retournait une liste vide lors du listage du contenu d'un dossier
* Correction de l'affichage des blocs, des tableaux et de l'ordre de lecture dans l'écran d'annotation
* Restauration d'un ordre cohérent pour les étiquettes primaires (_primary labels_) manquantes
* Correction de la sélection d'entités
* Correction du tableau des fournisseurs d'OCR
* Correction de la détection et de la gestion des types de fichiers dans le pipeline d'extraction de documents
* Correction de la réutilisation de l'OCR dans polyvore
* Diverses améliorations sur les dépôts de blueprints (modèles)
* Approuvé par : Nicolas Cadet
* Modification de la fonction d'obtention de l'utilisateur par ID pour s'appuyer sur les données du jeton (_token_)
* Suppression du clignotement des valeurs numériques dans les onglets du Studio
* Correction de la mise au point (_focus_) sur une valeur située dans une autre section du document
* Augmentation de la durée de validité du jeton de session (_token_)
* Prise en charge de l'importation de fichiers ODS
* Correction : gestion des fichiers de tâches avec une clé nulle lors de l'exécution d'étapes Python
* Exposition de l'endpoint POST avec document de référence dans le Swagger de révision d'extraction
* La rotation des coordonnées des mots n'est plus requise pour les cas d'usage de Document AI
* Activation de la pré-révision des fichiers de workflow
* Fusion des branches de migration de base de données pour l'extraction
* Fermeture des notifications au clic de la souris pour cibler directement les éléments de l'interface situés en arrière-plan
* Refactorisation du code pour vérifier si l'OCR de l'agent fait partie des moteurs d'OCR pris en charge afin de résoudre les problèmes d'importation d'agents et de jeux de données
* Correction des données manquantes lors de l'instanciation de la tâche (_Job_)
* Correction de la gestion des paramètres Webhook dans le module d'extraction
* Correction de l'analyse des pièces jointes d'e-mails
* Correction d'un problème d'expiration (_timeout_) du LLM
* Sélection automatique de la collection par défaut lors de l'importation d'une tâche de test
* Correction du statut de traitement des documents en cours de workflow (_in\_workflow_)
* \[SER-387] Tous environnements : correction du bug où le clic sur un jeu de données ne redirigeait pas vers ses 20 premiers documents
* Correction de la désélection automatique du modèle d'extraction
* Autorisation pour un utilisateur anonyme de copier un document vers un jeu de données
* Ajout du montage de volume libpolyvore manquant pour extract-janitor
* Correction d'URL cassées et enrichissement des informations liées aux notifications
* Garantie de la récupération des données de la tâche lors de la mise à jour de ses informations
* Correction des cas spécifiques de configuration de l'OCR
* Ignorer les vignettes (_thumbnails_) inexistantes lors de la suppression d'une page d'entrée au lieu de planter
* Prise en charge de la copie d'extracteurs configurés comme génératifs dans le nouveau processus de création
* Corrections appliquées dans l'écran de performances
* Généralisation de la pagination de handyman sur l'ensemble du dépôt
* Réduction des requêtes MongoDB lors de la manipulation des fichiers pendant l'entraînement (_training_)
* Ajout de données d'information lors du rejet (_discard_) de documents pour prévenir les durées d'évaluation négatives
* \[SER-460] Sur staging : résolution de l'erreur de conflit lors de la création d'un jeu de données avec rotation de pages lorsque Azure est utilisé comme OCR de l'organisation
* Initialisation correcte de l'agent d'extraction sélectionné dans l'éditeur de workflow
* Correction de la sélection d'agents d'extraction dans une étape de workflow
* Ajout de remplissage (_padding_) aux noms courts de jeux de données
* Correction des paramètres OCR de la file d'attente
* Correction de la migration b0fa7f3de6ad dans authenticator
* Correction d'une instanciation manquante dans les étapes de LLM
* Amélioration de l'importation des agents d'extraction obsolètes
* Correction du traitement des documents
* L'importation d'agents d'extraction contenant des champs obsolètes fonctionne désormais correctement
* Correction de la localisation linguistique d'OIDC
* Possibilité de supprimer les entrées de jeu de données en échec
* Interdiction de soumettre un document présentant des erreurs d'inter-section
* Correction d'un mauvais nom de colonne dans l'activité de révision d'extraction (_Extract Review_)
* Optimisation de certaines requêtes et opérations de base de données
* Mise à jour du statut de l'entrée en cas d'échec du traitement
* Correction de la marge dans l'interface de gestion des étiquettes (_labels_) de jeux de données
* Meilleure normalisation des dates
* Fusion des branches de migration de base de données pour l'extraction
* Optimisation des performances de `/production/files/` — jusqu'à 47 fois plus rapide, débit triplé
* Diverses optimisations sur les tâches d'import/export de jeux de données
* Correction de la liaison d'adresses IP (_IP-binding_) et des jetons d'API générés par les workflows
* Correction de la reconnexion automatique à la base de données
* \[SER-1051] \[Jeton d'API] Résolution du bug empêchant de renommer un jeton d'API
* Interdiction de réutiliser des étiquettes en cache pour les entraînements futurs
* Amélioration de la logique de stockage des étiquettes de groupe en mémoire pour prévenir les bugs d'exécution
* Correction de la copie générative avec des étiquettes
* Correction de la création de points de données génératifs
* Correction de l'icône d'itération des workflows
* Correction de l'entraînement du modèle de classification de documents
* \[SER-530] \[Traduction] Résolution d'un affichage de texte en anglais au sein de l'environnement français
* Correction de l'entraînement des modèles de classification lorsque la liaison d'adresses IP (_IP-binding_) est activée
* Correction d'une faille de traversée de chemin (_path traversal_) dans les workflows (test d'intrusion Elysium)

#### Sécurité

* Mise à niveau de sécurité : mise à jour de la bibliothèque idna de la version 3.10 à 3.15

#### Autres modifications

* Mise à jour des éléments et totalItems après l'appel de loadItems
* Suivi de l'état de chargement pour éviter des requêtes redondantes
* Centralisation de la récupération des éléments dans une seule méthode appelée lors du montage (_mount_)
* Suppression de scrollKey et ré-instanciation de la méthode loadItems
* Correction du bug rovo
* Correction de la sélection d'entités
* Fichier docker-compose.prod.yml édité en ligne avec Bitbucket
* Correction de tests unitaires et d'intégration
* Augmentation de la durée de validité du jeton (_token_) de session
* Annulation du commit 14cf1fa198027a3acb14aa2a043c875c723fb38c
* Les fichiers importés via l'interface utilisateur avant l'exécution d'une étape peuvent avoir la valeur key=None dans l'environnement
* Fichier .env édité en ligne avec Bitbucket

***

### Version 26.6.2 (2026-06-18)

#### Nouvelles fonctionnalités

* Possibilité de réessayer les extractions génératives

#### Corrections de bugs

* Correction du pilote du système de fichiers qui retournait une liste vide lors du listage du contenu d'un dossier



### Version 26.6.3 (2026-06-22)

#### Corrections de bugs

* Toujours stocker les images chiffrées avec l'extension « .enc »

#### Autres modifications

* Journaux des modifications (_changelogs_)

***

### Version 26.6.4 (2026-06-23)

#### Nouvelles fonctionnalités

* Restauration des endpoints `/resources` de workflows à l'aide de la couche de stockage partagée handyman
* Permettre de spécifier l'obligation de révision des valeurs extraites et la rendre visible pour les utilisateurs
* Ajout de la prise en charge des types de saisie booléen, case à cocher (_checkbox_) et bouton à bascule (_toggle_) dans les champs de mise en page (_layout_)

#### Corrections de bugs

* Rendre publique la référence des fichiers dans le module d'extraction (_Extract_)
* Amélioration de la robustesse des opérations de manipulation d'images
* Correction de l'endpoint `POST /dataset/entry`
* Gestion plus fluide et transparente des éléments envoyés à la corbeille
* Prise en charge du cas d'usage limite de la normalisation des dates contenant des espaces blancs adjacents
* Correction de la synchronisation de la recherche de la combobox de ressources, de la réactivité et des suggestions en double dans les files d'attente de révision

***

### Version 26.6.5 (2026-06-24)

#### Nouvelles fonctionnalités

* Ajout de la référence du document dans les données envoyées par le webhook de classification de documents

***

### Version 26.6.6 (2026-06-25)

#### Nouvelles fonctionnalités

* Ajout de la prise en charge des options dynamiques pour les valeurs de points de données (_data point values_)

#### Corrections de bugs

* Correction de l'absence des lignes d'en-tête uniquement dans les groupes de lignes non répétés de la section de révision (_ReviewSection_)

***

### Version 26.6.7 (2026-06-25)

#### Corrections de bugs

* Correction de la résolution du nom de section pour les valeurs de points de données (_data point values_) sur les documents traités par polyvore

***

### Version 26.6.8 (2026-06-25)

#### Nouvelles fonctionnalités

* Résolution du problème du bouton de soumission qui restait désactivé après la mise à jour des champs calculés lors de la révision en file d'attente
