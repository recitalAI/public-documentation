# Gestion des utilisateurs

L'écran **Settings → Users** répertorie les utilisateurs de l'organisation (colonnes **Status, Email, Name, Role**) et permet d'en créer via le bouton **Create user** (champs Prénom, Nom, Email, Role).

## Les rôles

À la création d'un utilisateur, le champ **Role** propose **cinq rôles** :

| Rôle | Vocation |
|---|---|
| **Reviewer** | Correction / review des documents (vidéo-codage). Accès restreint à l'écran de Review. |
| **Operator** | Opérateur de correction. |
| **Expert** | Opérateur avec un périmètre étendu. |
| **Supervisor** | Suivi de l'activité et gestion de certains comptes/réglages. |
| **Orgadmin** | Administration complète de l'organisation (Studio, Agents, Workflows, Review, Settings). |

{% hint style="info" %}
Le périmètre exact de chaque rôle se gère côté plateforme. Pour les rôles **Operator**, **Expert** et **Supervisor**, se référer à l'écran **Settings → Users** de votre déploiement.
{% endhint %}

{% hint style="warning" %}
Un rôle **Sysadmin** existe également mais est réservé à l'administration interne reciTAL : il n'est **pas** proposé à la création d'un utilisateur.
{% endhint %}

## Rôles personnalisés

Au-delà des rôles ci-dessus, des **rôles personnalisés** peuvent être définis (champ **User role** à la création, géré via l'onglet **User Roles** des Paramètres selon le déploiement). Voir [Paramètres](../production/parametres.md).
