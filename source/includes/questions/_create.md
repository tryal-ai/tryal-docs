# <span class="post">POST</span> /questions/create

## Create a Question from the given ID

> Allows you to generate a question from the chosen ID

```shell
#Authorisation headers omitted
curl "https://api.tryal.ai/questions/create"
```

```javascript
// We recommend cross-fetch to write easy HTTP requests
var fetch = require('cross-fetch');

// Promise based request
var promise = fetch('https://api.tryal.ai/questions/create', {
  method: 'post',
  headers: {
    'Content-Type': 'application/json'
    //Authorisation headers omitted
  }, 
  body: JSON.stringify({
    id: "007d3281ec51592e6d2a9b1955d9061e610627a159e1a914702868ca7ef854ba",
    keyword: "somerandomword",
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

async function generateQuestion() {
  try {
    const response = await fetch(`https://api.tryal.ai/questions/create`, {
      method: 'post',
      headers: {
        'Content-Type': 'application/json'
        //Authorisation headers omitted
      },
      body: JSON.stringify({
				id: "007d3281ec51592e6d2a9b1955d9061e610627a159e1a914702868ca7ef854ba",
        keyword: "somerandomword",
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

> The body below shows the result of this request

```json
{
    "answer": [
        {
            "marks": 2,
            "text": "For correct answer only (-4, 8)"
        }
    ],
    "id": "9b3b7b18cc71f1e86ce32511078646fbe98a6d9c828fa6b5b412d305c81a0ff8",
    "question": [
        "The equation of a curve is $y = (x +4)^2 +8$",
        "Which of the following is the turning point of the curve?",
        {
            "type": "multichoice",
            "values": [
                "(-4, 8)",
                "(4, 8)",
                "(4, -8)",
                "(-4, -8)"
            ]
        }
    ],
    "seed": "erratic"
}
```

Our generator allows you to produce a unique question for a given question `id` either using a keyword you choose, or by allowing us to supply one. This means you can control and reliably retrieve the same responses each time you interact with our API. 

### HTTP Request

`POST https://api.tryal.ai/questions/create`


### Body Parameters

Parameter | Required | Type | Description 
--------- | ------- | ----- | -----------
`id` | Yes | SHA256 | The ID of the question you wish to see a sample for
`keyword` | No | Any Alphanumeric String | The keyword to use to provide entropy for the question. The same keyword will generate the same question for the same `id`.

### Billing

Generate is usually billed at 5 marks, or if the question you generate is 6 or more marks (which most questions aren't) at the number of marks the question consists of. Whichever of the two is higher.

### Response

- **200**
  - A JSON object containing the question, as well as related metadata that is useful for delivery and rendering to your end user

Key | Value
--- | -----
`seed` | The seed is used to provide entropy and provides a deterministic identifier for the question. When submitting answers the `seed` will need to be returned in order for a correct marking to take place
`question` | The text of the question, consists of an array of string and/or objects. Strings should be treated as regular text, whilst objects are typed and carry additional metadata about what is to be rendered. For a complete set of objects see Question Markup
`answer` | An array of JSON objects consist of answer text and mark pairs. Our mark scheme is designed to followed progressively, step by step awarding marks. In some cases, marks will return `0` for a line in the mark scheme, this typically indicates that the line is an additional information note
`marks` | Note that for longer questions, the marks in the answer may not add up to the marks included in the metadata. This is because the question may have multiple solutions for which we've provided different methods and shown how they would be marked. the marks key in the main body indicates the actual marks value of the question
`id` | The ID of the question. This is the same as the `id` submitted, but is returned for convenience and completeness as in order to get a question answer marked, you will need to submit any and all working the student provides, the `id` and the `seed` for marking.

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

