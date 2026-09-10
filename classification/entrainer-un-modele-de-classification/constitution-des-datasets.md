# Constitution des Datasets

Chez reciTAL, la notion de « Dataset » fait référence à une collection de documents de même nature. Pour la classification, il faudra donc créer autant de Datasets qu'il y a de classes différentes.

{% hint style="info" %}
Quelques conseils pour constituer correctement un Dataset de classification :

* Miser sur la qualité plutôt que sur la quantité. Commencer par une cinquantaine de documents par Dataset et s'assurer que ces documents appartiennent à la bonne classe.
* Ne pas confondre nature du document et usage métier. Par exemple, une CNI et un passeport devraient être dans deux Datasets séparés. Il en va de même pour les justificatifs de domicile (facture d'électricité, de gaz, de téléphone, etc.).
* Ne pas inclure de classe « Photo ». Nos modèles de classification se basent sur le contenu textuel d'un document. Ils ne sont donc pas en mesure de prédire correctement si un document est une photo. D'autres outils sont à disposition pour repérer les photos : contactez l'équipe projet.
{% endhint %}

## Création des Datasets de documents

{% content-ref url="../../extraction/entrainer-un-modele-dextraction/constituer-un-dataset.md" %}
[Constituer un Dataset](../../extraction/entrainer-un-modele-dextraction/constituer-un-dataset.md)
{% endcontent-ref %}

## Création des Datasets d'e-mails

La constitution d'un Dataset d'e-mails suit le même principe : un Dataset par classe, regroupant des e-mails d'exemple représentatifs. Les fichiers attendus sont des `.eml` et/ou des `.msg`.
