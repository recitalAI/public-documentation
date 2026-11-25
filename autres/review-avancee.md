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

## 2. Configurer, importer ou exporter le layout

{% hint style="info" %}
Le layout se configure directement dans le viewer de classeur intégré à la
plateforme ; aucun fichier ODS n'est nécessaire pour le modifier. L'import et
l'export de fichiers de layout utilisent le format ODS. Un fichier Excel doit
être exporté au format ODS avant de pouvoir être importé.
{% endhint %}

### Configurer le layout dans l'application

1. Dans l'onglet `Versions`, choisissez `Nouveau Layout` pour ouvrir une
   version à configurer.
2. Modifiez les feuilles dans le viewer de classeur intégré, puis choisissez
   `Enregistrer` pour sauvegarder le layout.
3. Utilisez `Aperçu` pour vérifier le rendu, puis `Activer` lorsque la version
   est prête.

Pour partir d'un fichier, choisissez `Importer le layout` et fournissez un
fichier ODS. Pour récupérer un fichier, utilisez `Télécharger la Configuration`.

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

### Titres, bordures et styles de cellule

Une cellule contenant du texte simple affiche un libellé ou un titre ; elle ne
crée pas de champ. Dans le viewer de classeur intégré, vous pouvez mettre en
forme les cellules des feuilles de section et des composants. La Review App
reprend les couleurs de fond, les quatre bordures (haut, bas, gauche, droite)
et le gras, cellule par cellule, y compris pour une cellule contenant
`$champ`. Ces styles ne modifient ni le champ défini dans `Config` ni son
comportement.

**Exemple : titre et séparateur**. Dans la feuille `Informations contrat`,
utilisez la grille `30% | 70%`. Saisissez `Informations du contrat` sur une
ligne, puis les libellés et champs ci-dessous. Mettez le titre en gras avec un
fond coloré et ajoutez une bordure inférieure aux cellules de la ligne de titre
ou aux cellules de libellé pour séparer visuellement les lignes.

```text
Feuille : Informations contrat
30% | 70%
Informations du contrat |
Type de contrat | $contract_type
Numéro de contrat | $contract_number
```

Pour reproduire l’essai, appliquez les styles dans le viewer de classeur
intégré, puis choisissez `Enregistrer`. Utilisez `Aperçu` pour contrôler le
rendu avant d’`Activer` la version. La première ligne reste la grille de
largeur : le formatage ne la remplace pas et ne configure pas les champs.

### Cellules fusionnées et débordement du texte

La fusion de cellules dans le viewer de classeur est prise en compte comme une
largeur couvrant plusieurs colonnes de la grille. Par exemple, avec
`30% | 30% | 40%`, fusionnez les trois cellules de la ligne du titre et
saisissez `Informations du contrat` dans la cellule fusionnée : elle occupe la
largeur totale de la grille. Choisissez `Enregistrer`, puis `Aperçu` pour
vérifier le résultat. La fusion est distincte du style et ne change pas la
grille définie sur la première ligne.

Le parseur peut aussi regrouper certaines cellules vides adjacentes lorsque
leurs fonds et bordures sont compatibles ; ce regroupement automatique ne
remplace pas une fusion explicite dans le viewer de classeur.

Le texte statique est rendu dans une grille CSS sans règle spécifique de
débordement ni de texte sur une seule ligne. Le comportement dépend donc du
contenu et de la largeur disponible : un texte avec des espaces peut revenir à
la ligne ; une chaîne sans point de coupure peut élargir la piste de la
grille, puis être coupée au bord de la section, dont le conteneur masque le
débordement. Le code ne garantit pas que le texte s’étende dans une cellule
voisine vide. Pour un titre large, fusionnez les cellules ou répartissez la
grille pour lui réserver l’espace ; ne confondez pas une éventuelle extension
visuelle avec une fusion de cellules.

Les styles pris en charge se limitent au fond, aux bordures et au gras. Les
autres propriétés du tableur ne sont pas garanties ; dans les tableaux de
groupes, l’en-tête est rendu par l’interface avec son propre style. Vérifiez
chaque mise en page dans l’aperçu.

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

### Composants réutilisables et zone fixe

Pour réutiliser un bloc de mise en page dans plusieurs onglets, créez une
feuille composant nommée `#EN_TETE`, avec sa propre grille en première ligne
et son contenu dans les lignes suivantes. Dans chaque feuille de section,
insérez `#EN_TETE` dans une cellule d’une ligne. Le composant reprend ses
cellules, libellés, champs et styles ; vous pouvez ainsi partager un en-tête
ou un bloc d’information sans le recopier.

Pour garder ce bloc visible pendant le défilement de la section, nommez sa
feuille `##EN_TETE` ; la référence dans la section reste `#EN_TETE`. Le
préfixe `##` est porté par le nom de la feuille composant, pas par la
référence. L’application rend alors ses lignes dans une zone fixe au-dessus du
contenu défilant. Le composant fixe reste dans l’onglet où il est inséré : il
ne fixe pas toute la section et ne crée pas un nouvel onglet.

**Exemple reproductible** : créez une feuille `##EN_TETE` avec la grille
`100%` et une ligne de texte `Dossier de revue` mise en gras. Dans chaque
feuille de section, ajoutez une ligne contenant `#EN_TETE`. Le titre reste
visible au-dessus du contenu défilant dans la Review App. L’aperçu du layout
affiche le composant, mais ne reproduit pas cette séparation fixe/défilante :
vérifiez ce comportement dans la Review App. Pour un bloc réutilisable mais
défilant, nommez plutôt la feuille `#EN_TETE`.

### Afficher un groupe sous forme de tableau

Un groupe rassemble des champs liés, par exemple les champs d’une remise. La colonne `Group` de `Config` identifie les champs du groupe, mais ne crée pas à elle seule le tableau : une feuille composant du même nom définit les champs et les colonnes à afficher. Dans une feuille de section, la référence `#NomDuGroupe[*]` affiche ce composant sous forme de tableau.

Exemple pour un groupe `DISCOUNT` :

1. Dans `Config`, renseignez `DISCOUNT` dans la colonne `Group` pour les champs `discount_name` et `discount_amount`.
2. Créez une feuille `#DISCOUNT` avec une ligne d’en-tête sans référence de champ, suivie des références aux champs du groupe :

```text
Feuille : #DISCOUNT
50% | 50%
Libellé | Montant
$discount_name | $discount_amount
```

3. Dans la feuille de section, insérez `#DISCOUNT[*]` pour afficher le composant en tableau.

Chaque occurrence enregistrée apparaît sur une ligne. En l’absence d’occurrence, une ligne vide au moins reste disponible pour la saisie. Lorsque la revue autorise la modification, l’opérateur peut ajouter ou supprimer des lignes. Cette notation répète un composant ; elle est distincte de l’insertion simple `#NomDuComposant` ou fixe `##NomDuComposant`.

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
