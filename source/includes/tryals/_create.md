# <span class="post">POST</span> /tryals/create

## Generate a new Tryal Material

```shell
#Authorisation headers omitted
curl "https://api.tryal.ai/tryals/create"
  -H "Content-Type: application/json"
  -d '{}'
#See example JSON body below
```

```javascript
// We recommend cross-fetch to write easy HTTP requests
var fetch = require('cross-fetch');

var body = {
  //See below for an example JSON body
};

// Promise based request
var promise = fetch('https://api.tryal.ai/tryals/create', {
  method: 'post',
  headers: {
    'Content-Type': 'application/json'
    //Authorisation headers omitted
  }, 
  body: JSON.stringify(body)
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

async function generateTryal(body) {
  try {
    const response = await fetch(`https://api.tryal.ai/tryals/create`, {
      method: 'post',
      headers: {
        'Content-Type': 'application/json'
        //Authorisation headers omitted
      },
      body: JSON.stringify(body) //see below for an example request body
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

> The JSON below shows an example body, with every configurable option

```json
{
    "board": "edexcel",
    "level": "higher",
    "keyword": "severally",
    "marks": 80,
    "no_of_questions": 0, //alternative to marks
    "is_calculator": true,
    //Visual info
    "cover": true,
    "whitespace": true,
    "graphics": true,
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

> The JSON body below shows an example response from Tryal.AI

```json
{
  "id": "3377312e51895bac154c1d9f3d4d7a413e7cd4558ad42cc698e69df78a5cf7a9",
  "keyword": "squaws"
}
```

This method is used to generate a new Tryal.AI Material. Tryal.AI not only generates exam papers and mark schemes but any material you really desire. By default, Tryal material requires only two values `board` and `level`. Additional options and what they control are documented below.


### HTTP Request

`POST https://api.tryal.ai/tryals/create`

### Request Body

