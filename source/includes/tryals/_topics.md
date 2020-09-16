# <span class="get">GET</span> /tryals/topics

## List all Topics currently available via Tryal's API

> Allows you to retrieve a list of all topics and their subtopics

```shell
#Authorisation headers omitted
curl "https://api.tryal.ai/tryals/topics"
```

```javascript
// We recommend cross-fetch to write easy HTTP requests
var fetch = require('cross-fetch');

// Promise based request
var promise = fetch('https://api.tryal.ai/tryals/topics', {
  method: 'get',
  headers: {
    'Content-Type': 'application/json'
    //Authorisation headers omitted
  }
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

async function getTopics() {
  try {
    const response = await fetch(`https://api.tryal.ai/tryals/topics`, {
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
    "name": "algebra",
    "subvalues": [
      "express",
      "simplify",
      "solve",
      "expand",
      "factorise",
      "rearrange",
      "formula",
      "suvat",
      "inequalities",
      "lineequations",
      "sketch",
      "fractions",
      "quadratics",
      "functions",
      "simultaneous"
    ]
  },
  // More topics omitted
]
```

This endpoint retrieves all topics and subtopics available via our API. As our question modelling system grows, we will implement new subtopics, and increase your ability to filter topics according to what specification your focusing on. For example, `algebraic fractions` does not typically appear on foundation material, but this method has no way of filtering down to the the topics specific for that level.

<aside class="success">
  Tryal.AI will ignore any topic exclusion rule that doesn't apply for the given level, board and difficulty. 
</aside>

<aside class="notice">
  As we release new material every week, this endpoint should be actively polled. 
</aside>

### HTTP Request

`GET https://api.tryal.ai/tryals/topics`

### Body Parameters

No parameters. Search and filter parameters may be introduce in the feature, as a non-breaking change.

### Billing

There is no applicable fee for accessing this endpoint.

### Response
- **200**
  - An array of JSONs containing

Key | Value
--- | -----
`name`  | The name of the overall topic, e.g. `numbers`, `algebra` etc.
`subvalues` | An array of the support subtopics

A single topic metadata response is shown on the right. The full request will contain all topics and all subtopics 

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
    available, see our policies on Tryal material storage and retrieval and time limits. This generally shouldn't occur when polling `/tryals/topics`
