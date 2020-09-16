# <span class="get">GET</span> /version

## Get the current API version

> Allows to get the current API version

```shell
#Authorisation headers omitted
curl "https://api.tryal.ai/version"
```

```javascript
// We recommend cross-fetch to write easy HTTP requests
var fetch = require('cross-fetch');

// Promise based request
var promise = fetch('https://api.tryal.ai/version', {
  method: 'get',
  headers: {
    'Content-Type': 'application/json'
    //Authorisation headers omitted
  }, 
}).then(function(res) {
  if (res.status !== 200) {
    //handle failed response
  } else {
    // unpack the response JSON
    res.text().then(responseBody) {
      //Creates output as shown below
      console.log(responseBody)
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

async function getVersion() {
  try {
    const response = await fetch(`https://api.tryal.ai/version`, {
      method: 'get',
      headers: {
        'Content-Type': 'application/json'
        //Authorisation headers omitted
      },
    });
    if (response.status == 200) {
      //Produces result shown
      console.log(await response.text());
    } else {
      throw REQUEST_ERROR
    }
  } catch(err) {
    // Do error handling
  }
}
```

> The above command returns a version string

```text
v1.0.0
```

The version endpoint lets your retrieve the current version of the API. This is mostly useful for testing endpoint accessibility. Particularly, this endpoint will only serve authenticated applications, so it's a good way to test if credentials work.

### HTTP Request

`GET https://api.tryal.ai/version`

### Body Parameters

There are currently no parameters for version. We have no plan to change this

### Billing

There is no applicable fee for accessing the version.

### Response
- **200**
  - A text string with the current version number

### Errors
- **401**
  - *MISSING_AUTHENTICATION_HEADERS* - The required authentication headers were not included in the
    request, we recommend checking for typos, or null values in your `tryal-app-id` and `tryal-app-token`
    variables
  - *INVALID_APPLICATION_TOKEN* - The application with the given id `tryal-app-id` does not correspond to
    the token provided, most likely, your ID is correct, however your token is incorrect
- **403**
  - *NO_SUBSCRIPTION* - The account associated with this application does not currently have an active
    subscription, or the billing is in arrears and access has been suspended.
- **404**
  - *APPLICATION_NOT_FOUND* - An application with the given `tryal-app-id` could not be found in our
    currently listed active applications

