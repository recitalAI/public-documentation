# Review

L'écran **Review** liste tous les documents et emails qui nécessitent une correction humaine avant validation finale, regroupés par agent. C'est ici que s'effectue la **correction** des données extraites par les modèles en production.

## Accès

Dans la sidebar, cliquer sur **Review**. L'écran est segmenté en trois onglets correspondant aux trois types de tâches :

- **Extraction** — documents passés par un agent d'extraction avec des champs à valider
- **Classification** — documents dont la classe prédite est en revue
- **Emails** — emails à classifier

<figure><img src="../.gitbook/assets/production_review_list.png" alt="Liste des agents d'extraction avec documents à reviewer"><figcaption>Vue Review &#x2014; tab Extraction. Chaque ligne représente un agent et le nombre de documents en attente.</figcaption></figure>

Le badge sur chaque onglet indique le nombre total de documents en attente pour le type concerné. Le champ de recherche en haut filtre les agents par nom.

## Corriger un document

L'écran de correction présente tous les documents extraits par des modèles en production et envoyés en correction.

![Liste des bannières de correction](<../.gitbook/assets/corrections_1 (1).png>)

Cliquez sur le nom de l'Agent pour accéder aux documents à corriger.

![corrections\_2.png](../.gitbook/assets/corrections_2.png)

Cliquez sur le nom d'un document pour rentrer en mode Correction.

<figure><img src="../.gitbook/assets/2024_06_07_11_59_36_Window (1).png" alt=""><figcaption></figcaption></figure>

Si le document ne convient pas ou est hors scope, il est possible de le rejeter.

![corrections\_8.png](<../.gitbook/assets/corrections_8 (1).png>)

Pour valider un champ, cliquer sur la coche « correcte ».

Pour corriger un champ, cliquez sur la valeur puis entourez dans le document la bonne valeur.

Une fois tous les data points validés ou corrigés, cliquer sur **valider**.

![corrections\_6.png](<../.gitbook/assets/corrections_6 (1).png>)

## Ajouter un document au dataset

Si un document présente trop d'erreurs car le modèle n'a pas été suffisamment entraîné sur ce format, il est possible de l'ajouter au dataset pour enrichir l'entraînement futur. Pour ce faire, cliquez sur le bouton **« Copier dans le dataset »**.

![corrections\_9.png](../.gitbook/assets/corrections_9.png)

Puis sélectionnez le dataset et cliquer sur confirmer.

![corrections\_10.png](../.gitbook/assets/corrections_10.png)

## Permissions

Seuls les utilisateurs ayant le rôle approprié peuvent accéder à Review. La gestion des rôles se fait depuis [Paramètres](parametres.md).
