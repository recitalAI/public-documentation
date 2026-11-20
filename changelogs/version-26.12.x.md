<!-- Brouillon de la PR #83 : contenu à valider avant fusion. Les incertitudes sont suivies dans la description de la PR. -->

# Version 26.12.x — notes de version en préparation

Ces notes de version sont en cours de validation en vue de la prochaine mise à jour. Elles décrivent les évolutions identifiées entre les versions `26.8.x` et la version candidate `26.12.1` ; leur disponibilité finale dépendra de la version déployée. Si vous utilisez une version antérieure de la série 26.8, consultez également les [notes de version 26.8.x](version-26.8.x-06-10-2026.md).

## Points d'attention avant la mise à niveau

- **API Extract :** si vous utilisez d'anciennes URL sans le préfixe `/extract`, vérifiez les sondes et scripts concernés et préparez leur migration vers les routes sous `/extract/api/v1`.
- **API Extract :** le champ `pages` d’une réponse fichier est déprécié et peut être absent ou `null`. Les champs `image_path` et `image` des schémas de page concernés peuvent aussi être `null`. Adaptez vos lecteurs et validateurs stricts.
- **Agents d’extraction générative :** vérifiez qu’un modèle est explicitement sélectionné. Les chemins de traitement qui utilisaient par défaut le modèle OpenAI lorsque l’agent n’en indiquait pas ne le font plus.
- **Installations On-Prem :** vérifiez avec votre équipe technique les prérequis applicables à votre installation, notamment les paramètres de déploiement et la reprise des documents historiques.

## Blueprints et portabilité

- Les blueprints peuvent inclure davantage d’objets : agents et modèles d’extraction ou de classification, workflows et dépendances supplémentaires de workflows.
- La liste des blueprints précise leur type et leur contenu. Les dépôts en lecture seule sont identifiables et la sélection des agents tient mieux compte des filtres et de la pagination.
- Vous pouvez transférer la configuration de files de révision et d’agents entre environnements. L’export d’un agent d’extraction peut être effectué sans ses fichiers. Vérifiez les modèles, ressources et autorisations sur la destination avant de réutiliser une configuration importée.

## Workflows

- Exportez et importez des lots de workflows avec leurs dépendances. Consultez le rapport d’import pour vérifier quels objets ont été repris. Ne présumez pas que les secrets et connexions propres à l’environnement cible sont transférés.
- Ajoutez une étape de sélection de pages. Les étapes Python prennent en charge les tunnels SSH si l’environnement le permet.
- Consultez les jobs enfants et les détails d’un job depuis le panneau latéral. Les filtres et les états sont plus lisibles dans la liste des jobs.
- Limitez les notifications webhook aux événements utiles avec le paramètre facultatif `webhook_events` sur les opérations concernées. Omettre le filtre conserve le comportement sans filtre.
- Un correctif empêche certains sous-workflows de rester bloqués sur leur propre webhook de démarrage. Un autre corrige certains cas d’activation ou de modification d’un workflow publié.

## Review

- Ouvrez un document de Review avancée à partir d’un identifiant externe transmis par le workflow. L’étape peut également utiliser une entrée d’extraction par défaut.
- Relancez manuellement les règles d’une file et affichez un message sur les champs qui nécessitent une correction. La navigation entre pages, la copie d’une valeur depuis le champ suivant et l’affichage de certaines valeurs évoluent.
- Les échecs de webhook de Review avancée font l’objet de nouvelles tentatives. Si votre intégration reçoit ces notifications, vérifiez qu’elle traite sans doublon la réception répétée d’un même événement.
- La suppression en lot des documents anciens d’une file est traitée en arrière-plan. Vérifiez votre politique de conservation avant de lancer l’opération.
- Plusieurs correctifs améliorent l’édition des cellules, l’effacement des valeurs de groupe et l’actualisation du bouton d’envoi. Les intégrations qui lisent le texte OCR de Review doivent comparer les résultats : les routes concernées tiennent compte de l’OCR utilisé pour traiter le document dans la file.

## Extraction, classification et recherche

- La configuration des fournisseurs et modèles LLM évolue. Les agents génératifs d’extraction et de classification acceptent un délai configurable ; les traces peuvent inclure des informations de coût à distinguer de la facturation.
- L’API des groupes d’extraction expose `group_labels` et prend en charge sa mise à jour. Vérifiez votre intégration si elle lit ou écrit la configuration de ces groupes.
- Validez des préannotations en lot et retrouvez plus facilement les documents en attente. L’export/import des jeux de données conserve davantage d’annotations de position et d’étiquettes d’entrées.
- L’éditeur des agents de classification améliore la validation et la sauvegarde de la configuration. La liste des documents indique leur état de correction.
- La lecture du texte de fichiers et son utilisation dans la recherche évoluent. Vérifiez les résultats pour vos documents OCR historiques si vous utilisez ces données dans une intégration.

## Autres améliorations

- L’interface peut proposer d’actualiser la page lorsqu’une nouvelle version est détectée. Les jetons API sont moins exposés visuellement dans les paramètres.
- La suppression de certaines boîtes de messagerie et l’ouverture du premier onglet de performance d’un workflow ont été corrigées.
