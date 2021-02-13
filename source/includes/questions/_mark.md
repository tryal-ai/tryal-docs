# <span class="post">POST</span> /questions/mark (Closed Beta)

```shell
#Authorisation headers omitted
curl "https://api.tryal.ai/questions/mark"
```

```javascript
// We recommend cross-fetch to write easy HTTP requests
var fetch = require('cross-fetch');

// Promise based request
var promise = fetch('https://api.tryal.ai/questions/mark', {
  method: 'post',
  headers: {
    'Content-Type': 'application/json'
    //Authorisation headers omitted
  }, 
  body: JSON.stringify({
    id: "007d3281ec51592e6d2a9b1955d9061e610627a159e1a914702868ca7ef854ba",
    keyword: "somerandomword",
    workings: ["2x+3=9", "2x=6", "x=3"]
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
    const response = await fetch(`https://api.tryal.ai/questions/mark`, {
      method: 'post',
      headers: {
        'Content-Type': 'application/json'
        //Authorisation headers omitted
      },
      body: JSON.stringify({
            id: "007d3281ec51592e6d2a9b1955d9061e610627a159e1a914702868ca7ef854ba",
            keyword: "somerandomword",
            workings: ["2x+3=9", "2x=6", "x=2"]
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

> The body below shows the an example of the result of a marking request (not the same result)

```json
{
    "breakdown": [ 
        {
            "feedback": "For correct answer only (-5, -8)",
            "marks": 2, //number of marks awarded for this feedback
            "max_marks": 2 //max number available
        }
    ],
    "marks": 2 //total marks awarded for all feedback
}
```

Using Tryal UI and Tryal Questions API, it's possible to not only generate questions, have them rendered for students and collect workings, but also to get those workings marked. This API endpoint allows you submit workings taken from Tryal UI for marking. Here is an example of the typical flow:

1. You fetch question metadata using [<span class="get">GET</span> /questions](#get-questions)

2. Using the metadata get the ID of the question you want to use, making sure that the "markable" property is `true` otherwise Tryal AI does not support marking it

3. Use this `id` to generate a question using [<span class="post">POST</span> /questions/create](#post-questions-create) 

4. With the returned data, use Tryal UI elements to render the relevant markup as documented here [Tryal Markup](#question-markup)

5. Using the workings taken from Tryal UI, you can now create a request to the Tryal Marking API to get the working marked. 

Working for questions is typically submitted as an array of strings `workings: ["2x+5=11", "2x=6"]`, however, if dealing with a multi-part question, as some questions are in Tryal Markup, you will need to submit workings as a JSON Object, with each key in the object being the part identifier (i.e. `a`, `b`, `c` etc) and an array of strings e.g. `workings: { a: ["2x=4", "x=2"], b: ["because 4 divided by 2 is 2"] }`

### HTTP Request

`POST https://api.tryal.ai/questions/mark`

### Body Parameters

Key | Value
--- | -----
`id`  | The unique identifier of the question you wish to mark, this will be the same ID as the one used to generate the question and will be returned as part of the question body
`keyword` | The keyword identifier provided when the question was generated, if you did not supply a keyword when you generated the question, a keyword will still be part of the returned body for the generated question
`workings` | An array of strings taken from Tryal UI workings field, or for a multipart question, a JSON object where the keys are the name of each part (e.g. `a`, `b`, `c`) with values that are an array of strings from the fields for each respective part

### Billing

Billing is at standard rate for questions, i.e. if the question is below 5 marks, marking is billed at 5 marks, if the question is above 5 marks, then question is billed as the number of marks for the question

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

