# Paramètres

L'écran **Paramètres** regroupe les préférences de l'utilisateur connecté et les configurations de l'organisation auxquelles son rôle et les produits activés lui donnent accès.

## Accès et navigation

Dans la barre latérale, cliquez sur **Paramètres**. Cette entrée est disponible pour les rôles `basic`, `orgadmin` et `sysadmin` lorsqu'Extract ou Extract Review est activé.

La navigation est dynamique : les onglets affichés dépendent du rôle de l'utilisateur connecté et des produits activés pour son organisation. Les onglets disponibles en production peuvent être :

- **Système** : préférence de langue et, selon le rôle et les produits, configurations de l'organisation ;
- **Utilisateurs** : visible pour les rôles `orgadmin` et `sysadmin` ;
- **Fournisseurs OCR** : affiché dans la navigation ; sa gestion requiert le rôle `orgadmin` ou `sysadmin` ;
- **Jetons API** : affiché dans la navigation ; sa gestion requiert le rôle `orgadmin` ou `sysadmin` ;
- **Groupes** : affiché lorsque Search est activé ; sa gestion requiert le rôle `orgadmin` ou `sysadmin` ;
- **Organisations** : réservé au rôle `sysadmin`.

Les routes et API de gestion des **Utilisateurs**, des **Fournisseurs OCR** et des **Jetons API** requièrent le rôle `orgadmin` ou `sysadmin`.

## Système

### Paramètres de langue

La langue est une préférence propre à l'utilisateur connecté, et non un paramètre commun à toute l'organisation. Les choix sont **Anglais (États-Unis)** et **Français (France)**.

Les autres sections de l'onglet **Système** configurent l'organisation. Leur affichage dépend du rôle et des produits activés.

### Configuration OCR par défaut

Cette section, disponible pour le rôle `orgadmin`, définit la configuration OCR par défaut de l'organisation utilisée par les services concernés, notamment l'extraction, la classification et le traitement de documents :

- **Fournisseur OCR** : fournisseur OCR par défaut parmi ceux configurés dans l'onglet [Fournisseurs OCR](#fournisseurs-ocr) ;
- **Forcer l'OCR** : exécute l'OCR sur tous les documents, y compris ceux qui contiennent déjà du texte exploitable ;
- **Effectuer l'OCR sur les images** : extrait le texte des images du document ;
- **Rotation automatique des pages** : redresse les pages tournées de 90°, 180° ou 270° ; cette option requiert **Forcer l'OCR** et un fournisseur Google ou Azure ;
- **Redresser les documents inclinés** : corrige l'inclinaison légère des pages numérisées ; cette option requiert **Forcer l'OCR** ;
- **Détecter les cases à cocher** : détecte les cases à cocher pour l'annotation ;
- **Utiliser le modèle le plus récent** : utilise la version la plus récente du modèle OCR sélectionné. En production 26.6.21, ce paramètre est activé (`use_latest=true`).

### Exporter l'organisation

Pour une organisation utilisant Extract, un `orgadmin` peut exporter sa propre organisation depuis **Système** avec **Exporter l'organisation**, puis **Télécharger en zip**. Un `sysadmin` peut également lancer l'export d'une organisation depuis l'onglet **Organisations**.

L'export démarre de manière asynchrone. Une notification de téléchargement est fournie lorsqu'il est terminé.

Le fichier exporté contient les données et paramètres de l'organisation ainsi que, selon les produits utilisés par celle-ci, les configurations Extract et Classify applicables, les Datasets, les modèles ou Agents et les Workflows pris en charge par l'export.

Cet export ne constitue pas à lui seul une stratégie de sauvegarde et ne fournit pas de procédure de restauration en libre-service.

### Paramètres de callback

Cette section est disponible pour un `orgadmin` lorsque Extract est activé. Le callback est utilisé par le traitement d'extraction à la fin de l'extraction.

Les paramètres sont :

- **URL** : adresse du callback, avec le protocole **http://** ou **https://** ;
- **Token** : valeur facultative envoyée sous la forme d'un jeton Bearer lorsqu'elle est renseignée ;
- **Header d’autorisation personnalisé** : nom facultatif du header qui transporte le jeton. Par défaut, ce header est `Authorization` ; ce champ permet d'en modifier le nom, pas d'ajouter des headers personnalisés arbitraires.

