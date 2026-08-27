# LLM et souveraineté

L’utilisation d’IA générative passe par l’usage de grands modèles de langues, les LLM (Large Language Models).

Ces grands modèles de langue sont des modèles d’intelligence artificielle occupant un espace mémoire important et nécessitant des infrastructures spécialisées, notamment l’usage de GPU (Graphical Processing Units, versions spécialisées pour les calculs nécessaires à l’IA des processeurs CPU de nos ordinateurs).

La plateforme reciTAL permet d’utiliser des LLM pour réaliser les tâches d’automatisation (classification, extraction, résumé…). L’usage de LLM entraîne un léger surcoût en plus de la licence d’utilisation de la plateforme. Ce surcoût est calculé sur la base du nombre de tokens utilisés, i.e. de la taille des données envoyées au LLM et du résultat renvoyé. Cette taille est calculée en fonction de la taille de l’image et de la tâche demandée (classification, extraction, résumé…).

Par ailleurs, les LLM peuvent être _opérés_ par des acteurs de différentes nationalités et _hébergés_ à différents endroits

Lorsque l’opérateur est américain, le Cloud Act et le FISA s’appliquent, c’est-à-dire qu’un juge américain peut demander un accès aux données stockées sur ses serveurs, où qu’ils soient, même en Europe donc.

L’hébergement et la localisation des serveurs est également à prendre en compte. Une compagnie européenne opérant des LLM sur le sol étasunien est également soumis aux réglementations US concernant les données.

En somme, pour traiter des données sensibles ou confidentielles, il faut utiliser des LLM hébergés en Europe par des sociétés européennes comme OVH et Scaleway. Les LLM hébergés par ces acteurs sont souvent open-source, donc d’un peu moins bonne qualité que leurs concurrents fermés. Pour les données publiques ou sans contrainte particulière de sensibilité, les LLM proposés par des acteurs américains comme OpenAI ou Google peuvent également être utilisés.

| **Fournisseur de LLM** | **Sensibilité des données** | **Performances** | **Nationalité** |
| ---------------------- | --------------------------- | ---------------- | --------------- |
| Scaleway ou OVH        | Élevée                      | Bonnes           | France          |
| OpenAI ou Google       | Faible                      | Excellentes      | États-Unis      |
