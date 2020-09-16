# API Overview

Our API breaks into three major sections

- `/tryals` - Allows you to create, retrieve, recycle and remove PDF based examination and question
  materials.
  - [<span class="post">post</span> /tryals/create](#post-tryals-create) Generates a new Tryal.AI material according to your parameters
  - [<span class="get">get</span> /tryals](#get-tryals) gets all tryals generated for this application 
  - [<span class="get">get</span> /tryals/meta](#get-tryals-meta) gets the metadata for a given Tryal ID
  - [<span class="get">get</span> /tryals/paper](#get-tryals-paper) retrieves a PDF of the question material in blob form that can either be rendered or forwarded on to a client browser for viewing or download 
  - [<span class="get">get</span> /tryals/markscheme](#get-tryals-markscheme) retrieves a PDF of the corresponding mark scheme in blob form that can either be rendered or forwarded on to a client browser for viewing or download
  - [<span class="delete">delete</span> /tryals <span class="post">post</span> /tryals/delete](#post-tryals-delete) deletes a Tryal material record from our database and associated PDF paper and mark scheme material
- `/questions` - Allows you to see, sample and generate our question base, to embed questions in your
  site
- `/usage` - Allows you access and monitor the usage of your application
- `/version` - Allows you to see the current operating version of the server serving your requests