---
description: Rendez-vous dans l'onglet "Extracteurs".
---

# Configurer les extracteurs d'un Agent

{% hint style="info" %}
Si l'Agent a été créé à partir d'un modèle existant (custom ou sur étagère), tous les labels du modèle sont créés comme des champs uniques.
{% endhint %}

## Créer un nouveau champs unique

### Création

Un champ unique, comme son nom l'indique, ne sera extrait qu'une seule fois dans un document. Par exemple, sur une facture : la date d'émission, le montant HT ou le n° de facture.

<figure><img src="../../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>

Ajoutez un Datapoint, donnez un nom à votre nouveau champs, puis enregistrez.

### Configuration

<figure><img src="../../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>

#### Type de valeur

Le type de valeur est la nature du champs à extraire. Sélectionner le bon type de valeur permet de normaliser le champs extrait.

<table><thead><tr><th width="172">Type de valeur</th><th width="370">Détail</th><th>Exemples</th></tr></thead><tbody><tr><td>Tout</td><td>N'importe quelle chaîne de caractère, c'est la valeur par défaut</td><td>Nom, Prénom, Désignation</td></tr><tr><td>Date</td><td><p>Permet de normaliser une date au format </p><p>YYYY-MM-DD</p></td><td><p>Date d'émission, </p><p>Date d'expiration</p></td></tr><tr><td>Nombre Entier (integer)</td><td>Permet de normaliser un nombre entier</td><td><p>Nombre d'unité, </p><p>Age</p></td></tr><tr><td><p>Nombre Décimal </p><p>(float)</p></td><td>Permet de normaliser un nombre décimal</td><td>Montant HT, Pourcentage, Volume,</td></tr><tr><td>Personnalisé</td><td>Utilise les expressions régulières pour normaliser un champ extrait. <br>La première expression doit correspondre au champ extrait pour permettre sa normalisation.<br>Dans la deuxième expression, il est possible de réutiliser les groupes capturés dans la première expression (\1, \2, ...) afin de réaliser la normalisation.</td><td><p>Numéro de téléphone, Référence client, </p><p>Code barre</p></td></tr></tbody></table>



#### **Méthode d'extraction**

C'est la façon dont le champ sera extrait dans le document.&#x20;

La principale méthode d'extraction est depuis un modèle entraîné. Sélectionnez le modèle et sa version, puis sélectionnez le label du modèle correspondant.

{% hint style="info" %}
À noter que plusieurs modèles peuvent être utilisés pour des champs différents. Cela permet, par exemple, d'associer 2 labels à un même mot (ce qui est impossible avec un seul modèle). Cependant, lors d'une prédiction, chaque modèle sera appelé, ce qui augmentera le temps de traitement.

Par exemple, configurer un premier extracteur "Adresse", qui capture une adresse entière, et un deuxième extracteur "Code Postal" depuis un autre modèle. Ainsi, dans le document, le code postal aura à la fois le label "Code Postal" et "Adresse".
{% endhint %}

Il est également possible d'utiliser les expressions régulières pour extraire un champ dans le document. Pour cela, sélectionnez "Règles", saisissez la regex, puis vous avez l'option de délimiter une zone dans le document où chercher l'expression.

{% hint style="info" %}
**Nouvelle fonctionnalité**

Il est désormais possible de sélectionner "Génératif" comme méthode d'extraction.

Un agent génératif est créé dynamiquement à partir du nom et de la description de l'extracteur. Il est également possible d'ajouter une description au niveau de votre agent d'extraction.
{% endhint %}

## Créer un nouveau groupe de champs

### Création

Lorsqu’il s’agit de lignes d’un tableau par exemple, il convient de les mettre sous forme de groupe. Si vous avez créé un agent depuis un modèle, supprimez les champs concernés de la partie "Data point" et créez un groupe d’étiquettes.

<figure><img src="../../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

Rentrez le nom du groupe et choisissez le modèle d'extraction utilisé pour ce groupe.

### Configuration

#### Méthode d'agrégation

* **Ligne** : Les éléments sur une même ligne sont rassemblés en un groupe. Par exemple avec les factures pour rassembler dans un même groupe la désignation, le prix, la quantité, etc.
* **Ligne divisée** : Identique à ligne, mais permet d'avoir 2 groupes différents si 2 tableaux sont côte à côte sur la même ligne par exemple.
* **Colonne** : Les éléments sur une même colonne sont rassemblés en un groupe.
* **Bloc** : Les éléments successifs dans un document sont rassemblés en un groupe sans contrainte de ligne ou de colonne (seul l'ordre de lecture compte). Un nouveau groupe est créé à chaque nouvelle itération.
* **Cluster** : Permet de rapprocher des champs par proximité dans le document, sans tenir compte d'un alignement vertical ou horizontal. Par exemple pour les adresses (rue, code postal, ville)
* **Ligne de tableau** : Utilisation de la détection de tableau requis. Identique à LIGNE mais se base sur la reconnaissance de tableau.
* **Cellule de tableau** : Utilisation de la détection de tableau requis. Identique à COLONNE mais se base sur la reconnaissance de tableau.

<figure><img src="../../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

**Conserver les valeurs des X meilleurs pages :** Par défaut dans un groupe, toutes les occurrences du groupe (sous-groupe) seront extraites, mais il est possible de limiter les extractions seulement aux "meilleures" pages. Particulièrement utile si on sait par exemple que l'information n'est que sur 1 page. Si l'option est activée, alors on peut exclure lors du calcul des meilleures pages les pages contenant une valeur spécifique.

**Créer des sous-groupes vides pour les étiquettes primaires sans valeur :** Utilisation avancée des groupes, permettant de créer autant de sous-groupe qu'il existe de champs primaires.

**Afficher les étiquettes primaires sans valeur** : Par défaut les champs d'un sous-groupe non-extrait n'apparaissent pas dans les résultats. Si cette option est activée, ils apparaitront avec la valeur "N/A". Cela permet d'avoir un squelette de JSON fixe lors de l'envoie des résultats.

**Afficher les groupes à valeur unique** : Par défaut un sous-groupe est créé si 2 champs ou plus sont présents. Si cette option est activée, un groupe est créé même avec 1 seul champ, si ce dernier est primaire.

#### Ajouter les champs à extraire dans le groupe

Ajoutez un par un les champs provenant du modèle à ajouter dans le groupe. Pour chaque champ, vous pouvez configurer un type de valeur (voir "[Créer un nouveau champ unique](configurer-les-extracteurs-dun-agent.md#creer-un-nouveau-champs-unique)"), et désigner si ce dernier est primaire ou non. Un champ primaire autorise la création d'un sous-groupe s'il est extrait. Le sous-groupe n'est pas créé si aucun champ primaire n'est extrait.
