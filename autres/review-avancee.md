# Review avancée

Ce guide explique comment concevoir le fichier de layout utilisé par la Review App. Il est destiné aux administrateurs fonctionnels et aux équipes projet qui préparent une file de revue.

## 1. Comprendre la Review App

La Review App permet à un opérateur de consulter un document, contrôler les données extraites, les corriger si nécessaire, puis valider le document. Le layout détermine les onglets, les libellés, la position des champs, les listes de choix et les règles de cohérence.

{% hint style="info" %}
**Principe de lecture**\
Les noms de champs sont des identifiants techniques. Ils doivent être identiques dans Config, dans les feuilles de section (préfixés de $) et dans les éventuelles règles. Les libellés visibles, eux, sont placés directement dans les cellules des sections.
{% endhint %}

Un layout est composé de trois éléments :

* Config : le dictionnaire des champs et de leurs propriétés.
* Sections : la mise en page affichée à l'opérateur, sous forme d'onglets.
* Feuilles optionnelles : règles de gestion, constantes, ressources de référence et composants réutilisables.

{% hint style="success" %}
**Bon réflexe**

Commencez avec une version simple : une feuille Config et une ou deux sections. Ajoutez les règles, composants et référentiels après avoir validé le parcours de revue de base.
{% endhint %}

## 2. Préparer et importer le fichier de layout

{% hint style="info" %}
Format pris en charge : fichier .ods (LibreOffice Calc). Un fichier créé dans Microsoft Excel doit être enregistré ou exporté au format ODS avant son import.
{% endhint %}

**Étapes recommandées**

* Créez le fichier dans LibreOffice Calc, ou préparez-le dans Excel puis enregistrez-le au format ODS.
* Créez la feuille Config avec les en-têtes exacts et dans l'ordre attendu.
* Ajoutez une feuille par onglet à afficher dans l'application.
* Importez le fichier dans une nouvelle version de la file, puis utilisez l'aperçu avant de la rendre active.

{% hint style="warning" %}
**Attention aux noms**\
Ne créez pas de feuille dont le nom commence par un caractère réservé par inadvertance :&#x20;

* \# est réservé aux composants
* \## aux composants fixes
* ! aux sections optionnelles
* @ aux mappages de ressources.
{% endhint %}

## 3. La feuille Config : définir les champs

Chaque ligne, après l'en-tête, définit un champ pouvant être affiché dans une section. Les en-têtes de cette&#x20;feuille sont stricts : respectez leur orthographe, leur casse et leur ordre.

| Colonne     | Rôle                                                                          | Exemple                                      |
| ----------- | ----------------------------------------------------------------------------- | -------------------------------------------- |
| Name        | Identifiant unique du champ. Sans espace de préférence.                       | CONTRACT\_TYPE                               |
| Group       | Nom du groupe si le champ appartient à un groupe                              | LIGNE                                        |
| Input type  | Composant de saisie : Texte, Liste déroulante, Case à cocher ou Interrupteur. | text, select, checkbox ou toggle             |
| data type   | Type de donnée : Texte, Nombre entier, Nombre décimal, Date ou Oui/Non        | string, integer, float, date ou boolean      |
| editable    | Autorise la modification par l'opérateur                                      | true                                         |
| mandatory   | Indique qu'une valeur est attendue pour valider                               | true                                         |
| constraints | Règle JSON Logic appliquée au champ.                                          | {"!!":\[{"var":""}]}                         |
| default     | Valeur initiale si aucune valeur n'est fournie.                               | 0                                            |
| options     | Liste de choix fixe d'un select : code vers libellé.                          | {"new":"Nouveau","renewal":"Renouvellement"} |
| factor      | Champ technique servant de facteur dans certaines règles.                     | false                                        |
| action      | Champ de décision pouvant déclencher une action de rejet.                     | false                                        |
| cleanable   | Autorise l'effacement lors de l'action de nettoyage de section.               | true                                         |

{% hint style="info" %}
**Valeur obligatoire ou chaîne non vide**\
La colonne mandatory couvre l'absence de valeur. Pour refuser également une chaîne vide, ajoutez dans constraints : {"!!":\[{"var":""}]}. Une chaîne contenant seulement des espaces reste considérée comme non vide.
{% endhint %}

## 4. Les feuilles de section : organiser l'interface

Toute feuille qui ne porte pas un nom réservé devient un onglet de la Review App. Le nom de la feuille est le titre affiché à l'opérateur.

{% hint style="success" %}
**Mise en forme**\
Les couleurs de fond, bordures et le gras appliqués dans le tableur sont repris dans la Review App. Utilisez-les sobrement : par exemple, une ligne de titre et des libellés lisibles.
{% endhint %}

### Première ligne : grille de colonnes

La première ligne définit la largeur des colonnes ; les lignes suivantes constituent l'interface.

La somme doit être égale à 100 %. Exemple : 30% | 70%

### Variables

Une cellule de section contenant $nom\_du\_champ affiche le champ défini dans Config. Une cellule sans $ est affichée comme un libellé ou un texte statique.

