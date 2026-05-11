# Entraîner un modèle

## Lancer l'entraînement

Une fois les documents annotés, passons à la phase d'entraînement.

<figure><img src="../../.gitbook/assets/image (108).png" alt=""><figcaption></figcaption></figure>

En haut à droite de l'écran, cliquez sur le bouton `Entrainer un modèle d'extraction`.

Donnez un nom au nouveau modèle, ou sélectionnez un modèle précédemment entraîné pour créer une nouvelle version.

Lancez l'entraînement.&#x20;

<figure><img src="../../.gitbook/assets/image (109).png" alt=""><figcaption></figcaption></figure>

Chaque entraînement prend entre 30min et 2h en fonction du nombre de documents utilisés.

## Interpréter les résultats

Une fois que l'entraînement du modèle est terminé, on le retrouvera dans la liste des modèles d'extraction.&#x20;

Lors de l'entraînement des modèles, 20 % des documents de chaque classe sont mis de côté afin d'évaluer les performances du modèle une fois l'entraînement terminé. Ce sont ces scores que l'on retrouve dans ce tableau.

<figure><img src="../../.gitbook/assets/image (38).png" alt=""><figcaption><p>modèles d'extraction entrainés</p></figcaption></figure>

On retrouve le rappel, la précision et le score f1 du modèle d'extraction, ainsi que le détail pour chaque label. Pour plus d'informations sur les métriques d'évaluation, nous avons une [page dédiée](../../autres/metriques-devaluation.md).
