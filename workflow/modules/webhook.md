---
description: Envoyer des données d'un job Workflow vers une URL HTTP.
---

# Module Workflow : Webhook

Le module **Webhook** envoie des données du job vers une URL HTTP. Il peut utiliser une URL fixe ou la sélectionner dynamiquement dans les données.

## Entrée

Le module attend un objet de données sérialisable en JSON. Pour n'envoyer qu'un résultat final, utilisez par exemple `data['final_result']`. Cette expression sélectionne une valeur dans `data` ; elle ne transmet pas les collections de fichiers. L'expression `data` transmet l'ensemble des données du job.

## Paramètres

| Paramètre | Explication et exemple |
| --- | --- |
| URL | Adresse de destination, par exemple `https://example.com/callback`. |
| Expression d'URL | Sélection dynamique, par exemple `data['callback_url']`. Lorsqu'elle renvoie une valeur, celle-ci remplace l'URL fixe. |
| Méthode | Méthode HTTP, par exemple `post`. |
| Token d'authorisation | Secret transmis selon le type d'authorisation choisi. |
| Type d'authorisation | `bearer` envoie `Bearer <token>` dans l'en-tête configuré ; `header` envoie directement le token dans cet en-tête ; `param` l'envoie comme paramètre de requête. |
| Nom de l'en-tête d'authorisation | Par exemple `Authorization`, utilisé avec `bearer` ou `header`. |
| Nom du paramètre d'authorisation | Par exemple `access_token`, utilisé avec `param`. |
| Ignorer les erreurs | Permet au Workflow de continuer lorsqu'une livraison échoue. |
| Réessayer en cas d'erreur | Effectue de nouvelles tentatives après une erreur de livraison. |
| Encapsuler dans une enveloppe de Webhook | Ajoute les informations du job autour de la donnée envoyée. |
| Expression d'entrée | Par exemple `data['final_result']` pour envoyer uniquement le résultat final. |
| Itérer sur l'entrée | Envoie une requête par élément lorsque l'expression renvoie une liste. |
| Clé de sortie | Par exemple `webhook` : les informations de livraison sont accessibles avec `data['webhook']`. |

## Résultat

La sortie décrit la dernière livraison et, lorsqu'il y en a eu, les tentatives précédentes :

```json
{
  "webhook": {
    "retries": [
      {
        "url": "https://example.com/callback",
        "delivery": "faa7f0ee-79fc-4f7f-8bd7-13fe507b431b",
        "timestamp": "2025-02-12T16:51:21.738001+00:00",
        "time": 0.16692353412508965,
        "status": 403
      }
    ],
    "url": "https://example.com/callback",
    "delivery": "016fc069-077a-4026-a122-d03e044fc67a",
    "timestamp": "2025-02-12T16:51:26.989879+00:00",
    "time": 0.17694886191748083,
    "status": 200
  }
}
```
