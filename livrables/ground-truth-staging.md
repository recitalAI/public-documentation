# Vérité terrain — staging.recital.ai (v26.4.19)

> Faits observés EN LIVE pendant la session (UI en anglais, org Studio). Sert de référence pour l'audit de conformité.
> Tout ce qui n'est PAS ici n'a PAS été vérifié en live → à marquer "à vérifier".

## Global / navigation
- Sidebar : **Studio, Agents, Workflows, Review, Activity, Performance, Settings**.
- Accueil : « Good morning, <prénom> » ; cartes **Studio** (Document datasets, Email datasets, Classification models, Extraction models), **Agents** (Extraction agents, Classification agents), **Workflows** (Workflows, Jobs), **Review** (Extractions to review, Document classifications, Email classifications).
- Langue de l'UI = **anglais** (réglage org "English (USA)"). La **page 404** s'affiche en **français** (« Cette page est introuvable »).
- Version affichée en pied de page : **26.4.19**.

## Review — file d'attente (`/production/review/extraction`)
- 3 onglets : **Extraction / Classification / Emails** (badge = nombre en attente).
- Champ de recherche : placeholder **« Type agent name »**.
- Table agents : colonnes **« Extraction agents | Documents to review »**.
- Liste docs d'un agent : colonnes **checkbox, Document name, workflow, Size, Added on, Processed on** ; champ **« Filter by name »**.

## Review — écran de correction (`/production/review/extraction/<id>/file/<fid>`)
- En-tête : **« All reviews »** (retour) ; pager **« 1 / 1 »** + flèches ; menu déroulant du nom de fichier.
- Barre d'actions (4 boutons, tooltips confirmés) : **Validate, Discard, Copy to dataset, Add comment**.
- Par champ : zone éditable + bouton **« Mark as reviewed »** (coche bleue) → **coche verte** une fois vérifié ; **« Revert to extracted value »** (flèche circulaire) si la valeur a été modifiée.
- Groupes répétables : **EMETTEUR_ADRESSE, CLIENT_ADRESSE, LIGNE, DETAIL_TVA** ; bouton **« Add subgroup »**.
- Visualiseur : **« Show detected text »** (¶, haut gauche), **« Search text »** (loupe haut droite, id `docSearchButton`), **miniatures** (colonne droite), **zoom +/−**, **rotation**, **« Add page to dataset »** (bas droite).
- Modale « Add page to dataset » : « Select a dataset to copy page to: » + sélecteur + case **« Generate labels and prefill annotations from the extraction models associated with the extraction agent »** + boutons **Cancel / Save**.

## Agents — liste (`/agents/extraction`)
- Onglets : **Extraction / Classification**.
- Colonnes : checkbox, **Extraction agent, Docs validated, Docs OK, Global precision**, Actions.
- Boutons : **« Create extraction agent »**, **« Import extraction agent »**.

## Agent d'extraction — détail (`/agents/extraction/<id>`)
- Onglets : **Validation, Extractors, Business Rules, Review configuration, Configuration**.
- **Extractors** : colonnes **« Data point | Value type | Precision | Extraction model »** ; boutons **« Add »**, **« Replace extraction model »**. Table groupes : **« Label group | Labels | Precision | Extraction model »**.
- **Panneau config d'un data point** : Name, Description, bouton « Generative copy » ; **Value type** (radios **Any / Predefined / Custom**) ; **Extraction method** (radios **Model / Rules / Group / Generative**) ; cases **« Replace value by text of surrounding paragraph »** et **« …surrounding table cell »** ; **Select model** + version + **Select label** ; **Save**.
- **Review configuration** : **Active review** (« Activate human review for extraction under the automation threshold ») ; **External validation** (« Share a URL with users without an account for review » + « Token valid for … hours ») ; **Allow anonymous copy to dataset** (+ sélecteur de dataset) ; **Auto validated review order** (« Show N/A values first, followed by incorrect, pending, and correct values ») ; **FIFO review order** (« Review documents in the order they were added to the queue ») ; **Only show pages with pending values** ; **Authorize reviewers** (« Authorize document review to specific non-admin users ») ; **Automatically delete documents in review** (« Keep documents for … days »). Tables : **« Extractors displayed in review »** (Data point | Display) et **« Label group »** (Display | As table).
- **Configuration** : **Lock extraction agent** ; **Generative AI settings** (Prompt prefix, Use text, LLM Provider [ex. openai], LLM Model [ex. gpt-4.1-2025-04-14], Temperature 0–1, Reasoning) ; **OCR configuration** (OCR Provider, Force OCR, Perform OCR on images, Automatically rotate pages, Straighten skewed documents, Detect checkboxes, Use latest model) ; **Processing** (Delete documents after processing, Enable automatic retry, Detect signatures, Detect QR code [+ pages], Split entities overlapping tables) ; **Normalization** (Supported languages) ; **Prevalidation** (Prevalidation of rule-based data points) ; **Callback** (Extraction results format : List/Array vs Dict/Object).
- ⚠️ **Aucun champ « seuil d'automatisation » trouvé** ni dans Configuration ni dans le panneau extracteur.

