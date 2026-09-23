# Paramètres

L'écran **Paramètres** regroupe les préférences de l'utilisateur connecté et les configurations de son organisation.

## Accès et navigation

Dans la barre latérale, cliquez sur **Paramètres**.

La page présente les sections disponibles dans la vue d'administration de l'organisation :

- **Système** : préférence de langue et configurations de l'organisation ;
- **Utilisateurs** : liste et gestion des utilisateurs ;
- **Fournisseurs OCR** : configuration des fournisseurs OCR ;
- **Jetons API** : création et gestion des jetons API.

## Système

### Paramètres de langue

La langue est une préférence propre à l'utilisateur connecté, et non un paramètre commun à toute l'organisation. Le choix met immédiatement à jour la langue de l'interface et est enregistré dans le profil de l'utilisateur pour ses prochaines connexions. Les choix sont **Anglais (États-Unis)** et **Français (France)**.

Les autres sections de l'onglet **Système** configurent l'organisation.

### Configuration OCR par défaut

Cette section définit la configuration OCR par défaut de l'organisation utilisée par les services concernés, notamment l'extraction, la classification et le traitement de documents :

- **Fournisseur OCR** : fournisseur OCR par défaut parmi ceux configurés dans l'onglet [Fournisseurs OCR](#fournisseurs-ocr) ;
- **Forcer l'OCR** : exécute l'OCR sur tous les documents, y compris ceux qui contiennent déjà du texte exploitable ;
- **Effectuer l'OCR sur les images** : extrait le texte des images du document ;
- **Rotation automatique des pages** : redresse les pages tournées de 90°, 180° ou 270° ; cette option requiert **Forcer l'OCR** et un fournisseur Google ou Azure ;
- **Redresser les documents inclinés** : corrige l'inclinaison légère des pages numérisées ; cette option requiert **Forcer l'OCR** ;
- **Détecter les cases à cocher** : détecte les cases à cocher pour l'annotation ;
- **Utiliser le modèle le plus récent** : utilise la version la plus récente du modèle OCR sélectionné. En production 26.6.21, ce paramètre est activé (`use_latest=true`).

### Exporter l'organisation

Exportez l'organisation depuis **Système** avec **Exporter l'organisation**, puis **Télécharger en zip**.

L'export démarre de manière asynchrone. Une notification de téléchargement est fournie lorsqu'il est terminé.

Le fichier exporté contient les données et paramètres de l'organisation, ainsi que les configurations Extract et Classify applicables, les Datasets, les modèles ou Agents et les Workflows pris en charge par l'export.

Cet export ne constitue pas à lui seul une stratégie de sauvegarde et ne fournit pas de procédure de restauration en libre-service.

### Paramètres de callback

Le callback est appelé à la fin du traitement d'extraction.

Les paramètres sont :

- **URL** : adresse du callback, avec le protocole **http://** ou **https://** ;
- **Token** : valeur facultative envoyée sous la forme d'un jeton Bearer lorsqu'elle est renseignée ;
- **Header d’autorisation personnalisé** : nom facultatif du header qui transporte le jeton. Par défaut, ce header est `Authorization` ; ce champ permet d'en modifier le nom, pas d'ajouter des headers personnalisés arbitraires.

L'URL, le token et le nom du header d'autorisation personnalisé peuvent être laissés vides. Cliquez sur **Enregistrer** pour appliquer la configuration.

### Paramètres de la Corbeille

Cette section définit uniquement la **Période de conservation des éléments dans la Corbeille** pour les Agents d'extraction et les Datasets supprimés.

Deux choix sont proposés :

- **1 semaine** : 7 jours, valeur par défaut ;
- **30 jours**.

### Paramètres Classify dépréciés des e-mails et de l'OCR

{% hint style="warning" %}
Cette configuration est dépréciée. Elle concerne uniquement les anciens traitements de classification d'e-mails et leur OCR, et n'a aucun impact sur les Agents de classification. Ne l'utilisez pas pour configurer les Agents de classification ni l'OCR de la plateforme.
{% endhint %}

Les valeurs par défaut et les choix disponibles en production sont :

- **Lire les pièces jointes** : activé (`true`) par défaut ;
- **Nombre maximal de pièces jointes à lire** : `10` par défaut. Il s'agit d'un champ numérique ; l'interface n'impose pas de plage minimale ou maximale ;
- **Temps de rétention des e-mails (en mois)** : `2` mois par défaut, avec les choix de `1` à `12` mois ;
- **Utiliser l'OCR** : activé (`true`) par défaut ;
- **Utiliser l'OCR de Google** : désactivé (`false`) par défaut ;
- **Nombre maximal de pages OCRisées** : `3` pages par défaut, avec les choix de `1` à `12` ou **Tout**.

