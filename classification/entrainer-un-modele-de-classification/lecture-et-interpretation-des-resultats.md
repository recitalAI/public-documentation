---
description: >-
  Cliquez sur votre modèle de classification pour voir les résultats de
  l'entraînement.
---

# Lecture et interprétation des résultats

<figure><img src="../../.gitbook/assets/image (110).png" alt="Résultats de l'entraînement d'un modèle de classification"><figcaption><p>Résultats de l'entraînement d'un modèle de classification</p></figcaption></figure>

Pour rappel, nos modèles (classification et extraction) sont entraînés sur 80 % des données initiales. 20 % des données sont mises de côté pour la validation. Les scores de performance affichés sont les scores de validation.

Le premier tableau indique, pour chaque catégorie, le nombre de documents d'entraînement (**Initial**) et le score obtenu (**Training score**).

On retrouve ensuite une **matrice de confusion** (en-tête **Expected / Predicted**) : les vraies catégories figurent en ligne (*Expected*) et les catégories prédites par le modèle en colonne (*Predicted*).

La performance d'un modèle de classification dépend principalement de deux facteurs :

* **Le nombre de classes** : plus le nombre de classes à discriminer est important, plus le modèle a de chances de se tromper.
* **La similarité entre plusieurs classes** : certaines classes trop proches les unes des autres (par exemple, une facture de location par rapport à une facture de vente) seront difficiles à discriminer par le modèle de classification. Une extraction de certains éléments clés du document peut permettre d'affiner une classification.
