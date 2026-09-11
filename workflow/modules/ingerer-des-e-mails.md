---
description: Ingérer un e-mail et placer ses pièces jointes dans une collection Workflow.
---

# Module Workflow : Ingérer des e-mails

**Dépendance produit : Classify.** Le module **Ingérer des e-mails** lit un message reçu d'une boîte mail configurée, ou un fichier `.msg` ou `.eml`. Il enregistre les informations du message dans `data` et place les pièces jointes acceptées dans une collection de fichiers.

## Entrée

Le module attend un seul fichier d'e-mail. L'expression `files['email']` sélectionne la collection de fichiers `email`, utilisée par les jobs issus d'une source IMAP. Si plusieurs e-mails doivent être traités, activez **Itérer sur l'entrée** afin que chaque exécution ne reçoive qu'un fichier.

## Paramètres

| Paramètre | Explication et exemple |
| --- | --- |
| Expression d'entrée | `files['email']` sélectionne le fichier d'e-mail à lire. |
| Itérer sur l'entrée | Traite séparément chaque fichier sélectionné. |
| Renommer les doublons de pièces-jointes | Évite les collisions lorsque plusieurs pièces jointes portent le même nom. |
| Taille minimale des pièces-jointes (ko) | Par exemple `10` pour écarter les très petits fichiers comme certains pixels de suivi. |
| Taille maximale des pièces-jointes (ko) | Par exemple `10240` pour limiter les pièces jointes à 10 Mo. |
| Ignorer pièce-jointe si le nom contient | Par exemple `["logo", "signature"]` pour ignorer les éléments récurrents correspondants. |
| Extensions acceptées pour les pièces-jointes | Limite les pièces jointes aux extensions prises en charge et utiles au Workflow, par exemple `["pdf", "png", "jpg"]`. |
| Clé de sortie | Par exemple `email` : les informations du message sont enregistrées dans `data['email']`. |
| Collection de sortie | Par exemple `attachments` : les pièces jointes sont accessibles avec `files['attachments']`. |

## Résultat

```json
{
  "email": {
    "date": "2024-07-01 14:59:43",
    "subject": "Objet du message",
    "from": {
      "name": "Demo Test",
      "address": "demo.test@recital.ai"
    },
    "to": [
      {
        "name": "Service client",
        "address": "service.client@example.com"
      }
    ],
    "cc": [],
    "body": "Contenu du message",
    "attachments": ["facture.pdf"],
    "skipped_attachments": ["logo.jpg"]
  }
}
```

L'étape **Classification E-mail** peut ensuite lire `data['email']` et joindre `files['attachments']`.
