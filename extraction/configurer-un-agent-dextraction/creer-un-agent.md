# Créer un Agent

Depuis la sidebar, cliquer sur **Agents** puis sur le bouton **Create extraction agent**. Le wizard de création comporte 4 étapes.

## Étape 1 — Agent configuration

Nommer l'agent puis configurer les **langues supportées** et l'**OCR**. La configuration OCR par défaut de l'organisation est utilisée si rien n'est précisé.

<figure><img src="../../.gitbook/assets/demarrage_agent_step1_config.png" alt="Étape Configuration"><figcaption>Étape 1 — langues supportées + configuration OCR.</figcaption></figure>

## Étape 2 — Extraction agents (optionnel)

Importer les extracteurs d'un autre agent existant (équivalent d'une duplication). Utile pour partir d'un agent déjà configuré et le personnaliser sans casser l'original.

<figure><img src="../../.gitbook/assets/demarrage_agent_step2_agents.png" alt="Étape Extraction agents"><figcaption>Étape 2 — sélection d'agents sources pour reprise des extracteurs.</figcaption></figure>

## Étape 3 — Extraction models (optionnel)

Attacher un ou plusieurs modèles d'extraction entraînés dans Studio. C'est ici qu'on choisit un modèle sur étagère (Facture, RIB, CNI, KBIS, etc.) ou un modèle custom préalablement entraîné.

<figure><img src="../../.gitbook/assets/demarrage_agent_step3_models.png" alt="Étape Extraction models"><figcaption>Étape 3 — modèles disponibles avec leur précision indicative.</figcaption></figure>

## Étape 4 — Custom extractors

Confirmer la création de l'agent. La configuration des extracteurs (datapoints, groupes, datapoint génératif) se fait ensuite sur la page dédiée — voir [Configurer les extracteurs d'un Agent](configurer-les-extracteurs-dun-agent.md).

<figure><img src="../../.gitbook/assets/demarrage_agent_step4_extractors.png" alt="Étape Custom extractors"><figcaption>Étape 4 — finalisation et bascule vers la configuration des extracteurs.</figcaption></figure>

## À noter

- Les étapes 2 et 3 sont **optionnelles** : on peut créer un agent vide et lui ajouter des extracteurs custom à la main (utile pour un agent purement génératif basé sur des prompts).
- Le bouton **Import extraction agent** (à côté de Create) permet d'importer un agent depuis un export `.zip` (cas de migration ou de duplication cross-organisation).
