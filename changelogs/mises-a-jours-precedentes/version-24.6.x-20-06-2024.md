---
description: >-
  Refonte de l'interface utilisateur, nouvelle fonctionnalité majeure 
  "Workflow", nouvelles fonctionnalités mineures, bugs corrigés. Vulnérabilités
  patchées.
---

# Version 24.6.x (20/06/2024)

## Refonte générale de l'interface utilisateur

### Page d'accueil

Une nouvelle page d'accueil "Dashboard".

<div><figure><img src="../../.gitbook/assets/image (20).png" alt=""><figcaption><p>Avant</p></figcaption></figure> <figure><img src="../../.gitbook/assets/image (19).png" alt=""><figcaption><p>Après</p></figcaption></figure></div>

### Navigation

<div align="left" data-full-width="false"><figure><img src="../../.gitbook/assets/image (14).png" alt=""><figcaption><p>Avant</p></figcaption></figure> <figure><img src="../../.gitbook/assets/2024-06-03 14_24_13-Paramètres.png" alt=""><figcaption><p>Après</p></figcaption></figure></div>

La navigation a été repensée pour avoir une partie Studio et une partie Production.

Dans la partie Studio on retrouve les datasets, les modèles de classification et d'extraction, les agents d'extraction (anciens "Doctypes"), et les workflows.

Dans la partie Production on retrouve la Review d'extraction et de classification, et le monitoring du flux en production.

### Les tableaux

L'UI a été revu, et il est désormais possible d'inverser l'ordre des lignes de tous les tableaux.

<figure><img src="../../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

### Écran de vidéo-codage d'extraction

L'écran de vidéo-codage a été repensé pour ressembler à un formulaire à compléter. Le nombre de clics pour vidéo-coder un document a drastiquement diminué.

<div><figure><img src="../../.gitbook/assets/image (17).png" alt=""><figcaption><p>Avant</p></figcaption></figure> <figure><img src="../../.gitbook/assets/image (18).png" alt=""><figcaption><p>Après</p></figcaption></figure></div>

### Écran de vidéo-codage de classification (vidéo-typage)

Un nouvel écran a été designé pour la review de déliassage. Il sera également utilisé pour la review de classification "simple" (WIP).&#x20;

<figure><img src="../../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

## Fonctionnalités

<table><thead><tr><th width="71">#</th><th width="548">Description</th></tr></thead><tbody><tr><td>1</td><td>Workflow. Pour plus de détail sur la nouvelle fonctionnalité, voir sa page dédiée dans la documentation.</td></tr><tr><td>2</td><td>Les Datasets peuvent maintenant être téléchargé en local, et également uploadé.</td></tr><tr><td>3</td><td>Un dataset ne peut être supprimé que par son créateur.</td></tr><tr><td>4</td><td>La fonctionalité Règles de Gestion "Externe" a été supprimée. Les appels API vers des références externes seront maintenant configurables via le Workflow.</td></tr><tr><td>5</td><td>Google OCR est utilisable lors de l'inférence pour la classification.</td></tr><tr><td>6</td><td>Nouveau champs <code>callback_custom_headers</code> dans la route pour uploader un document en production. Ce champs permet d'ajouter des key:valeur au header du callback.</td></tr><tr><td>7</td><td>Le paramètre "Supprimer les documents après traitement" dans la configuration d'un agent est désormais activée par défaut.</td></tr><tr><td>8</td><td>Amélioration de l'interface pour ajouter des datasets dans l'entrainement d'un modèle de classification.</td></tr><tr><td>9</td><td>Ajout de l'option "Etiquette en colonne" dans la création des étiquettes dans les datasets. L'activation de cette option permet à l'extracteur de comprendre que ce champs fait partie d'un tableau, et permet de mieux gérer les extraction partielles. </td></tr><tr><td>10</td><td>Homogénéisation des webhook/callback sur Classify. Plus besoin d'enregistrer un webhook au préalable.</td></tr><tr><td>11</td><td>Les fichier .doc sont maintenant supportés pour la classification</td></tr><tr><td>12</td><td>La connexion à une boite mail pour la classification et/ou l'extraction est maintenant géré depuis l'écran de Workflow (Input).</td></tr><tr><td>13</td><td>Le nom des modèles sont maintenant affichés dans l'écran des extracteurs (Agent d'extraction).</td></tr><tr><td>14</td><td>Nouvelle option dans la configuration d'un extracteur "Bloc" permettant de construire un bloc sur du multi-page.</td></tr></tbody></table>

## Bugs corrigés

<table><thead><tr><th width="71">#</th><th width="548">Description</th></tr></thead><tbody><tr><td>1</td><td>Temps de chargement pour la navigation sur les documents en correction.</td></tr><tr><td>2</td><td>Le paramètre de la langue pour la normalisation est maintenant pis en compte lors de la duplication d'agent d'extraction.</td></tr><tr><td>3</td><td>Split and Merge (le déliassage de document) fonctionne désormais avec tous les types de documents supporté par la plateforme</td></tr><tr><td>4</td><td>Empêche l'utilisateur d'envoyer son modèle en entrainement plusieurs fois (en cliquant plusieurs fois).</td></tr><tr><td>5</td><td>Copier une page avec les annotations depuis le vidéo-codage vers un dataset est maintenant fonctionnel.</td></tr><tr><td>6</td><td>La suppression des labels ajoutés dans la création d'un extracteur de groupe est maintenant fonctionnelle.</td></tr><tr><td>7</td><td>Les documents en erreur dans un dataset n'empêchent plus l'entrainement d'un modèle de classification (et sont ignorés).</td></tr><tr><td>8</td><td>Le compteur de document en review a été corrigé</td></tr></tbody></table>

## Sécurité

Diverses vulnérabilités ont été patchées.
