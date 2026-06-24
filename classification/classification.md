# 🗃️ Classification

## Qu’est-ce que la classification ?

La classification est une tâche d’intelligence artificielle dont l’objet est d’attribuer une classe (ou catégorie / label / étiquette / tag) à un objet, ici un mail ou un document. Dans un centre de relation client par exemple, les mails reçus peuvent appartenir à différentes catégories, comme “Réclamation”, “Demande de remboursement”, “Modification des coordonnées”, “Question sur le produit” etc. De la même manière, un courtier d’assurances peut recevoir des documents appartenant à l’une des catégories “Extrait KBis”, “Carte d’identité”, “Bulletin de souscription”, “Déclaration de sinistre” etc. La tâche de classification consiste à attribuer automatiquement la catégorie correcte à un document ou un mail.

## Qu’est-ce qu’un classifieur ?

Un classifieur est un modèle d’intelligence artificielle entraîné à réaliser une tâche de classification.

## Sur quelles informations le classifieur base-t-il sa prédiction ?

### Mails

Dans le cas d’un mail, le classifieur analyse le contenu du message (sujet et corps) et, le cas échéant, ses pièces jointes.

### Documents

Dans le cas d’un document, le classifieur analyse le texte du document (océrisé) pour réaliser sa prédiction.

## Le déliassage de documents

Dans certains cas, les documents sont reçus sous forme de liasses. C'est à dire que plusieurs sous-documents compose le fichier à analyser. Il est possible de se servir de la classification de document (page par page) afin de séparer la liasse aux bons endroits.
