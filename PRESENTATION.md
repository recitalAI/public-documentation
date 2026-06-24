# Refonte doc — récap pour JB

Toutes tes 5 priorités sont traitées et mergées sur `preview`. Pas encore publié en prod.

**Preview cumulative** : https://docs.recital.ai/products/~/revisions/ia1CYzzPm6k66WpV22oh/

## Ce qui a été fait

1. **Réorga Design** — sidebar restructurée selon ta hiérarchie : `Démarrage rapide / Guide utilisateur (Design > Studio, Agents, Workflow ; Production) / Intégration API`.
2. **Pages et sections créées** — Configurer agent classification (existait déjà), Datapoint génératif (section dans Configurer extracteurs), Connexion boîte mail (squelette, attend ton docx), Ressources (squelette, sous Workflow).
3. **Démarrage rapide** — *En quelques clics* (Créer un agent génératif, Créer un workflow) + *Pour aller plus loin* avec cross-refs vers Studio / Agents / Modules / Production.
4. **Intégration API** — ordre Auth → Workflow → Structures de résultats → Swaggers. Section *Nos APIs* fusionnée dedans.
5. **Production** — pages Review (avec écran de correction fusionné), Activité, Performance, Paramètres rédigées avec captures réelles staging + prod.

## Reste à faire

- **Docx Connexion boîte mail** — à intégrer (en attente de ta part).
- **Quelques captures** — bulk Review, dashboard Performance configuré, onglets Settings (Users/Roles/OCR/Tokens).
- **Publication prod** — merge final `preview → main` quand tu valides.
- **Redirects** — 18 anciennes URLs à rediriger vers les nouvelles (liste prête, à appliquer dans GitBook après publication).

## Workflow mis en place

`feature/* → preview → main`. La branche `preview` agrège tout avant publication. La draft PR [#2](https://github.com/marwane-6/Suite-reciTAL---Documentation/pull/2) sert d'anchor pour visualiser l'état cumulé.
