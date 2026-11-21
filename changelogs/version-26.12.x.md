# Version 26.12.x

## Quoi de neuf dans cette version ?

La version 26.12 facilite la réutilisation et le transfert de configurations entre environnements. Les **Blueprints** prennent en charge davantage de types de ressources, tandis que l'**export et l'import des Workflows** peuvent inclure leurs dépendances.

Cette version améliore également le suivi des exécutions, la **Review avancée**, la configuration des modèles de langage et la portabilité des Agents et des files de révision. Plusieurs correctifs renforcent la fiabilité des éditions et des échanges de documents.

## Changements incompatibles et actions nécessaires

### API Extract : retrait d'anciennes routes

Si une URL commence directement par `/config`, `/dataset`, `/files`, `/health`, `/image`, `/logs`, `/model`, `/production` ou `/system_2`, ajoutez **`/extract/api/v1`** devant le chemin complet. Conservez la méthode HTTP, les identifiants, les paramètres et le corps de l'appel. Par exemple, `GET /files/{file_id}/` devient `GET /extract/api/v1/files/{file_id}/`.

Deux chemins canoniques prennent aussi une barre oblique finale : `POST /extract/api/v1/model/{model_id}/version/{version}/compatible/` et `DELETE /extract/api/v1/system_2/extract_path/{ext_path_id}/`. Utilisez ces URL directement, sans dépendre d'une redirection.

**Sondes de supervision :** appelez `GET /extract/api/v1/health/`, sans paramètre. Même si vous utilisiez déjà cette URL, adaptez la lecture du JSON : `status` n'est plus une chaîne (`starting`, `running`, `error`) mais un booléen ; `services.sql` remplace `sql_connection`. La sonde répond **200** si la connexion SQL fonctionne, **500** sinon.

### Extraction générative : sélection explicite du modèle

Sélectionnez un modèle dans l'Agent **avant** de lancer une extraction comportant un champ ou un groupe génératif. Sans modèle explicite, Extract ne choisit plus OpenAI par défaut : les appels ci-dessous peuvent répondre **412** avec un message dans `detail`, avant la mise en file. Une extraction exclusivement classique ne nécessite pas de modèle LLM.

Tous ces appels utilisent `POST`, sous `/extract/api/v1` :

