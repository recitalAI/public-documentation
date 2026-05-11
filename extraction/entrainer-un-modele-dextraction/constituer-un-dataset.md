---
description: >-
  La première tâche à réaliser lorsque l'on souhaite automatiser un flux
  documentaire est la constitution du dataset.
---

# Constituer un Dataset

### Conseils sur la constitution d'un Dataset

Le dataset (ou jeu de données) est l'ensemble des documents métier utilisés pour l'apprentissage du modèle. Ces documents seront utilisés uniquement pour cette phase d'apprentissage et ne devront pas servir de jeu de test.

{% hint style="info" %}
Le dataset doit être le plus représentatif possible du flux réel : plus le dataset est représentatif, plus le modèle sera performant lors de la phase de production.
{% endhint %}

Selon la complexité du type de document, un dataset doit contenir un certain nombre de documents. Nos recommandations sont les suivantes :&#x20;

* Documents structurés : 30 à 50 documents
* Documents non structurés "simples" : 100 documents
* Documents non structurés "complexes" : 100 à 300 documents

### Créer un nouveau Dataset

<figure><img src="../../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

Donner un nom à votre Dataset. Divers option s'offrent ensuite à vous:

* **Force OCR :** Si l'option est activée, l'OCR se fera même si votre document contient du texte sélectionnable. Nous conseillons vivement d'utiliser cette option car elle permet d'assurer une stabilité dans l'ordre de lecture des mots d'un document.
* **Pivoter automatiquement les pages** : Redresse les pages retournées à 90°, 180° ou 270°. Nécessite l'activation de Google OCR (Paramètres généraux de l'organisation) et de Force OCR.
* **Redresser les documents de travers** : Redresse les documents scannés ou pris en photo avec une légère rotation.
* **Utiliser les tableaux** : Détecte les tableaux dans un document et modifie l'ordre de lecture en fonction de ces derniers (La lecture des mots cellule par cellule est forcée). Cette option est nécessaire pour certaines méthodes d'agrégation lors de la configuration d'un Extracteur (voir [Configurer les extracteurs d'un Agent](../configurer-un-agent-dextraction/configurer-les-extracteurs-dun-agent.md)).
* **Afficher les étiquettes 'emplacement' et 'document'** : Lors de la phase d'annotation, les "emplacements" permettent d'annoter des éléments autre que du texte (par exemple les signatures, des images, etc.). Le champs "document" permet d'ajouter du texte libre pour chaque document.
* **Personnaliser l'ordre de Lecture :**&#x20;
  * **Tableaux** : Ordre de lecture conçu pour lire des documents contenant des tableaux ou factures.
  * **Par défaut** : Ordre de lecture renvoyé par l'OCR.
  * **Mots** : Ordre de lecture conçu pour lire un document de gauche à droite puis haut en bas, indépendamment des structures interne du document (paragraphes, sections, etc.).
