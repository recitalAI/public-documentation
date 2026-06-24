# Audit de conformité doc ↔ plateforme (staging v26.4.19)

> Généré par le workflow `doc-conformity-audit` (30 agents) — plan de travail pour rendre la doc 100 % conforme.
> Stats : **21 écrans · 118 claims · 89 high severity · ~38 captures à refaire**.

## Synthèse des risques par zone

### Workflow (zone la plus à risque)
- **Steps Library** : `demarrage-rapide/creer-un-workflow.md` annonce « 16 catégories » (✅ vrai : 16) mais en liste 11, et « **83 modules** » → **faux, c'est 22** (vérifié en live).
- **Deux taxonomies concurrentes** : `les-modules-workflow.md` (types d'action / media / type) vs catégories de la Steps Library → à réconcilier.
- **Libellés réels (EN) ≠ doc (FR)** : Start/Done vs Début/Fin · Create workflow · Add step · Publish version · Ingest Email · Classify Email · Extract · llm · Custom Code · Classification/Extraction Review · Ensemble Validation.
- **Modules absents de `les-modules`** : Email Classification Review, Cleanup (+ Barcodes/Forward Email/Send Email).
- **Périmé** : « Validation d'ensemble en cours de développement » → **Ensemble Validation existe** ; S3/FTP « à venir » ; cleanup « actuellement » ; `connexion-boite-mail.md` = stub.
- **Toutes les captures workflow sont legacy** `image (N).png`.

### Extraction
- Wizard 4 étapes (libellés à confirmer) ; option « Se replier sur valeurs génératives » marquée **WIP** ; hint « nouvelle fonctionnalité génératif » (temporel) ; nombreuses captures legacy (Extracteurs, Règles de Gestion, Dataset figcaptions vides, annotation, modèles).

### Classification
- Sections datasets documents/mails marquées **WIP** ; toutes captures legacy ; case « Ne se reposer que sur le contenu » + orientation matrice de confusion à confirmer.

### Production
- Settings : 5 onglets (✅ vérifié) mais **captures manquantes** (Users, User Roles, OCR Providers, API Tokens) ; Performance : dashboard configuré manquant ; **`autres/gestion-des-utilisateurs.md` obsolète** (rôles « Administrateur/Correcteur » + capture datée 2023) à réconcilier avec Settings.

### Démarrage
- Source des incohérences workflow (16 cat / 83 modules) — page d'onboarding à arbitrer en priorité.

**Priorité de relecture** : (1) Steps Library / taxonomie modules · (2) cohérence EN/FR éditeur workflow · (3) statuts WIP (Règles de Gestion, datasets classif, connecteurs Input) · (4) refonte captures legacy · (5) page gestion-des-utilisateurs.

## Écrans à vérifier (21)

| Écran | urlPath | #claims | Pages dépendantes |
|---|---|---|---|
| Liste agents d'extraction | /agents | 5 | creer-un-agent-generatif, creer-un-agent, valider-un-agent |
| Wizard création agent (4 étapes) | /agents | 6 | creer-un-agent-generatif, creer-un-agent |
| Agent — onglet Extracteurs | /agents/<id>/extractors | 9 | creer-un-agent-generatif, configurer-les-extracteurs |
| Agent — onglet Paramètres | /agents/<id>/configuration | 8 | configurer-les-parametres, valider-un-agent, review |
| Écran de correction (vidéo-codage) | /review | 8 | ecran-de-correction, valider-un-agent, review, gestion-utilisateurs |
| Review (file d'attente) | /review | 4 | review, ecran-de-correction |
| Validation d'un agent | /agents/<id>/validation | 3 | charger-des-documents-en-validation, valider-un-agent |
| Liste des workflows | /workflows | 4 | creer-un-workflow (×2), introduction |
| Éditeur de workflow | /workflows/<id> | 10 | creer-un-workflow (×2), introduction, jobs |
| Steps Library | /workflows/<id> | 6 | creer-un-workflow, actions-workflows-standard, les-modules, introduction |
| Config des modules workflow | /workflows/<id> | 11 | les-modules-workflow, creer-un-workflow |
| Workflow — onglet Input | /workflows | 7 | inputs, introduction, connexion-boite-mail |
| Workflows — onglet Jobs | /workflows | 8 | jobs, introduction |
| Classification — Datasets | /classification | 2 | constitution-des-datasets |
| Classification — Modèles | /classification | 5 | entrainement-du-modele, lecture-resultats |
| Studio — Dataset extraction | /studio/datasets/<id> | 5 | constituer-un-dataset, annoter-un-dataset |
| Studio — Modèles extraction | /studio | 4 | entrainer-un-modele |
| Activity | /activity | 5 | activite |
| Performance | /production/performance | 4 | performance |
| Settings | /settings | 9 | parametres, gestion-utilisateurs, constituer-un-dataset |

> Détail complet des 118 claims : sortie du workflow `doc-conformity-audit` (run `wf_dcb36f7a-f28`).
