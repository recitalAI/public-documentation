# Charger des documents en validation

Avant de démarrer la validation, il faut s’assurer d’avoir un jeu de données de test :

* **Indépendant** : il ne doit pas faire partie de l’entraînement
* **Représentatif** : il doit reprendre un panel de documents tels que présents dans le Dataset d'entraînement. Si suffisamment de formats différents ont été vus à l'entraînement, le modèle devrait être en capacité de généraliser à des formats non-vus. Il est possible d'en inclure quelques-uns pour mesurer la capacité du modèle à généraliser.&#x20;

Avant de charger un fichier, consultez les [formats acceptés par un Agent d'extraction](../configurer-un-agent-dextraction/README.md#formats-des-documents-traites-par-un-agent-dextraction). Le contenu du fichier doit correspondre à un format accepté ; changer son extension ne suffit pas.

Cliquez sur "Ajouter des documents", puis chargez autant de documents que souhaité. Lors du chargement, les documents sont analysés, et une extraction est effectuée sur l'ensemble des extracteurs définis. (cf. [Configurer les extracteurs d'un Agent](../configurer-un-agent-dextraction/configurer-les-extracteurs-dun-agent.md)).
