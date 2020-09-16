# Questions API (Beta)

Our Questions API provides programmatic access to our question generation service. The Questions API is designed for services who want to utilise Tryal.AI's state-of-the-art AI to dynamically produce questions for their users. This API is a work in progress and we will be releasing new features to it based on user feedback and our learning process.

We value your feedback, and we'd love to hear what aspects of our datasets and tooling you'd like to utilise in our platform. You can file a features request via issues on [your dashboard](https://app.tryal.ai).

Questions has a much simpler user flow and is designed to be built upon by providing only the bare bones of what you need

1. Retrieve a list of questions from our database via [<span class="get">get</span> /questions](#get-questions), this data does not include a sample of the material. 

2. Retrieve a sample of a question with a given `id`, note that the same call for the same question, will result in the same response. Sample questions are static and not meant to be served to end users. The [<span class="post">post</span> /questions/sample](#post-questions-sample) is rate limited and primarily intended for testing purposes and for occasional calls.

3. 