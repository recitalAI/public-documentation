# Version {{SERIE}}

## En bref

{{Phrase courte sur ce que cette version change pour vos utilisateurs.}}

- **{{Bénéfice client}} :** {{fonctionnalité qui l'apporte et résultat concret}}.
- **{{Autre bénéfice}} :** {{usage rendu plus simple}}.

## Changements incompatibles : actions à prévoir

- **{{Utilisateurs ou intégration concernés}} :** si {{condition d'impact}}, {{ce qui change}}. {{Action concrète à effectuer}}.

Pour adapter une intégration concernée, consultez le [tableau des migrations API](#migrations-api).

## Nouvelles fonctionnalités et améliorations

### {{Nom du premier usage ou domaine produit}}

- **{{Fonctionnalité}} :** {{action désormais possible et bénéfice}}.

### {{Nom du deuxième usage ou domaine produit}}

- **{{Fonctionnalité}} :** {{action désormais possible, avec un exemple concret si utile}}.

## Principaux correctifs

- **{{Parcours concerné}} :** {{comportement désormais corrigé du point de vue de l'utilisateur}}.

Consultez aussi les [notes de la version précédente]({{PAGE_PRECEDENTE}}).

## Détails avancés pour les intégrations API

Consultez selon votre besoin : [exemple d'intégration](#exemple-dintegration), [migrations API](#migrations-api), [nouvelles opérations](#nouvelles-operations) ou [autres évolutions API](#autres-evolutions-de-parametres-et-de-reponses).

### Exemple d'intégration

{{Remplacez ce titre par l'objectif métier et adaptez son ancre dans l'index. Indiquez les prérequis, la requête minimale vérifiée et son résultat attendu. Utilisez un hôte d'exemple et aucune donnée réelle.}}

### Migrations API

| API | Route canonique | Avant | Après et action |
| --- | --- | --- | --- |
| {{Service}} | `{{METHODE /chemin/canonique/}}` | {{Paramètre, réponse ou comportement précédent}} | {{Nouveau contrat et adaptation requise}} |

### Nouvelles opérations

| Route | Utilisation |
| --- | --- |
| `{{METHODE /chemin/canonique/}}` | {{Quand l'utiliser, paramètres utiles, réponse attendue}} |

### Autres évolutions de paramètres et de réponses

| Surface | Évolution et action |
| --- | --- |
| {{Route ou schéma concerné}} | {{Évolution, éventuel impact sur les clients à validation stricte et vérification à effectuer}} |
