# Authentication

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Content-Type: application/json"
  -H "clientToken: <Some Client Token>"
```

```javascript
var syrinx_client = require('syrinx-client');
syrinx_client.config({url: 'http://example.com:8080/rest/v1', clientToken: '123456789'});
```

```java
// Syrinx Client Object
import org.merck.syrinx.client;
SyrinxClient syrinx = new SyrinxClient();

// Configure
String url, clientToken;
syrinx.setUrl(url);
syrinx.setClientToken(clientToken);
```


> Make sure to set clientToken with your API key.

Syrinx uses API keys to allow access to the API.  Syrinx expects for the API key to be included in all API requests to the server in a header that looks like the following:

`clientToken: <Some Client Token>`

<aside class="notice">
You must replace <code>Some Client Token</code> with your API key.
</aside>
