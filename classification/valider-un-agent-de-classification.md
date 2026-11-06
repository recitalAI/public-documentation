---
description: Tester les modes Classification simple et Déliassage d'un Agent, puis vérifier et corriger les catégories prédites.
---

# Valider un Agent de classification

Valider un [Agent de classification](configurer-un-agent-de-classification.md) consiste ici à lui envoyer des documents de test, examiner les catégories prédites et corriger les résultats si nécessaire. L'onglet **Documents** de l'Agent affiche ces tests ; il ne s'agit pas de la liste des documents à traiter en Review de production.

{% hint style="warning" %}
**Documentation en cours de vérification — ne pas publier cette page en l'état.** Le parcours ci-dessous est vérifié dans le code de la version `26.8.38`, mais la version déployée sur l'environnement du client reste à confirmer. Les libellés et le parcours doivent également être contrôlés dans cette application avant publication.
{% endhint %}

## Accéder à l'onglet Documents

1. Dans **Agents**, ouvrez l'onglet **Classification**.
2. Ouvrez l'Agent de classification concerné.
3. Sélectionnez l'onglet **Documents**.

## Choisir entre Classification simple et Déliassage {#a-quoi-correspond-telecharger-un-bundle}

L'onglet **Documents** permet de tester deux modes : **Classification simple** et **Déliassage**. Ces termes décrivent les traitements ; les libellés actuels du menu **Télécharger des documents** sont respectivement **Télécharger un document** et **Télécharger un bundle**.

### Classification simple

La classification simple attribue une catégorie au fichier dans son ensemble. Utilisez ce mode lorsque le fichier représente un seul document, même s'il comporte plusieurs pages.

Pour lancer ce test, ouvrez **Télécharger des documents**, puis choisissez **Télécharger un document** et sélectionnez le fichier sur votre poste.

### Déliassage

Le déliassage traite une **liasse** : un fichier unique contenant plusieurs sous-documents, par exemple un PDF réunissant des factures et des devis. Chaque page est classée indépendamment, puis, par défaut, les pages consécutives portant la même catégorie sont regroupées dans un même sous-document. Une catégorie identique réapparaissant après une autre catégorie forme un nouveau groupe.

Par exemple, les pages **Facture, Facture, Devis, Facture** donnent trois sous-documents : les pages **1–2**, la page **3** et la page **4**. Le regroupement par catégorie ne distingue pas automatiquement deux sous-documents consécutifs de même catégorie ; vérifiez aussi les séparations entre leurs pages.

Pour lancer ce test, ouvrez **Télécharger des documents**, puis choisissez **Télécharger un bundle**, le libellé actuel correspondant à l'envoi d'une liasse.

### Libellé actuel : « Télécharger un bundle »

**Télécharger un bundle** sert à **envoyer une liasse vers l'application pour tester le déliassage**. Cette action ne télécharge aucun fichier sur votre poste et n'exporte ni archive ZIP, ni résultats, ni configuration de l'Agent. Les sous-documents sont des groupes de pages consultables dans l'application, pas des fichiers téléchargés par cette action.

Le sélecteur permet de choisir plusieurs fichiers, mais chacun est envoyé et traité séparément. Le déliassage ne sert donc pas à regrouper les fichiers sélectionnés dans une archive.

## Envoyer une liasse et retrouver ses résultats {#envoyer-un-bundle-et-retrouver-ses-resultats}

1. Dans l'onglet **Documents** de l'Agent, ouvrez **Télécharger des documents**, puis choisissez **Télécharger un bundle** pour tester le déliassage.
2. Sélectionnez le fichier à tester sur votre poste, par exemple un PDF multipage. Pour les formats pris en charge par l'Agent, consultez [les formats des documents traités](configurer-un-agent-de-classification.md#formats-des-documents-traites-par-un-agent-de-classification).
3. Retrouvez le fichier dans la liste **Documents** de cet Agent : la colonne **Type** indique **Bundle**. Les catégories prédites apparaissent dans **Prédiction** lorsque le traitement est terminé.
4. Ouvrez la ligne du fichier ou l'action **Réviser** pour consulter les pages et leurs catégories. Cette ouverture n'est disponible qu'après la fin du traitement.

Il n'y a donc aucun bundle téléchargé à chercher dans le dossier de téléchargements du navigateur : le résultat se consulte dans l'application.

## Examiner, corriger et soumettre les résultats

Après un test de classification simple ou de déliassage, attendez la fin du traitement puis ouvrez **Réviser** depuis l'onglet **Documents** de l'Agent.

1. Dans l'écran **Révision**, sélectionnez les pages pour les consulter dans la visionneuse et comparez leur contenu à la catégorie proposée.
2. Si nécessaire, changez la catégorie dans la liste déroulante du groupe. Pour un document simple, ce choix concerne tout le document ; pour une liasse, il concerne les pages du groupe, ou uniquement les pages sélectionnées si une sélection est active.
3. Pour une liasse, vérifiez aussi les groupes de pages correspondant aux sous-documents. L'action **Séparer toutes les pages en groupes individuels** permet de dissocier un groupe avant d'attribuer des catégories différentes à ses pages.
4. Désélectionnez les pages si nécessaire pour retrouver le bouton **Soumettre**, puis cliquez dessus. Si l'Agent exige une catégorie autre que **Unknown**, attribuez une catégorie aux pages ou au document encore classés **Unknown** pour pouvoir soumettre.
5. Après soumission, vous revenez à l'onglet **Documents** de l'Agent. Les catégories corrigées et la validation du document sont enregistrées.

Pour rejeter le fichier entier plutôt que valider ses catégories, désélectionnez toutes les pages, cliquez sur **Rejeter**, choisissez un motif puis confirmez avec **Rejeter** dans la boîte de dialogue. Vous revenez ensuite à la liste des documents.

**Soumettre** valide le résultat du document examiné ; cette action ne réentraîne pas le modèle et ne constitue pas une activation de l'Agent. Pour modifier le comportement de classification, consultez [la configuration de l'Agent](configurer-un-agent-de-classification.md) et, si nécessaire, [l'entraînement du modèle](entrainer-un-modele-de-classification/entrainement-du-modele.md).

## Voir aussi

- [Configurer un Agent de classification](configurer-un-agent-de-classification.md)
- [Entraîner un modèle de classification](entrainer-un-modele-de-classification/entrainement-du-modele.md)
