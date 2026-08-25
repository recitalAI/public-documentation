# Authentification

## Via un token de service (recommandé)

Des tokens de service peuvent être générés depuis **Settings → API Tokens** (bouton **Generate API Token**).

<figure><img src="../.gitbook/assets/image (42).png" alt=""><figcaption><p>Générer un token de service</p></figcaption></figure>

<pre class="language-python"><code class="lang-python"># Tester la connexion via le token API de service
import requests

URL_SERVER = "https://extract.api.recital.ai/extract/api/v1"
API_TOKEN = "....."

<strong>r = requests.get(
</strong>    url=f"{URL_SERVER}/config/", 
    headers={"Authorization": f"Bearer {API_TOKEN}"}
)
</code></pre>

## Via ID / Mot de passe (token d'accès)

L'authentification via ID/mot de passe vous permet d'obtenir un token d'accès qui sera utilisé dans chaque appel ultérieur de l'API. Le token d'accès a une durée de vie d'une heure.

{% code fullWidth="false" %}
```python
# Tester la connexion via ID / MDP (token d'accès)
import requests

URL_SERVER = "https://extract.api.recital.ai/extract/api/v1"
URL_AUTH = "https://extract.auth.recital.ai/auth/api/v1/login/?noAuth=true"
USER = "...."
PWD = "...."

def get_headers():
    r = requests.post(url=URL_AUTH, data={"username":USER,"password":PWD})
    if r.status_code == 200:
        return {"Authorization":f"Bearer {r.json()['access_token']}"}
    else:
        raise Exception(f'Authentication Error - {r.status_code} - {r.reason} - {r.content}')
 
r = requests.get(
    url=f"{URL_SERVER}/config/", 
    headers=get_headers()
)
```
{% endcode %}
