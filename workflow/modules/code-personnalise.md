---
description: Écrire, exécuter et diagnostiquer un traitement Python dans un Workflow.
---

# Module Workflow : Code personnalisé

**Code personnalisé** exécute une fonction Python pour un traitement propre à votre Workflow : calcul, transformation ou contrôle qui ne se configure pas simplement avec un module standard. Préférez un [module Workflow dédié](../les-modules-workflow.md) lorsqu'il répond déjà au besoin, par exemple pour extraire ou classer un document.

## Écrire la fonction

Le code doit définir `execute_action(job, input)` et renvoyer **deux valeurs** : un état `StepActionType` et un dictionnaire de données. Exemple minimal à copier dans l'éditeur du module :

```python
def execute_action(job, input):
    return StepActionType.done, {}
```

La fonction s'exécute de façon synchrone : elle doit finir et renvoyer son résultat pendant l'exécution du module. `StepActionType.done` termine l'étape et laisse suivre les transitions automatiques. `StepActionType.error` met l'étape et le job en échec ; utilisez-le pour un contrôle métier bloquant :

```python
def execute_action(job, input):
    if input is None:
        return StepActionType.error, {}
    return StepActionType.done, {"valide": True}
```

Ne modifiez pas `job.data` pour transmettre des données : `job` est une vue de l'état du job au moment de l'appel ; seules les valeurs renvoyées sont propagées au Workflow.

## Utiliser `job` et `input`

### Choisir la valeur de `input`

**Expression d'entrée** choisit une valeur qui est transmise à la fonction Python dans l'argument `input`. Les noms ci-dessous sont disponibles **dans cette expression**, pas automatiquement comme variables dans `execute_action(job, input)` :

