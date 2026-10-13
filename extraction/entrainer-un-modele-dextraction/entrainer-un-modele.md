# Entraîner un modèle

## Lancer l'entraînement

Une fois les documents annotés, passons à la phase d'entraînement.

<figure><img src="../../.gitbook/assets/image (108).png" alt="Bouton Entrainer un modèle d'extraction"><figcaption><p>Lancement de l'entraînement d'un modèle d'extraction</p></figcaption></figure>

En haut à droite de l'écran, cliquez sur le bouton `Entrainer un modèle d'extraction`.

Donnez un nom au nouveau modèle ou sélectionnez un modèle précédemment entraîné pour créer une nouvelle version. Lancez l'entraînement.

<figure><img src="../../.gitbook/assets/image (109).png" alt="Sélection du modèle d'extraction à entraîner"><figcaption><p>Sélection du modèle d'extraction à entraîner</p></figcaption></figure>

### Choisir les annotations utilisées pour l’entraînement <a href="#choisir-les-annotations-utilisees-pour-lentrainement" id="choisir-les-annotations-utilisees-pour-lentrainement"></a>

Pour un Dataset pré-annoté, le formulaire de lancement propose deux choix :

| Option | Par défaut | Effet |
| --- | --- | --- |
| `Utiliser les annotations À valider` | Cochée | Inclut les propositions localisées, sans exiger leur validation préalable. Décochez-la pour les exclure. |
| `Utiliser les annotations À terminer` | Décochée | Exclut ces propositions par défaut. Si vous la cochez, un avertissement rappelle qu’elles peuvent ne pas avoir de position exacte sur le document. |

Ces choix filtrent les **annotations**, pas les documents entiers. Les annotations validées restent incluses. Une proposition sans texte localisé n’est pas utilisée pour entraîner un modèle d’extraction de texte, même si l’option est cochée ou si elle a été marquée comme validée. Complétez d’abord sa sélection sur le document.

La validation de toutes les propositions n’est donc pas obligatoire pour lancer l’entraînement, mais leur vérification reste essentielle à la qualité du modèle. Voir [Vérifier, corriger, compléter et valider les pré-annotations](annoter-un-dataset.md#verifier-corriger-completer-et-valider).

Chaque entraînement prend entre 30 min et 2 h en fonction du nombre de documents utilisés.

## Interpréter les résultats

Une fois l'entraînement du modèle terminé, celui-ci apparaît dans la liste des modèles d'extraction.

Lors de l'entraînement des modèles, 20 % des documents de chaque classe sont mis de côté afin d'évaluer les performances du modèle une fois l'entraînement terminé. Ce sont ces scores que l'on retrouve dans ce tableau.

<figure><img src="../../.gitbook/assets/image (38).png" alt="Modèles d'extraction entraînés"><figcaption><p>Modèles d'extraction entraînés</p></figcaption></figure>

On retrouve le rappel, la précision et le score F1 du modèle d'extraction, ainsi que le détail pour chaque label.

Pour plus d'informations sur les métriques d'évaluation, consultez la [page dédiée aux métriques d'évaluation](../../autres/metriques-devaluation.md).
