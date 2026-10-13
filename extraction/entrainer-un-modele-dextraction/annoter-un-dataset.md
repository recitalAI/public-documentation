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

## Pré-annoter un Dataset avec un Agent génératif <a href="#pre-annoter-un-dataset-avec-un-agent-generatif" id="pre-annoter-un-dataset-avec-un-agent-generatif"></a>

**Oui, vous pouvez fournir des documents à reciTAL pour que l’IA propose leurs annotations.** En version 26.8.28, la pré-annotation peut utiliser un **Agent d’extraction génératif**, qui extrait les champs à l’aide d’un modèle de langage (LLM). Elle ne nécessite ni modèle extractif préalablement entraîné ni documents déjà annotés manuellement.

L’annotation manuelle consiste à sélectionner vous-même les valeurs. La pré-annotation propose des valeurs à vérifier et à compléter : l’onglet `Modèles` utilise un modèle d’extraction entraîné ou sur étagère ; l’onglet `Agents d'extraction` utilise les extracteurs d’un Agent, y compris génératifs, non génératifs ou mixtes.

### Prérequis et correspondance des étiquettes

* Importez vos documents dans un Dataset et attendez la fin de leur traitement.
* Disposez d’un Agent d’extraction actif, avec ses champs génératifs, ses instructions d’extraction et son LLM configurés. Voir [Créer un Agent génératif](../../demarrage-rapide/creer-un-agent-generatif.md). Aucune modification spécifique du prompt n’est nécessaire pour lancer la pré-annotation.
* Faites correspondre vos étiquettes aux noms des champs de l’Agent : les propositions utilisent ces noms **en majuscules** (pour un groupe, les noms de ses champs). Les étiquettes absentes du Dataset sont ajoutées à partir des résultats retournés. Une étiquette d’un autre nom n’est pas automatiquement associée au champ.

### Lancer la pré-annotation et gérer les annotations existantes

1. Dans Studio, ouvrez le Dataset, puis l’onglet `Docs`.
2. Sélectionnez les documents à traiter, ou ne sélectionnez rien pour traiter le Dataset entier. Seuls les documents prêts sont retenus dans une sélection ; les filtres d’affichage seuls ne limitent pas le traitement à ces documents.
3. Ouvrez le menu `Annoter` → `Pré-annoter (automatique)` (ou l’action `Pré-annoter (N)` pour une sélection), puis l’onglet `Agents d'extraction`. Sélectionnez votre Agent. Pour utiliser un modèle, choisissez plutôt l’onglet `Modèles` et sa version.
4. Avant de lancer, choisissez la `Gestion des conflits` :

| Choix | Effet pour un document et une étiquette |
| --- | --- |
| `Toujours remplacer` **(par défaut)** | Remplace les annotations existantes des champs retournés, y compris celles déjà validées. |
| `Remplacer si vide` | Ajoute des propositions seulement si aucune annotation n’existe pour cette étiquette dans le document. Une annotation existante, même vide ou validée, est conservée. |
| `Toujours conserver` | Conserve les annotations existantes et ajoute les propositions qui ne recouvrent pas une annotation existante. |

5. Cliquez sur `Lancer la pré-annotation`. `Annuler` ferme la fenêtre avant le lancement ; cette fenêtre ne propose pas d’annuler un traitement déjà lancé.
6. Attendez la fin du traitement dans la liste des documents. Les documents en cours affichent un indicateur de chargement, puis redeviennent ouvrables. Un indicateur d’erreur signale un document en échec : ne le considérez pas comme pré-annoté avec succès.

L’opération ajoute ou remplace les annotations **dans le Dataset existant**, sans créer de copie. Un traitement terminé ne garantit pas que tous les champs ont été trouvés.

### Comprendre les statuts des annotations

| Statut affiché | Identifiant | Signification |
| --- | --- | --- |
| `À valider` | `to_validate` | Proposition localisée sur le document, dont la valeur et la position restent à vérifier. |
| `À terminer` | `to_complete` | Valeur proposée sans position identifiée : sélectionnez le texte correspondant sur le document. |
| `Validées` | `validated` | Annotation manuelle ou proposition marquée comme validée. Ce statut ne prouve pas à lui seul sa justesse ni sa localisation. |

### Vérifier, corriger, compléter et valider <a href="#verifier-corriger-completer-et-valider" id="verifier-corriger-completer-et-valider"></a>

Ouvrez un document depuis l’onglet `Docs`. Les propositions sont regroupées par étiquette et par statut.

* **À valider** : comparez la valeur et sa position au document, puis cliquez sur la coche `Valider l'annotation` si elles sont correctes. Supprimez une proposition erronée avec sa croix.
* **À terminer** : cliquez sur le stylo `Terminer l'annotation`, puis sélectionnez la valeur correcte sur la bonne page. La sélection remplace la proposition par une annotation manuelle validée. Si la valeur n’existe pas dans le document, supprimez la proposition.
* **Valeurs manquantes ou à corriger** : après avoir traité les propositions de l’étiquette, utilisez son stylo pour annoter les valeurs correctes et les occurrences manquantes. Le stylo de l’étiquette n’est pas affiché tant qu’elle contient des propositions non validées, sauf pendant une annotation en cours.

Vérifiez **chaque occurrence pertinente**, même si l’IA en a déjà proposé une : les principes d’annotation exhaustive et cohérente restent applicables.

Pour valider plusieurs propositions déjà vérifiées :

* Dans le document, `Tout valider` marque les annotations `À valider` du document comme validées ; le même bouton dans un groupe d’étiquette se limite à cette étiquette.
* Depuis `Docs`, ouvrez `Annoter` → `Valider automatiquement`. `Annotations À valider` est cochée par défaut ; `Annotations À terminer` ne l’est pas. Confirmez avec `Valider automatiquement`. Cette action concerne **tout le Dataset**, même si des documents sont sélectionnés.

{% hint style="warning" %}
Valider, individuellement ou en masse, change un statut : cela ne vérifie pas les valeurs et ne localise pas les propositions `À terminer`. Contrôlez et complétez les annotations avant de vous y fier.
{% endhint %}

### Utiliser les annotations pour l’entraînement

L’entraînement peut utiliser des propositions non validées : par défaut, les annotations `À valider` sont incluses et les annotations `À terminer` sont exclues. Vous pouvez modifier ces choix au lancement. Une proposition sans texte localisé n’est toutefois pas utilisable pour entraîner un modèle d’extraction de texte, même si vous l’incluez ou la marquez comme validée : terminez sa sélection sur le document. Voir [Choisir les annotations utilisées pour l’entraînement](entrainer-un-modele.md#choisir-les-annotations-utilisees-pour-lentrainement).

**TL;DR :** importez vos documents, choisissez un Agent génératif et le mode de remplacement, puis lancez la pré-annotation. Vérifiez les valeurs et toutes leurs occurrences, complétez leur localisation et choisissez les annotations à utiliser pour l’entraînement. Aucun modèle extractif entraîné ni annotation manuelle initiale n’est requis.
