# <span class="get">GET</span> /usage

## Get All Usage for this application

> Allows you to retrieve a record of all usage for this application

```shell
#Authorisation headers omitted
curl "https://api.tryal.ai/usage"
```

```javascript
// We recommend cross-fetch to write easy HTTP requests
var fetch = require('cross-fetch');

// Promise based request
var promise = fetch('https://api.tryal.ai/usage', {
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
    res.json().then(responseBody) {
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

async function getTryals() {
  try {
    const response = await fetch(`https://api.tryal.ai/usage`, {
      method: 'get',
      headers: {
        'Content-Type': 'application/json'
        //Authorisation headers omitted
      },
    });
    if (response.status == 200) {
      //Produces result shown
      console.log(await response.json());
    } else {
      throw REQUEST_ERROR
    }
  } catch(err) {
    // Do error handling
  }
}
```

> The above command returns JSON structured like this:

```json
[
  {
    "when": "2020-09-13T19:27:27.228Z",
    "marks": 80
  },
  {
    "when": "2020-09-14T11:48:17.237Z",
    "marks": 5
  },
  // More Tryals omitted
]
```

The usage results are very basic and designed to give you a clear overview of how many marks where used and when for this application. For a full usage report across all your applications, you'll need to log in to your dashboard.

For your security, we do not include full usage access with any application API, meaning that this application can only access information it produced itself.

### HTTP Request

`GET https://api.tryal.ai/usage`

### Body Parameters

There are currently no parameters for usage, we may release non-breaking additional parameters in the future

### Billing

There is no applicable fee for accessing your usage.

### Response
- **200**
  - An array of usage, catalogued by when and how much

Key | Value
--- | -----
`when`  | An ISO DateTime String indicating when the usage occurred
`marks` | The number of marks this usage instance consisted of

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

