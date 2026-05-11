---
description: >-
  Introduction des agents génératifs pour l’extraction, Amélioration des écrans
  de revue de classification, Refonte des interfaces de monitoring et de suivi
  des métriques.
---

# Version 25.7.x (01/07/2025)

### Version 25.7.1 (01/07/2025)

#### Extraction - Agents Génératifs

Il est désormais possible de configurer les extracteurs d’un agent d’extraction en s’appuyant sur un **modèle génératif (LLM)**. Ce modèle utilise le **nom** et la **description** de chaque extracteur pour générer automatiquement la logique d’extraction.

{% hint style="warning" %}
Cette approche vise à **compléter**, et non remplacer, les modèles d’extraction entraînés sur des données spécialisées, et se révèle utile dans les cas suivants :

* Difficulté à obtenir des données annotées pour un label spécifique
* Résultats insatisfaisants des modèles traditionnels
* Besoin rapide d’une baseline fonctionnelle
{% endhint %}

La plateforme reciTAL permet l'accès aux LLMs d'OpenAI — d'autres fournisseurs sont en cours d'ajout. Comme indiqué sur le schéma, certains LLM peuvent être finetunés par l'équipe reciTAL pour répondre à des contraintes spécifiques de langues et/ou de formats.

#### Extraction - Autre

{% hint style="danger" %}


L'API a été modifiée concernant la mise à jour d’un agent d’extraction. La route suivante :

```
PUT /extract/api/v1/system_2/document_type/{document_type_id}/
```

n'est **plus utilisée**.

Elle est désormais remplacée par une requête **PATCH** :

```
PATCH /extract/api/v1/system_2/document_type/{document_type_id}/
```
{% endhint %}

* Correction de quelques bugs liés au "re-traitement" d'un document en validation dans un agent d'extraction.
* Amélioration de la détection des QR codes : ajout d’une balise `"text"` dans `"extra"` contenant le contenu déchiffré du QR code.

#### Écran de review d'extraction

* Possibilité de restreindre la copie d’un document en cours de revue à un dataset spécifique.

#### Classification

* Meilleure gestion des formats non pris en charge : un message `"Type de fichier non supporté"` est désormais retourné pour les fichiers `.eml` et `.msg` pour la classification de documents.

#### Écran de review de classification

* Possibilité de zoomer et de réorienter les pages dans le viewer.
* Correction d’un bug empêchant la fermeture des listes déroulantes.
* À la fin d’une correction, le reviewer est automatiquement redirigé vers le document suivant à corriger (au lieu du tableau).
* **Ctrl + clic** sur une page permet de sélectionner toutes les pages de la même catégorie.
* **Drag & drop** activé pour déplacer les pages.
* Un document en cours de revue par un utilisateur est désormais **verrouillé** (icône cadenas) pour les autres utilisateurs.
* Taper “Autre” ou “Other” permet d’accéder directement à toutes les classes additionnelles définies dans l’agent de classification.

#### Workflows

* Recherche de job possible par ID via l’interface.
* Timeout des étapes de code personnalisé étendu de **30s à 1 min**.
* L’étape “Cleanup” ne génère plus de données de sortie.
* Amélioration des vérifications des entrées (inputs) à chaque étape, avec des logs plus détaillés.
* Correction d’un bug lié aux étapes avec l’option **"iterate over input"** sur une liste vide.
* Les étapes de revue disposent désormais de deux nouveaux champs :
  * **Review expiration deadline expression** (format ISO 8601)
  * **Action d’expiration :** Si un document n’est pas corrigé à l’heure définie, il est automatiquement soumis ou rejeté selon l’action spécifiée.

#### Monitoring

* Refonte complète de la page pour un meilleur suivi des consommations liées à la classification, à l’extraction et aux workflows.

#### Métriques

*   Nouvelle page dédiée au suivi des performances par workflow, incluant notamment :

    * Nombre total de jobs traités
    * Nombre de jobs traités **sans intervention humaine (STP)** et leur temps moyen
    * Nombre de jobs **avec intervention humaine** et leur temps moyen

    **Par agent de classification** :

    * % de STP
    * Temps moyen de revue
    * Erreurs les plus fréquentes en revue

    **Par agent d’extraction** :

    * % de STP
    * Temps moyen de revue
    * Score de chaque datapoint et groupe lors des revues
* Ajout d’un bouton **"Rafraîchir"** sur la page pour mettre à jour les métriques en temps réel.
