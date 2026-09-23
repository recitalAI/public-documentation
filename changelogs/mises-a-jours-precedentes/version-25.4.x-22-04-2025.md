# Version 25.4.x (22/04/2025)

### Version 25.4.1 (22/04/2025)

#### **Extraction**

* Un message d'alerte apparait lorsqu'un utilisateur déverrouille un agent d'extraction verrouillé.

#### Écran de review d'extraction

* Ajout d'une option dans les configurations de review de l'agent d'extraction pour n'afficher que les pages du document à corriger.

#### Classification

* La mise à jour d'un agent de classification n'engendre plus de bugs (durée de validité du lien de correction externe).

#### Annotation

* Amélioration des différentes options d'annotation

#### Écran de review de classification

* La review d'un agent de classification peut être restreint à certains utilisateurs.
* Correction d'un bug lié à l'ouverture d'une review de classification externe (redirection vers la page de login)
* En review de classification externe, la page se ferme automatiquement après la soumission ou le rejet du document.
* En review de classification authentifiée, redirection vers la bannière de correction une fois le document soumis ou rejeté.
* Ajout d'une option dans l'agent de classification pour forcer le reviewer à classer ou rejeter les documents/les pages en "Unknown".
* Correction d'un bug qui apparaissait lorsque le reviewer faisait à la fois la suppression d'une page, et l'ajout d'un break entre 2 pages.
* Ajout d'une option dans l'agent de classification pour autoriser/interdire un reviewer anonyme (review externe) à copier le document dans un dataset.
* L'écran d'accueil pour les reviewer a été retravaillé et affiche désormais le nombre de documents en attente de review de classification et extraction.&#x20;

#### Workflows

* Les jobs de test peuvent désormais être relancés à tout moment, lancés et suivis directement depuis l’éditeur de workflow, et recréés facilement avec les paramètres précédents (données, métadonnées, fichiers).
* Un job en production peut également être relancé manuellement.
* La sortie JSON de l'étape "Ingérer des e-mails" intègre maintenant le nom des pièces jointes traitées, et le nom des pièces jointes filtrées. Voir [#structure-des-resultats](../../workflow/modules/ingerer-des-e-mails.md#resultat "mention")
* Les logs de fonctionnement normaux dans les jobs ne sont plus affichés.
* Dorénavant, si le timeout d'une étape de code personnalisé (30s) est déclenché, on force l'arrêt du process, et l'étape se met en erreur.
* Pour les étapes d'Extraction et de Classification, une option a été ajoutée pour renvoyer le texte détecté par l'OCR.

#### OCR

* L'OCR Azure pour la classification n'était pas fonctionnel, c'est maintenant corrigé

#### Monitoring

* Ajout des métriques pour la classification.&#x20;
* Les écrans de monitoring et métriques sont en cours d'amélioration, ils seront disponibles pour la prochaine mise à jour.&#x20;
