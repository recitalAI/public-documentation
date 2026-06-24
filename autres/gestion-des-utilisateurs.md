# Gestion des utilisateurs

L'écran **Settings → Users** répertorie les utilisateurs de l'organisation (colonnes **Status, Email, Name, Role**) et permet d'en créer via le bouton **Create user** (champs Prénom, Nom, Email, Role).

## Les rôles

Le champ **Role** (à la création comme à l'édition d'un utilisateur) propose **deux rôles** intégrés :

| Rôle | Vocation |
|---|---|
| **Reviewer** | Correction / review des documents (vidéo-codage). Accès restreint à l'écran de Review. |
| **Orgadmin** | Administration complète de l'organisation (Studio, Agents, Workflows, Review, Settings). |

Un champ distinct **user_role** permet d'assigner un **rôle personnalisé** (au-delà des deux rôles intégrés) lorsque l'organisation en a défini.

{% hint style="warning" %}
Un rôle **Sysadmin** existe également mais est réservé à l'administration interne reciTAL : il n'est **pas** proposé dans le menu Role.
{% endhint %}

Le rôle s'assigne dans l'onglet **Settings → Users** (bouton **Create user**, ou menu **Actions → Edit** d'un utilisateur existant). Voir [Paramètres](../production/parametres.md).
