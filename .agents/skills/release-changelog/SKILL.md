---
name: release-changelog
description: Prépare ou révise les notes de version client de reciTAL dans le dépôt de documentation, à partir des versions de release vérifiées, des changements produit et des écarts API. À utiliser pour un changelog de release, pas pour un changelog technique interne ni pour publier une release.
---

# Préparer le changelog client d'une release reciTAL

Lisez `AGENTS.md` du dépôt de documentation. Utilisez le [modèle Markdown](assets/changelog-template.md) pour une nouvelle page ; pour une PR en cours, reprenez sa page et sa navigation plutôt que d'en ouvrir une concurrente.

**Entrées attendues :** série de release, versions réellement installées servant de référence, tag cible (ou dernier candidat), sources Produit/QA et PR existante s'il y en a une. Si une référence manque, avancez sur le périmètre vérifiable, indiquez l'incertitude en revue et ne concluez pas à l'absence de rupture.

## 1. Fixer le périmètre et les preuves

- Dans le monorepo, vérifiez le remote et récupérez les tags et la branche de release à jour. N'écrasez ni le checkout ni les changements en cours : utilisez les références distantes ou un worktree isolé si vous devez ouvrir la branche. Relevez les tags et SHA exacts comparés ; couvrez aussi les écarts significatifs entre les correctifs de la série déjà installée. Ne confondez pas tête de branche, tag candidat et version effectivement déployée.
- Croisez le changelog technique et les changements de code avec les retours Produit/QA : disponibilité réelle, interface, effets observables sur les parcours et principaux correctifs. Retirez les fonctionnalités non livrées ou hors périmètre. Une intention de correction ou une décision dans un fil n'est pas une preuve qu'elle figure dans le tag cible. Par exemple, ne présentez un paramètre requis comme facultatif qu'après avoir vérifié le nouveau défaut et, si possible, testé l'appel sans ce paramètre sur la cible.
- Comparez les spécifications OpenAPI servies sur les deux versions quand elles sont disponibles et rattachées aux bons builds. Utilisez tout comparatif existant comme aide à l'inventaire, pas comme source de vérité. Vérifiez dans les routeurs, les schémas, leurs dépendances et les tests : montage et exposition des routes, alias et barres finales, champs requis ou nullable, valeurs par défaut, statuts, types, exemples et effets à l'exécution. Une route qui disparaît du Swagger n'est pas nécessairement supprimée ; une route dans le code n'est pas nécessairement accessible derrière l'ingress.
- Sans paire de spécifications fiable, faites une comparaison statique et signalez ses limites à la revue. Rejouez, quand c'est possible et autorisé, les appels de migration sur les versions vérifiées ; séparez **lecture du code**, **contrat observé** et **tests restant à faire**. Gardez un relevé réutilisable par changement : surface, avant/après, impact/action, preuve, état de validation. Ne publiez ni données client, ni secrets, ni liens privés dans ce dépôt ou ses PR.

## 2. Rédiger pour deux publics, sur une seule page

- **En bref :** quelques bénéfices concrets dans les termes du client. **Changements incompatibles :** pour chaque cas avéré, dites qui est concerné, ce qui ne fonctionne plus et quoi adapter. Décrivez la configuration qui déclenche le problème (« aucun modèle n'était configuré »), sans jargon interne ni motivation non confirmée.
- **Nouveautés :** regroupez par usage plutôt que par service interne ou ticket ; décrivez les tâches rendues possibles, avec un exemple quand il aide. **Principaux correctifs :** ne gardez que ceux dont l'effet est perceptible et vérifié, sans réannoncer un correctif déjà livré dans la référence.
- **Détails API en fin de page :** créez un index d'ancres et des exemples ciblés. Recensez dans les tableaux les méthodes et chemins canoniques exacts, les paramètres (emplacement, type, caractère requis et valeur par défaut quand pertinents), l'ancien et le nouveau comportement, puis l'action. Précisez codes de réponse et effets de bord utiles. Séparez ruptures, opérations nouvelles et autres évolutions de contrat. N'appelez pas rupture un ajout compatible ; précisez le risque pour les consommateurs stricts (champ absent ou nul, nouvelle valeur d'énumération, structure de réponse modifiée). Ne promettez ni couverture exhaustive ni compatibilité sans preuve.
- Gardez les tags provisoires, tests non confirmés, arbitrages de périmètre et préparatifs de publication hors de la page client. Dans la PR de ce dépôt public, résumez les versions, la méthode, les limites et les validations restantes sans y copier de sources ou données non publiques.

## 3. Vérifier, faire relire et préparer le GO

1. Complétez la page sous `changelogs/` et son entrée dans `SUMMARY.md`. Gardez les URL déjà publiées. Supprimez les sections et marqueurs du modèle qui ne s'appliquent pas. Vérifiez liens, ancres et exemples, y compris le rendu des tableaux et la navigation.
2. Appliquez le workflow Git d'`AGENTS.md`. Proposez le changement dans la PR GitHub prévue, en brouillon si elle n'est pas prête ; ne modifiez pas directement GitBook. Vérifiez les checks et l'aperçu GitBook du **nouveau commit**, et signalez ce que vous n'avez pas pu valider.
3. Faites valider par Produit/QA les changements annoncés, les migrations et les cas encore ouverts. Avant le GO, comparez le tag final au candidat et actualisez la page et les vérifications si besoin. Préparez à part, si nécessaire, une communication client une à deux semaines avant le déploiement prévu ; elle ne vaut ni accord d'envoi ni mise en ligne. Ne fusionnez ou ne publiez que sur autorisation explicite au GO de la release.
