---
description: Envoyer une invite et, si nécessaire, un fichier à un modèle de langage dans un Workflow.
---

# Module Workflow : LLM

Le module **LLM** envoie une **Invite** à un modèle de langage. Il peut insérer des données du job dans l'invite et joindre un ou plusieurs fichiers.

## Remplir l'Invite et insérer des variables

Rédigez dans **Invite** l'instruction complète donnée au modèle. Une variable de la forme `{data.chemin}` insère une valeur de `data` avant l'appel au modèle. Les points parcourent les objets imbriqués.

Par exemple, avec les données :

```json
{
  "client": {
    "name": "Entreprise Démo"
  },
  "extract": {
    "invoice_number": "F-2026-0042"
  }
}
```

l'invite suivante est valide :

```text
Rédige un message pour {data.client.name} au sujet de la facture {data.extract.invoice_number}.
```

Une variable dont la valeur est absente est remplacée par une chaîne vide. Cette syntaxe ne sélectionne pas un fichier : utilisez **Inclure le fichier** et **Collection de fichiers** pour joindre des documents.

## Entrée et fichiers

Sans **Expression d'entrée** explicite, le module n'attend pas de données d'entrée. Si **Inclure le fichier** est activé, il prend les fichiers de la **Collection de fichiers** configurée. Avec plusieurs fichiers, activez **Itérer sur l'entrée** pour créer une exécution par fichier.

Une expression telle que `data['extract']` peut aussi sélectionner des données pour piloter l'itération. Les variables de l'**Invite** continuent toutefois à être résolues dans l'objet `data` complet du job.

## Paramètres

| Paramètre | Explication et exemple |
| --- | --- |
| Fournisseur | Champ affiché dans la configuration. Dans cette version, le **Modèle** sélectionné détermine le fournisseur effectivement utilisé. |
| Modèle | Modèle de langage disponible dans l'organisation. |
| Invite | Instruction envoyée au modèle, par exemple `Résume le document pour {data.client.name}.` |
| Inclure le fichier | Joint au message le ou les fichiers sélectionnés. |
| Collection de fichiers | Nom de la collection à joindre, par exemple `file` ou `split-file`. |
| Expression d'entrée | Sélection facultative utilisée notamment pour l'itération, par exemple `files['split-file']`. |
| Itérer sur l'entrée | Exécute un appel au modèle pour chaque élément sélectionné. |
| Clé de sortie | Par exemple `llm_output` : la réponse est accessible avec `data['llm_output']`. |
| Température | Valeur comprise entre `0` et `2`. Une valeur basse, par exemple `0.2`, privilégie des réponses plus stables. |

## Résultat

La valeur de sortie est le texte de la réponse :

```json
{
  "llm_output": "Le document concerne la facture F-2026-0042."
}
```
