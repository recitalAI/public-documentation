---
description: >-
  Une fois les documents rassemblés, nous pouvons passer à la phase suivante :
  l'annotation.
---

# Annoter un Dataset

## Configurer les étiquettes

Avant de pouvoir annoter, il est nécessaire de configurer les étiquettes (labels) qui seront entraînées.

Pour ce faire, cliquez sur l'onglet `Etiquettes`, en haut à droite.

<figure><img src="../../.gitbook/assets/image (43).png" alt=""><figcaption><p>Gestion des labels d'un dataset</p></figcaption></figure>

Saisir le nom du label, puis cliquez sur `AJOUTER`.

<figure><img src="../../.gitbook/assets/image (43).png" alt=""><figcaption><p>Gestion des labels d'un dataset</p></figcaption></figure>

{% hint style="info" %}
Une bonne pratique pour les tableaux peut être d'ajouter un préfixe "LIGNE\_" ou "ITEM\_" avant le nom du champ (par exemple, LIGNE\_DESIGNATION, LIGNE\_QUANTITE, LIGNE\_PRIX\_HT). Cela permettra de les identifier plus facilement lors de la configuration de l'Agent d'extraction.
{% endhint %}

**Autoriser les sauts de ligne:** Désactiver l'option pour les champs ne pouvant pas être sur plusieurs lignes (eg. un montant).

**Etiquette en colonne**: Activer l'option permet de faire en sorte qu'un champ numérique ne soit pas extrait partiellement (par exemple, "1 035 684" -> "1 035").

Réitérer pour toutes les étiquettes à ajouter.

## Configurer les paramètres du Dataset

Vous pouvez modifier les paramètres définis lors de la création du dataset en accédant à l'onglet `Configuration`.

<figure><img src="../../.gitbook/assets/image (44).png" alt=""><figcaption><p>Configuration d'un Dataset</p></figcaption></figure>

## Annoter les documents

Revenez sur l'onglet Document. Pour annoter les documents, cliquez sur `Annoter` ou ouvrez simplement un document en cliquant dessus.

### Principes d'annotation

Afin d'annoter correctement, il est indispensable de suivre les deux principes d'annotation suivants.

1.  **Annotation exhaustive** : Il est essentiel d'annoter de manière exhaustive chaque information pertinente sur le document, et ce à chaque occurrence. L'objectif est de montrer explicitement au modèle où et comment localiser l'information, indépendamment des pratiques usuelles ou de l'intuition des utilisateurs.

    _Exemple: Si le numéro SIREN de l'émetteur apparaît à la fois dans l'entête et le pied de page d'un document, il doit être annoté dans ces deux sections pour assurer une extraction complète et précise._
2. **Annotation cohérente** : Il est crucial que chaque information soit annotée de manière uniforme sur tous les documents similaires. Cela évite d'introduire des ambiguïtés qui pourraient désorienter le modèle.\
   \&#xNAN;_Exemple: Si un fournisseur place systématiquement le numéro de facture à un emplacement inattendu, par exemple après un libellé "numéro de commande", il est important de continuer à récupérer l'information à cet emplacement spécifique pour toutes les factures de ce fournisseur. Bien que non conventionnel, cet emplacement reste constant et doit être respecté pour garantir la précision de l'extraction des données._

Respectez ces deux principes lors de l'annotation.

### Annotation

Annotez le document en cliquant sur le stylo situé à droite de l'étiquette et sélectionnez dans le document la valeur que vous souhaitez annoter. Répétez le processus pour toutes les étiquettes, puis passez au document suivant.

Toutes les fonctionnalités liées à l'annotation sont répertoriées sur [une page dédiée](../../autres/astuces-dannotation.md).

<figure><img src="../../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>
