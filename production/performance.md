# Performance

L'écran **Performance** permet de construire des tableaux de bord personnalisés autour d'un Workflow ou d'un Agent particulier, regroupant les indicateurs de qualité (précision, rappel, F1) et le volume associé.

## État initial

À la première ouverture, l'écran est vide : aucun tableau de bord n'est préconfiguré. Chaque utilisateur compose ses propres onglets.

<figure><img src="../.gitbook/assets/production_performance_empty.png" alt="Écran Performance sans onglet configuré"><figcaption>État initial &#x2014; aucun tableau de bord n'a encore été ajouté.</figcaption></figure>

## Ajouter un tableau de bord

Cliquer sur **Add your first tab** (ou le bouton **+** une fois le premier onglet créé). Une boîte de dialogue propose trois types :

<figure><img src="../.gitbook/assets/production_performance_add_tab.png" alt="Boîte de dialogue Add New Tab avec les trois types de tableaux de bord disponibles"><figcaption>Choix du type de tableau de bord à ajouter.</figcaption></figure>

| Type | Élément suivi |
|---|---|
| **Workflow Tab** | Un Workflow complet (toutes les étapes) |
| **Extraction Agent Tab** | Un Agent d'extraction spécifique |
| **Classification Agent Tab** | Un Agent de classification spécifique |

Sélectionner le type, cliquer **Add**, puis configurer l'Agent ou le Workflow ciblé.

## Indicateurs disponibles

Les indicateurs varient selon le type d'onglet. Pour obtenir le détail des métriques (précision, rappel, F1, matrice de confusion), voir [Métriques d'évaluation](../autres/metriques-devaluation.md).

## Comparaison avec Activity

[Activity](activite.md) montre le **volume** sur toute l'organisation. **Performance** offre une vue qualitative et ciblée par tableau de bord.
