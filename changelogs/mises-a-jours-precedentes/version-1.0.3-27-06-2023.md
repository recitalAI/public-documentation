---
description: Corrections de bugs, ajout de fonctionnalités.
---

# Version 1.0.3 (27/06/2023)

### Fonctionnalités ajoutées

<table><thead><tr><th width="71">#</th><th width="548">Description</th></tr></thead><tbody><tr><td>1</td><td>Créer des data points en utilisant la configuration des groupes d'extraction</td></tr><tr><td>2</td><td>Ajout d'une option pour désaligner les pages d'entrée du dataset</td></tr><tr><td>3</td><td>Ajouter la colonne date du dernier traitement à la table des fichiers vue</td></tr><tr><td>4</td><td>Corbeille pour les doctypes et les datasets</td></tr></tbody></table>



### Bugs corrigés

<table><thead><tr><th width="71">#</th><th width="548">Description</th></tr></thead><tbody><tr><td>1</td><td>Restauration de la variable d'environnement d'expiration du jeton de réinitialisation</td></tr><tr><td>2</td><td>Mise à jour et reconstruction de la base de données magique</td></tr><tr><td>3</td><td>Corriger l'erreur lors de la suppression du dernier document de la page de validation</td></tr><tr><td>4</td><td>Empêcher la duplication des noms de modèles personnalisés entre l'organisation principale et les autres organisations</td></tr><tr><td>5</td><td>Correction du problème où les noms de dp chevauchent leurs valeurs lorsqu'ils sont trop longs et contiennent des underscores.</td></tr><tr><td>6</td><td>Modifier la réinitialisation du mot de passe pour que le jeton ne puisse être utilisé qu'une seule fois et qu'il dure 24 heures sur demande.</td></tr><tr><td>7</td><td>Corriger le problème de Python 3 sur les images d'extraction</td></tr><tr><td>8</td><td>Restriction de la longueur des noms de jeux de données</td></tr><tr><td>9</td><td>Correction de l'édition du nom des entrées dans la table des jeux de données.</td></tr><tr><td>10</td><td>Correction d'un problème où la suppression de la première page générait une erreur 404</td></tr><tr><td>11</td><td>Correction de la suppression programmée</td></tr><tr><td>12</td><td>Correction du décalage de page dans la copie</td></tr><tr><td>13</td><td>Correction du bug empêchant de changer le nom d'un groupe</td></tr><tr><td>14</td><td>Affichage de repli du feat/générique</td></tr><tr><td>15</td><td>Clarification de la logique pour permettre la rotation automatique des pages. L'option existe déjà dans l'onglet de configuration</td></tr><tr><td>16</td><td>Correction de la validation du fallback génératif</td></tr><tr><td>17</td><td>Correction pour OpenID à l'échelle de l'installation</td></tr><tr><td>18</td><td>Correction de l'application du modèle après sa création</td></tr></tbody></table>