## Workflows (`/workflows/workflows`)
- Onglets : **Workflows / Jobs / Input / Resources**.
- Liste workflows : colonnes checkbox, **Workflow, Status** (Draft/Active), **Date created, Updated on**, Actions ; boutons **« Create workflow »**, **« Import workflow »**.

## Éditeur de workflow (`/workflows/workflows/configure/<id>`)
- Canvas avec **Start / Done** + étapes ; boutons **« Add step »**, **« Publish version »**, **« Test workflow »**.
- **Steps Library** (via Add step) : titre « Steps Library », champ « Filter by name ». **Catégories (16)** : **All(22), Action(7), AI agent(5), Archive(1), Automation(3), Classification(4), Code(1), Document(8), Email(6), Extraction(4), Generation(1), Input(1), Output(2), Post-processing(2), Review(3), State(3), Validation(1)**.
- **22 modules** (libellés exacts) : Barcodes, Classification Review, Classify Email, Cleanup, Custom Code, Documents classification agent, Done, Email Classification Review, Ensemble Validation, Extract, Extraction Review, Forward Email, Ingest Email, llm, Merge Documents, Send Email, Split Document, Start, State, Unpack, Webhook, Workflow.

## Jobs (`/workflows/jobs`)
- Colonnes : **Job ID, Date created, Test, Workflow, Step Name, State**, Actions.
- Bascule **Live / Test** ; filtres : statut, **From / To** (dates), **« Filter by job id or file name »**.
- Détail job : en-tête (Job ID, Workflow, Current Step, State, Created on, Updated on) + bouton **« Restart »** ; onglets **History** (+ bascule « Show detailed history? »), **Data** (JSON + « Show initial data? »), **Files** (colonnes Date, Collection, Name, Is Initial?).

## Activity (`/production/activity/extract`)
- Onglets : **Extraction / Classification / Workflows**.
- Sélecteur de dates (≈ 12 mois par défaut) ; **granularité** (combobox, ex. « Monthly ») ; filtres **Workflow / Extraction agent** ; bouton **« Download »**.
- Onglet Extraction : **3 histogrammes** — **« Total number of pages », « Number of useful pages », « Documents »**.

## Performance (`/production/performance`)
- État vide : **« No tabs available »**, **« Add a tab to get started »**, bouton **« Add your first tab »**.
- Dialogue **« Add New Tab »** : « Select the type of tab you want to add: » radios **Workflow Tab / Extraction Agent Tab / Classification Agent Tab** ; boutons **Cancel / Add**.

## Settings (`/settings/system`) — STAGING
- Onglets : **System, Users, OCR Providers, API Tokens**. ⚠️ **Pas d'onglet « User Roles » en staging** (alors qu'il EXISTE en prod extract.recital.ai). → divergence d'environnement à arbitrer.
- **System** : Language Settings (**English (USA) / French (France)**) ; Default OCR Configuration (OCR Provider, Force OCR, Perform OCR on images, Automatically rotate pages, Straighten skewed documents, Detect checkboxes, Use latest model) ; Export organization (Download as zip) ; Callback settings ; Recycle Bin settings (**1 week / 30 days**) ; Email and OCR settings for classification ; Login settings (Email and password authentication, Bind Access Tokens to IPs, providers OIDC) ; Documentation (liens API Extract/Classify/Workflows/Auth).
- **Users** : colonnes **Status, Email, Name, Role** ; bouton **« Create user »**. Rôles observés : **Orgadmin, Reviewer, Sysadmin**.

## NON vérifié en live cette session (→ "à vérifier", ne pas affirmer conforme)
- Wizard de création d'agent (les 4 étapes en détail).
- Panneaux de **paramètres de chaque module workflow** (Ingest Email, Extract, Classify, llm, Review, Split, Unpack, Merge, Webhook, Custom Code…).
- Studio : création/annotation de **dataset d'extraction**, **entraînement modèle d'extraction**.
- Classification : création **dataset**, **entraînement modèle**, **lecture des résultats** (F1, matrice de confusion).
- Workflow **Input** : config connecteur boîte mail (IMAP, OAuth, S3/FTP).
- Validation d'un agent (chargement docs de test).
- Settings : onglets **OCR Providers**, **API Tokens** (détails), **Users/User Roles** (matrice de permissions).
