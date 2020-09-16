# <span class="get">GET</span> /questions

## List all Questions available via Tryal.AI's Questions API

> Allows you to retrieve a list of all questions for which we can produce variation

```shell
#Authorisation headers omitted
curl "https://api.tryal.ai/questions"
```

```javascript
// We recommend cross-fetch to write easy HTTP requests
var fetch = require('cross-fetch');

// Promise based request
var promise = fetch('https://api.tryal.ai/questions', {
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

async function getTryals() {
  try {
    const response = await fetch(`https://api.tryal.ai/questions`, {
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
    "id": "45f1d3e0ed7914dba71da1fecad6a4464badc22b0ec7d2ebbedcd194110b3699",
    "board": [
      "edexcel"
    ],
    "level": [
      "foundation"
    ],
    "marks": 1,
    "suitability": {
      "calculator": false,
      "noncalculator": true
    },
    "ao": {
      "main": "3",
      "sub": "1",
      "sub_sub": "d"
    },
    "topics": {
      "algebra": [
        "simultaneous"
      ],
      "proportions": [
        "food"
      ]
    }
  }
  // More questions omitted
]
```

This endpoint retrieves all questions currently available via our API. As our question modelling system grows, we will implement search and filter features to allow you to narrow the field of questions. This method does not retrieve an example of the question itself. To generate a sample material, you can use `/questions/sample`.

<aside class="notice">
  As we release new material every week, this endpoint should be actively polled. 
</aside>
<aside class="notice">
  Whilst we try to minimise the id hash from changing, we may pull questions from our service from time to time, or reformat them in such a way that affects their hash code. To reduce the impact on your service, try not to depend on a single question id
</aside>

### HTTP Request

`GET https://api.tryal.ai/questions`

### Body Parameters

No parameters. Search and filter parameters may be introduce in the feature, as a non-breaking change.

### Billing

There is no applicable fee for accessing this endpoint.

### Response
- **200**
  - An array of questions available for generation via our platform. This is a metadata overview and doesn't include a sample of the actual question

Key | Value
--- | -----
`id`  | uniquely identifies this question, used for sampling and generation
`board` | An array of boards for which we consider this question to be appropriate
`level` | An array of levels for which we consider this material to be appropriate, typically if both 'foundation' and 'higher' are present this is indicative of it being harder for foundation and easier for higher
`marks` | The number of marks this question is marked out of
`suitability` | boolean key value pairs indicating if a question is suitable for calculator or non-calculator
`ao` | A record of the assessment objective this question most closely corresponds to. We catalog main, sub and where available, sub sub objectives to give a high level of precision over what is expected on this question. In some cases, the `sub_sub` value is arrayed to indicate that it corresponds to more than one sub-sub objective 
`topics` | A JSON object with topic keys corresponding to an array of subtopics. 

A single question metadata response is shown on the right. 

### Errors
- **500**
  - *GENERATOR_CALL_FAILED* - The generator didn't respond to our API server, this may be indicative of a server
    update or maintenance period. If this problem persists, please contact support and we will investigate.
  - *GENERATOR_FAILED* - Generally indicates that material with the given parameters could not be generated. Typically indicative of a bad generation request, but may be due to failed generation. You will not be billed for any failed generation attempts.
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

