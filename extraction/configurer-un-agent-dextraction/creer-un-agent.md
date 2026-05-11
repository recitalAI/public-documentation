# Créer un Agent

## À partir d'un modèle d'extraction sur étagère

La bibliothèque reciTAL vous permet de créer un Agent à partir d’un modèle pré-entraîné sur nos données (modèles sur étagère). Voici la liste des modèles disponibles :&#x20;

* Carte Grise (Carte Grise v3)
* CNI (CNI v5)
* Permis de conduire (Driving License - country ISO)
* Facture (Invoice 750 v3)
* KBIS (KBIS)
* Passeport (Passport)
* Relevé d'informations (Relevé d'info - V6 - NFR)
* RIB (RIB v2)

{% hint style="info" %}
Plus de modèles sur étagère sont disponibles à la demande, contactez le service client pour en savoir plus.&#x20;
{% endhint %}

<figure><img src="../../.gitbook/assets/image (102).png" alt=""><figcaption><p><em>Création d’un nouvel Agent</em></p></figcaption></figure>

Sélectionnez le modèle entraîné.

<figure><img src="../../.gitbook/assets/image (56).png" alt=""><figcaption><p><em>Création d’un nouvel Agent à partir d'un modèle</em></p></figcaption></figure>

Saisissez le nom du nouvel Agent et cliquez sur `ENREGISTRER`.

<figure><img src="../../.gitbook/assets/image (67).png" alt=""><figcaption><p><em>Nommage du nouveau doc type</em></p></figcaption></figure>

## À partir d'un modèle "Custom"

Si nos modèles sur étagère ne suffisent pas, il faut créer son propre modèle d'extraction. Toutes les étapes de création d'un modèle custom sont détaillées [ici](../entrainer-un-modele-dextraction/). &#x20;

Une fois le modèle créé, il suffit de le sélectionner parmi les modèles disponibles.

## À partir d'un autre Agent (Dupliquer un Agent)

Il est également possible de dupliquer un Agent. Pour cela, il suffit de créer un nouvel agent à partir d'un agent existant. Le nouvel agent va copier en tout points ses caractéristiques (extracteurs, règles de gestion, review et configuration). Cette fonctionnalité est utile pour tester différentes configurations, sans casser l'originale.
