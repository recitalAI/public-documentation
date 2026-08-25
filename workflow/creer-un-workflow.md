---
description: Rendez-vous dans Workflows
---

# Créer un Workflow

## Créer un nouveau workflow

Cliquez sur **Create workflow** puis donnez-lui un nom.

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Un workflow doit obligatoirement être composé au minimum d'un état **Start** et d'un état **Done**.
{% endhint %}

## Ajouter des actions

Pour ajouter des actions entre le début et la fin d'un workflow, cliquez sur le bouton **Add step**.

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

Nous avons une page dédiée à la liste de tous les modules disponibles : [Les modules Workflow](les-modules-workflow.md).

## Ajouter des transitions

Une transition entre 2 modules peut être créée en reliant leurs points d'attache.

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

Un module peut avoir plusieurs transitions sortantes : le workflow se divise alors en autant de branches, qui peuvent se rejoindre plus loin sur le module **Done**.

<figure><img src="../.gitbook/assets/workflow_transitions_multiples.png" alt="Un module avec trois transitions sortantes"><figcaption>Le module <strong>Aiguillage</strong> a trois transitions sortantes, vers trois branches distinctes.</figcaption></figure>

Une transition peut être libre (par défaut) ou bien conditionnelle. Pour créer une transition conditionnelle, cliquez sur la transition, cochez la case "Use code for transition", et écrivez en code Python la condition à respecter pour passer par cette transition.

<figure><img src="../.gitbook/assets/workflow_transition_conditionnelle.png" alt="Panneau de configuration d'une transition conditionnelle"><figcaption>Transition conditionnelle : un nom, la case <strong>Use code for transition</strong> cochée, et la condition Python. Le nom de la transition s'affiche sur le lien dans le canvas.</figcaption></figure>

## Terminer un workflow

Chaque branche d'un workflow doit se terminer par le module **Done**.

Une fois que l'architecture d'un Workflow est valide, on peut le tester et le publier.

### Publier un workflow

Publier un workflow permet de le tester, et de l'utiliser en production. Pour ce faire, cliquez sur le bouton **Publish version**.

<figure><img src="../.gitbook/assets/image (111).png" alt=""><figcaption></figcaption></figure>

Une fois qu'un workflow est publié, on peut toujours retourner en mode Draft. Tant que le nouveau Draft ne sera pas publié, il n'aura aucun impact sur la production.

### Tester un workflow

Une fois que votre Workflow est publié, vous pouvez le tester.

<figure><img src="../.gitbook/assets/image (125).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (126).png" alt=""><figcaption></figcaption></figure>

**Donnée Initiale :**\
Chaque étape du workflow génère des données qui peuvent être utilisées au sein même de ce workflow et que l'on retrouve dans les résultats finaux. Il est également possible de transmettre des données initiales avant même la première étape. Un champ au format JSON est attendu.

{% hint style="info" %}
Transmettre des données initiales peut être très pratique pour les transitions conditionnelles, notamment si vous savez à l'avance dans quelle branche le workflow doit se diriger (par exemple, effectuer un vidéo-codage ou non).
{% endhint %}

**Métadonnées personnalisées :**\
Des données sous forme de chaînes de caractères, non exploitables dans le workflow, mais renvoyées telles quelles dans les résultats.

{% hint style="info" %}
Ce paramètre est souvent utilisé pour transmettre des identifiants internes (ID), afin de les retrouver dans les résultats et d'associer ces derniers à l'ID correspondant.
{% endhint %}

**Fichiers :**\
Les documents qui seront envoyés dans le workflow. Trois collections par défaut sont disponibles :

* **File :** Utilisée par la plupart des modules par défaut.
* **Email :** Utilisée par le module "Ingest Email" par défaut.
* **Attachment :** Non utilisée par défaut par aucun module, mais pratique dans certains cas spécifiques, comme tester une partie du workflow.