La désactivation de **Lire les pièces jointes** désactive le champ du nombre maximal de pièces jointes. La désactivation de **Utiliser l'OCR** désactive et efface **Utiliser l'OCR de Google**, puis désactive le choix du nombre de pages.

Ces paramètres concernent le comportement des routes Classify suivantes :

- `/docs/predictions/` ;
- `/emails/attachments/` ;
- `/emails/attachments/predict/{model}/` ;
- `/emails/predict/{model}/`.

La configuration OCR de production comprend également **Utiliser le modèle le plus récent**, activé dans la version 26.6.21 (`use_latest=true`).

### Paramètres de connexion

Cette section configure les méthodes de connexion de l'organisation :

- **Authentification par e-mail et mot de passe** active ou désactive ce mode de connexion ;
- **Lier les tokens d’accès aux IPs** configure la liaison des tokens aux adresses IP au niveau de l'organisation ;
- **Ajouter un fournisseur d’authentification personnalisé** permet de configurer un fournisseur OIDC. Le nom du fournisseur est défini par l'organisation ;
- **URL de connexion personnalisée** définit une URL propre à l'organisation à partir de son slug.

La production autorise au maximum un fournisseur OIDC personnalisé actif. Utilisez **Enregistrer** pour sauvegarder la configuration. Les actions **Copier l'URL de callback dans le presse-papiers** et **Copier l'URL de connexion personnalisée dans le presse-papiers** sont disponibles pour les URL correspondantes.

Le slug de l'URL de connexion personnalisée est normalisé en minuscules et limité aux caractères pris en charge en production : lettres de `a` à `z`, chiffres et tirets.

Lorsque **Lier les tokens d’accès aux IPs** s'applique, les nouvelles sessions ouvertes par mot de passe ou OIDC enregistrent l'adresse IP de connexion. Un token contenant une adresse IP est rejeté si l'adresse IP de la requête est différente ou absente. Les jetons API créés depuis une telle session héritent de cette adresse IP. L'activation de ce paramètre ne modifie pas rétroactivement les tokens existants qui ne contiennent pas d'adresse IP.

### Documentation des API

La section répertorie les documentations d'API accessibles à l'utilisateur connecté. Elle peut inclure :

- **Extract API** ;
- **Classify API** ;
- **Workflows API** ;
- **Extract Review API**.

**Authenticator API** est toujours incluse. L'action **Ouvrir la documentation** ouvre la documentation du service. Si le contrôle de santé indique qu'un service n'est pas en cours d'exécution, cette action peut être désactivée.

## Utilisateurs

L'onglet **Utilisateurs** présente les utilisateurs de l'organisation avec leur statut, leur adresse e-mail, leur nom et leur niveau d'accès. Il permet de filtrer la liste, de créer ou modifier un utilisateur, de réinitialiser son mot de passe et de le supprimer.

Voir [Gestion des utilisateurs](../autres/gestion-des-utilisateurs.md) pour en savoir plus.

## Fournisseurs OCR

Le tableau des fournisseurs OCR utilise les colonnes :

- **Nom** ;
- **Type de fournisseur** ;
- **Point d’accès** ;
- **QPS max** : nombre maximal de requêtes par seconde (QPS, « queries per second ») configuré pour ce fournisseur.

La production 26.6.21 prend en charge les types de fournisseurs suivants :

- **Azure OCR** — `AZURE` ;
- **Google Vision** — `GOOGLE` ;
- **PaddleOCR** — `PADDLEOCR` ;
- **DocTR** — `DOCTR`.

Ces valeurs sont des types de fournisseurs pris en charge, et non la garantie que des fournisseurs correspondants sont déjà configurés pour l'organisation.

## Jetons API

Vous voyez vos jetons personnels ainsi que les jetons disponibles pour toute l'organisation.

Le tableau contient **Nom** et **Jeton**. Les actions disponibles comprennent :

- **Générer un jeton API** ;
- **Copier le token API** ;
- **Renommer** ;
- **Pour toute l’organisation** ;
- **Révoquer**.

L'interface ne propose pas de contrôle de permissions ou de portées fines à sélectionner. L'action **Pour toute l’organisation** permet aux autres administrateurs de l'organisation de lister et de gérer le jeton. Sa valeur reste disponible et peut être copiée jusqu'à sa révocation. Les jetons générés par ce mécanisme n'ont pas d'expiration automatique configurée.

Lorsqu'une adresse IP est enregistrée dans la session qui crée le jeton, celui-ci en hérite.

{% hint style="warning" %}
Les valeurs des jetons API sont des identifiants sensibles. Protégez-les et révoquez les jetons qui ne sont plus nécessaires.
{% endhint %}

Pour utiliser un jeton avec les API reciTAL, voir [Authentification](../integration-api/authentification.md).