Argument | Type | Required | Valid Values | Description
-------- | ---- | -------- | ------------ | -----------
`board` | string | Yes | `['edexcel']` | The board for which questions should be targeted. Currently Edexcel is our only supported board
`level` | string | Yes | `['foundation', 'higher']` | The level for which questions should be targeted
`keyword` | string | No | Any Alphanumeric | The keyword used for entropy, and printed on the front of the paper. The same keyword with identical configuration should always produce the same result
`marks` | integer | No | `10` to `200` | The number of marks the material should consist of. Note that the chance of generation failure increases as you reach both extremes. **mutually exclusive with `no_of_questions`, use one or the other**
`no_of_questions` or `quantity` | integer | No | `1` to `50` | The number of questions the material should consist of. Note that chance of generation failure increases as you reach both extremes. **mutually exclusive with `marks`, use one or the other**
`is_calculator` | boolean | No | ~ | Indicates whether material should be targeted for calculator or non-calculator students
`cover` | boolean | No | ~ | indicates if a cover page should be placed on the front. Note, this does not remove copyright notices from footers. Please see [Usage and Licensing](#licencing-and-usage).
`whitespace` | boolean | No | ~ | indicates if whitespace should be included between questions. Questions that use graphical drawing space (such as graph paper) will not be affected by this option
`markstable` | boolean | No | ~ | indicates if a marks table should be included at the start. A marks table will be printed regardless of the cover if this is on. This is useful when producing papers where the number of questions means the marks table needs to be placed on a separate page (e.g. a lot of questions)
`position` | string | No | either `all` or `q1/q2` where `q1` and `q2` are digits | A string indicating what position in a GCSE paper questions should be taken from.
`repeat` | string | No | `['off', '2mol', 'ssdd', 'all']` | Indicates whether a question structure can be repeated more than once. See below for more info.
`share` | string | No | `['default', 'even', 'off']` | Indicates how major topics should be shared across the material. See below for more info.
`tolerance` | integer | No | value between `2` and `9` | Indicates how much margin of error there is when splitting marks between topic share.
`inclusive` | boolean | No | ~ | Indicates whether to inclusively or exclusively filter topics excluded.
`exclude` | Array | No | see exclude for details | Indicates which topics and subtopics should be excluded. See section on exclude below 

<aside class="warning">
  It's not possible for us to accurately validate your request server side before we attempt generation. Certain configurations of a generator call with always fail. For more info, see <a href="#configuring-generation">configuring generation</a>
</aside>

### Position

Position allows you to capture a specific difficulty, by saying e.g. `1/10` any question that would normally be between question `1` and question `10` on a GCSE paper. Note argument must satisfy `q1 < q2` and the narrower the range chosen, the more likely generation will fail.  

All questions are marked as lying between question number `1` to `30`. Of course, as most papers never run to question `30` but every paper has a question `1` there's a natural increase in availability as your range covers lower question numbers.

Additionally, every question has a different question number for both `higher` and `foundation` (with the exception of questions that would only naturally appear on the `foundation` or `higher` spec)

### Repeat

Counter-intuitively, repeat does not mean repeating the same question, it means repeating the same structure with new values and possibly new contexts. Additionally the underlying calculations may vary slightly. Controlling this option appropriately is important for all papers

<aside class="notice">
  A mock exam paper typically won't want to repeat the same question style more than once, but a worksheet covering solving algebra, may want to repeat the same question with different values several times
</aside>

Tryal.AI also recognises Same Style, Different Deep Structure, [a concept developed by Craig Barton](https://ssddproblems.com/), and filters mock material for these as well. In essence, if a question has the same style, even if the underlying structure is different, we won't include it twice.

Repeat offers four options

- *off* - Do not repeat any question structure more than once, regardless of whether the structure underneath is different.
- *2mol* - "2 Marks or Less", questions may be repeated if they are questions consisting of 2 marks or less. This is useful when generating worksheets covering fundamentals e.g. "write the prime factors of x". No repeat restrictions apply to any questions above 2 marks.
- *ssdd* - "Same Style, Different Deep Structure", questions which have the same surface but different deep structure may be repeated, however no same structure may be repeated more than once.
- *all* - Any question may be repeated more than once. Note that this turns off all checks on repeating, and doesn't even ensure that if there are two options it at least uses both. This is literally chaos theory questioning.

### Share

Controls how major topics are shared across the material generated. To achieve a realistic mock experience, Tryal.AI follows standard syllabus breakdowns. These are

Level | Algebra | Geometry | Proportions | Numbers | Statistics
----- | ------- | -------- | ----------- | ------- | ----------
foundation | 20% | 15% | 25% | 25% | 15%
higher | 30% | 20% | 20% | 15% | 15%

Note, that Tryal.AI has a `tolerance` which defines our acceptable margin of error. By default this tolerance is set to `2` as in 2 marks over or under the required share for this topic. Where a question consists of multiple major topics, as is the case in some harder or wordier questions, we cut marks in half and equally proportion them to each major topic.

When entire topics are excluded, share is maintained as if by ratio e.g. a foundation excluding Algebra would be in the ratio `15:25:25:15` for the remaining topics. 

<aside class="notice">
  Topic share will be ignored when using the `quantity` or `no_of_questions` parameter
</aside>

Of course, when creating end of topic tests, or worksheets, you may want a more even distribution across topics, to get better coverage Tryal.AI provides options for how topic share works

- *default* - Standard GCSE pattern as documented above
- *even* - Even distribution across all topics that have been included
- *off* - No restriction applied on how topics are shared, note that as our question base increases, the `off` option will tend towards behaving like `even`

`tolerance` provides a way to control margin of error, tolerance can be set between `2` and `9`, tolerance lower than `2` has been shown to frequently fail generation.

### Exclude

> This is correct at time of publication, use `/tryals/topics` to get an up to date list or see the Generator page on the Tryal.AI dashboard

```yaml
algebra: 
  - express
  - simplify
  - solve
  - expand
  - factorise
  - rearrange
  - formula
  - suvat
  - inequalities
  - lineequations
  - sketch
  - fractions
  - quadratics
  - functions
  - simultaneous
geometry:
  - trigonometry
  - transformations
  - loci
  - area
  - measure
  - algebraic
  - sdt
  - units
  - circletheorems
  - vectors
  - anglerules
  - mechanics
numbers:
  - primes
  - sequences
  - money
  - general
  - indices
  - fdp
  - calculator
  - time
  - hcflcm
  - rounding
  - settheory
  - estimate
  - standardform
  - factors
  - proof
  - surds
proportions:
  - compound
  - fdp
  - ratio
  - conversion
  - food
  - scale
  - inverse
statistics:
  - data
  - combinatorics
  - probability
  - charts
  - averages 
```

Arguably the most powerful feature of Tryal.AI is the ability to filter what questions appear on the material you generate. This allows an unparalleled level of control over the material Tryal.AI can produce. Information on currently available topics is retrievable via the `/tryals/topics` endpoint. This endpoint will return a listing of each topic as a key of the JSON object, and each subtopic as an array there in.

With these topic and subtopics you can now indicate what topics you wish to exclude. To exclude an entire topic, simply include it in the array for exclude e.g.

`"exclude": ["algebra", "numbers/primes"]`

will exclude the entire algebra catalog of questions and will exclude the subtopic `primes` from the overall numbers syllabus. By default, we assume all topics should be included. You only ever need to tell Tryal.AI what you **do not** want.

The `inclusive` option alters how Tryal.AI decides what topics to exclude. The two behaviours are described below

- `inclusive: false` - Tryal.AI will exclude any question that contains at least one of the excluded topics or subtopics. E.g. in the scenario above a question marked as both `algebra` and `geometry` would be excluded as it contains some element of `algebra`
- `inclusive: true` - Tryal.AI will exclude only questions where all of its topics or subtopics have been excluded. E.g. in the above example, a question marked `algebra` and `geometry` would be included because `geometry` has not been exclude. 

`inclusive` is designed to broaden question horizons when dealing with narrower definitions. For example, an end of topic test on `algebra` shouldn't necessarily be narrowly scoped to `algebra` instead it should `algebra` related material.

### Billing

Tryal material generated on this endpoint is billed at the number of marks on the material. E.g. if you generate a 50 mark material, we will deduct 50 marks from your included amount and then bill into your overage once you exceed your included amount.

If you generate based on `quantity` or `no_of_questions` we will bill based on the resulting marks included on that material. As a result, billing for `quantity` or `no_of_questions` can vary.

A minimum billing amount applies. If the material generated is less than 5 marks, we will bill 5 marks only. This minimum billing does not apply if you generate material above 5 marks.

<aside class="success">
  Tryal.AI does not bill for failed generation attempts
</aside>

### Response
- **200**
  - A JSON object containing metadata regarding the Tryal material you've just generated. This is not the PDF copy itself, using the data here you can retrieve the PDF Paper and Mark Scheme as a PDF binary

Key | Value
--- | -----
keyword | The keyword used to provide entropy during generation
id | A SHA256 hash used to retrieve the PDFs for paper and mark scheme and the metadata. Also used to delete and recycle the material

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