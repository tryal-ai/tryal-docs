# <span class="get">GET</span> /tryals

## Get All Tryal Material

> Allows you to retrieve records of previously generated Tryal.AI material

```shell
#Authorisation headers omitted
curl "https://api.tryal.ai/tryals"
```

```javascript
// We recommend cross-fetch to write easy HTTP requests
var fetch = require('cross-fetch');

// Promise based request
var promise = fetch('https://api.tryal.ai/tryals', {
  method: 'get',
  headers: {
    'Content-Type': 'application/json'
    //Authorisation headers omitted
  }, 
  body: JSON.stringify({
    before: new Date(),
    after: "2020-09-14T15:30:00.000Z",
    board: "aqa",
    level: "foundation",
  })
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
    const response = await fetch(`https://api.tryal.ai/tryals`, {
      method: 'get',
      headers: {
        'Content-Type': 'application/json'
        //Authorisation headers omitted
      },
      body: JSON.stringify({
        before: new Date(), // must be before current data
        after: "2020-09-14T15:30:00.000Z", //must be after 14th Sept 2020 at 3:30PM UTC
        board: "aqa", // board must be AQA
        level: "foundation", // level must be foundation
      })
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
    "id": "738ae716b7842dcc6b25a819c32ca2baad64f72f74ac42fedb74184d258bb647",
    "keyword": "severally",
    "created": "2020-09-14T15:37:59.625Z",
    "board": "edexcel",
    "level": "higher",
    "marks": 80
  },
  {
    "id": "06eef2535f0787bb5353078abed4f8a63ad4738ba7fd389831d25e96626e4541",
    "keyword": "costar",
    "created": "2020-09-14T15:38:09.722Z",
    "board": "edexcel",
    "level": "higher",
    "marks": 80
  }
  // More Tryals omitted
]
```

This endpoint retrieves all previously generated Tryals. If you have not previously generated
material for this *Application* this will return `[]`.

<aside class="notice">
  A record of a Tryal material in our database doesn't necessarily indicate that a Tryal is 
  definitely retrievable. Please see our policy of storage and retrieval limits for material 
</aside>

### HTTP Request

`GET https://api.tryal.ai/tryals`

### Body Parameters

Parameter | Default | Type | Description 
--------- | ------- | ----- | -----------
`before` | null | Any ISODate | If set to a date, will filter for all Tryals before a give date.
`after` | null | Any ISODate | If set to a date, will filter for all Tryals after a given date.
`board` | null | Any String | If set, filters tryals by board
`level` | null | Any String | If set, filters tryals by level

### Billing

There is no applicable fee for accessing this endpoint.

### Response
- **200**
  - An array of Tryals generated using this application's identity. Includes basic information 
  regarding when, how many marks and the Tryal's ID as well as the keyword used for the material's
  entropy.

Key | Value
--- | -----
`id`  | identifies the tryal in question and generally is used for fetching metadata, PDF binaries, recycling and deleting
`keyword` | The keyword used for entropy in generating a Tryal. The keyword is provided for convenience and isn't necessarily a unique identifier
`created` | The date this Tryal material was generated
`board` | The board for which this tryal is targeted
`level` | The level at which this tryal is targeted
`marks` | The number of marks this Tryal consists of

### Errors
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

