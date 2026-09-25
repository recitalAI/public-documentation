---
description: Rendez-vous dans Workflows
---

# Créer un Workflow

## Créer un nouveau Workflow

Cliquez sur **Create workflow** puis donnez-lui un nom.

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Un Workflow doit obligatoirement être composé au minimum d'un état **Start** et d'un état **Done**.
{% endhint %}

## Ajouter des actions

Pour ajouter des actions entre le début et la fin d'un Workflow, cliquez sur le bouton **Add step**.

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

Nous avons une page dédiée à la liste de tous les modules disponibles : [Les modules Workflow](les-modules-workflow.md).

## Ajouter des transitions

Une transition entre deux modules peut être créée en reliant leurs points d'attache.

<figure><img src="../.gitbook/assets/image (3).png" alt="Transition entre deux modules"><figcaption><p>Transition entre deux modules reliée par leurs points d'attache.</p></figcaption></figure>

Un module peut avoir plusieurs transitions sortantes : le Workflow se divise alors en autant de branches, qui peuvent se rejoindre plus loin sur le module **Done**.

<figure><img src="../.gitbook/assets/workflow_transitions_multiples.png" alt="Un module avec trois transitions sortantes"><figcaption><p>Le module <strong>Aiguillage</strong> a trois transitions sortantes, vers trois branches distinctes.</p></figcaption></figure>

Une transition peut être libre (par défaut) ou bien conditionnelle. Pour créer une transition conditionnelle, cliquez sur la transition, cochez la case **Use code for transition** et écrivez en code Python la condition à respecter pour passer par cette transition.

<figure><img src="../.gitbook/assets/workflow_transition_conditionnelle.png" alt="Panneau de configuration d&#x27;une transition conditionnelle"><figcaption><p>Transition conditionnelle : un nom, la case <strong>Use code for transition</strong> cochée et la condition Python. Le nom de la transition s'affiche sur le lien dans le canvas.</p></figcaption></figure>

## Terminer un Workflow

Chaque branche d'un Workflow doit se terminer par le module **Done**.

Une fois que l'architecture d'un Workflow est valide, on peut le tester et le publier.

### Publier un Workflow

Publier un Workflow permet de le tester, et de l'utiliser en production. Pour ce faire, cliquez sur le bouton **Publish version**.

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

Une fois qu'un Workflow est publié, on peut toujours retourner en mode Draft. Tant que le nouveau Draft ne sera pas publié, il n'aura aucun impact sur la production.

### Tester un Workflow

Une fois que votre Workflow est publié, vous pouvez le tester.

<figure><img src="../.gitbook/assets/image (125).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (125).png" alt=""><figcaption></figcaption></figure>

**Donnée Initiale :**\
Chaque étape du Workflow génère des données qui peuvent être utilisées au sein même de ce Workflow et que l'on retrouve dans les résultats finaux. Il est également possible de transmettre des données initiales avant même la première étape. Un champ au format JSON est attendu.

{% hint style="info" %}
Transmettre des données initiales peut être très pratique pour les transitions conditionnelles, notamment si vous savez à l'avance dans quelle branche le Workflow doit se diriger (par exemple, effectuer un vidéocodage ou non).
{% endhint %}

**Métadonnées personnalisées :**\
Des données sous forme de chaînes de caractères, non exploitables dans le Workflow, mais renvoyées telles quelles dans les résultats.

{% hint style="info" %}
Ce paramètre est souvent utilisé pour transmettre des identifiants internes (ID), afin de les retrouver dans les résultats et d'associer ces derniers à l'ID correspondant.
{% endhint %}

**Fichiers :**\
Les documents qui seront envoyés dans le Workflow. Trois collections par défaut sont disponibles :

* **File :** Utilisée par la plupart des modules par défaut.
* **Email :** Utilisée par le module **Ingest Email** par défaut.
* **Attachment :** Non utilisée par défaut par aucun module, mais pratique dans certains cas spécifiques, comme tester une partie du Workflow.
