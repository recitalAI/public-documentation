---
description: Ajout de fonctionnalités, Corrections de bugs
---

# Version 24.1.x (12/01/2024)

### Fonctionnalités ajoutées

<table><thead><tr><th width="71">#</th><th width="548">Description</th></tr></thead><tbody><tr><td>1</td><td>Ajout d'une option multipage pour l'aggregateur "Block". Cette fonctionnalité est particulièrement utile lorsqu'un block de datapoint commence sur une page et se termine sur la suivante. </td></tr><tr><td>2</td><td>Un message d'avertissement apparait lors d'une copie d'une page ou d'un document d'un doctype vers un dataset si ces 2 derniers ont des configurations différentes impactantes.</td></tr><tr><td>3</td><td>L'utilisateur peut désormais pré-annoter un dataset avec des modèles sur étagère. </td></tr></tbody></table>

### Bugs corrigés, modifications

<table><thead><tr><th width="71">#</th><th width="548">Description</th></tr></thead><tbody><tr><td>1</td><td>Corrections de certaines traductions en Français. </td></tr><tr><td>2</td><td>Lors d'une pré-annotation, les nouvelles annotations utilisent l'ordre de lecture choisi sur le dataset.</td></tr><tr><td>3</td><td>Amélioration de l'agrégateur "Ligne Divisée": Les sous-groupes qui étaient traversés par une extracteur primaire d'un autre groupe étaient auparavant éliminés. Désormais, les entités du sous-groupe situées à gauche de l'entité externe sont conservées.</td></tr><tr><td>4</td><td>Correction dans le calcul du nombre de documents validés, valides, et documents OK.</td></tr></tbody></table>

### Fonctionnalités ajoutées (Classify)

<table><thead><tr><th width="71">#</th><th width="548">Description</th></tr></thead><tbody><tr><td>1</td><td>Possibilité de définir et modifier un seuil de confiance pour chaque modèle. Si la prédiction avec la plus grande probabilité est inférieur à ce seuil, la prédiction sera "Unknown".</td></tr></tbody></table>
