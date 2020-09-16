# <span class="post">POST</span> /questions/sample

## Sample a given questions

> Allows you to see a static example of a question generated with the given ID

```shell
#Authorisation headers omitted
curl "https://api.tryal.ai/questions/sample"
```

```javascript
// We recommend cross-fetch to write easy HTTP requests
var fetch = require('cross-fetch');

// Promise based request
var promise = fetch('https://api.tryal.ai/questions/sample', {
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
    res.json().then(function (result) {
        // print the result of the recycle request
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

async function sampleQuestion() {
  try {
    const response = await fetch(`https://api.tryal.ai/questions/sample`, {
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

> The body below shows the sample result of this request

```json
{
  "keyword": "tryal",
  "question": "$6$ tins of soup have a total weight of 1950g\n\n$4$ tins of soup and $3$ packets of soup have a total weight of 2170g\n\nWork out the total weight of 7 tins of soup and 5 packets of soup\n\n\n\n",
  "answer": [
    {
      "marks": 1,
      "text": "Method to find the weight of 1 tins of soup e.g. $1950 / 6$ (325)"
    },
    {
      "marks": 1,
      "text": "Method to find weight of 3 packets of soup e.g. $2170 - 1300 = 870$"
    },
    {
      "marks": 1,
      "text": "Method to find weight of 7 tins of soup and 5 packets of soup $7 \\times 325$, $5 \\time 290$"
    },
    {
      "marks": 1,
      "text": "Correct answer only 3725"
    }
  ],
  "marks": 4
}
```

Our sample endpoint allows you to retrieve an example of the question you may wish to generate to see what the general structure of it looks like. Both question and answer text are generated as they would appear in a paper and follow LaTeX Maths formatting. 

We strip out most, but not all, whitespace formatting on the material. This is something you can either change or use as an indication of how the resource should be rendered to your client.

### HTTP Request

`POST https://api.tryal.ai/questions/sample`


### Body Parameters

Parameter | Required | Type | Description 
--------- | ------- | ----- | -----------
`id` | Yes | SHA256 | The ID of the question you wish to see a sample for

### Billing

Sampling is not billed, however, it is rate limited to 30 requests per minute. Additionally, sampling a given question more than once will result in the same sample response.

### Response

- **200**
  - A JSON object containing the sample question, as well as related metadata that is useful for delivery and rendering to your end user

Key | Value
--- | -----
keyword | The keyword used to provide entropy during generation, all sample questions are fixed to `tryal`
question | The text of the question, including line breaks and LaTeX math formatting
answer | An array of JSON objects consist of answer text and mark pairs. Our mark scheme is designed to followed progressively, step by step awarding marks. In some cases, marks will return `0` for a line in the mark scheme, this typically indicates that the line is an additional information note
marks | Note that for longer questions, the marks in the answer may not add up to the marks included in the metadata. This is because the question may have multiple solutions for which we've provided different methods and shown how they would be marked. the marks key in the main body indicates the actual marks value of the question


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

