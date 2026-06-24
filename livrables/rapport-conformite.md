# Rapport de conformité doc ↔ plateforme (staging v26.4.19)

> Vérification multi-agents (54 agents) : chaque page confrontée à la vérité terrain observée en live ([ground-truth-staging.md](ground-truth-staging.md)), chaque divergence contre-vérifiée (adversarial).

## Verdict chiffré
- **31 pages** vérifiées · **131 éléments confirmés conformes** · **12 divergences confirmées** (8 high) · **30 points encore à vérifier en live**.
- Honnêteté : un élément n'est « conforme » que si observé en live. Les 30 points « à vérifier » ne sont **ni** confirmés **ni** infirmés (zones non encore ouvertes au navigateur).

## Divergences confirmées (12)

### ✅ Corrigées (8 — libellés EN vérifiés en live)
| Page | Avant → Après |
|---|---|
| `workflow/creer-un-workflow.md` | « Créer un Workflow » → **Create workflow** |
| `workflow/creer-un-workflow.md` | États « Début/Fin » → **Start/Done** |
| `workflow/creer-un-workflow.md` | « Ajouter une étape » → **Add step** |
| `workflow/creer-un-workflow.md` | « module d'état Terminal » → module **Done** |
| `workflow/creer-un-workflow.md` | « Publier la version » → **Publish version** |
| `extraction/ecran-de-correction.md` | coche « correcte » → bouton **Mark as reviewed** |
| `extraction/ecran-de-correction.md` | « Copier dans le dataset » → **Copy to dataset** |
| `extraction/ecran-de-correction.md` | rejet → précisé bouton **Discard** |

### ✅ Reportées → vérifiées en live (4) — résultat
| Page | Verdict live | Action |
|---|---|---|
| `configurer-les-extracteurs-dun-agent.md` | **Vraie divergence** : top-level = 3 radios **Any / Predefined / Custom** (Date/Entier/Décimal sont des sous-types de *Predefined*). | **Corrigé** : table restructurée en 2 niveaux. |
| `autres/gestion-des-utilisateurs.md` | **Vraie divergence, pire que prévu** : *Create user* propose **5 rôles** (Reviewer, Operator, Expert, Supervisor, Orgadmin), + **Sysadmin** interne (non assignable). La doc disait 2. | **Corrigé** : page réécrite avec les 5 rôles + note Sysadmin/rôles custom. |
| `production/activite.md` | **Faux positif** : la combobox offre bien **daily / Weekly / Monthly** → doc correcte. | Aucune (doc conforme). |
| `production/parametres.md` | **Faux positif** : le bouton est bien **« Download as zip file »** → doc correcte. | Aucune (doc conforme). |

> Bilan des 12 divergences : **10 réelles → toutes corrigées** · **2 faux positifs** (ma vérité terrain initiale était incomplète, pas la doc).

## Conformité par zone
- **Démarrage** : 0 divergence confirmée (mais wizard/flux workflow non vérifiés live).
- **Extraction** : écran de correction corrigé ; Value type à reconfirmer ; entraînement/datasets non vérifiés live.
- **Classification** : **non vérifiable** — la vérité terrain ne couvre rien (tout à vérifier live).
- **Workflow** : `creer-un-workflow` corrigé (libellés EN) ; params des modules + captures legacy à vérifier live.
- **Production** : très bon (Review, Settings alignés) ; 2 nuances low en attente.
- **Autres** : `gestion-des-utilisateurs` — nombre de rôles à arbitrer (+ page globalement obsolète).

## Plan « vers zéro divergence » — 30 écrans à observer en live
Wizard création agent (4 étapes) · menu Add extracteurs + sous-types Value type · création/annotation **dataset extraction** · **entraînement modèle** (extraction & classification) · **lecture résultats classification** (F1/matrice) · **Business Rules** (Interne/Sous-groupe, STP, seuil d'automatisation) · connecteur **Input boîte mail** (IMAP/OAuth, S3/FTP) · validation agent (bouton ajout docs) · params **de chaque module workflow** · transitions (libre/conditionnelle) + panneau de test · Jobs (états, cleanup) · Activity (Weekly/Daily, Classification agent, onglets Classif/Workflows) · Performance (config post-Add, indicateurs) · Settings (**User Roles** prod vs staging, OCR Providers, API Tokens) · périmètres des rôles.

> Détail complet : sortie du workflow `doc-conformity-verify` (run `wf_182a00ee-e7f`).