| Opération | Chemins concernés |
| --- | --- |
| Envoi d'un fichier ou de son contenu en base64 | `/files/`, `/files/base64/`, `/production/files/`, `/production/files/base64/` |
| Copie vers un Agent (configurer le modèle de destination) | `/files/{file_id}/copy_to_doctype/{document_type_id}/`, `/production/{file_id}/copy_to_doctype/{document_type_id}/` |
| Retraitement | `/files/reprocess/` (corps JSON : `file_ids`), `/files/{file_id}/rerun/system1/`, `/production/files/{file_id}/rerun/system1/` |
| Préannotation ou actualisation ciblée | `/dataset/annotation/{dataset_id}/prefill_agent/{document_type_id}/`, `/system_2/data_point/{document_type_id}/refresh/` (paramètre de requête facultatif `data_point_id` pour ne contrôler qu'un champ) |

### Blueprints : publication et lecture par API

- `POST /auth/api/v1/repositories/{repository_id}/blueprints/` : dans le corps JSON, **supprimez `path`** (le service génère le chemin) et fournissez un nom avec `org.opencontainers.image.title`, auparavant facultatif (1 à 64 caractères, sans séparateur de chemin ni caractère de contrôle). Gardez `latest_version_tag` et `objects`. Un ancien corps contenant `path` est rejeté (422) ; un envoi accepté répond toujours 202. Publiez dans un dépôt accessible en écriture : la publication dans un dépôt en lecture seule répond 403.
- `GET /auth/api/v1/repositories/blueprints` : dans chaque élément de la liste, `object_types` n'est plus une liste de chaînes mais une liste d'objets. Lisez `type` dans chaque objet ; `count` peut être nul.

### Import d'Agent et mise à jour de document Review

- `POST /extract/api/v1/system_2/document_type/import` : continuez d'envoyer le ZIP dans le champ multipart `zipfile`. La réponse 202, auparavant sans résultat exploitable, contient désormais un objet JSON (`id`, `name`, `created`, `updated`, `warnings`). Utilisez `id` pour identifier l'Agent ; l'acceptation de l'import ne signifie pas que son traitement est terminé.
- `POST /extract-review/api/v1/queues/{queue_id}/documents/` : le paramètre de requête `reference` reste obligatoire. Si vous omettez `filename`, le document prend désormais le nom du fichier source au lieu de `document_{reference}`. Transmettez `filename` explicitement si votre intégration dépend de ce nom.
- `PATCH /extract-review/api/v1/queues/{queue_id}/documents/{document_id}/` : le corps JSON n'accepte plus `"status": "PROCESSING"` (422). Cet état reste lisible mais il est géré par le service. Pour modifier un autre champ, omettez `status` plutôt que de renvoyer tel quel l'état lu. Une mise à jour réussie répond toujours 204.

## Nouvelles fonctionnalités et améliorations

### Blueprints et ressources réutilisables

- Les Blueprints peuvent représenter des Agents et modèles d'extraction ou de classification, ainsi que des Workflows.
- Les Blueprints de Workflows peuvent inclure davantage de dépendances, afin de faciliter leur réutilisation dans d'autres environnements.
- La liste des Blueprints présente plus clairement le type et le contenu des ressources. Les dépôts en lecture seule sont identifiables et la sélection des Agents prend mieux en compte les filtres et la pagination.

### Workflows et suivi des exécutions

- **Export et import avec dépendances :** un Workflow peut être exporté avec ses objets associés, puis importé dans un autre environnement. L'import prend en charge certains cas de mise à jour d'objets existants et affiche les références nécessaires aux fournisseurs. Les connexions et secrets spécifiques à la destination restent à vérifier après l'import.
- **Étapes Python :** prise en charge des tunnels SSH lorsque l'environnement est configuré pour les utiliser.
- **Suivi des jobs :** amélioration de la liste des exécutions, de la consultation des sous-workflows et de l'affichage des détails d'un job dans un panneau latéral.
- **Notifications :** pour filtrer les notifications, ajoutez le paramètre de **requête** facultatif `webhook_events`, par exemple `POST /workflows/api/v1/jobs/?webhook_events=success,failure`. Séparez les valeurs par des virgules, sans espaces. Si vous l'omettez, les événements ne sont pas filtrés. Ce paramètre ne se nomme pas `events` dans l'URL.

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

### Évolutions de contrats API

**Fichiers Extract.** Le champ `FileOut.pages` est déprécié, pas supprimé. Même si `GET /extract/api/v1/files/{file_id}/` renvoie encore une liste, acceptez un champ absent ou `null`. `FilePage.image_path`, `FileRepresentationOut.image` (résultats de `GET /extract/api/v1/files/{file_id}/info/`) et `NextFilePageOut.image` ne sont plus des chaînes obligatoires. Si votre lecteur impose l'ancien schéma, adaptez-le avant la mise à niveau. Pour télécharger l'image d'une page, utilisez `GET /extract/api/v1/image/file/{file_id}/{page_nb}/` plutôt qu'un chemin de stockage.

**Texte OCR en Review avancée.** Sous `/extract-review/api/v1/queues/{queue_id}/documents/{document_id}/page/{page_number}/`, les trois routes `GET` restent disponibles : `text/` renvoie du texte brut, `text/location/` du texte brut limité aux coordonnées de requête `x_min`, `x_max`, `y_min`, `y_max` (de 0 à 1), et `words/` une liste JSON de mots (`word`, `coordinates`). Elles utilisent le type d'OCR enregistré pour le document plutôt que la seule configuration actuelle de la file ; si aucun type n'est enregistré, la configuration de la file s'applique. Le texte et les positions peuvent donc différer sans changement d'URL. Dans la réponse documentaire, utilisez `polyvore_reference` à la place de `reference`, toujours fourni mais déprécié.

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
