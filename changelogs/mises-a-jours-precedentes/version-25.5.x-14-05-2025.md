---
description: >-
  Sécurité des tokens de service, nouvelle classe "No Text", connexion aux
  boîtes mail Microsoft, optimisation des performances, et amélioration de
  l’API.
---

# Version 25.5.x (14/05/2025)

### Version 25.5.1 (14/05/2025)

#### Sécurité

{% hint style="warning" %}
Une **mise à jour de sécurité** a été déployée. Nous vous demandons de **modifier vos tokens de service** dans les paramètres généraux **avant le 15 juin 2025**.

Pour les utilisateurs n'ayant pas accès à leur token de service, vous pouvez nous contacter à l'adresse suivante: **support@recital.ai**&#x20;
{% endhint %}

#### Écran de review d'extraction

* Les comptes **reviewer** sont désormais configurables par agent d'extraction afin de définir qui peut accéder à la revue ou non.
* Les **datasets de feedback** (utilisés pour collecter les retours utilisateurs) sont maintenant configurables dans les agents d'extraction.

#### Classification

* Ajout d’une nouvelle classe par défaut **"No Text"** (en complément de **"Unknown"**) pour identifier les documents ou pages sans texte (ex. : photos, pages blanches). Cette classe est renommable dans les agents de classification.

#### Écran de review de classification

* Les comptes **reviewer** sont également configurables par agent de classification pour restreindre ou autoriser l'accès à la revue.

#### Workflows

* La connexion aux boîtes mail **Microsoft** est désormais configurable via : _Workflow > Input > Créer une boîte e-mail_.
* Amélioration des performances du système de **file d'attente** pour les jobs Workflow.

#### Reviewer

* Amélioration de l’écran d’accueil des profils **reviewer** pour un accès plus rapide aux documents à corriger.

#### API

* Ajout d’un paramètre `with_data` (défini par défaut à `true`) sur la route de **listing des jobs**. Le passer à `false` permet d’alléger la taille de la réponse.
