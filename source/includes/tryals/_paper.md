# <span class="get">GET</span> /tryals/paper

## Get a specific Tryal PDF Paper

> Allows you to retrieve the PDF question material generated

```shell
#Authorisation headers omitted
curl "https://api.tryal.ai/tryals/paper"
```

```javascript
// We recommend cross-fetch to write easy HTTP requests
var fetch = require('cross-fetch');

// Promise based request
var promise = fetch('https://api.tryal.ai/tryals/paper', {
  method: 'get',
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
    res.blob().then(paper) {
			// Can add this to an anchor tag's href to download it/open it in browser
    	var canAddToAHref = window.URL.createObjectURL(paper);
    }
		// Can pipe the result on elsewhere e.g. to client
		res.body.pipe(somewhereElse);
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
    const response = await fetch(`https://api.tryal.ai/tryals/paper`, {
      method: 'get',
      headers: {
        'Content-Type': 'application/json'
        //Authorisation headers omitted
      },
      body: JSON.stringify({
				id: "007d3281ec51592e6d2a9b1955d9061e610627a159e1a914702868ca7ef854ba",
			})
    });
    if (response.status == 200) {
      //Produces result shown
      const pdfBlob = await response.blob();
			// Can stick this in a href on the client
			const href = window.URL.createObjectURL(pdfBlob);

			//forward result on to client
			response.body.pipe(forwardInResponse);
    } else {
      throw REQUEST_ERROR
    }
  } catch(err) {
    // Do error handling
  }
}
```

> The above command returns a blob, which we cannot show here

This endpoint retrieves a PDF blob, which when downloaded or rendered in browser, is the Tryal.AI
material we generated as a result of [<span class="post">post</span> /tryals/create](#post-tryals-create). It is retrieved using the `id` produced when calling `/create`. 

<aside class="notice">
  A record of a Tryal material in our database doesn't necessarily indicate that a Tryal is 
  definitely retrievable. Please see our policy on storage and retrieval limits for material 
</aside>

### HTTP Request

`GET https://api.tryal.ai/tryals/paper`

### Body Parameters

Parameter | Required | Type | Description 
--------- | ------- | ----- | -----------
`id` | Yes | SHA256 | The ID of the material for which to retrieve the PDF paper for

### Billing

There is no applicable fee for retrieving a mark scheme or a paper. Please note our [retrieval time limits](#storage-and-retrieval) though.

### Response
- **200**
  - A blob response containing a PDF copy of the Tryal.AI material generated and associated with this ID

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
  - *RETRIEVE_FAILED* - Generally indicates that the requested Tryal material, or associated metadata is no longer
    available, see our policies on Tryal material storage and retrieval and time limits.  

