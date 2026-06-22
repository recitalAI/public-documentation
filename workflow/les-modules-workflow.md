---
description: >-
  Un grand nombre de modules existent pour compléter votre Worflow. Cette page
  permet d'avoir une vue d'ensemble sur ces derniers, et ce qu'ils permettent de
  faire.
---

# Les modules Workflow

{% hint style="info" %}
Dans l'éditeur de workflow, le bouton **Add step** ouvre la **Steps Library** : le catalogue des modules disponibles, filtrable par recherche de nom et par étiquette. Chaque module porte une ou plusieurs étiquettes parmi **16 catégories** : **Action, AI agent, Archive, Automation, Classification, Code, Document, Email, Extraction, Generation, Input, Output, Post-processing, Review, State, Validation**. Le nombre de modules par catégorie dépend des agents et modèles configurés dans votre organisation (les agents d'extraction/classification y apparaissent comme modules).
{% endhint %}

<figure><img src="../.gitbook/assets/workflow_steps_library.png" alt="Steps Library — catalogue des modules de workflow"><figcaption>La <em>Steps Library</em> (bouton <strong>Add step</strong>) — catalogue de modules filtrable par étiquette.</figcaption></figure>

{% hint style="info" %}
**Libellés en anglais.** L'éditeur est en anglais : les paramètres ci-dessous sont décrits en français, mais s'affichent en anglais dans l'UI. Les paramètres communs à la plupart des modules (souvent regroupés sous **Advanced options**) correspondent ainsi :

| Description (doc, FR) | Libellé UI (EN) |
|---|---|
| Nom | Name |
| Expression d'entrée | Input expression |
| Clé de sortie | Output key |
| Itérer sur l'entrée | Iterate over input |
| Ignorer les erreurs | Ignore errors? |
{% endhint %}

## 1. Input

### Ingest Email — ingérer des emails

Ce module suppose qu'une boite mail a été configurée (voir [lnputs](inputs.md)), ou bien que les documents envoyés sont des .msg ou .eml.

Cette étape permet de récupérer toutes les informations concernant le mail et de les conserver dans "data". Elle permet également de mettre les pièces jointes dans une collection.

#### Paramètres

<table><thead><tr><th width="234">Paramètre</th><th>Description</th></tr></thead><tbody><tr><td><strong>Nom</strong></td><td>Le nom de l'étape</td></tr><tr><td><strong>Expression d'entrée</strong></td><td>La collection d'email à traiter. files["email"] par défaut.</td></tr><tr><td><strong>Taille minimale de pièce jointe (ko)</strong></td><td>Limite minimale à une pièce jointe pour qu'elle soit prise en compte.</td></tr><tr><td><strong>Taille maximale de pièce jointe (ko)</strong></td><td>Limite maximale à une pièce jointe pour qu'elle soit prise en compte.</td></tr><tr><td><strong>Ignorer pièce jointe si le nom contient</strong></td><td>Si le nom de la pièce jointe contient le texte renseigné, alors la pièce jointe sera ignorée. Mettre le nom entre "".</td></tr><tr><td><strong>Extensions acceptées pour pièce jointe</strong></td><td>Les fichiers n'ayant pas ces extensions ne seront pas pris en compte. Si aucune extension n'est renseignée, tous les fichiers sont pris en compte.</td></tr><tr><td><strong>Clé de sortie</strong></td><td>La clé de "data" dans lesquels seront stockées toutes les informations relatives à ce module.</td></tr><tr><td><strong>Collection de sortie</strong></td><td>La collection dans laquelle seront envoyées les pièces jointes. "attachments" par défaut.</td></tr><tr><td><strong>Itérer sur l'entrée</strong></td><td>Activer l'option si l'entrée est une liste. Le module traitera les documents 1 à 1, et la sortie sera une liste de résultats.</td></tr></tbody></table>

#### Structure des résultats

