# Tryals API
Our Tryals API allows you to generate our service's AI to generate question material and mark scheme PDFs, programmatically. Then distribute them to your users under licence, for them to use as practice material. Our Tryals API is designed to expose the most useful features of our underlying core generation system in an easy to use format.

Although Tryal.AI was initially designed to generate GCSE mock exams, it is capable of so much more in terms of material generation. That said, to stay true to our initial goal, most default options will be configured to create a realistic GCSE experience. Not all material you generate necessarily calls for this. Consider reading the section on [configuring generation](#configuring-generation), and our [blog](https://blog.tryal.ai). 

<aside class="notice">
    We refer to our material as a Tryal (read as trial) or Tryals, as they can vary wildly depending on how you use the generator and this is a good way to encompass the general concept of any material produced by Tryal.AI
</aside>

Most behaviours available on our web platform can be replicated via our API, and even some things our web platform can't do. Below we detail a complex flow which takes you through each of the end points and how we generally view the generation, retrieval and recycling flow.

1. You create a new Tryal via [<span class="post">post</span> /tryals/create](#post-tryals-create)
    This has lots of configuration and very few required options. You may choose to allow your users to access and control as many or as few of these options as you like, or even dynamically reconfigure options as they progress through configuring their material

2. If the generator can produce the material you requested, it'll return an `id` which can then be used to retrieve PDFs of both the paper and the mark scheme via [<span class="get">get</span> /tryals/paper](#get-tryals-paper) [<span class="get">get</span> /tryals/markscheme](#get-tryals-markscheme), which retrieve papers and mark schemes for the `id` given respectively.

3. If you want to retrieve data about the material later, you can use [<span class="get">get</span> /tryals/meta](#get-tryals-meta)

4. If you want to create a Try and Repeat flow for a user, where they repeat material with the same structure, but different values, you might consider using our [<span class="post">post</span> /tryals/recycle](#post-tryals-recycle)

5. If you want to get an overview of all the material generated by your application (for the purposes of monitoring) you can use [<span class="get">get</span> /tryals](#get-tryals)

6. You can delete a previous generated material from our database, and all associated material, by using the [<span class="delete">delete</span> /tryals or <span class="post">post</span> /tryals/delete](#delete-tryals)

