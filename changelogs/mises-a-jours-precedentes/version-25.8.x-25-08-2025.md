---
description: >-
  Optimisation de l’OCR Azure, Refonte de l’éditeur de code dans le workflow,
  amélioration de la review de classification, début des travaux sur la refonte
  UI de la plateforme
---

# Version 25.8.x (25/08/2025)

### Version 25.8.7 (25/08/2025)

#### Extraction

* Ajout d’une option « retraitement global » dans la table de validation des agents d’extraction
* Amélioration de la détection des QR codes
* Normalisation de la partie horaire des dates\
  &#xNAN;_⚠️ Breaking change : l’heure est supprimée de la valeur normalisée si elle n’est pas extraite (au lieu de T00:00:00)_
* Création d’une règle métier utilisant un groupe d’extraction désormais fonctionnelle
* Correctif du bouton « normaliser en fonction de la réponse » dans la configuration des règles métier

#### Écran de review d'extraction

* Affichage du nom du Workflow dans les tables de revue de documents
* Mise en place d’une fonctionnalité de date limite d’expiration de revue

#### Écran de review de classification

* Conserver le focus sur la page sélectionnée lorsque le visualiseur de page est ouvert ou fermé
* Affichage du nom du Workflow dans les tables de revue de classification.
* Correction de la sérialisation de la date/heure dans le workflow d’extraction review
* Sélection de pages d’une même catégorie avec `Ctrl+clic` dans la revue de classification
* Support du réordonnancement des pages de documents de classification par glisser-déposer
* Mise en place d’une fonctionnalité de date limite d’expiration de revue
* Correctif de l’interface : récupération du "document suivant" dans la revue de classification

#### Workflows

* Refonte du système de filtrage sur la page des jobs. Filtres disponibles :
  * ID
  * Statut (standard et custom)
  * Test (Oui on Non)
  * Job "enfant" (sous-workflow)
* Filtres à venir :
  * Date
  * Nom de document
* Activation de l’éditeur Python en mode lecture seule
* Ajout de `custom_metadata` aux données des Jobs pour la rétrocompatibilité
* Nouvel éditeur Python dans les Workflows
* Correction de la gestion des cas particuliers lors du découpage de PDF (Etape "Split Document")

#### OCR

* Accélération des appels Azure OCR
* Le texte OCR peut désormais être récupéré après les étapes de classification ou d’extraction dans le workflow

#### UI/UX

* Refonte de la navigation latérale avec une section de profil améliorée et une disposition d’icônes intuitive
* Suppression de la longueur minimale pour les noms de Dataset et de modèles afin d’uniformiser les restrictions de nommage dans l’application
* Renommage de « Monitoring » en « Activité » et de « Metrics » en « Performance »

#### Autre

* Amélioration de la traçabilité des KPI et de la visibilité des métriques des extracteurs
* Correction du support des domaines avec chemins dans les fournisseurs OpenID