Exemple:&#x20;

```
Feuille : Informations contrat
30% | 70%
Type de contrat | $contract_type
Numéro de contrat | $contract_number
Date de signature | $signature_date
```

### Sections optionnelles

Préfixez le nom de la feuille par ! pour rendre la section optionnelle, par exemple !Annexes. La section ne sera visible que si au moins une des valeurs est non-nulle.

### Composants réutilisables

Pour réutiliser la même mise en page dans plusieurs sections, créez une feuille nommée #NomDuComposant, puis insérez #NomDuComposant dans une ligne de section. Avec le préfixe ##, le composant est affiché dans la zone fixe de la section. Cette fonctionnalité est particulièrement utile pour les headers / pied de page identiques sur toutes les sections.

## 5. Listes, référentiels et listes en cascade

### Liste statique

Pour un champ select, renseignez la colonne options avec un objet JSON. La clé est la valeur enregistrée ; la valeur est le libellé visible par l'opérateur.

Exemple: {"lease":"Bail habitation","amendment":"Avenant","other":"Autre"}

{% hint style="info" %}
Vous pouvez aussi saisir un format simple lease:Bail habitation, amendment:Avenant. Le JSON est\
toutefois recommandé, notamment lorsque les libellés contiennent des caractères particuliers.
{% endhint %}

### Référentiel tabulaire

Une feuille dont le nom commence par @ associe des champs à un fichier de ressource externe. Exemple :&#x20;@clients.csv. La première ligne est un en-tête ; chaque ligne suivante associe un champ à une colonne du référentiel.

Exemple :&#x20;

```
Feuille : @clients.csv
Datapoint | Colonne
client_name | name
client_id | id
client_city | city
```

Lorsqu'un opérateur sélectionne une valeur du référentiel, les autres champs mappés à la même ligne sont renseignés automatiquement.

### Liste en cascade

Une cascade convient lorsque le choix du premier champ détermine les valeurs possibles du suivant. Créez une feuille de ressource, par exemple @document\_types.json, dont les lignes ne contiennent que les champs, dans l'ordre des niveaux. La première ligne sert d'en-tête.

Exemple :&#x20;

```
Feuille : @document_types.json
Datapoint
family
document_type
```

La ressource associée est un JSON hiérarchique. Avec deux niveaux, le format suivant est correct :

```
{
    "CL1": {
        "renewal": "Actes de renouvellement",
        "amendment": "Avenant"
    },
    "CL2": {
        "maintenance": "Attestation d'entretien",
        "transfer": "Avis de virement"
    }
}
```

## 6. Règles, contraintes et automatisations

### Contraintes de champ

La colonne constraints de Config accepte une expression JSON Logic. Dans cette expression, {"var":""} représente le champ de la ligne courante.

<table data-search="false"><thead><tr><th>Besoin</th><th>Expression à placer dans constraints</th></tr></thead><tbody><tr><td>Refuser une valeur vide ou absente</td><td>{"!!":[{"var":""}]}</td></tr><tr><td>Interdire une valeur précise</td><td>{"!=":[{"var":""},"Inconnu"]}</td></tr><tr><td>Exiger un entier positif</td><td>{">":[{"var":""},0]}</td></tr><tr><td>Limiter une liste de valeurs</td><td>{"in":[{"var":""},["A","B","C"]]}</td></tr></tbody></table>

Les règles apparaissent dans l'interface de revue lorsqu'elles échouent. Testez toujours une contrainte avec une valeur valide et une valeur invalide dans l'aperçu de la file.

### Feuille `Rules`

La feuille `Rules` sert à créer des contrôles de cohérence entre plusieurs champs, surtout pour des montants ou quantités. Une règle est valide lorsque sa condition retourne `true`; sinon les champs concernés sont signalés dans la Review App.

Les colonnes fonctionnelles disponibles sont les suivantes :

<table data-search="false"><thead><tr><th>Colonne</th><th>Contenu attendu</th><th>Effet</th></tr></thead><tbody><tr><td><code>All !=0</code></td><td>Noms de champs séparés par des virgules</td><td>Tous les champs doivent être différents de 0.</td></tr><tr><td><code>Any!=0</code></td><td>Noms de champs séparés par des virgules</td><td>Au moins un champ doit être différent de 0.</td></tr><tr><td><code>All>=0</code></td><td>Noms de champs séparés par des virgules</td><td>Tous les champs doivent être supérieurs ou égaux à 0.</td></tr><tr><td><code>Sum!=0</code></td><td>Noms de champs séparés par des virgules</td><td>La somme doit être différente de 0.</td></tr><tr><td><code>Sum>0</code></td><td>Noms de champs séparés par des virgules</td><td>La somme doit être strictement positive.</td></tr><tr><td><code>Plus fields</code></td><td>Expression ou liste de champs</td><td>Partie gauche d’un contrôle de rapprochement.</td></tr><tr><td><code>Minus fields</code></td><td>Expression ou liste de champs</td><td>Partie droite d’un contrôle de rapprochement.</td></tr><tr><td><code>Abs ?</code></td><td><code>true</code> / <code>false</code></td><td>Applique une valeur absolue à l’écart entre les deux côtés.</td></tr><tr><td><code>Epsilon ?</code></td><td><code>true</code> / <code>false</code></td><td>Autorise un écart de tolérance. Activé par défaut.</td></tr><tr><td><code>Factor</code></td><td>entier</td><td>Facteur fixe, réservé aux règles inter-sections avancées.</td></tr></tbody></table>

