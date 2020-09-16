# <span class="get">GET</span> /tryals/meta

## Get a Specific Tryal's Metadata

```shell
#Authorisation headers omitted
curl "https://api.tryal.ai/tryals/meta?id=<id>"
```

```javascript
// We recommend cross-fetch to write easy HTTP requests
var fetch = require('cross-fetch');

var id = "738ae716b7842dcc6b25a819c32ca2baad64f72f74ac42fedb74184d258bb647";

// Promise based request
var promise = fetch('https://api.tryal.ai/tryals/meta?id=' + id, {
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

async function getTryalMetadata(id) {
  try {
    const response = await fetch(`https://api.tryal.ai/tryals/meta?id=${id}`, {
      method: 'get',
      headers: {
        'Content-Type': 'application/json'
        //Authorisation headers omitted
      }
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
{
    //Basic info
    "keyword": "severally",
    "board": "edexcel",
    "level": "higher",
    "marks": 80,
    "no_of_questions": 0,
    "is_calculator": true,
    //Visual info
    "cover": true,
    "whitespace": true,
    "graphics_enabled": true,
    "markstable": true,
    //configuration info
    "position": "all",
    "repeat": "off",
    "share": "default",
    "tolerance": 2,
    //topic info
    "inclusive": false,
    "exclude": {
        "algebra": "all",
        "numbers": ["fdp"]
    }
}
```

This endpoint retrieves metadata for a specific Tryal. The metadata retrieved is more comprehensive than
that provided by the [<span class="get">get</span> /tryals](#get-tryals) endpoint. This is because we request
the data from the generation system. 

As such, this response takes longer, and should not be requested for every Tryal, but only specific Tryals. This data is the same as the generation settings sent when requesting a newly generate tryal. We recommend storing generation
settings and the ID of the generated material together, on your server to save retrieval.

<aside class="notice">
  A record of a Tryal material in our database doesn't necessarily indicate that a Tryal is 
  definitely retrievable. Please see our policy of <a href="#storage-and-retrieval">storage and retrieval</a> limits for material. This applies for metadata too.
</aside>

### HTTP Request

`GET https://api.tryal.ai/tryals/meta?id=<id>`

### URL Parameters

Parameter | Valid | Description
--------- | ----- | -----------
id | A SHA256 Hash | The ID of the tryal you wish to request metadata for, IDs are returned when generating tryals and when using [<span class="get">get</span> /tryals](#get-tryals) to retrieve previously generated tryals.

### Billing

There is no applicable fee for retrieving metadata on material generated. Please note our [retrieval time limits](#storage-and-retrieval) though as this applies to metadata too.

### Response
- **200**
  - A JSON object containing metadata regarding the Tryal material. All this metadata is exposed during the generation call, either it is settings provided by you to Tryal.AI regarding the material you want generated or it is control values such as `ID` or `keyword` provided back once the material is generated

Key | Value
--- | -----
`keyword` | The keyword used to provide entropy during generation
`board` | The board this Tryal material was targeted at
`level` | The level this Tryal material was targeted at
`marks` | The number of marks on this material. This is not necessarily equivalent to the billed marks, see minimum billing for more details. This value is 0, if `no_of_questions` is set.
`no_of_questions` | The number of questions present on this paper
`is_calculator` | Boolean indicating if this material is a calculator material
`cover` | Boolean indicating if this material has a cover
`whitespace` | Boolean indicating if the material has writing space (whitespace) included
`graphics_enabled` | Boolean indicating if graphics questions (e.g. ones containing rendered assets) are included in this material
`markstable` | Boolean indicating if the material includes a markstable, a markstable will be rendered, even if a cover page is excluded
`position` | A string with either the value `all` or `x/y` where `x` is the start question position and `y` is the end question position. For more information on how positions work, look at /generate
`repeat` | A string with one of `off`, `2mol`, `ssdd` or `2mol`, for more information on repeat question patterns, look at /generate.
`share` | A string with one of `default`, `even` or `off`, for more information on topic share patterns, look at /generate.
`tolerance` | A integer value indicating what level of tolerance for deviation from the topic share breakdown is acceptable, this is measured in marks e.g. `tolerance: 2` indicates 2 mark deviation
`inclusive` | A boolean indicating if topic exclusion follows an inclusive exclusion pattern e.g. if at least one subtopic for a question is not excluded, then that question will be considered for inclusion
`exclude` | A JSON object containing keys corresponding to major topics that have been excluded, or lists of subtopics that have been excluded. See below for more info on how we record exclusion.

### Excluding topics

```yaml
exclude:
  algebra: all
  numbers:
    - fdp
    - primes
```
When keeping a record of excluded topics, we produce a JSON as shown in the YAML on the right. Any major topic without a key is considered to be *included*. Any major topic which corresponds to a value `all` is considered to be totally excluded. Any topic that corresponds to an array of strings e.g. `numbers: ['fdp', 'primes']` has only those subtopics excluded. 

This recording method was designed to be more human readable, only by saying what we didn't want included, rather than listing everything we want included. It can essentially be read verbatim "exclude all subtopics from algebra", and "exclude fdp and primes subtopic from numbers".

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