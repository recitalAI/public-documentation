# Version 26.4.x (20/05/2026)

### 26.4.1 (2026-04-15)

#### Nouvelles fonctionnalités

* Ajout d'index supplémentaires sur les jobs de workflows
* Notification lorsque le workflow est dans la corbeille
* Ajout d'un format JSON repliable sur les vues historique et données, et d'un bouton de copie sur l'historique
* Inclusion de totaux à divers endroits dans le Studio
* Suppression de la détection inutile de cases à cocher
* Affichage du nom et de l'ID du jeu de données (dataset) sur les lignes des modèles d'extraction
* Ajout d'une analyse de la distribution des labels du jeu de données pour l'évaluation de la qualité
* Gestion des JSON mal formés dans les sorties génératives des LLM
* Activation de la récupération des entités (get entities) sur les fichiers de production et restriction de l'accès
* Ajout de Paramiko au worker chroot
* Ajout de la fonctionnalité max\_depth à l'étape de décompression (Unpack) des workflows
* Vérification des actions Python des workflows avec Bandit et des heuristiques
* Notification de l'utilisateur lorsque le jeu de données est dans la corbeille
* Notification lorsque l'agent d'extraction est dans la corbeille et refactorisation des injections de dépendances
* Exposition des URL privées dans les workflows
* Création de groupes d'extraction regex via l'API
* Amélioration de l'API des jeux de données et des entrées (Entries)
* Affichage du total et de tous les états au survol sur la page d'activité des workflows
* Standardisation de la création de la configuration OCR pour une meilleure maintenabilité
* Correction de la pré-annotation sur les entrées sélectionnées
* Gestion des fichiers longs dans l'extraction générative
* Ajout de Mistral à la liste des fournisseurs
* Ajout du support du fournisseur Google OCR
* Amélioration de l'affichage des états personnalisés des jobs sur la page d'activité
* Prise en charge du format de date ISO 8601 dans les payloads des webhooks
* Activation de la validation des champs de date avec le stockage sous forme de timestamp Unix
* Préparation des agents de classification pour la migration requise de la configuration OCR
* Ajout d'options avancées de configuration OCR aux agents d'extraction et aux jeux de données
* Ajout d'une configuration OCR par agent pour les agents de classification
* Affichage des KPI des workflows sous forme de graphiques temporels
* Gestion de la configuration OCR au niveau de l'organisation
* Intégration du modèle de cases à cocher d'Azure
* Activation du tri sur la précision (accuracy) et d'autres métriques de performances
* Ajout de la gestion centralisée de la configuration du fournisseur OCR
* Correction d'un cache de liaison IP (IP binding) obsolète entraînant le blocage de requêtes après la désactivation de son application
* Correction de la validation de la liaison IP pour les requêtes inter-services
* Liaison de la configuration OCR au cycle de vie de l'artefact et centralisation des paramètres
* Ajout du support de l'extraction des codes-barres
* Ajout de la négociation de jetons basée sur l'identité (Identity-based) pour Azure
* Ajout de l'auto-négociation pour les jetons Azure
* Suppression des modèles OCR inutilisés

#### Corrections de bugs

