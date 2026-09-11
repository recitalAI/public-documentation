# Astuces d'annotation

## La pré-annotation

La pré-annotation consiste à intégrer des étiquettes et des annotations en fonction d’un modèle existant. Elle reprend les étiquettes du modèle et les utilise pour déduire les textes à annoter. Elle est disponible lorsque des modèles ont déjà été entraînés, ou avec les modèles sur étagère.

{% hint style="warning" %}
La pré-annotation utilise un modèle d’intelligence artificielle et peut présenter des erreurs. Il est indispensable de vérifier l’exactitude des annotations et de les corriger si besoin est.
{% endhint %}

Pour pré-annoter le Dataset:&#x20;

* Cliquez sur "Pré-annoter".
* Sélectionnez le modèle souhaité et cliquez sur "Confirmer". Le modèle importe les étiquettes et annote les documents.
* Une fois le traitement terminé, les documents sont prêts et pré-annotés.

Il faut alors vérifier et compléter précautionneusement les annotations.

## Raccourcis d'annotation

Différents contrôles sont présents afin de fluidifier la phase d'annotation:

{% hint style="info" %}
Le bouton "Clavier" en bas à droite permet d’afficher tous les raccourcis d’annotation
{% endhint %}

1. **Naviguer entre les documents**
   * **Ctrl + Entrer** pour passer au document suivant.
   * La liste déroulante avec le nom du document permet de choisir un document à ouvrir.
   * La liste déroulante du dessus permet de filtrer les documents déjà annotés pour l'étiquette choisie.
2. **Naviguer entre les pages d'un document**
   * **Tab** ou **Ctrl + Bas** pour passer à la page suivante.
   * Le bouton **Recherche** permet de trier les pages contenant un mot ou une expression.
3. **Naviguer entre les étiquettes**
   * Les flèches du clavier permettent de passer aux étiquettes suivantes ou précédentes.
   * Il est possible de réordonner les étiquettes en les déplaçant (drag & drop).
   * Il est possible de créer une nouvelle étiquette directement depuis cette interface ; elle sera prise en compte pour l'ensemble des documents.
4. **Les contrôles d’annotation**
   * Le bouton **Entrer** permet de commencer / arrêter une annotation.
   * Les quatre boutons en haut à droite permettent de :
     * **Sélectionner le texte détecté** : Option par défaut. On annote ce que l'on encadre.
     * **Fractionner la sélection en ligne** : Permet d'annoter toute une colonne d'un tableau en une seule sélection. Les lignes seront séparées en plusieurs occurrences.
     * **Sélectionner du texte au même endroit sur d'autres pages** : Particulièrement utile lorsqu'un en-tête ou pied de page se répète tout au long d'un document.
     * **Annoter toutes les occurrences du texte** : Permet d'annoter en une seule sélection un champ qui se répète dans le document (par exemple Nom Prénom).
   * **Ctrl + Z** permet d'annuler la dernière annotation.
   * **Ctrl + Y** permet de rejouer la dernière annotation annulée.
5. **Les contrôles d’affichage**
   * Zoomer ou dézoomer : **Ctrl + Scroll** ou boutons de zoom.
   * Pivoter un document.
