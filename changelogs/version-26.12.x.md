<!-- Brouillon pour revue GitHub #84. Ne pas fusionner avant confirmation du tag final, des tests QA et des impacts API. -->

# Version 26.12.x — notes de version en préparation

Cette page présente les principales évolutions identifiées dans le tag candidat `v26.12.1` du 19 novembre 2026, en comparaison de la série 26.8.x. **La version définitive, la date de déploiement en production et la disponibilité selon les installations restent à confirmer.** Ces fonctionnalités et correctifs font l'objet de tests sur l'environnement de préproduction.

Les améliorations déjà introduites par les correctifs 26.8.x ne sont pas présentées ici comme de nouvelles fonctionnalités. Si votre installation utilise une version antérieure, consultez également les [notes de version 26.8.x](version-26.8.x-06-10-2026.md).

## À vérifier avant votre mise à niveau

- **API Extract — anciennes routes :** si des intégrations utilisent d'anciens chemins sans le préfixe `/extract`, vérifiez leur compatibilité avec les routes sous `/extract/api/v1`. Certains anciens alias ont été retirés ; le périmètre exact des routes concernées doit encore être confirmé.
- **API File — schémas des pages :** le champ `pages` est déprécié et n'est plus obligatoire dans `FileOut`. Plusieurs champs de texte ou d'image peuvent être `null`. Si vous validez strictement les réponses JSON, adaptez les schémas et contrôlez les usages dépendant de ces champs.
- **Extraction générative — choix du modèle :** plusieurs chemins de traitement ne sélectionnent plus automatiquement le modèle OpenAI lorsqu'aucun modèle n'est renseigné. Vérifiez la configuration de vos Agents et testez ceux qui ne précisent pas de modèle.
- **Intégrations de Review et OCR :** les routes de texte documentaire utilisent le texte issu du traitement OCR de la file ; les évolutions et dépréciations associées doivent être vérifiées avant de modifier les intégrations.
- **Installations On-Prem :** faites vérifier les paramètres de déploiement et la compatibilité des documents historiques avec la nouvelle gestion du stockage avant la mise à niveau.

## Blueprints : davantage de contenus réutilisables

- Les blueprints peuvent cibler des Agents et modèles d'extraction et de classification ainsi que des Workflows.
- Les blueprints de Workflows peuvent inclure des dépendances supplémentaires, pour faciliter leur réutilisation dans un autre environnement.
- La liste des blueprints affiche plus clairement les types et le contenu. Les dépôts en lecture seule sont identifiables et la recherche ou sélection des Agents a été améliorée.
- Plusieurs correctifs portent sur les noms des blueprints, la synchronisation et l'affichage de leurs dépôts.

## Workflows : transferts et suivi des exécutions

- **Export et import avec dépendances :** exportez un Workflow avec les objets associés, puis importez-le dans un autre environnement. Des évolutions permettent de mettre à jour des objets existants et de retrouver les références aux fournisseurs. Vérifiez après import les références, les connexions et les secrets propres à l'environnement cible.
- **Étapes Python :** la prise en charge des tunnels SSH est ajoutée, sous réserve de la configuration réseau de votre installation.
- **Suivi des jobs :** la liste et les détails des jobs sont améliorés, notamment pour consulter les exécutions des sous-workflows dans un panneau latéral.
- **Notifications webhook :** un nouveau filtrage permet de sélectionner les événements qui déclenchent un callback, lorsque la fonctionnalité est utilisée. Vérifiez les notifications reçues avant de modifier vos intégrations.
- **Correctifs :** plusieurs cas d'activation ou de modification d'un Workflow publié ont été corrigés, ainsi que des problèmes liés à l'affichage des jobs.

## Review : portabilité, navigation et corrections

- **Export et import des files :** les configurations de files de révision peuvent être transférées entre environnements.
- **Review avancée :** les workflows peuvent transmettre un identifiant externe pour accéder à un document et fournir une entrée d'extraction par défaut.
- **Contrôles de saisie :** relance manuelle de la vérification des règles, message personnalisé pour certains champs nécessitant une intervention, raccourci de copie depuis le champ suivant et navigation entre pages améliorée.
- **Webhooks :** une reprise des notifications en échec est ajoutée. Si votre application reçoit ces notifications, vérifiez son comportement en cas de tentative répétée.
- **Documents anciens :** une opération permet de demander leur suppression en lot dans une file de révision. Vérifiez la portée et la politique de conservation applicables avant utilisation.
- **Correctifs :** des problèmes d'édition des cellules, d'effacement des groupes avec `CLEAR ALL` et d'actualisation du bouton d'envoi ont été corrigés.

## Extraction, classification et modèles de langage

- **Groupes d'extraction :** l'API expose le champ `group_labels` et prend en charge sa mise à jour. Les intégrations qui manipulent des groupes doivent vérifier les formats échangés.
- **Configuration LLM :** possibilité d'ajouter un fournisseur ou modèle LLM propre à une organisation et de configurer le délai maximal de certains traitements génératifs. Vérifiez la disponibilité et les valeurs applicables à votre environnement.
- **Portabilité des Agents :** export et import des configurations des Agents et modèles, avec notamment une option permettant d'exporter un Agent d'extraction sans les fichiers associés.
- **Classification :** l'interface de configuration et de validation évolue, et l'état de correction des documents est mieux visible.
- **Suivi de la consommation :** des informations de coût sont ajoutées aux traces de traitement. Elles ne constituent pas nécessairement le montant facturé.
- **Données et OCR :** les mécanismes de copie, d'export et d'import des documents et annotations ont été renforcés pour le stockage commun, y compris la conservation d'annotations de position. Vérifiez le résultat lors de transferts de données historiques.

## Autres améliorations et correctifs

- L'interface peut proposer un rafraîchissement lorsqu'une nouvelle version est détectée.
- Les jetons API sont mieux masqués visuellement dans les paramètres.
- Des corrections concernent notamment la suppression de boîtes de messagerie, l'ouverture d'un premier onglet dans la page Performance et l'affichage des colonnes ignorées dans Search.

Les changements concernant exclusivement les services, les images de déploiement, la sécurité technique ou les composants internes ne sont pas détaillés ici. Ils sont suivis dans la préparation de la release et la validation des installations concernées.