* Ajout de l'option FIFO à la révision d'extraction (Extract review) legacy
* Suppression des données excédentaires dans les jobs enfants
* Correction du dépassement de la zone de texte des notifications par un nom de dataset/agent très long en production
* Correction de la page des jobs de workflows chargeant des données inutilisées
* Correction de l'alignement du bouton bascule (toggle) avec l'email de l'utilisateur dans la configuration de l'agent d'extraction / révision / réviseurs
* Passage de l'ID (email) en mode insensible à la casse
* Correction de la case à cocher permettant de sélectionner tous les éléments dans l'en-tête du tableau
* Correction d'une traduction manquante dans la configuration du groupe d'extraction
* Prévention de la superposition des lignes d'attente (skeleton rows) sur les lignes existantes lors d'un changement de page
* Gestion des caractères non ASCII dans le payload du webhook
* Application de l'état de validation sur le bouton d'envoi de révision de document
* Correction de la vue d'entraînement du modèle d'extraction lors d'une ouverture depuis un jeu de données
* Activation de l'entraînement du modèle d'extraction à partir de la vue des jeux de données de documents
* Correction du debounce de la recherche d'agent dans l'interface utilisateur (UI)
* Correction de la réactivité de l'UI pour la sélection de la version du modèle d'extraction
* Garantie que « documents » est l'onglet actif dans les jeux de données de documents
* Prise en compte exclusive des entrées ingérées pour l'entraînement du modèle et l'import-export de jeux de données
* Correction de la liste déroulante (dropdown) de l'agent d'extraction dans les workflows
* Correction du bug de la liste déroulante lors de la sélection de l'agent d'extraction de workflow
* Bascule sur Poppler en repli (fallback) en cas d'échec du rendu de la page par MuPDF
* Correction de la fonction de sauvegarde sur la page du groupe d'extraction
* Correction de la liaison IP (IP binding) des jetons d'accès pour les connexions OpenID
* Correction du support des fichiers CSV dans Magic / handyman
* Correction de l'importation d'anciens agents d'extraction (regex et label\_regex manquants)
* Affichage du statut d'erreur d'un fichier (en validation et en production)
* Correction du dysfonctionnement de l'annotation sur des zones similaires et de la boîte à outils de localisation lorsque la détection de cases à cocher est active sous Google OCR
* Gestion plus cohérente des erreurs dans l'indexation Extract et les Workflows
* Ajout d'une option PUT pour la boîte de réception (mailbox)
* Réduction du bundle CSS de production de Suite UI d'environ 14MB à 1-2MB
* Correction du bouton de suppression d'annotation dans les jeux de données
* Correction de la gestion des pages de codes QR dans la configuration de l'agent
* Correction du lien vers le document sur la matrice de confusion pointant vers le mauvais chemin et de mauvais paramètres
* Modification de la valeur par défaut de la température et gestion des valeurs par défaut évaluées comme fausses (falsy) de la configuration des étapes
* Ajout d'ellipses aux noms lorsqu'ils sont trop longs et retrait des icônes de stylo lorsqu'elles ne sont pas strictement nécessaires
* Correction d'un problème de traitement de fichier où l'extraction générative échoue
* Suppression du bouton de retraitement (reprocess) dans la révision d'extraction
* Correction du routage de la vue d'accueil et du design des cartes
* Correction du passage du statut des notifications de "non lu" à "lu" (unseen to seen)
* Correction de l'affichage des scores des labels
* Affichage du hashtag avec l'ID sur les pages des modèles d'extraction et de classification
* Correction de la pagination des jobs enfants
* Correction des filtres de statut des jobs
* Correction de la rotation des pages
* Ajout de rapidfuzz au worker chroot
* Correction de la gestion des pages de l'extracteur génératif et de la non-concordance des labels
* Correction du décompte des documents révisés

### 26.4.2 (2026-04-21)

#### Nouvelles fonctionnalités

* Copie des labels de jeu de données avec la configuration correcte des labels
* Ajout de la possibilité de renommer un modèle de classification
* Ajout de l'étape workflow pour les codes-barres
* Calcul du statut d'achèvement (complete status) d'un jeu de données au moment de la requête
* Refonte de la navigation des sous-workflows sur les pages Jobs
* Ajout d'un défilement infini (infinite scroll) pour consulter toutes les notifications
* Sauvegarde garantie des valeurs des champs calculés par l'assistant après le chargement du document
* Création d'agents d'extraction à partir de modèles d'extraction

#### Corrections de bugs

* Définition du modèle de document comme choix par défaut dans l'UI d'entraînement du classificateur
* Activation de la suppression de jeux de données pendant et après une importation erronée
* Désactivation de l'ouverture de document dans la révision de classification lorsque le document est révisé par un utilisateur externe
* Correction de la suppression de la langue dans la configuration de l'agent d'extraction
* Correction de la création de points de données (data points) basés sur des groupes
* Tri des agents d'extraction par ID
* Correction de l'historique des jobs pour les sous-workflows ayant une output\_key personnalisée

### 26.4.3 (2026-04-24)

#### Nouvelles fonctionnalités

* Ajout du support pour les regex de groupe dans le front-end

### 26.4.4 (2026-04-27)

#### Nouvelles fonctionnalités

* Gestion des pièces jointes en double dans l'action d'ingestion par email
* Autorisation des variables d'environnement proxy dans les étapes de code des workflows
* Nouveau flux de création d'agent et interface (UI) modernisée pour le tableau des agents
* Simplification de l'action sur les codes-barres dans les Workflows
* Réduction au maximum de la manipulation des champs data, preliminary\_data et logs dans les Workflows

