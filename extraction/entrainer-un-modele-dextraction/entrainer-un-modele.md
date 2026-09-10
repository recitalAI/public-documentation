# Entraîner un modèle

## Lancer l'entraînement

Une fois les documents annotés, passons à la phase d'entraînement.

<figure><img src="../../.gitbook/assets/image (108).png" alt="Bouton Entrainer un modèle d'extraction"><figcaption><p>Lancement de l'entraînement d'un modèle d'extraction</p></figcaption></figure>

En haut à droite de l'écran, cliquez sur le bouton `Entrainer un modèle d'extraction`.

Donnez un nom au nouveau modèle ou sélectionnez un modèle précédemment entraîné pour créer une nouvelle version. Lancez l'entraînement.

<figure><img src="../../.gitbook/assets/image (109).png" alt="Sélection du modèle d'extraction à entraîner"><figcaption><p>Sélection du modèle d'extraction à entraîner</p></figcaption></figure>

Chaque entraînement prend entre 30 min et 2 h en fonction du nombre de documents utilisés.

## Interpréter les résultats

Une fois l'entraînement du modèle terminé, celui-ci apparaît dans la liste des modèles d'extraction.

Lors de l'entraînement des modèles, 20 % des documents de chaque classe sont mis de côté afin d'évaluer les performances du modèle une fois l'entraînement terminé. Ce sont ces scores que l'on retrouve dans ce tableau.

<figure><img src="../../.gitbook/assets/image (38).png" alt="Modèles d'extraction entraînés"><figcaption><p>Modèles d'extraction entraînés</p></figcaption></figure>

On retrouve le rappel, la précision et le score F1 du modèle d'extraction, ainsi que le détail pour chaque label.

Pour plus d'informations sur les métriques d'évaluation, consultez la [page dédiée aux métriques d'évaluation](../../autres/metriques-devaluation.md).