```json
{
    "email": {
        "date": "2024-07-01 14:59:43",
        "subject": ".....",
        "from": {
            "name": "Demo Test",
            "address": "demo.test@recital.ai"
        },
        "to": [
            {
                "name": "Demo Test",
                "address": "demo.test@recital.ai"
            }
        ],
        "cc": [],
        "body": "....",
        "attachments" : ["test1.pdf", "test2.pdf"],
        "skipped_attachments" : ["logo.jpg"]
    }
}
```



## 2. AI Agent

### Documents classification agent — classification de document

#### Paramètres

<table><thead><tr><th width="234">Paramètre</th><th>Description</th></tr></thead><tbody><tr><td><strong>Nom</strong></td><td>Le nom de l'étape</td></tr><tr><td><strong>Modèle</strong></td><td>La sélection parmi les modèles de classification de document existants</td></tr><tr><td><strong>Per page</strong></td><td>(SAM) Permet d'avoir une classification page à page. Il peut être couplé à un module split-pdf à la suite pour déliasser un document.</td></tr><tr><td><strong>Use Google OCR</strong></td><td>Utilisation de google OCR si activé. Sinon, utilisation d'un modèle OCR open-source. </td></tr><tr><td><strong>Expression d'entrée</strong></td><td>Le ou les fichiers à classifier. files["file"] par défaut.</td></tr><tr><td><strong>Expression du modèle</strong></td><td>Sélection dynamique du modèle d'extraction.</td></tr><tr><td><strong>Clé de sortie</strong></td><td>La clé de "data" dans lesquels seront stockées toutes les informations relatives à ce module.</td></tr><tr><td><strong>Itérer sur l'entrée</strong></td><td>Activer l'option si l'entrée est une liste. Le module traitera les documents 1 à 1, et la sortie sera une liste de résultats.</td></tr></tbody></table>

#### Structure des résultats

Voir [Structure des résultats de Classification](../integration-api/classification/structure-des-resultats-de-classification.md)

### Classify Email — classification de mails

#### Paramètres

<table><thead><tr><th width="234">Paramètre</th><th>Description</th></tr></thead><tbody><tr><td><strong>Nom</strong></td><td>Le nom de l'étape</td></tr><tr><td><strong>Expression d'entrée</strong></td><td>Le mail à classifier.  La valeur par défaut est : data['email']</td></tr><tr><td><strong>Expression des pièces jointes</strong></td><td>La collection des pièces jointes. La valeur par défaut est : files['attachments']</td></tr><tr><td><strong>Modèle</strong></td><td>La sélection parmi les modèles de classification de mails existants</td></tr><tr><td><strong>Expression du modèle</strong></td><td>Sélection dynamique du modèle d'extraction.</td></tr><tr><td><strong>Clé de sortie</strong></td><td>La clé de "data" dans lesquels seront stockées toutes les informations relatives à ce module.</td></tr><tr><td><strong>Itérer sur l'entrée</strong></td><td>Activer l'option si l'entrée est une liste. Le module traitera les documents 1 à 1, et la sortie sera une liste de résultats.</td></tr></tbody></table>

#### Structure des résultats

Voir [Structure des résultats de Classification](../integration-api/classification/structure-des-resultats-de-classification.md)

### Extract — extraction

#### Paramètres

<table><thead><tr><th width="234">Paramètre</th><th>Description</th></tr></thead><tbody><tr><td><strong>Nom</strong></td><td>Le nom de l'étape</td></tr><tr><td><strong>Agent d'extraction</strong></td><td>La sélection parmi les agents d'extraction existant</td></tr><tr><td><strong>Expression d'entrée</strong></td><td>Le ou les fichiers à classifier. files["file"] par défaut.</td></tr><tr><td><strong>Expression du modèle</strong></td><td>Sélection dynamique du modèle d'extraction.</td></tr><tr><td><strong>Clé de sortie</strong></td><td>La clé de "data" dans lesquels seront stockées toutes les informations relatives à ce module.</td></tr><tr><td><strong>Itérer sur l'entrée</strong></td><td>Activer l'option si l'entrée est une liste. Le module traitera les documents 1 à 1, et la sortie sera une liste de résultats.</td></tr></tbody></table>