#### Corrections de bugs

* Diverses optimisations de la tâche d'import/export de jeux de données
* Publication de la version 26.4.4

### 26.4.5 (2026-04-28)

#### Nouvelles fonctionnalités

* Nouveau flux de création de jeu de données et meilleure gestion de la configuration OCR

#### Corrections de bugs

* Activation de la suppression des entrées de jeu de données en erreur
* Mise à jour du statut de l'entrée en cas d'échec du traitement

### 26.4.6 (2026-04-28)

#### Corrections de bugs

* Correction de l'importation d'agents d'extraction incluant d'anciens champs (legacy)
* Correction de la localisation OIDC

### 26.4.7 (2026-04-29)

#### Nouvelles fonctionnalités

* Unification du workflow de création des agents d'extraction

#### Corrections de bugs

* Amélioration de l'importation des anciens agents d'extraction (legacy)

### 26.4.8 (2026-04-29)

#### Corrections de bugs

* Correction de l'instanciation manquante dans les étapes LLM

### 26.4.9 (2026-05-04)

#### Corrections de bugs

* Correction de la sélection des agents d'extraction dans une étape de workflow
* Ajout de padding (remplissage) aux noms courts de jeux de données

### 26.4.11 (2026-05-07)

#### Nouvelles fonctionnalités

* Amélioration du workflow de mise à jour des agents d'extraction
* Correction d'une colonne entière en une seule fois

#### Corrections de bugs

* Support de la copie d'extracteurs en tant que modèles génératifs dans le nouveau workflow de création
* Corrections dans la vue des performances
* Ajout d'informations supplémentaires lors du rejet (discarding) de documents afin d'éviter des temps négatifs

### 26.4.12 (2026-05-11)

#### Nouvelles fonctionnalités

* Scission des entités lorsqu'elles chevauchent plusieurs cellules de tableau

#### Corrections de bugs

* Correction de l'état et de la gestion de la configuration OCR

### 26.4.13 (2026-05-11)

#### Corrections de bugs

* Vérification de la récupération des données du job lors de la mise à jour de ses informations

### 26.4.14 (2026-05-12)

#### Nouvelles fonctionnalités

* Ajout du tri des colonnes dans le tableau de répartition des labels (étiquettes)



## ENDPOINTS API

Voici les endpoints qui ont été modifiées :&#x20;

#### 1) Endpoint datapoint:

PUT /extract/api/v1/system\_2/extraction\_groups/{group\_id}/ PUT /extract/api/v1/system\_2/data\_point/{data\_point\_id}/&#x20;

**Nouveaux paramètres:**  &#x20;

`"extraction_regexes": [     "string"   ],`  &#x20;

`"labels_descriptions": [     "string"   ],`  &#x20;

`"label_regex": [     {       "pattern": "",       "substitute": ""     }   ],`



#### 2) Endpoint Workflow Jobs

GET /workflows/api/v1/jobs/{job\_id} GET /workflows/api/v1/jobs/{job\_id}/history/

* **Nouveaux paramètres:**  &#x20;
* "parent\_id": 0,  &#x20;
* "related\_job\_id": 0



* **Paramètres supprimés:**&#x20;
* "children": null,  &#x20;
* "data": "string",  &#x20;
* "preliminary\_data": {},  &#x20;
* "logs": "string"



#### 3) Nouveaux endpoints pour récupérer les données json: &#x20;

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



#### 5) Endpoints modifiées :&#x20;

* POST /auth/api/v1/users/password/reset/{id}/&#x20;
* GET /auth/api/v1/users/{id}/&#x20;
* PUT /auth/api/v1/users/{id}/&#x20;
* DELETE /auth/api/v1/users/{id}/&#x20;
* POST /auth/api/v1/users/{id}/check-password/&#x20;
* PUT /auth/api/v1/users/{id}/toggle-datascientist
* **Paramètre avant:** user\_id and org\_id
* **Paramètre après :** id



#### 6) Nouveaux endpoints :&#x20;

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



#### 7) Endpoints supprimés :&#x20;

* POST /extract/api/v1/dataset/annotation/prefill\_entries/{model\_id}/&#x20;
* DELETE /extract/api/v1/dataset/annotation/prefill\_entries/{entry\_id}&#x20;
* GET /extract/api/v1/production/files/next/{document\_type\_id}/ POST /classify/api/v1/documents/
