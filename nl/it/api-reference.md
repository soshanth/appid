---

copyright:
  years: 2017, 2018
lastupdated: "2018-4-24"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Gestione di {{site.data.keyword.appid_short_notm}} con l'API
{: manging-api}

Puoi utilizzare l'API di gestione per l'automazione DevOps, la personalizzazione e la gestione delle tue istanze di {{site.data.keyword.appid_full}}.
{: shortdesc}

L'API di gestione è protetta con i token generati da IBM Cloud Identity and Access Management. Ciò significa che i proprietari degli account possono specificare chi ha nel proprio team un determinato livello di accesso per ciascuna istanza del servizio. Per ulteriori informazioni su come funzionano insieme IAM e {{site.data.keyword.appid_short_notm}}, vedi [Gestione dell'accesso al servizio](/docs/services/appid/iam.html).

Con l'API, puoi:
* automatizzare la configurazione di {{site.data.keyword.appid_short_notm}} nella tua applicazione del processo DevOps.
* impostare e personalizzare le funzionalità attraverso il back-end della tua applicazione, come la configurazione del widget di accesso, il processo di registrazione e la gestione degli utenti.


Le chiamate all'endpoint API di gestione assumono la seguente struttura:

```
appid-management.<region>.bluemix.net
```
{: codeblock}

Puoi trovare la regione cercando nella seguente tabella.

<table>
  <tr>
    <th>Regione {{site.data.keyword.Bluemix}}</th>
    <th>Endpoint</th>
  </tr>
  <tr>
    <td>Regno Unito</td>
    <td><code>appid-management.eu-gb.bluemix.net</code></td>
  </tr>
  <tr>
    <td>Stati Uniti Sud</td>
    <td><code>appid-management.ng.bluemix.net</code></td>
  </tr>
  <tr>
    <td>Sydney</td>
    <td><code>appid-management.au-syd.bluemix.net</code></td>
  </tr>
  <tr>
    <td>Germania</td>
    <td><code>appid-management.eu-de.bluemix.net</code></td>
  </tr>
</table>



## Prerequisiti
{: #api-prereq}

<ul><ul><li>Un'istanza del servizio creata dopo il 15 marzo 2018. Se hai un'istanza del servizio creata prima di questa data, crea una nuova istanza e configurala in modo che corrisponda alla tua istanza corrente. Assicurati di aggiornare le tue applicazioni per utilizzare la nuova istanza.</li>
<li>La [CLI {{site.data.keyword.Bluemix_notm}}](/docs/cli/reference/bluemix_cli/get_started.html) installata.</li></ul></ul>

## Utilizzo di esempio
{: #api-example}

Nel seguente esempio, puoi vedere come utilizzare l'API per modificare il logo del tenant {{site.data.keyword.appid_short_notm}} con Python.

```
import requests
import json

tenantId = '<App ID instance>'
Img = '<Logo file location>'
apiKey = '<IAM AI key>'

# get IAM token
headers = {'Content-Type': 'application/x-www-form-urlencoded', 'Accept':'application
/json'}
data = 'grant_type=urn:ibm:params:oauth:grant-type:apikey&apikey=' + apiKey;

r = requests.post("https://iam.ng.bluemix.net/oidc/token", data=data, headers=
headers);
token = 'Bearer ' + json.loads(r.text)['access_token'];

#  set login widget logo
headers = {'Authorization': token , 'Accept':'application/json'}
files = {'file': open(img,'rb')}

r = requests.post("https://appid-management.<region>.bluemix.net/management/v4/" + te
nantId + "/config/ui/media?mediaType=logo", files=files, headers=headers);

#  get login widget logo
headers = {'Authorization': token , 'Accept':'application/json'}

r = requests.get("https://appid-management.<region>.bluemix.net/management/v4/" + ten
antId + "/config/ui/media", headers=headers);

if (r.status_code >= 200) :
    print(r.text)
    print("success! the logo was changed")
```
{: screen}


## Fasi successive
{: #api-try}

Per provare tu stesso, vedi <a href="https://appid-management.ng.bluemix.net/swagger-ui/
" target="_blank">{{site.data.keyword.appid_short_notm}} Management Rest API <img src="../../icons/launch-glyph.svg" alt="Icona link esterno"></a>
