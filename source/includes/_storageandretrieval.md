# Storage and Retrieval

When a Tryal.AI material is generated (e.g. using [<span class="post">post</span> /tryals/create](#post-tryals-create)), it is not returned immediately on that call. Instead a record of that newly generated material is returned to allow you to retrieve the paper and mark scheme on separate future calls.

As a result, it is possible to delay the retrieval of material to a later date. In some use cases, this is practical, for example provisioning a question paper and waiting to release the mark scheme until the student has made some attempt. 

Tryal.AI does have limits on how long material can be retrieved for.

**Our general policy is that material will be available for 30 days after first generation, or until your subscription expires, whichever is sooner**

After that, it may still be possible to retrieve material, however we provide no guarantees on the availability of that material. You should assume that any material older than 30 days is no longer available. 

<aside class="notice">
    Calling `/tryals/delete` will immediately make material unavailable and is irreversible.
</aside>