Exemple:

<table><thead><tr><th width="218">Name</th><th>Plus fields</th><th>Minus fields</th><th>Abs ?</th><th>Epsilon ?</th></tr></thead><tbody><tr><td>Total TTC cohérent</td><td>montant_ht + montant_tva</td><td>montant_ttc</td><td>true</td><td>false</td></tr></tbody></table>

Avec `Epsilon ? = false`, le résultat impose :

```
abs(montant_ht + montant_tva - montant_ttc) = 0
```

Avec `Epsilon ? = true`, l’écart autorisé est la constante `epsilon`, qui vaut `5` par défaut si elle n’est pas définie dans la feuille `Constants`.

{% hint style="info" %}
**Points d’attention :**

* Les noms de champs doivent correspondre exactement aux valeurs de la colonne `Name` de `Config`.
* Dans une formule, utilisez uniquement les opérateurs `+`, `-`, `*`, `/` et `%`.
* Les parenthèses et les valeurs numériques saisies directement dans une formule ne sont pas prévues : utilisez plutôt un champ ou une constante.
* Une règle qui implique des champs de plusieurs sections devient une règle inter-sections.
* Les champs marqués `action` ne sont pas mis en évidence comme champs en erreur.
{% endhint %}

### Feuille `Assistants`

La feuille `Assitants` calcule automatiquement la valeur d’un champ à partir d’autres champs. Les deux premières colonnes sont utilisées :

* Nom du champ cible
* Formule

Exemple:

```
Target      | Formula
montant_ttc | montant_ht + montant_tva
```

{% hint style="info" %}
**Conditions de fonctionnement :**

* Le champ cible doit exister dans `Config`.
* Les champs sources doivent eux aussi exister dans `Config`.
* La formule utilise les opérateurs `+`, `-`, `*`, `/` et `%`.
* La valeur cible est recalculée dès qu’un des champs sources change.
* Après le chargement du document, le calcul initialise le champ cible si sa valeur initiale est vide, nulle ou égale à 0.
* Une fois la revue chargée, toute modification d’un champ source met à jour la cible : une correction manuelle de la cible peut donc être remplacée par le calcul suivant.
{% endhint %}

Les constantes de la feuille `Constants` ne sont pas disponibles dans les formules `Assistants`. Si un seuil fixe est nécessaire, privilégiez une règle de validation avec constante.

### Feuille `Constants`

La feuille `Constants` entralise des valeurs techniques réutilisables dans les règles de validation. Les en-têtes sont :

* name
* data type
* value

Exemple :

```
name        | data type | value
min_amount  | float     | 100
epsilon     | float     | 0.01
```

Une constante est remplacée dans les conditions de validation par sa valeur réelle. Par exemple, dans la colonne `constraints` (feuille `Config`):

```
{">=":[{"var":""},{"var":"min_amount"}]}
```

{% hint style="info" %}
**Cas particulier**

La constante nommée `epsilon` définit la tolérance utilisée par les règles de rapprochement `Plus fields` / `Minus fields` lorsque `Epsilon ?` est activé. Sans cette constante, la tolérance par défaut est `5`.
{% endhint %}

### Feuille `Codes`

La feuille `Codes` définit des codes de décision, généralement utilisés dans une liste déroulante de motif de rejet ou d’escalade.

Les en-têtes requis sont :

* code
* action
* no-delete (facultatif)

Exemple :

```
code              | action                 | no-delete
OK                |                        | false
DOC_INCOMPLETE    | #invalidate-page       | false
DOC_OUT_OF_SCOPE  | #invalidate-document   | true
```

Pour qu’un code ait un effet, il doit aussi être proposé dans les `options` d’un champ `select` dont la colonne `action` de `Config` vaut `true` :

```
{
  "OK": "Conforme",
  "DOC_INCOMPLETE": "Document incomplet",
  "DOC_OUT_OF_SCOPE": "Document hors périmètre"
}
```

Les actions possibles sont :

* `#invalidate-page` : neutralise les règles de validation liées à la section concernée lorsque ce code est choisi.
* `#invalidate-document` : neutralise l’ensemble des règles de validation du document lorsque ce code est choisi.
* Valeur vide : le code est informatif et ne neutralise aucune validation.

`no-delete = true` est une option technique : lors de la suppression du document de la file, la ressource source n’est pas automatiquement libérée si ce code d’action a été sélectionné.

Un code dont l’action n’est pas reconnue est importé sans action.