#### Structure des résultats

Voir [Structure des résultats d'Extraction](../integration-api/extraction/structure-des-resultats-dextraction.md)

### llm — GenAI

<table><thead><tr><th width="234">Paramètre</th><th>Description</th></tr></thead><tbody><tr><td><strong>Nom</strong></td><td>Le nom de l'étape</td></tr><tr><td><strong>Fournisseur</strong></td><td>Fournisseur du modèle de langue</td></tr><tr><td><strong>Modèle</strong></td><td>Version du modèle de langue</td></tr><tr><td><strong>Prompt</strong></td><td>Prompt qui sera envoyé au modèle de langue. Des variables peuvent être ajoutées en les mettant entre accolade. <br>Exemple : <em>Quelle est la capitale de {data.country} ?</em></td></tr><tr><td><strong>Inclure fichier</strong></td><td>Est-ce qu'un fichier doit être passé en plus du prompt ?</td></tr><tr><td>Expression d'entrée</td><td>Le ou les fichiers qui seront passés. files["file"] par défaut.</td></tr><tr><td>Clé de sortie</td><td>La clé de "data" dans lesquels seront stockées toutes les informations relatives à ce module.</td></tr><tr><td>Température</td><td>Valeur entre 0 et 1. Une température proche de 0 donnera un modèle déterministe, et inversement.<br>Certains modèles nécessite une température de 1.</td></tr><tr><td>Itérer sur l'entrée</td><td>Activer l'option si l'entrée est une liste. Le module traitera les documents 1 à 1, et la sortie sera une liste de résultats.</td></tr></tbody></table>



## 3. Review

{% hint style="info" %}
Les étape de review sont bloquantes tant que le document n'a pas été validé par un opérateur.
{% endhint %}

### Classification Review — review de classification (vidéo-typage)

Active le vidéo-typage suite à une classification.

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

#### Paramètres

<table><thead><tr><th width="241">Paramètre</th><th>Description</th></tr></thead><tbody><tr><td><strong>Nom</strong></td><td>Le nom de l'étape</td></tr><tr><td><strong>Contexte</strong></td><td>Permet d'afficher des informations pendant la review</td></tr><tr><td><strong>Expression d'entrée</strong></td><td>Le ou les fichiers à classifier. files["file"] par défaut.</td></tr><tr><td><strong>Expression du contexte</strong></td><td>Permet d'afficher des informations pendant la review. Le paramètre supporte des expressions dynamiques.</td></tr><tr><td><strong>Clé de sortie</strong></td><td>La clé de "data" dans lesquels seront stockées toutes les informations relatives à ce module.</td></tr><tr><td><strong>Itérer sur l'entrée</strong></td><td>Activer l'option si l'entrée est une liste. Le module traitera les documents 1 à 1, et la sortie sera une liste de résultats.</td></tr><tr><td>Expression de date d’expiration</td><td>Une fois cette date et heure dépassées, un document en attente de review sera automatiquement libéré (validé ou rejeté en fonction du paramètre Action d'expiration).<br>Le mécanisme de libération des documents passe 1 fois / heure.<br>Format de date : <code>2025-09-23T16:00:00+02:00</code> <br><br></td></tr><tr><td>Action d'expiration</td><td>Action à prendre une fois la date d'expiration dépassée.</td></tr></tbody></table>

#### Structure des résultats

Voir [Structure des résultats de Classification](../integration-api/classification/structure-des-resultats-de-classification.md).



### Email Classification Review — review de classification d'emails

Équivalent du vidéo-typage pour les emails : active la review de la classification d'un email après une étape **Classify Email**. Paramètres analogues à la *Classification Review*.

### Extraction Review — review d'extraction (vidéo-codage)

Active le vidéo-codage suite à une extraction.

