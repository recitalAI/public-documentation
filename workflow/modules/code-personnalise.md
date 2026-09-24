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

Définissez ici **Expression d'entrée** sur `data.get('final_result')`. Une signature différente du contrat documenté, une exception ou un résultat impossible à interpréter provoquent un échec plutôt qu'un résultat exploitable. Ne modifiez pas `job.data` pour transmettre des données : `job` est une vue de l'état du job au moment de l'appel ; seules les valeurs renvoyées sont propagées au Workflow.

## Utiliser `job` et `input`

`job.data` est le dictionnaire courant des données du Workflow (il peut être `None` si aucune donnée n'a encore été produite). `job.custom_metadata` donne les métadonnées personnalisées du job, sous forme de texte ou `None` ; `job.state` donne son état courant et `job.is_test` indique s'il s'agit d'un test. `job.id` identifie le job, notamment pour une requête à l'API reciTAL. Ces valeurs sont une vue de l'exécution, non un moyen de modifier directement le job.

`input` est **la valeur calculée par Expression d'entrée**, et non automatiquement l'ensemble des données du job. Dans cette expression, `data` désigne les données courantes, `initial_data` les données initiales, `custom_metadata` les métadonnées personnalisées, `files` les références des fichiers du job regroupées par collection et `zip` permet d'associer deux séquences. Ces noms sont disponibles **dans l'expression** ; dans la fonction, utilisez `input` pour sa valeur et `job` pour les informations du job exposées ci-dessus. Par exemple, `initial_data` est disponible dans l'expression, mais n'est pas un attribut documenté de `job`.

| Paramètre | Utilisation |
| --- | --- |
| Expression d'entrée | `data['final_result']` transmet cette valeur à `input` ; `files['file']` sélectionne les références de la collection `file` ; `None` transmet `None`. Une clé absente dans une expression avec `[...]` provoque une erreur : adaptez l'expression aux données du job. |
| Itérer sur l'entrée | Si l'expression produit une liste ou un `zip(...)`, appelle la fonction pour chaque élément. Sans cette option, la liste entière est transmise en un seul appel. |

Dans une expression, chaque référence de `files['file']` porte notamment `collection` (ici `file`) et `name` (le nom du fichier) : `files['file'][0].name` transmet ainsi le nom du premier fichier à `input`. La liste brute des références n'est **pas** le contenu binaire des fichiers, ni une liste de chemins directement ouvrables en Python. Pour lire les fichiers, utilisez la procédure de la section [Lire les fichiers du job](#lire-les-fichiers-du-job). Une expression telle que `zip(data['montants'], data['taux'])` construit des couples par position ; avec **Itérer sur l'entrée**, chaque appel reçoit un couple `(montant, taux)`. `zip` s'arrête à la séquence la plus courte : vérifiez que les deux listes correspondent avant de les associer.

## Renvoyer des données au Workflow

Le dictionnaire renvoyé en deuxième position est fusionné dans `job.data`. Les dictionnaires imbriqués sont fusionnés récursivement ; les listes et les valeurs simples remplacent les valeurs précédentes de même clé. Une étape ultérieure accède au résultat avec, par exemple, `data['score']`. Ce module n'a pas de **Clé de sortie** ordinaire : choisissez vous-même les clés du dictionnaire renvoyé.

Par exemple, si `job.data` contient `{"client": {"nom": "Ada", "score": 1}, "tags": ["ancien"]}`, le retour `(StepActionType.done, {"client": {"score": 2}, "tags": ["nouveau"]})` conserve `client.nom`, remplace `client.score` par `2` et remplace la liste `tags`.

Avec **Itérer sur l'entrée**, les dictionnaires des appels réussis sont regroupés par clé en listes ordonnées. Pour une liste de deux éléments, deux retours `{"total": 12}` et `{"total": 18}` donnent `data['total'] == [12, 18]`. Renvoyez les mêmes clés à chaque itération pour obtenir des listes cohérentes.

### Exemple : calculer une décision

**Expression d'entrée** : `data['final_result']` (dictionnaire contenant `montant` et `seuil`). Sans itération :

```python
def execute_action(job, input):
    montant = float(input["montant"])
    seuil = float(input["seuil"])
    return StepActionType.done, {"decision": {"revision_requise": montant > seuil}}
```

Une étape suivante peut exploiter `data['decision']['revision_requise']` dans sa configuration ou une transition. Adaptez les noms des champs à vos données.

### Exemple : traiter une liste ou des couples

Pour calculer individuellement des montants avec TVA, définissez **Expression d'entrée** sur `data['montants']`, activez **Itérer sur l'entrée**, puis utilisez :

```python
def execute_action(job, input):
    return StepActionType.done, {"montant_ttc": round(float(input) * 1.20, 2)}
```

`data['montant_ttc']` sera une liste de résultats, dans l'ordre des montants. Si chaque montant possède son propre taux, utilisez plutôt **Expression d'entrée** : `zip(data['montants'], data['taux'])`, toujours avec **Itérer sur l'entrée** :

```python
def execute_action(job, input):
    montant, taux = input
    return StepActionType.done, {"montant_ttc": round(float(montant) * (1 + float(taux)), 2)}
```

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

Ce code suppose qu'une collection `file` existe ; adaptez-la à la collection utilisée par votre Workflow. Pour de gros fichiers, lisez progressivement plutôt que de charger tout le contenu avec `read_bytes()`. Les fichiers que votre code crée localement sont temporaires : ils ne deviennent pas automatiquement des fichiers du job. Pour joindre un fichier produit au job, utilisez l'[API des fichiers du job](#utiliser-les-api-recital-avec-librecital).

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

## Utiliser les API reciTAL avec `librecital`

`librecital.Client` permet d'appeler les API reciTAL depuis le code personnalisé sans renseigner vous-même une adresse ou un jeton. Par exemple, pour attacher au job courant un fichier texte généré par votre code, faites un `POST` multipart vers l'API des fichiers du job :

```python
from librecital import Client


def execute_action(job, input):
    contenu = f"Total : {float(input):.2f}\n".encode("utf-8")
    with Client() as client:
        response = client.post(
            f"/workflows/api/v1/jobs/{job.id}/files/",
            files={"rapports": ("total.txt", contenu, "text/plain")},
        )
        response.raise_for_status()
    return StepActionType.done, {"rapport_genere": True}
```

Définissez **Expression d'entrée** sur `data['total']`. Ici `rapports` est la collection du fichier joint, `total.txt` son nom ; le résultat de l'API est une liste de références de fichiers. Contrairement à un fichier créé seulement sur le disque temporaire, le fichier envoyé par cette API est associé au job. Utilisez cette opération seulement lorsqu'un module standard ne répond pas au besoin de création du fichier. Une erreur de l'API remonte comme une exception dans l'historique du job.

## Environnement Python et diagnostic

Le code peut importer la bibliothèque standard Python (`json`, `csv`, `re`, `datetime`, `pathlib`, etc.). Parmi les bibliothèques tierces utiles présentes dans l'environnement de production figurent `pandas` (tableaux), `openpyxl` (classeurs Excel) et `PyPDF2` (lecture/manipulation de PDF), ainsi que `librecital` pour les API reciTAL. Importez seulement ce dont vous avez besoin, par exemple `import pandas as pd`. Cette sélection décrit des outils disponibles, **pas** une garantie que toutes les dépendances de l'environnement sont prises en charge comme API stable. Le traitement dispose actuellement de **60 secondes** pour se terminer. Les fichiers locaux créés pendant l'exécution ne sont pas conservés entre les appels ou les jobs ; utilisez les données renvoyées ou l'API des fichiers du job pour conserver un résultat.

Pour diagnostiquer un échec, consultez l'**historique du job** : les sorties de `print()` et les traces d'erreur Python y sont enregistrées. Une erreur de syntaxe, une exception pendant l'exécution ou un retour mal formé échouent ; un dépassement du délai de 60 secondes interrompt le traitement et produit un message d'erreur. `StepActionType.error` marque aussi l'étape et le job en échec. Corrigez le code ou les données d'entrée avant de relancer le job ; ne comptez pas sur une reprise automatique du code échoué. Évitez d'afficher des données sensibles avec `print()` puisque ses sorties sont conservées dans l'historique.

Pour plus d'aide, [contactez l'équipe reciTAL](../../contact/nous-contacter.md).
