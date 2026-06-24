# Plan de corrections — chapitre par chapitre / section par section

> Fusion : audit multi-agents (run w03t4z2z5) + vérité terrain live prod 26.4.19 ([ground-truth-prod-26.4.19.md](ground-truth-prod-26.4.19.md)).
> Légende : 🗑️ placeholder/temporel/lien cassé à **retirer** (sans plateforme) · 🔴 **divergence confirmée live** (corriger) · 🔍 claim **à vérifier live** · ✅ vérifié conforme.

---

## Chapitre 1 — 🚀 Démarrage rapide
### Créer un agent génératif
- 🔍 Wizard 4 étapes (Configuration / Extraction agents / Extraction models / Custom extractors), menu Add (Single data point / Label Group / Extractors), méthodes Model/Rules/Group/Generative, avertissement LLM.
### Créer un workflow
- 🗑️ 11 figcaptions vides.
- 🔍 Save, Test workflow, Publish version, panneau d'erreurs, 7 actions standard.
### Utiliser un modèle d'extraction sur étagère
- 🗑️ Lien cassé `/broken/pages/dZ3...` (carte « Intégrer un Agent par API »).

## Chapitre 2 — 📘 Guide utilisateur

### Design › Studio › Classification
- **Constituer un Dataset** : 🗑️ « WIP » ×2 · 🔍 zip sous-dossiers, « 50aine de docs ».
- **Entraîner un modèle** : 🗑️ 5 figcaptions vides · 🔍 onglet Modèle-Classification, « Créer un modèle de classification/split », « Ne se reposer que sur le contenu », lancer entraînement.
- **Lecture des résultats** : 🗑️ 1 figcaption vide · 🔍 F1 par classe, matrice de confusion, split 80/20.

### Design › Studio › Extraction
- **Constituer un Dataset** : 🗑️ 1 figcaption vide · 🔍 options OCR (Force OCR, Pivoter, Redresser, Tableaux), ordres de lecture, volumes recommandés.
- **Annoter un Dataset** : 🗑️ 1 figcaption vide · 🔍 onglet Étiquettes/AJOUTER, sauts de ligne, étiquette en colonne, annotation au stylo.
- **Entraîner un modèle** : 🗑️ 2 figcaptions vides · 🔍 « 30 min–2 h », 20 % validation, rappel/précision/f1.

### Agents › Classification
- 🔍 Page entière (configurer-un-agent-de-classification) — non vérifiée.

### Agents › Extraction
- **Créer un Agent** : 🔍 wizard 4 étapes, modèles sur étagère, Import extraction agent.
- **Configurer les extracteurs** : 🗑️ 4 figcaptions vides + « Nouvelle fonctionnalité… désormais » · 🔍 Value type (Any/Predefined/Custom), méthodes d'agrégation, options groupe.
- **Configurer les paramètres** : 🗑️ « WIP » + lien cassé `/broken/pages/U2Y...` + 1 figcaption vide · 🔍 note « ne plus utiliser cet écran », Business Rules (Interne/Sous-groupe, STP, N/A→0).
- **Valider un Agent** (charger + valider) : 🔍 Ajouter des documents, écran validation, Docs OK / Précision globale, verrouillage.

### Workflow
- **Créer un workflow** : 🗑️ 11 figcaptions vides · 🔍 2 points d'attache, transition libre/conditionnelle (Use code for transition), Draft après publication, collections File/Email/Attachment.
- **Les modules de workflow** :
  - 🗑️ 1 figcaption vide · lien cassé `/broken/pages/9GN...` · « n'a pas encore été préconçu » (temporel) · **note « Libellés en anglais » à retirer (JB : langue configurable)**.
  - 🔴 **Code** : signature doc `execute_action(job, input)` → réelle **`execute_action(job)`** (1 arg).
  - 🔴 **Extract** : ajouter le param **Include OCR Text** (= « renvoyer le texte OCR » de JB, manquant).
  - 🔍 **Classify** : vérifier params + présence Include OCR Text (JB : manquant aussi).
  - 🔍 Webhook (ajouter Authorization Token, Ignore errors?), Extraction Review (champ « Context » à confirmer — absent en live), + tous les autres modules (Classify Email, Email Classif Review, Split, Unpack, Ingest Email, Cleanup, AI agent/LLM…).
- **Les jobs** : 🗑️ « actuellement » · ✅ onglets Workflows/Jobs/Input/Resources + colonne Status confirmés · 🔍 états started/waiting/done/error/custom, onglets History/Data/Files.
- **Actions workflows standard** : 🔍 les 7 actions (Barcodes, Forward Email, Merge Documents, Send Email, Split Document, Unpack, Workflow).
- **Connexion boîte mail** : 🗑️ STUB entier (🚧 + TODO 5 cases) → contenu réel IMAP à écrire (docx JB).
- **Ressources** : 🗑️ STUB entier (🚧 + TODO 2 cases).

### Production
- **Review** : 🔍 écran de correction (Validate/Discard/Copy to dataset/Mark as reviewed, recherche, badges, sous-groupes…).
- **Activité** : ✅ granularité Monthly/Weekly/Daily + filtres (déjà confirmé) · 🔍 histogrammes, Download CSV.
- **Performance** : 🗑️ « Capture requise » · 🔍 indicateurs précision/rappel/F1, bouton +.
- **Paramètres** : 🔴 **réécriture** — 4 onglets réels **System / Users / OCR Providers / API Tokens**, **PAS d'onglet User Roles** ; retirer les 4 « Capture requise » ; documenter les sections réelles du System (Language, Default OCR Config, Export org, Callback, Recycle Bin, Email/OCR classif, Login, Documentation). ✅ vérifié live.

## Chapitre 3 — 🔌 Intégration API
- **Authentification** : 🔍.
- **Workflow (README + envoyer des documents)** : 🔍.
- **Workflow callback (structure des résultats)** : 🗑️ 🚧 « À reprendre » + 2 figcaptions vides → ajouter mécanisme de callback (JB).
- **Structures classif & extraction** : 🔴 JB : structures JSON ont évolué → récupérer la structure réelle et MAJ.

## Chapitre 4 — 📺 Autres
- **Gestion des utilisateurs** : ✅ colonnes Status/Email/Name/Role + Create user · rôles en usage : Orgadmin/Reviewer/Sysadmin · 🔴 retirer la référence à un « onglet User Roles » (n'existe pas) · 🔍 menu des 5 rôles à la création (bouton Create user bloqué — capture/autorisation à demander).
- **Glossaire / Astuces d'annotation / Métriques d'évaluation / OIDC** : 🔍 à passer en revue (non audités en détail).

---

### Synthèse chiffrée
- 🗑️ **~42 retraits** sans plateforme (placeholders, figcaptions vides, temporels, liens cassés).
- 🔴 **divergences live confirmées** : parametres (User Roles), Code signature, Extract Include OCR Text, gestion-utilisateurs (User Roles tab).
- 🔍 **~130 claims** à confirmer via parcours plateforme (en cours).