L'URL, le token et le nom du header d'autorisation personnalisé peuvent être laissés vides. Cliquez sur **Enregistrer** pour appliquer la configuration.

### Paramètres de la Corbeille

Cette section est disponible pour un `orgadmin` lorsque Extract est activé. Elle définit uniquement la **Période de conservation des éléments dans la Corbeille** pour les Agents d'extraction et les Datasets supprimés.

Deux choix sont proposés :

- **1 semaine** : 7 jours, valeur par défaut ;
- **30 jours**.

### Paramètres Classify historiques des e-mails et de l'OCR

Cette section est disponible pour un `orgadmin` lorsque Classify est activé. Ces paramètres historiques sont propres à Classify ; ils ne configurent pas l'OCR de toute la plateforme.

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

Cette section, disponible pour le rôle `orgadmin`, configure les méthodes de connexion de l'organisation :

- **Authentification par e-mail et mot de passe** active ou désactive ce mode de connexion ;
- **Lier les tokens d’accès aux IPs** configure la liaison des tokens aux adresses IP au niveau de l'organisation ;
- **Ajouter un fournisseur d’authentification personnalisé** permet de configurer un fournisseur OIDC. Le nom du fournisseur est défini par l'organisation ;
- **URL de connexion personnalisée** définit une URL propre à l'organisation à partir de son slug.

La production autorise au maximum un fournisseur OIDC personnalisé actif. Utilisez **Enregistrer** pour sauvegarder la configuration. Les actions **Copier l'URL de callback dans le presse-papiers** et **Copier l'URL de connexion personnalisée dans le presse-papiers** sont disponibles pour les URL correspondantes.

Le slug de l'URL de connexion personnalisée est normalisé en minuscules et limité aux caractères pris en charge en production : lettres de `a` à `z`, chiffres et tirets.

Lorsque **Lier les tokens d’accès aux IPs** s'applique, les nouvelles sessions ouvertes par mot de passe ou OIDC enregistrent l'adresse IP de connexion. Un token contenant une adresse IP est rejeté si l'adresse IP de la requête est différente ou absente. Les jetons API créés depuis une telle session héritent de cette adresse IP. L'activation de ce paramètre ne modifie pas rétroactivement les tokens existants qui ne contiennent pas d'adresse IP.

### Documentation des API

Cette section est disponible pour le rôle `orgadmin`. Les entrées dépendent des produits activés pour l'utilisateur connecté. Elles peuvent inclure :

- **Extract API** ;
- **Classify API** ;
- **Workflows API** ;
- **Search API** ;
- **Extract Review API**.

**Authenticator API** est toujours incluse. L'action **Ouvrir la documentation** ouvre la documentation du service. Si le contrôle de santé indique qu'un service n'est pas en cours d'exécution, cette action peut être désactivée.

## Utilisateurs

L'onglet **Utilisateurs**, accessible aux rôles `orgadmin` et `sysadmin`, présente les utilisateurs de l'organisation avec leur statut, leur adresse e-mail, leur nom et leur rôle. Il permet de filtrer la liste, de créer ou modifier un utilisateur, de réinitialiser son mot de passe et de le supprimer.

Voir [Gestion des utilisateurs](../autres/gestion-des-utilisateurs.md) pour le détail des rôles.

## Fournisseurs OCR

La gestion des fournisseurs OCR requiert le rôle `orgadmin` ou `sysadmin`. Le tableau utilise les colonnes :

- **Nom** ;
- **Type de fournisseur** ;
- **Point d’accès** ;
- **QPS max**.

La production 26.6.21 prend en charge les types de fournisseurs suivants :

- **Azure OCR** — `AZURE` ;
- **Google Vision** — `GOOGLE` ;
- **PaddleOCR** — `PADDLEOCR` ;
- **DocTR** — `DOCTR`.

Ces valeurs sont des types de fournisseurs pris en charge, et non la garantie que des fournisseurs correspondants sont déjà configurés pour l'organisation.

## Jetons API

La gestion des jetons API requiert le rôle `orgadmin` ou `sysadmin`. Un `orgadmin` voit ses jetons personnels ainsi que les jetons disponibles pour toute l'organisation.

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
