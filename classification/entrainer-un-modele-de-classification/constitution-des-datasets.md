# Constitution des datasets

Chez reciTAL, la notion de "Dataset" fait référence à une collection de documents de même nature. Pour la classification, il faudra donc créer autant de datasets qu'il y a de classes différentes.

{% hint style="info" %}
Quelques conseils pour constituer correctement un dataset de classification:

* Miser sur la qualité plutôt que sur la quantité. Commencer par une 50aine de documents par dataset, et s'assurer que ces documents soient dans la bonne classe.
* Ne pas confondre nature de document et usage métier. Par exemple une CNI et un passeport devraient être dans 2 datasets séparés. Idem pour les justificatif de domicile par exemple (facture d'électricité, de gaz, de téléphone, ...)
* Ne pas inclure de classe "Photo". Nos modèles de classification se basent sur le contenu textuel d'un document. Ils ne sont donc pas en mesure de prédire correctement si document est une photo. D'autres outils sont à disposition pour repérer les photos, contacter l'équipe projet.
{% endhint %}

## Création des dataset de documents

{% content-ref url="../../extraction/entrainer-un-modele-dextraction/constituer-un-dataset.md" %}
[constituer-un-dataset.md](../../extraction/entrainer-un-modele-dextraction/constituer-un-dataset.md)
{% endcontent-ref %}

## Création des dataset de mails

La constitution d'un dataset de mails suit le même principe : un dataset par classe, regroupant des emails d'exemple représentatifs. Les fichiers attendus sont des `.eml` et/ou des `.msg`.
