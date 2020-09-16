# Errors

```json
{
    "status": 400,
    "message": "Request body for this endpoint was missing content or had invalid content",
    "errorCode": "INCORRECT_REQUEST_CONTENT",
    "invalidArgs": [
        "id"
    ]
}

{
    "status": 500,
    "message": "Something went wrong on a Tryal.AI Server, if this issue persists please contact support",
    "errorCode": "SERVER_SIDE_ERROR",
    "supportCode": "018A"
}
```

Errors are listed below each endpoint detailing why they typically occur. All errors Tryal.AI returns have a JSON body. To test if a call failed, you should check if a call returned 200. If not, you can still safely unpack a JSON body, as all failed calls respond with JSON.

In general, most errors indicate only one possible problem, e.g. *NO_SUBSCRIPTION* is pretty self explanatory. In some cases, errors contain more detailed information. The errors on the left are some notable exceptions. Generally when validating request bodies and parameters, we try to indicate what arguments we think are invalid. 

No programmer plans to write code that crashes or has bugs in, but it is possible, and even inevitable that some bugs do emerge. Tryal.AI is designed to fail gracefully when this happens. Any failure on our server is indicated with a `errorCode: "SERVER_SIDE_ERROR"`. A supportCode is attached, which should be used when reporting the issue, as well as information about what you were trying to do when making a call. 

<aside class="notice">
    Issues can be reported via our online dashboard.
</aside>