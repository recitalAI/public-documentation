# Configurer les paramètres d'un Agent

## Règles de Gestion

{% hint style="info" %}
Nous conseillons de ne plus utiliser cet écran, et de passer par le [Workflow](../../workflow/les-modules-workflow.md) pour créer et gérer vos règles de gestion dans un module custom.
{% endhint %}

Les règles de gestion permettent de valider ou d'invalider automatiquement des champs extraits. Si tous les champs extraits sont automatiquement validés par des règles de gestion, un document ne passera pas par la phase de vidéo-codage, il sera considéré comme STP (Straight-Through Process).

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

#### Type

<table><thead><tr><th width="148">Type</th><th width="286">Description</th><th>Exemples</th></tr></thead><tbody><tr><td>Interne</td><td>Toutes les règles de gestion qui valident des champs uniques, ou des sommes (SUM) de groupe.</td><td>Montant_HT + Montant_TVA = Montant_TTC</td></tr><tr><td>Sous-groupe</td><td>Toutes les règles de gestion qui valident la cohérence d'un groupe</td><td>LIGNE.PU * LIGNE.Qty = LIGNE.Prix_HT</td></tr></tbody></table>

#### Choix

**Calculer les valeurs N/A comme 0 et remplacer si Vrai :** Si une valeur dans la règle de gestion n'est pas extraite, on vérifie si la règle de gestion est respecté en remplaçant cette valeur par 0.

{% hint style="warning" %}
Les règles de gestion ne concernent que les champs numériques (Nombre entier ou décimal). Tous les champs n'ayant pas le type "Nombre" ne pourront pas être utilisés dans les règles de gestion.
{% endhint %}

## Review configuration

Cet onglet autorise et configure la review (correction humaine) de l'agent : activer la review (**Active review**), générer une URL publique pour corriger un document sans compte (**External validation**), choisir l'ordre de traitement de la file, restreindre la correction à des utilisateurs désignés (**Authorize reviewers**), et sélectionner les extracteurs et groupes affichés sur l'écran de correction.

{% hint style="info" %}
Le détail de ces options et le déroulé de la correction sont décrits dans [Review](../../production/review.md).
{% endhint %}

## Configuration

Cet onglet sert à configurer différents paramètres pouvant influer sur la qualité d’extraction ou le traitement en production des documents.&#x20;

Il est possible d'activer la détection de signature et de QR code (voir [Objets](../../integration-api/extraction/structure-des-resultats-dextraction.md#objets)).

Il est également possible de verrouiller un Agent afin qu'il ne soit pas modifié par accident.