| Dans Expression d'entrée | Ce que cela représente et quand l'utiliser | Dans la fonction Python |
| --- | --- | --- |
| `data` | Données courantes du Workflow, enrichies par les étapes précédentes. Utilisez `data['completude_dossier']` pour transmettre une valeur déjà produite. | `job.data` (dictionnaire, ou `None` si aucune donnée n'a encore été produite). |
| `custom_metadata` | Métadonnées personnalisées associées au job : contexte fourni par l'appelant, à garder distinct des données produites par le Workflow. Sélectionnez `custom_metadata` si le code doit exploiter ce contexte. | `job.custom_metadata` (texte, ou `None`). |
| `files` | Références des fichiers du job, regroupées par collection. Utilisez `[f.name for f in files['file']]` pour sélectionner les noms des documents de la collection `file`. | Pas d'attribut `job.files` documenté : transmettez les noms par `input`, puis lisez les fichiers comme indiqué plus bas. |

`job` est une vue du job disponible directement dans la fonction Python ; `job.state` donne son état courant et `job.id` son identifiant. L'expression, elle, est évaluée **avant** l'appel : son résultat devient `input`. Sans **Itérer sur l'entrée**, la fonction reçoit ce résultat en une seule fois ; avec cette option, si l'expression sélectionne une liste, chaque appel reçoit séparément un élément dans `input`. L'expression peut aussi être `None` pour transmettre la valeur `None`. Une clé absente dans une expression avec `[...]` provoque une erreur ; choisissez une expression adaptée aux données du job.

Sans itération, transmettez généralement un seul objet, souvent un dictionnaire comme `data['donnees_financieres']`. Avec itération, sélectionnez généralement une liste, par exemple les noms des documents avec `[f.name for f in files['file']]` ; chaque appel recevra un nom. Il s'agit de conseils de configuration, pas de restrictions de type : sans itération, le code peut aussi recevoir un nombre ou une liste entière. L'itération s'applique aux listes ; l'association de deux séquences avec `zip(...)` est expliquée plus bas.

| Paramètre | Utilisation |
| --- | --- |
| Expression d'entrée | Choisit la valeur transmise à `input`, par exemple `data['completude_dossier']`. |
| Itérer sur l'entrée | Appelle séparément la fonction pour chaque élément d'une liste sélectionnée par l'expression. Sans cette option, la liste entière est transmise à un seul appel. |

Dans une expression, chaque référence de `files['file']` porte notamment `collection` (ici `file`) et `name` (le nom du fichier). La référence brute n'est **pas** le contenu du fichier et ne fournit pas un objet fichier utilisable tel quel dans la fonction : transmettez son nom par l'expression, puis ouvrez le fichier comme indiqué dans [Lire les fichiers du job](#lire-les-fichiers-du-job).

### Distinguer un job de test avec `job.is_test`

Dans `execute_action(job, input)`, `job.is_test` est un booléen Python : `True` pour un job de test, `False` pour un job de production. Cette valeur correspond à la distinction **Live / Test** de l'[onglet Jobs](../jobs.md#consulter-les-jobs) et reste celle du job pendant son exécution. Vous pouvez l'utiliser dans une condition pour adapter le traitement, par exemple renvoyer un résultat différent selon le mode :

```python
def execute_action(job, input):
    if job.is_test:
        return StepActionType.done, {"mode": "test"}
    return StepActionType.done, {"mode": "live"}
```

## Renvoyer des données au Workflow

Le dictionnaire renvoyé en deuxième position est fusionné dans `job.data`. Les dictionnaires imbriqués sont fusionnés récursivement ; les listes et les valeurs simples remplacent les valeurs précédentes de même clé. Une étape ultérieure accède au résultat avec, par exemple, `data['score']`. Ce module n'a pas de **Clé de sortie** ordinaire : choisissez vous-même les clés du dictionnaire renvoyé.

Par exemple, si `job.data` contient `{"client": {"nom": "Ada", "score": 1}, "tags": ["ancien"]}`, le retour `(StepActionType.done, {"client": {"score": 2}, "tags": ["nouveau"]})` conserve `client.nom`, remplace `client.score` par `2` et remplace la liste `tags`.

Avec **Itérer sur l'entrée**, les dictionnaires des appels réussis sont regroupés par clé en listes ordonnées. Pour une liste de deux éléments, deux retours `{"total": 12}` et `{"total": 18}` donnent `data['total'] == [12, 18]`. Renvoyez les mêmes clés à chaque itération pour obtenir des listes cohérentes.

### Exemple : calculer une décision

**Expression d'entrée** : `data['donnees_financieres']` (dictionnaire contenant `montant` et `seuil`). Sans itération :

```python
def execute_action(job, input):
    montant = float(input["montant"])
    seuil = float(input["seuil"])
    return StepActionType.done, {"decision": {"revision_requise": montant > seuil}}
```

Une étape suivante peut exploiter `data['decision']['revision_requise']` dans sa configuration ou une transition. Adaptez les noms des champs à vos données.

### Exemple : examiner chaque document PDF

Pour analyser séparément des PDF de la collection `file`, définissez **Expression d'entrée** sur `[f.name for f in files['file']]` et activez **Itérer sur l'entrée**. Chaque appel reçoit dans `input` le nom d'un PDF ; le code lit le document et repère les pages contenant une expression à contrôler :

```python
from pathlib import Path
from PyPDF2 import PdfReader


def execute_action(job, input):
    fichier = Path("files/file") / input
    pdf = PdfReader(str(fichier))
    pages_a_controler = [
        numero for numero, page in enumerate(pdf.pages, start=1)
        if "signature" in (page.extract_text() or "").lower()
    ]
    return StepActionType.done, {
        "analyse_pdf": {"nom": input, "pages": len(pdf.pages), "pages_a_controler": pages_a_controler}
    }
```

`data['analyse_pdf']` devient une liste de résultats, dans l'ordre des fichiers sélectionnés. Utilisez l'itération lorsqu'un élément demande un traitement significatif ; pour des transformations rapides d'une simple liste Python, traitez plutôt la liste en un seul appel lorsque c'est possible.

### Associer chaque fichier à ses données

Pour rechercher une expression différente dans chaque PDF, définissez **Expression d'entrée** sur `zip([f.name for f in files['file']], data['documents'])` et activez **Itérer sur l'entrée**. `zip(...)` associe les éléments de même position ; chaque appel reçoit un couple `(nom_du_fichier, données)`. Les deux listes doivent correspondre dans le même ordre : `zip(...)` s'arrête à la plus courte. Ici chaque élément de `data['documents']` contient un champ `terme_a_verifier` :

```python
from pathlib import Path
from PyPDF2 import PdfReader


def execute_action(job, input):
    nom_du_fichier, donnees = input
    pdf = PdfReader(str(Path("files/file") / nom_du_fichier))
    terme = donnees["terme_a_verifier"].lower()
    pages = [
        numero for numero, page in enumerate(pdf.pages, start=1)
        if terme in (page.extract_text() or "").lower()
    ]
    return StepActionType.done, {"verification": {"nom": nom_du_fichier, "pages": pages}}
```

Une expression `zip(files['file'], data['documents'])` peut être évaluée, mais la référence de fichier brute n'arrive pas comme objet fichier exploitable dans le code Python. Sélectionnez `f.name` dans l'expression comme ci-dessus, puis ouvrez le document par son nom.

## Lire les fichiers du job

Les fichiers du job sont accessibles en lecture pendant l'exécution sous `files/<collection>/<nom du fichier>`. La collection `file` contient par exemple les documents fournis à l'entrée du job ; d'autres modules peuvent produire d'autres collections. `files['file']` dans **Expression d'entrée** sélectionne des références, pas un chemin à ouvrir directement. Pour un traitement de la collection `file`, sélectionnez `None` comme expression et parcourez les fichiers disponibles :

```python
from pathlib import Path


def execute_action(job, input):
    noms = []
    for fichier in sorted(Path("files/file").iterdir()):
        if fichier.is_file():
            contenu = fichier.read_bytes()
            noms.append({"nom": fichier.name, "octets": len(contenu)})
    return StepActionType.done, {"fichiers_analyses": noms}
```

Ce code suppose qu'une collection `file` existe ; adaptez-la à la collection utilisée par votre Workflow. Pour de gros fichiers, lisez progressivement plutôt que de charger tout le contenu avec `read_bytes()`. Les fichiers que votre code crée localement sont temporaires : ils ne deviennent pas automatiquement des fichiers du job. `librecital.Client` ne permet pas de les joindre au job via l'API Workflows depuis ce module.

## Lire une Ressource

Les **Ressources** sont des fichiers mis à disposition dans l'espace **Ressources** de reciTAL. Utilisez leur chemin relatif, dossiers et nom compris, sous `resources/`. Par exemple, après avoir ajouté une Ressource nommée `regles/seuil.txt` contenant un nombre :

```python
from pathlib import Path


def execute_action(job, input):
    seuil = float(Path("resources/regles/seuil.txt").read_text(encoding="utf-8").strip())
    montant = float(input)
    return StepActionType.done, {"depasse_seuil": montant > seuil}
```

Définissez **Expression d'entrée** sur `data['montant']`. Respectez la casse et les dossiers du nom de la Ressource ; si le fichier manque ou ne contient pas un nombre, la fonction échoue et l'erreur apparaît dans l'historique du job. La lecture n'altère pas la Ressource d'origine.

## Adapter la configuration entre plusieurs environnements

Pour garder le même **Code personnalisé** (Custom Code) entre un environnement de développement et un environnement de production (dev / prod), séparez du code les valeurs de configuration **non sensibles** : UUID du Workflow cible, adresse d'une API externe ou seuil de traitement. Les **Ressources** sont propres à chaque organisation. Créez dans chacune une Ressource au même chemin relatif, `config/environnement.json`, avec des valeurs différentes. Ces fichiers sont gérés séparément : exporter ou importer un Workflow ne transfère pas ses Ressources.

Par exemple, dans l'organisation de développement, le contenu de `config/environnement.json` peut être :

```json
{"target_workflow_uuid": "11111111-1111-4111-8111-111111111111", "api_base_url": "https://api-dev.example.com", "seuil": 50}
```

Dans l'organisation de production, conservez le même chemin avec un contenu propre à la production :

```json
{"target_workflow_uuid": "22222222-2222-4222-8222-222222222222", "api_base_url": "https://api-prod.example.com", "seuil": 80}
```

Le même code lit alors la Ressource de l'organisation où le job s'exécute. Définissez **Expression d'entrée** sur `data['montant']`, sans activer **Itérer sur l'entrée** :

```python
import json
from pathlib import Path


def execute_action(job, input):
    config = json.loads(Path("resources/config/environnement.json").read_text(encoding="utf-8"))
    return StepActionType.done, {
        "depasse_seuil": float(input) > config["seuil"],
        "workflow_cible": config["target_workflow_uuid"],
        "api_base_url": config["api_base_url"],
    }
```

Les valeurs ci-dessus sont fictives et non sensibles. Vérifiez que la Ressource existe dans chaque organisation avant d'exécuter le Workflow ; les données renvoyées seront visibles dans le job. Pour la lecture des fichiers, voir [Lire une Ressource](#lire-une-ressource).

**Identifiants (credentials) et secrets :** ne confondez pas cette configuration avec un coffre à secrets. Une Ressource est un fichier ordinaire : son contenu peut être consulté ou téléchargé par les personnes autorisées à gérer les Ressources de l'organisation et lu par le Code personnalisé. Il n'est pas masqué comme un secret. Pour un appel avec `requests` à une API externe exigeant un token, une clé API (API key) ou un `client_id` / `client_secret`, ne mettez pas ces identifiants dans cette Ressource de configuration ni en clair dans le code ou les sorties de `print()`. Le module ne propose pas de gestion client de variables d'environnement arbitraires ni de secrets masqués. Si votre intégration nécessite des identifiants, [contactez l'équipe reciTAL](../../contact/nous-contacter.md) pour définir une solution adaptée avant de la déployer ; une Ressource contenant des identifiants reste un fichier consultable, pas un stockage de secrets dédié.

Pour l'authentification aux API reciTAL, utilisez `librecital.Client` uniquement pour l'API Extract prise en charge ici, comme indiqué dans [Utiliser les API reciTAL avec `librecital`](#utiliser-les-api-recital-avec-librecital). Ne transposez pas ce mécanisme à l'API Workflows et ne codez pas de token ou d'adresse interne reciTAL en dur.

## Utiliser les API reciTAL avec `librecital`

Dans le module **Code personnalisé**, `librecital.Client` permet d'appeler l'API Extract de reciTAL, mais pas l'API Workflows. Ne l'utilisez pas pour appeler l'API des fichiers du job : l'envoi d'un fichier créé localement par cette voie ne fonctionne pas.

## Interroger un référentiel externe avec `requests`

Pour enrichir les données du Workflow avec un service HTTP externe accessible depuis votre environnement, définissez **Expression d'entrée** sur `data['reference']` (un dictionnaire contenant `code`) et adaptez l'adresse et la structure de la réponse à votre référentiel :

```python
import requests


def execute_action(job, input):
    response = requests.get(
        "https://api.example.com/references",
        params={"code": input["code"]},
        timeout=10,
    )
    response.raise_for_status()
    return StepActionType.done, {"reference_verifiee": response.json()}
```

`api.example.com` est une adresse d'exemple, non un service reciTAL. Si le service est indisponible ou renvoie une erreur HTTP, l'étape échoue ; vérifiez l'accès réseau au référentiel concerné et adaptez le traitement de sa réponse. Ne placez pas d'identifiants sensibles en clair dans le code ni dans les sorties de `print()`.

## Environnement Python et diagnostic

Le code peut importer la bibliothèque standard Python (`json`, `csv`, `re`, `datetime`, `pathlib`, etc.). Parmi les bibliothèques tierces utiles présentes dans l'environnement de production figurent `requests` pour appeler des API HTTP externes (par exemple un référentiel de validation ou d'enrichissement), `pandas` pour les tableaux, `openpyxl` pour les classeurs Excel et `PyPDF2` pour les PDF, ainsi que `librecital` pour l'API Extract de reciTAL. Importez seulement ce dont vous avez besoin, par exemple `import requests`. La connexion à un service externe dépend de l'accès réseau disponible vers ce service dans votre environnement ; ne présumez pas que tous les hôtes sont joignables. Cette sélection décrit des outils disponibles, **pas** une garantie que toutes les dépendances de l'environnement sont prises en charge comme API stable. Le traitement dispose actuellement de **60 secondes** pour se terminer. Les fichiers locaux créés pendant l'exécution ne sont pas conservés entre les appels ou les jobs ; renvoyez les données à conserver dans le Workflow.

### Identifier les versions pour écrire du code compatible

Le code, y compris celui proposé par un assistant IA, doit cibler les versions disponibles dans l'environnement où le Workflow s'exécute, et non la dernière version publiée d'une bibliothèque. Pour relever la version exacte de Python et celles des bibliothèques citées dans ce guide, exécutez ce code dans un **job de test** du même environnement, avec **Expression d'entrée** à `None` et sans **Itérer sur l'entrée** :

```python
import sys
from importlib.metadata import version


def execute_action(job, input):
    print("Python :", sys.version)
    for paquet in ("requests", "pandas", "openpyxl", "PyPDF2", "librecital"):
        print(f"{paquet}=={version(paquet)}")
    return StepActionType.done, {}
```

Les versions apparaissent dans l'**historique du job** ; ce code ne renvoie aucune nouvelle donnée métier. `version(...)` attend le nom du paquet installé : pour le PDF, utilisez bien `from PyPDF2 import PdfReader`, comme dans les exemples PDF ci-dessus, et non un exemple destiné à un autre paquet comme `pypdf`.

Conservez ce relevé avec la date, l'environnement et la version reciTAL concernée, puis transmettez ces informations à votre assistant de génération de code. Refaites le relevé après une mise à jour de reciTAL ou avant de réutiliser le code dans un autre environnement. La présence d'un paquet ne garantit pas que toutes ses interfaces sont officiellement prises en charge ; les [limites de `librecital.Client`](#utiliser-les-api-recital-avec-librecital) restent applicables. Ne supposez pas pouvoir installer un paquet supplémentaire ou mettre à niveau les bibliothèques du module : [contactez l'équipe reciTAL](../../contact/nous-contacter.md) pour vérifier la disponibilité et la prise en charge de votre besoin.

### Diagnostiquer un échec

Pour diagnostiquer un échec, consultez l'**historique du job** : les sorties de `print()` et les traces d'erreur Python y sont enregistrées. Une erreur de syntaxe, une exception pendant l'exécution ou un retour mal formé échouent ; un dépassement du délai de 60 secondes interrompt le traitement et produit un message d'erreur. `StepActionType.error` marque aussi l'étape et le job en échec. Corrigez le code ou les données d'entrée avant de relancer le job ; ne comptez pas sur une reprise automatique du code échoué. Évitez d'afficher des données sensibles avec `print()` puisque ses sorties sont conservées dans l'historique.

Pour plus d'aide, [contactez l'équipe reciTAL](../../contact/nous-contacter.md).
