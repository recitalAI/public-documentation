# Paramètres

L'écran **Settings** regroupe la configuration de l'organisation : langue, OCR, utilisateurs, rôles et tokens API.

## Accès

Dans la sidebar, cliquer sur **Settings**. Cinq onglets sont disponibles :

- **System** — langue de l'application, configuration OCR par défaut, export de l'organisation
- **Users** — liste et gestion des utilisateurs
- **User Roles** — définition des rôles et permissions
- **OCR Providers** — configuration des fournisseurs OCR (Azure, Google, etc.)
- **API Tokens** — création et gestion des jetons d'accès API

## System

<figure><img src="../.gitbook/assets/production_settings_system.png" alt="Onglet System des Paramètres"><figcaption>Settings &#x2014; tab System. Langue, configuration OCR par défaut, et export de l'organisation.</figcaption></figure>

### Language Settings

Définit la langue de l'interface pour tous les utilisateurs de l'organisation. Choix : **English (USA)** ou **French (France)**.

### Default OCR Configuration

Configuration OCR appliquée par défaut à tous les services (extraction, classification, traitement de documents). Options :

- **OCR Provider** : Azure OCR, Google OCR, ou autre fournisseur configuré dans l'onglet [OCR Providers](#ocr-providers)
- **Force OCR** : effectue l'OCR sur tous les documents, y compris ceux déjà searchable
- **Perform OCR on images** : extrait le texte des images (logos, schémas) intégrées au document
- **Automatically rotate pages** : redresse les pages tournées de 90 / 180 / 270° (requiert Force OCR + Google ou Azure OCR)
- **Straighten skewed documents** : corrige les pages scannées avec une légère rotation (requiert Force OCR)
- **Detect checkboxes** : détecte les cases à cocher pour annotation
- **Use latest model** : utilise systématiquement la dernière version du modèle OCR sélectionné

### Export organization

Bouton **Download as zip file** : exporte datasets, modèles, agents, workflows et settings de toute l'organisation dans une archive ZIP. Utile pour les migrations ou sauvegardes.

## Users

> 📸 **Capture requise** : onglet Users avec liste des utilisateurs. À fournir.

Liste les utilisateurs de l'organisation avec leur email et leur rôle. Permet d'inviter de nouveaux utilisateurs, modifier les rôles, ou désactiver des comptes.

## User Roles {#user-roles}

> 📸 **Capture requise** : onglet User Roles avec matrice de permissions. À fournir.

Définit les rôles disponibles (Orgadmin, User, Reviewer, etc.) et les permissions associées (accès Studio, Agents, Workflows, Review, Settings).

Voir [Gestion des utilisateurs](../autres/gestion-des-utilisateurs.md) pour le détail des rôles par défaut.

## OCR Providers

> 📸 **Capture requise** : onglet OCR Providers avec la liste des providers configurés. À fournir.

Configure les fournisseurs OCR disponibles pour l'organisation : Azure OCR, Google Document AI, ou OCR souverain (Tesseract self-hosted). Chaque provider nécessite des credentials propres (clé API, endpoint).

## API Tokens

> 📸 **Capture requise** : onglet API Tokens avec création de token. À fournir.

Gère les jetons d'accès aux APIs reciTAL (extraction, classification, workflow). Permet de créer, lister et révoquer des tokens. Voir [Authentification](../integration-api/authentification.md) pour l'utilisation en pratique.
