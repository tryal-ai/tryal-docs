# Authentication


> To authenticate your requests, you place your Application ID (`tryal-app-id`) and Token (`tryal-app-token`) 
in your request headers:


```shell
# With shell, you can just pass the correct header with each request
curl "https://api.tryal.ai/<endpoint>"
  -H "tryal-application-id: <tryal-app-id>"
  -H "tryal-application-token: <tryal-app-token>"
```

```javascript
// We recommend cross-fetch to write easy HTTP requests
var fetch = require('cross-fetch');

// Promise based request
var promise = fetch('https://api.tryal.ai/<endpoint>', {
  method: 'get',
  headers: {
    'Content-Type': 'application/json',
    'tryal-application-id': '<tryal-app-id>',
    'tryal-application-token': '<tryal-app-token>'
  },
  body: JSON.stringify({})
}).then(function(res) {
  if (res.status !== 200) {
    //handle failed response
  } else {
    // unpack the response JSON
    res.json().then(responseBody) {
      // do what you want with the response body
    }
    // unpack a blob when dealing with PDF
    res.blob().then(pdfData) {

    }
  }
}).catch(function(err) {
  //Handle any errors
});
```
```javascript--ESNext
// We recommend cross-fetch to write easy HTTP requests
import fetch from 'cross-fetch';

const REQUEST_ERROR = {
  message: 'Our request to Tryal.AI failed'
}

async function doSomethingOnTryal(endpoint, method, body) {
  try {
    const response = await fetch(`https://api.tryal.ai/${endpoint}`, {
      method,
      headers: {
        'Content-Type': 'application/json',
        'tryal-application-id': '<tryal-app-id>', // Your App ID goes here
        'tryal-application-token': '<tryal-app-token>' // Your App Token goes here
      },
      body: JSON.stringify(body) //The body of the request
    });
    if (response.status == 200) {
      return await fetch.json();
      //or
      return await fetch.blob(); // for PDFs
    } else {
      throw REQUEST_ERROR
    }
  } catch(err) {
    // Do error handling
  }
}
```

> Our API works with any language that supports HTTP calls where you can specify headers.

Tryal.AI uses your Application ID (`tryal-app-id`) and Application Token (`tryal-app-token`) to authenticate
all requests. All requests to our developer API must be authenticated, regardless of whether they hold
secure information or are billable.

Tryal.AI expects for the Application ID and Application Token to be included in all API requests to the server 
in headers as shown:

`tryal-application-id: <tryal-app-id>`

`tryal-application-token: <tryal-app-token>`

<aside class="notice">
  All requests should be pointed to `https://api.tryal.ai/`, do not use `http` as our server will reject or
  redirect requests to `https`
</aside>