<figure><img src="../.gitbook/assets/image (122).png" alt=""><figcaption><p>Ecran de review d'extraction</p></figcaption></figure>

#### Paramètres

<table><thead><tr><th width="241">Paramètre</th><th>Description</th></tr></thead><tbody><tr><td><strong>Nom</strong></td><td>Le nom de l'étape</td></tr><tr><td><strong>Expression d'entrée</strong></td><td>Le ou les fichiers à classifier. files["file"] par défaut.</td></tr><tr><td><strong>Expression du contexte</strong></td><td>Permet d'afficher des informations pendant la review. Le paramètre supporte des expressions dynamiques.</td></tr><tr><td><strong>Clé de sortie</strong></td><td>La clé de "data" dans lesquels seront stockées toutes les informations relatives à ce module.</td></tr><tr><td><strong>Itérer sur l'entrée</strong></td><td>Activer l'option si l'entrée est une liste. Le module traitera les documents 1 à 1, et la sortie sera une liste de résultats.</td></tr><tr><td>Expression de date d’expiration</td><td><p>Une fois cette date et heure dépassées, un document en attente de review sera automatiquement libéré (validé ou rejeté en fonction du paramètre Action d'expiration).</p><p>Le mécanisme de libération des documents passe 1 fois / heure.<br>Format de date : <code>2025-09-23T16:00:00+02:00</code></p></td></tr><tr><td>Action d'expiration</td><td>Action à prendre une fois la date d'expiration dépassée.</td></tr></tbody></table>

#### Structure des résultats

Voir [Structure des résultats d'Extraction](../integration-api/extraction/structure-des-resultats-dextraction.md)

## 4. Actions

### Workflow (sous-workflow)

{% hint style="info" %}
Il est possible d'imbriquer des workflows enfants dans un workflow parent. Cela présente plusieurs utilités:

* Réutilisation d'un workflow dans plusieurs workflow.
* Dans le cas de création de sous-documents (module split-pdf) durant le workflow parent, pouvoir itérer sur l'ensemble de ces sous-documents. Un job 'enfant' sera crée par sous-documents.
* Utiliser la récursivité. Un workflow peut s'appeler lui-même. Attention cependant a bien vérifier les conditions d'arrêt.
{% endhint %}

#### Paramètres

<table><thead><tr><th width="234">Paramètre</th><th>Description</th></tr></thead><tbody><tr><td><strong>Nom</strong></td><td>Le nom du module</td></tr><tr><td><strong>Workflow</strong></td><td>La sélection parmi les workflow existants</td></tr><tr><td><strong>Expression d'entrée</strong></td><td>Les informations auxquels aura accès le sous-workflow. <br>Par exemple après un split-pdf, on peut avoir : <code>zip(data['split-pdf']['files'], files['classified'])</code><br>On prend les informations renvoyées par split-pdf, ainsi que l'ensemble des fichiers ajoutés dans la collection "classified" par split-pdf. A utiliser avec l'option "Iterate over input".</td></tr><tr><td><strong>Workflow Expression</strong></td><td>Permet de sélectionner dynamiquement un workflow en fonction d'une expression.</td></tr><tr><td><strong>Clé de sortie</strong></td><td>La clé de "data" dans lesquels seront stockées toutes les informations relatives à ce module.</td></tr><tr><td><strong>Itérer sur l'entrée</strong></td><td>Activer l'option si l'entrée est une liste. Le module traitera les documents 1 à 1, et la sortie sera une liste de résultats.</td></tr></tbody></table>

#### Structure des résultats

Voir [Structure des résultats du Workflow](../integration-api/workflow/structure-des-resultats-du-workflow.md)

### Diviser un document

Ce module permet de séparer un document en sous-documents. Très utile par exemple pour Déliasser un document après une classification page par page. Les sous-documents générés sont stocké dans la collection de sortie

#### Paramètres

<table><thead><tr><th width="234">Paramètre</th><th>Description</th></tr></thead><tbody><tr><td><strong>Nom</strong></td><td>Le nom du module</td></tr><tr><td><strong>Collection de sortie</strong></td><td>La collection dans laquelle sont envoyés les sous-documents générés.</td></tr><tr><td><strong>Expression d'entrée</strong></td><td>Le ou les fichiers à déliasser. files["file"] par défaut.</td></tr><tr><td><strong>Labels Expr</strong></td><td>La liste des labels de chaque page. Le document sera divisé à chaque fois qu'un label est différent de la page précédente.</td></tr><tr><td><strong>Labels To Ignore</strong></td><td>Les pages ayant ce label seront automatiquement mis à l'écart, et ne figureront pas dans les sous-documents générés.</td></tr><tr><td><strong>Clé de sortie</strong></td><td>La clé de "data" dans lesquels seront stockées toutes les informations relatives à ce module.</td></tr><tr><td><strong>Itérer sur l'entrée</strong></td><td>Activer l'option si l'entrée est une liste. Le module traitera les documents 1 à 1, et la sortie sera une liste de résultats.</td></tr></tbody></table>

#### Structure des résultats

```json
{
    "split-pdf": {
        "files": [
            {
                "name": xxxx-split-1.pdf",
                "label": "Carte grise",
                "pages": [
                    1,
                    2
                ],
                "file_id": ... // int
            },
            {
                "name": xxxx-split-2.pdf",
                "label": "CNI",
                "pages": [
                    3
                ],
                "file_id": ... // int
            }
        ]
    }
}
```

### Décompresser

Ce module permet de décompresser un dossier archivé et d'extraire les fichiers compressés dans une collection de sortie.

#### Paramètres

<table><thead><tr><th width="234">Paramètre</th><th>Description</th></tr></thead><tbody><tr><td><strong>Nom</strong></td><td>Le nom du module</td></tr><tr><td><strong>Collection de sortie</strong></td><td>La collection dans laquelle sont envoyés les fichiers décompressés.</td></tr><tr><td><strong>Expression d'entrée</strong></td><td>Le ou les fichiers à décompresser. files["file"] par défaut.</td></tr><tr><td><strong>Extensions</strong></td><td>Une liste d'extension valide. (Exemple : "pdf, doc, jpg, png"). Les fichiers ayant une extension différente dans l'archive seront ignorés.</td></tr><tr><td><strong>Clé de sortie</strong></td><td>La clé de "data" dans lesquels seront stockées toutes les informations relatives à ce module.</td></tr><tr><td><strong>Itérer sur l'entrée</strong></td><td>Activer l'option si l'entrée est une liste. Le module traitera les documents 1 à 1, et la sortie sera une liste de résultats.</td></tr></tbody></table>

#### Structure des résultats

```json
{
    "unpack": {
        "unpacked": [
            "ffe09da0-7e09-11ee-a133-dd18cdfb66cc.pdf",
            "ffc93a80-7e08-11ee-a133-dd18cdfb66cc.pdf",
            "ff7c3680-7e09-11ee-a133-dd18cdfb66cc.pdf"
        ],
        "skipped": []
    }
}
```

### Fusionner des documents

Ce module permet de fusionner les pages de plusieurs documents en entrée en un seul document.

#### Paramètres

<table><thead><tr><th width="234">Paramètre</th><th>Description</th></tr></thead><tbody><tr><td><strong>Nom</strong></td><td>Le nom du module</td></tr><tr><td><strong>Collection de sortie</strong></td><td>La collection dans laquelle est envoyé le fichier fusionné.</td></tr><tr><td><strong>Nom du fichier de sortie</strong></td><td>Le nom du fichier de sortie</td></tr><tr><td><strong>Expression d'entrée</strong></td><td>Les fichiers à fusionner. files["file"] par défaut.</td></tr><tr><td><strong>Clé de sortie</strong></td><td>La clé de "data" dans lesquels seront stockées toutes les informations relatives à ce module.</td></tr><tr><td><strong>Itérer sur l'entrée</strong></td><td>Activer l'option si l'entrée est une liste. Le module traitera les documents 1 à 1, et la sortie sera une liste de résultats.</td></tr></tbody></table>

#### Structure des résultats

```json
{
    "merge-pdf": {
        "file_id": ..., // int
        "name": "merged.pdf"
    }
}
```

### Ensemble Validation — validation d'ensemble

Module de validation (étiquettes **Validation / Document / AI agent**) disponible dans la Steps Library.



## 5. Code personnalisé (Custom Code)

Le module **Custom Code** permet de faire tout ce qui n'a pas encore été préconçu par d'autre module.

<pre class="language-python"><code class="lang-python"><strong>def execute_action(job, input):
</strong>    # Do smth here
    return StepActionType.done, {
        # Add data here          
    }
</code></pre>

Le paramètre d'entrée `job` contient toutes les informations utiles dans `job.data`.

Il est également possible de rajouter de nouvelles informations dans `data` avec le `return`.&#x20;

Pour plus de détails sur le fonctionnement du module de code personnalisé, et sur des détails du Workflow, veuillez [contacter l'équipe Projet](/broken/pages/9GNnOCH7oGDOTJ2Uz599).

## 6. État

{% hint style="info" %}
Rajouter un module d'état permet de:

* Recevoir automatiquement une notification de changement d'état. Avec les données accumulées jusque là.
* Filtrer / trier les jobs en fonction de leur état.
{% endhint %}

Les modules d'état peuvent être ajoutés à n'importe quelle transition. Ils permettent de mettre à jour l'état général d'un job, et de notifier si une url de callback a été définie lors de la création du job.

## 7. Output

### Cleanup

Le module **Cleanup** (étiquette **Output**) supprime les données (`data`) et tous les fichiers du job — utile en fin de workflow pour ne pas conserver les documents traités.

### Webhook

Permet de renvoyer les résultats en cours (ou une partie) vers une URL donnée.

#### Paramètres

<table><thead><tr><th width="234">Paramètre</th><th>Description</th></tr></thead><tbody><tr><td><strong>Nom</strong></td><td>Le nom du module</td></tr><tr><td><strong>URL</strong></td><td>L'url de callback</td></tr><tr><td><strong>Ignorer les erreurs</strong> (Ignore errors?)</td><td>Boolean. Si l'option est activée, le flux ne sera pas interrompu, même si le code de réponse est une erreur.</td></tr><tr><td><strong>Réessayer en cas d'erreur</strong> (Retry on error?)</td><td>Boolean. Relance l'appel en cas d'échec.</td></tr><tr><td><strong>Expression d'entrée</strong></td><td>Les données à renvoyer. "data" par défaut.</td></tr><tr><td><strong>Clé de sortie</strong></td><td>La clé de "data" dans lesquels seront stockées toutes les informations relatives à ce module.</td></tr><tr><td><strong>Itérer sur l'entrée</strong></td><td>Activer l'option si l'entrée est une liste. Le module traitera les documents 1 à 1, et la sortie sera une liste de résultats.</td></tr></tbody></table>

#### Structure des résultats

```json
{
    "webhook": {
        "retries": [
            {
                "url": "http://foo.bar.baz.example.com",
                "delivery": "faa7f0ee-79fc-4f7f-8bd7-13fe507d431b",
                "timestamp": "2025-02-12T16:51:21.738001+00:00",
                "time": 0.16692353412508965,
                "status": 403
            },
            {
                "url": "http://foo.bar.baz.example.com",
                "delivery": "016fc069-077a-4026-a122-d03e044fc67a",
                "timestamp": "2025-02-12T16:51:26.989879+00:00",
                "time": 0.17694886191748083,
                "status": 403
            }
        ],
        "url": "http://foo.bar.baz.example.com",
        "delivery": "016fc069-077a-4026-a122-d03e044fc67a",
        "timestamp": "2025-02-12T16:51:26.989879+00:00",
        "time": 0.17694886191748083,
        "status": 200
    }
}
```
