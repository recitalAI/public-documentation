---
description: >-
  La première tâche à réaliser lorsque l'on souhaite automatiser un flux
  documentaire est la constitution du Dataset.
---

# Constituer un Dataset

## Conseils sur la constitution d'un Dataset

Le Dataset (ou jeu de données) est l'ensemble des documents métier utilisés pour l'apprentissage du modèle. Ces documents seront utilisés uniquement pour cette phase d'apprentissage et ne devront pas servir de jeu de test.

{% hint style="info" %}
Le Dataset doit être le plus représentatif possible du flux réel : plus le Dataset est représentatif, plus le modèle sera performant lors de la phase de production.
{% endhint %}

Selon la complexité du type de document, un Dataset doit contenir un certain nombre de documents. Nos recommandations sont les suivantes :

* Documents structurés : 30 à 50 documents
* Documents non structurés « simples » : 100 documents
* Documents non structurés « complexes » : 100 à 300 documents

## Créer un nouveau Dataset

<figure><img src="../../.gitbook/assets/image (36).png" alt="Création d'un nouveau Dataset"><figcaption><p>Création d'un nouveau Dataset</p></figcaption></figure>

Donnez un nom à votre Dataset. Plusieurs options s'offrent ensuite à vous :

* **Force OCR :** si l'option est activée, l'OCR se fera même si votre document contient du texte sélectionnable. Cette option est recommandée : Force OCR uniformise l'ordre de lecture des mots, y compris pour les documents contenant déjà du texte sélectionnable.
* **Pivoter automatiquement les pages** : redresse les pages retournées à 90°, 180° ou 270°. Nécessite l'activation de Google OCR (Paramètres généraux de l'organisation) et de Force OCR.
* **Redresser les documents de travers** : redresse les documents scannés ou pris en photo avec une légère rotation.
* **Utiliser les tableaux** : détecte les tableaux dans un document et modifie l'ordre de lecture en fonction de ces derniers (la lecture des mots cellule par cellule est forcée). Cette option est nécessaire pour certaines méthodes d'agrégation lors de la configuration d'un Extracteur (voir [Configurer les extracteurs d'un Agent](../configurer-un-agent-dextraction/configurer-les-extracteurs-dun-agent.md)).
* **Afficher les étiquettes 'emplacement' et 'document'** : lors de la phase d'annotation, les « emplacements » permettent d'annoter des éléments autres que du texte (par exemple, des signatures, des images, etc.). Le champ « document » permet d'ajouter du texte libre pour chaque document.
* **Personnaliser l'ordre de Lecture :**
  * **Tableaux** : ordre de lecture conçu pour lire des documents contenant des tableaux ou des factures.
  * **Par défaut** : ordre de lecture renvoyé par l'OCR.
  * **Mots** : ordre de lecture conçu pour lire un document de gauche à droite puis de haut en bas, indépendamment des structures internes du document (paragraphes, sections, etc.).
