# Vérité terrain — Production extract.recital.ai (Version 26.4.19)

> Observé en live via Playwright le 2026-06-23. Org 149 PROD. Sert de base pour corriger la doc (zéro info non plateforme).

## Settings (`production/parametres.md`)

**4 onglets seulement : System · Users · OCR Providers · API Tokens.**
🔴 Il n'existe **PAS** d'onglet « User Roles ». → Retirer toute référence à un onglet User Roles (placeholder `parametres.md`, renvoi `gestion-des-utilisateurs.md`).

### Onglet System
Sections, dans l'ordre :
1. **Language Settings** — « Set the application language preference for all users in this organization. » Radios : **English (USA)** (coché) / **French (France)**. ✅ confirme : langue configurable (point JB).
2. **Default OCR Configuration** — « Configure the default OCR settings used across the entire platform… »
   - **OCR Provider** (dropdown) : Azure OCR (valeur courante).
   - Cases : **Force OCR** (Perform document OCR on all instances, including searchable documents) · **Perform OCR on images** (Extract text from the images e.g. logos) · **Automatically rotate pages** (requires Force OCR and either Google OCR or Azure OCR) · **Straighten skewed documents** (requires Force OCR) · **Detect checkboxes** · **Use latest model**.
3. **Export organization** — « Download datasets, models, agents, workflows and organization settings. » Bouton **Download as zip file**. ✅ (doc correcte)
4. **Callback settings** — « Configure the callback to be used by extraction agents at the end of the extraction process. » Champs : **URL** (préfixe http://), **Token**, **Custom authorization header** + bouton **Save**.
5. **Recycle Bin settings** — « Configure the retention period for extraction agents and datasets in the recycle bin. » Radios **1 week** (coché) / **30 days**.
6. **Email and OCR settings for classification** — « Configure the inclusion of attachments and the retention time of emails. Also configures the use of OCR (legacy). » : **Read attachments** (case) · **Maximum number of attachments to read** (10) · **Mail retention time (in months)** (2) · **Use OCR** (case) · **Use Google OCR** (case) · **Maximum number of OCRized pages** (3).
7. **Login settings** — « Configure the login methods for the organization. » : **Email and password authentication** (case cochée) · **Bind Access Tokens to IPs** (case) · **azure-oidc** (case cochée) · bouton **Add custom login provider** · **Custom login URL** (https://extract.recital.ai/login/org/ + nom org + Save).
8. **Documentation** — « Access the API documentation of the running services. » Liens : **Extract API**, **Classify API**, **Workflows API**, **Authenticator API**.

### Onglet Users
- Barre **Filter** + bouton **Create user**.
- Colonnes : **Status · Email · Name · Role**.
- Rôles réellement présents dans la liste : **Orgadmin** (majorité), **Reviewer** (Jiaqi Chang), **Sysadmin** (Clementine Gross).
- ✅ **Menu Role vérifié en live** (via Actions → Edit d'un utilisateur) : il ne propose que **DEUX** rôles intégrés — **Reviewer** et **Orgadmin**. PAS 5. Un champ distinct **user_role** (« select_user_role ») gère les rôles personnalisés. **Sysadmin** visible dans la liste mais **non** proposé dans le menu Role (interne).
- Menu **Actions** par ligne = **Edit / Reset password / Delete**. Edit ouvre un formulaire (Role, user_role, Status Open/Blocked).
- Pagination 20/page, 2 pages.

### Onglet OCR Providers
- Bouton **Add OCR Provider**.
- Colonnes : **Name · Provider Type · Endpoint · Max QPS**.
- Lignes système : **Google OCR** (GOOGLE, System) · **Azure OCR** (AZURE, System, endpoint cognitiveservices, Max QPS 15) · **DocTR OCR** (DOCTR, System, https://rocr.recital.ai/ocr).

### Onglet API Tokens
- Bouton **Generate API Token**.
- Colonnes : **Name · Token**.
- 1 token « Unnamed token » (JWT).

## Modules de workflow (`workflow/les-modules-workflow.md`)

> Observé dans l'éditeur via le workflow `sample_workflow` (actif, verrouillé). Étapes : start → **Financial statement extraction** (Extract) → **Code for automatic verification** (Code) → **Webhook** → **Extraction review** → done. Tous les modules ont une zone **Advanced options** repliable.

### Module Extract (heading « Extract », type « Action »)
- **Name**
- **Extraction agent** (Select extraction agent)
- Advanced options :
  - **Input expression** : `files['file']` (défaut)
  - **Extraction agent expression**
  - **Output key** : `extract` (défaut)
  - ☐ **Iterate over input**
  - ☐ **Include OCR Text** ← 🎯 c'est le « renvoyer le texte OCR » de JB. Libellé exact = **Include OCR Text**. À documenter (manquant).

### Module Webhook (heading « Webhook », type « Action »)
- **Name**, **URL**, **Authorization Token**, ☐ **Ignore errors?**, ☐ **Retry on error?**
- Advanced options : **Input expression** (`data`), **URL expression**, **Method**, **Authorization Type**, **Authorization Header Name**, **Authorization Parameter Name**, **Output key** (`webhook`), ☐ **Iterate over input**.

### Module Extraction Review (heading « Extraction Review », type « Action »)
- **Name**
- Advanced options : **Input expression** (`data['extract']`), **Review expiration deadline expression**, **Expiration action** (combobox, valeur `validate`), **Output key** (`review`), ☐ **Iterate over input**.
- ⚠️ Pas de champ « Context/Contexte » visible ici. La doc mentionne un param « Contexte » → à vérifier (peut-être absent ou ailleurs).

### Module Code (heading « Code », sous-titre « Python Code »)
- **Name**
- Éditeur Python, contenu par défaut :
  ```python
  def execute_action(job):
      return StepActionType.done, {
          # Add data here
      }
  ```
- 🔴 Divergence doc : `les-modules-workflow.md` écrit `def execute_action(job, input)` (2 args). Le vrai est **`def execute_action(job)`** (1 arg), retour = tuple `(StepActionType.done, {…})`.
- Advanced options (non déplié).

### Module Classify (heading « Documents classification agent », type « Action »)
> Workflow `CLASSIFY REVIEW` : Start → Documents classification agent (Classify) → Classification Review → Done.
- **Name**
- **Classification agent** (ex. QATEST) + bouton Clear
- ☐ **Per page**
- Advanced options : **Input expression** (`files['file']`), **Classification Agent Expr**, **Output key** (`classify`), ☐ **Iterate over input**, ☐ **Include OCR Text**.
- ✅ JB confirmé : **Include OCR Text** présent aussi pour Classify (manquant dans la doc).
- 🔴 Divergence doc : `les-modules-workflow.md` cite « Use Google OCR » pour le module Classify → **absent** en live (le param OCR est désormais **Include OCR Text**). Le « (SAM) » accolé à « Per page » n'apparaît pas non plus (libellé = juste **Per page**).

### Module Classification Review (heading « Classification Review », type « Action »)
- **Name**, **Context** (textbox).
- Advanced options : **Input expression** (`data['classify']`), **Context expression**, **Expiration deadline expression**, **Expiration action** (combobox, `validate`), **Output key** (`classify`), ☐ **Iterate over input**.
- Résout l'incertitude « Contexte » : ce champ existe pour **Classification Review** (et pas pour Extraction Review, qui n'a que Name + Advanced).

### À vérifier encore (non couverts)
- Modules **Classify Email**, **Email Classification Review**, **Split**, **Unpack**, **Ingest Email**, **Cleanup**, **AI agent / LLM**, etc.

## Structure JSON des résultats d'extraction (`integration-api/extraction/...`)

> Capturée en live via l'onglet **Data** d'un job (workflow → module Extract, clé de sortie `extract`). Org 149, doc FACTURE. **Version 26.4.19.**

La sortie du module Extract est rangée sous sa **clé de sortie** (`extract` par défaut) :

```json
{
  "extract": {
    "doctype": 4635,
    "job": 2791354,
    "result": {
      "id": 2791354,
      "name": "20260609-150257_106750_FACTURE_0.pdf",
      "review_details": {
        "verified_by_id": null,
        "verified_by": "unknown",
        "verified_at": null,
        "opened_at": null,
        "manual_corrections": 0,
        "reviewer_comment": null
      },
      "status": "in_workflow",
      "number_of_pages": 2,
      "values": [
        {
          "data_point_id": 101390,
          "data_point_name": "NUMERO_FACTURE",
          "value": {
            "origin": "custom_entity",
            "confidence": 0.9997750520706177,
            "status": "invalid",
            "page_nb": 2,
            "value": "K3020535",
            "location": { "x_min": 0.5, "x_max": 0.63059, "y_min": 0.12441, "y_max": 0.13852 },
            "valid_page_nb": 2,
            "valid_value": "modif",
            "valid_location": { "x_min": 0.5, "x_max": 0.63059, "y_min": 0.12441, "y_max": 0.13852 },
            "normalized_value_type": "string",
            "normalized_value": "modif",
            "prevalidated": false,
            "extra": { "confidence": 0.9997750520706177, "tables": [] }
          },
          "position": 0,
          "intermediate": false
        }
        // … un objet par data_point
      ]
    }
  }
}
```

**Schéma d'une valeur (`values[]`)** :
- `data_point_id` (int), `data_point_name` (str), `position` (int), `intermediate` (bool).
- `value` :
  - `origin` (ex. `custom_entity`), `confidence` (0–1), `status` (`pending` / `invalid` / …), `page_nb` (int|null), `value` (str|null brut extrait).
  - `location` : bbox normalisée `{x_min, x_max, y_min, y_max}` (0–1) ou `null`.
  - `valid_page_nb`, `valid_value`, `valid_location` : remplis après **correction humaine** (sinon `null`). Ex. valeur corrigée → `valid_value: "modif"`, `status: "invalid"`.
  - `normalized_value_type` (`string` / `datetime` / …), `normalized_value` (ex. date ISO `2026-06-08T00:00:00`).
  - `prevalidated` (bool|null), `extra` (`{confidence, tables[]}`|null).
- `result` : `id`, `name`, `review_details {verified_by_id, verified_by, verified_at, opened_at, manual_corrections, reviewer_comment}`, `status` (`in_workflow`…), `number_of_pages`, `values[]`.

### Structure JSON classification (job de test 1008416, CLASSIFY REVIEW)
> Job de test lancé (PDF manuel) → étape Classify **completed**, Classification Review **waiting**. Data accumulée :
```json
{
  "classify": {
    "classification_agent": "QATEST",
    "context": null,
    "job": 174114,
    "result": null,
    "expiration_deadline": null,
    "on_expiration_action": "validate"
  }
}
```
- C'est le **wrapper workflow** du module Classify (clé de sortie `classify`). `result` est **null** tant que la Classification Review n'est pas validée → le détail `prediction` (probabilities/label) n'a **pas** pu être capturé en live.
- La doc `integration-api/classification/...` documente ce **`result`/prediction** (Classification simple + Déliassage) : structure détaillée et plausible, mais **non revérifiée en live** (result null). Ne pas réécrire sans exemple complet → valider la review du job test OU demander un exemple à JB.

### Structure JSON extraction — VÉRIFIÉE conforme
- La doc `integration-api/extraction/structure-des-resultats-dextraction.md` correspond au live : `review_details` (mêmes champs), `status`, `number_of_pages`, `values[].value{origin, confidence, status, page_nb, value, location{bbox}, valid_*, normalized_value_type, normalized_value, prevalidated, extra}`. ✅ Pas de réécriture.
- Deltas mineurs (contexte job-data vs callback) : live `position` = int (doc dit null), live `extra` = `{confidence, tables[]}` (doc dit null), `business_rule_strings` non vu dans la job-data (présent dans la doc/callback). → non bloquant.

### Divergence mineure modules Review (input expression)
- Doc `les-modules-workflow.md` : Classification/Extraction Review « Expression d'entrée : files['file'] ». Live : **Classification Review** = `data['classify']`, **Extraction Review** = `data['extract']` (= sortie de l'étape précédente). À corriger lors d'une passe conformité.

