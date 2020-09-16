# <span class="delete">DELETE</span> /tryals
# <span class="post">POST</span> /tryals/delete

## Delete previously generated Tryal.AI material

> Allows you to delete a previously generated Tryal.AI material

```shell
#Authorisation headers omitted
curl "https://api.tryal.ai/tryals/delete"
curl -X DELETE "https://api.tryal.ai/tryals"
```

```javascript
// We recommend cross-fetch to write easy HTTP requests
var fetch = require('cross-fetch');

// Promise based request
var promise = fetch('https://api.tryal.ai/tryals/delete', {
  method: 'post',
  headers: {
    'Content-Type': 'application/json'
    //Authorisation headers omitted
  }, 
  body: JSON.stringify({
    id: "007d3281ec51592e6d2a9b1955d9061e610627a159e1a914702868ca7ef854ba",
  })
}).then(function(res) {
  if (res.status !== 200) {
    //handle failed response
  } else {
    // Note that we use blob here, not json
    res.json().then(function (result) {
        // print the result of the delete request
    	console.log(result)
    });
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

async function deleteTryal() {
  try {
    const response = await fetch(`https://api.tryal.ai/tryals/delete`, {
      method: 'post',
      headers: {
        'Content-Type': 'application/json'
        //Authorisation headers omitted
      },
      body: JSON.stringify({
				id: "007d3281ec51592e6d2a9b1955d9061e610627a159e1a914702868ca7ef854ba",
			})
    });
    if (response.status == 200) {
      const result = await response.json();
      console.log(result);
    } else {
      throw REQUEST_ERROR
    }
  } catch(err) {
    // Do error handling
  }
}
```

> The above command returns a json with a success message, this response is not meaningful

```json
{
  "message": "Paper successfully deleted"
}
```

These endpoints allow you to delete a Tryal. These endpoints are equivalent and are designed to support
different ways of structuring requests. Some developers favour using different types of explicit HTTP
request types, others favour endpoints which describe their action and predominantly rely on this to define
the action.

Deleting Tryals is particularly useful if you wish to provide a history service through your platform. E.g. allowing your users to access their previous material. Deleting allows you to use our database and storage solution to keep a record of the material still available.

<aside class="notice">
  Deleting a Tryal does not remove it from usage, which means you can still get an accurate picture of your usage 
</aside>

### HTTP Request

`DELETE https://api.tryal.ai/tryals`

`POST https://api.tryal.ai/tryals/delete`


### Body Parameters

Parameter | Required | Type | Description 
--------- | ------- | ----- | -----------
`id` | Yes | SHA256 | The ID of the material you wish to delete

### Billing

There is no applicable fee for deleting tryal material. Deleting a Tryal does not affect your usage record making usage representative of actual usage.

### Response
- **200**
  - A standard JSON success message is included. If you receive a code 200, you can assume this endpoint has been successful without checking the body of the request.

### Errors
- **500**
  - *GENERATOR_CALL_FAILED* - The generator didn't respond to our API server, this may be indicative of a server
    update or maintenance period. If this problem persists, please contact support and we will investigate.
- **400**
  - *INCORRECT_REQUEST_CONTENT* - Content provided was not in a valid format, or was incomplete. In 
    most cases, this means *type is incorrect* e.g. marks is a string, not a number, *something was missing*
    e.g. you asked for a specific material but didn't provide an ID, *something is invalid value*, you gave
    an argument which didn't conform to the required value.
    Note, not all of our server side validation does sense checking, if you send two values which are contradictory
    our server may not recognise this as an incorrect request. Validation is not a substitution for a well formed
    request. Provides *invalid* to show what data it thinks is invalid
  - *MISSING_REQUEST_CONTENT* - Content was specifically missing that was required. Generally speaking this means
    you've missed a request parameter that is explicitly required.
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
  - *DELETE_FAILED* - Generally doesn't happen because if the material doesn't exist we just mark it as already deleted, however this is coded in the event that the delete process goes wrong. Possibly if the file is being simultaneously accessed and thus subject to an access lock.  

