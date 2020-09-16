# API Overview

Our API breaks into three major sections

- [`/tryals`](#tryals-api) - Allows you to create, retrieve, recycle and remove PDF based examination and question
  materials.
  - [<span class="post">post</span> /tryals/create](#post-tryals-create) Generates a new Tryal.AI material according to your parameters
  - [<span class="get">get</span> /tryals](#get-tryals) gets all tryals generated for this application 
  - [<span class="get">get</span> /tryals/topics](#get-tryals-topics) get a list of currently available topics and subtopics
  - [<span class="get">get</span> /tryals/meta](#get-tryals-meta) gets the metadata for a given Tryal ID
  - [<span class="get">get</span> /tryals/paper](#get-tryals-paper) retrieves a PDF of the question material in blob form that can either be rendered or forwarded on to a client browser for viewing or download 
  - [<span class="get">get</span> /tryals/markscheme](#get-tryals-markscheme) retrieves a PDF of the corresponding mark scheme in blob form that can either be rendered or forwarded on to a client browser for viewing or download
  - [<span class="post">post</span> /tryals/recycle](#post-tryals-recycle) Recycle allows you to regenerate material with the same question makeup, but different values a contexts. 
  - [<span class="delete">delete</span> /tryals <span class="post">post</span> /tryals/delete](#post-tryals-delete) deletes a Tryal material record from our database and associated PDF paper and mark scheme material
- [`/questions`](#questions-api-beta) - Allows you to see, sample and generate our question base, to embed questions in your
  site
  - [<span class="get">get</span> /questions](#get-questions) gets all questions we can generate and supply on an individual basis
  - [<span class="post">post</span> /questions/sample](#post-questions-sample) Generate a static sample question to see the kind question Tryal.AI will produce for the given ID
  - [<span class="post">post</span> /questions/create](#post-questions-create) Create a new question for the given `id` and receive a JSON containing question and answer text
- [`/usage`](#get-usage) - Allows you access and monitor the usage of your application
  - [<span class="get">get</span> /usage](#get-usage) Gets the usage history for this application
- [`/version`](#get-version) - Allows you to see the current operating version of the server serving your requests
  - [<span class="get">get</span> /version](#get-version) Gets the current version of the API being served