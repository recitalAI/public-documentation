# Version 26.12.x

## Quoi de neuf dans cette version ?

La version 26.12 facilite la réutilisation et le transfert de configurations entre environnements. Les **Blueprints** prennent en charge davantage de types de ressources, tandis que l'**export et l'import des Workflows** peuvent inclure leurs dépendances.

Cette version améliore également le suivi des exécutions, la **Review avancée**, la configuration des modèles de langage et la portabilité des Agents et des files de révision. Plusieurs correctifs renforcent la fiabilité des éditions et des échanges de documents.

## Changements incompatibles et actions nécessaires

### API Extract : retrait d'anciennes routes

Les anciennes routes historiques d'Extract accessibles **sans le préfixe `/extract`** ne sont plus exposées. Les intégrations utilisant ces URL doivent appeler les routes correspondantes sous `/extract/api/v1`.

**Action requise :** vérifiez les URL appelées par vos scripts et intégrations, y compris les sondes de supervision, puis adaptez les appels concernés. Ce changement ne concerne pas les intégrations qui utilisent déjà les routes canoniques.

### Extraction générative : sélection explicite du modèle

Sur plusieurs chemins de traitement d'extraction générative, l'application ne sélectionne plus automatiquement un modèle OpenAI lorsqu'aucun modèle n'est indiqué dans la configuration de l'Agent.

**Action requise :** vérifiez les Agents d'extraction générative dont le modèle n'est pas renseigné et sélectionnez explicitement le modèle à utiliser avant la mise à niveau.

## Nouvelles fonctionnalités et améliorations

### Blueprints et ressources réutilisables

- Les Blueprints peuvent représenter des Agents et modèles d'extraction ou de classification, ainsi que des Workflows.
- Les Blueprints de Workflows peuvent inclure davantage de dépendances, afin de faciliter leur réutilisation dans d'autres environnements.
- La liste des Blueprints présente plus clairement le type et le contenu des ressources. Les dépôts en lecture seule sont identifiables et la sélection des Agents prend mieux en compte les filtres et la pagination.

### Workflows et suivi des exécutions

- **Export et import avec dépendances :** un Workflow peut être exporté avec ses objets associés, puis importé dans un autre environnement. L'import prend en charge certains cas de mise à jour d'objets existants et affiche les références nécessaires aux fournisseurs. Les connexions et secrets spécifiques à la destination restent à vérifier après l'import.
- **Étapes Python :** prise en charge des tunnels SSH lorsque l'environnement est configuré pour les utiliser.
- **Suivi des jobs :** amélioration de la liste des exécutions, de la consultation des sous-workflows et de l'affichage des détails d'un job dans un panneau latéral.
- **Notifications :** possibilité de filtrer les événements à l'origine des appels webhook sur les opérations concernées.

### Review et Review avancée

- **Portabilité :** export et import des configurations de files de révision.
- **Identification des documents :** ouverture d'un document de Review avancée à partir d'un identifiant externe transmis par un Workflow.
- **Entrée d'extraction :** possibilité de définir une entrée par défaut dans une étape de Review avancée.
- **Validation :** relance manuelle de la vérification des règles dans une file et affichage de messages personnalisés pour les champs nécessitant une correction.
- **Saisie et navigation :** raccourci de copie depuis le champ suivant et navigation plus fluide entre les pages.
- **Webhooks :** nouvelles tentatives de notification en cas d'échec. Les applications réceptrices doivent pouvoir reconnaître un événement déjà traité.
- **Entretien des files :** possibilité de demander la suppression en lot des documents anciens, selon la politique de conservation applicable.

### Extraction, classification et modèles de langage

- **Groupes d'extraction :** l'API expose le champ `group_labels` et prend en charge sa mise à jour.
- **Modèles LLM :** possibilité d'ajouter un fournisseur ou un modèle propre à l'organisation et de configurer le délai maximal de certains traitements génératifs.
- **Portabilité des Agents :** export et import des configurations d'Agents et de modèles ; l'export d'un Agent d'extraction peut exclure ses fichiers.
- **Classification :** amélioration de la configuration des Agents, de la validation et de l'affichage de l'état de correction des documents.
- **Traces de traitement :** ajout d'informations sur le coût des appels aux modèles de langage. Ces estimations ne correspondent pas nécessairement aux montants facturés.
- **Documents et OCR :** amélioration des opérations de copie, d'export et d'import, notamment pour les documents historiques et les annotations de position.

### Évolutions de contrats API

- **API File :** le champ `pages` de `FileOut` est déprécié et n'est plus obligatoire. Certains champs de texte et d'image des schémas de pages peuvent désormais être `null`. Les intégrations utilisant des validateurs JSON stricts doivent prendre en charge ces valeurs.
- **Review et OCR :** les routes de récupération du texte documentaire tiennent compte du texte OCR obtenu lors du traitement dans la file. Les intégrations exploitant ces résultats doivent vérifier le comportement attendu pour leurs documents.

Ces évolutions de schéma ne sont pas, à elles seules, des suppressions d'endpoints.

## Principaux correctifs

### Review : fiabilité des corrections

- L'action **`CLEAR ALL`** efface également les valeurs des groupes en Review avancée.
- L'édition d'une cellule n'affiche plus simultanément l'ancienne valeur et la nouvelle.
- Le bouton d'envoi est correctement actualisé lorsque les conditions de validation sont réunies.

### Blueprints et transferts de données

- Corrections de la création, de la synchronisation et de l'affichage des dépôts de Blueprints.
- Amélioration de la conservation des annotations de position et des libellés lors de certains exports et imports.
- Corrections de copie ou d'export de documents lorsque leurs données sont stockées dans le stockage documentaire commun.

### Interface et opérations courantes

- L'application peut demander un rafraîchissement de la page après le déploiement d'une nouvelle version de l'interface.
- Les jetons API sont masqués visuellement dans les paramètres.
- Corrections de la suppression de boîtes de messagerie, du premier affichage d'un onglet Workflow dans Performance et de la gestion des colonnes ignorées dans Search.

Pour les changements déjà introduits au cours de la série 26.8.x, consultez les [notes de version 26.8.x](version-26.8.x-06-10-2026.md).
