---
description: >-
  Agent d'extraction génératif, Nouveau modèle de détection de signature et
  cases à cocher, Video-typage pour les emails, Nouveau modèle OCR
---

# Version 24.11.x (2024-11-05)

## Fonctionnalités

<table><thead><tr><th width="71">#</th><th width="548">Description</th></tr></thead><tbody><tr><td>1</td><td>Il est désormais possible de configurer un extracteur pour qu'il soit extrait grâce à un modèle génératif (LLM). Une page dédiée dans la documentation a été créée : <a href="../../extraction/configurer-un-agent-dextraction/configurer-les-extracteurs-dun-agent.md#methode-dextraction">Agent d'extraction génératif</a>.</td></tr><tr><td>2</td><td>Nouveau module dans le workflow permettant de valider automatiquement un champ si le modèle extractif et le modèle génératif prédisent la même chose.</td></tr><tr><td>3</td><td>Il est désormais possible de créer très rapidement un ensemble de datasets à partir d'un fichier zip. 1 dossier = 1 dataset</td></tr><tr><td>4</td><td>Les restrictions pour supprimer un agent, un dataset ou un modèle ont été supprimées.</td></tr><tr><td>5</td><td>Le modèle OCR d'Azure a été ajouté aux modèles OCR disponibles. Vous pouvez le retrouver dans les paramètres généraux de votre organisation.</td></tr><tr><td>6</td><td>Dans la matrice de confusion d'un modèle de classification, on peut voir le nom des documents en erreur. Cette fonctionnalité permet de vérifier si ces documents sont bien rangés dans les bons jeux de données.</td></tr><tr><td>7</td><td>Dans un jeu de données de courriels, il est possible de déplacer un courriel d'un jeu de données vers un autre.</td></tr><tr><td>8</td><td>Nouveau module "Unpack" dans le workflow, permettant de lire les pièces jointes zippées ou les courriels imbriqués.</td></tr><tr><td>9</td><td>Les modèles de détection de signatures et de cases à cocher ont été améliorés.</td></tr></tbody></table>

## Bugs corrigés

<table><thead><tr><th width="71">#</th><th width="548">Description</th></tr></thead><tbody><tr><td>1</td><td>Correction de l'étape de Cleanup dans le workflow avec des sous-workflows.</td></tr><tr><td>2</td><td>Renommage de la clé "uuid" pour l'étape de webhook en "webhook_uuid".</td></tr><tr><td>3</td><td>Renommage de la collection "Attachment" en "Attachments" dans le workflow.</td></tr><tr><td>4</td><td>Correction du bouton pour télécharger les données de reporting</td></tr><tr><td>5</td><td>Correction de la page de vidéo-codage et des problèmes d'accès</td></tr><tr><td>6</td><td>Correction de la suppression des regex dans la configuration d'un extracteur </td></tr><tr><td>7</td><td>Changer le nom d'un label dans lors de l'annotation ne supprime plus les annotations de ce label.</td></tr></tbody></table>
