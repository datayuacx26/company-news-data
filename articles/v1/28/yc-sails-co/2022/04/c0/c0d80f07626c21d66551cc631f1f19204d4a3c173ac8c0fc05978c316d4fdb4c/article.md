---
schema_version: "1.0.0"
document_id: "c0d80f07626c21d66551cc631f1f19204d4a3c173ac8c0fc05978c316d4fdb4c"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/disabling-the-sails-global/"
published_at: "2022-04-13T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:e464f039524d9ee8aee638e4c9e0183b3f4b2161fdcb3fd61b78f5661b34b042"
---

# Disabling the sails global

With the` sails` global, you get a convenient access to the running Sails application instance and the plethora of useful methods and properties it exposes.


For example, in the` sails` global, you can access your helpers with` sails.helpers.helperName()` .


You can also get access to[captains-log](https://github.com/balderdashy/captains-log) the built-in lightweight logger for Sails using` sails.log()`


Though the` sails` global is automatically available to you out of the box in your sails app and it is both recommended and convenient to use, you might have a need to disable it for several reasons like…


- multiple sails app instances need to exist at once, or…
- using globals is not an option for you or your team.


With that said, let’s look at…


## How to disable the sails global


It’s super trivial to disable the sails global. All that is required is that you open the` config/globals.js` file, and locate the line where it says:


```text
sails  :   true
```


then change that line to read:


```text
sails  :   false
```


And that’s it! The` sails` global has been disabled and is no longer available globally across your Sails application.


## Ways to reference the running application instance


So now that we’ve lost access to the running Sails app instance by disabling the` sails` global, how can we still access this said instance, you may ask.


As with everything in sails, there is no shortage of options on how to access the running sails instance. Let’s look at other ways to access the it in the context of where you are in your Sails project.


### In actions2 style actions


From Sails v1.0, actions2 are the de facto way to author actions.


You can access the running Sails app instance inside the` fn` method via one of the following:


- ` this.sails`
- ` env.sails`
- ` this.req._sails`


Note:` env` is available when you pass it as an argument to the` fn` method of the action.


Below is an example of accessing the` sails` app instance in an actions2 style action.


```text
{
fn  :   async  (inputs, exits, env) {
// this.sails
this  .sails.  log  (  "Accessing the Sails app instance via this"  )


// env.sails
env.sails.  log  (  "Accessing the Sails instance via env"  )


// this.req._sails
this  .req._sails.  log  (  "Accessing the Sails instance via this.req"  )
}
}
```


### In helpers


You might have already guessed it that since[actions2](https://blog.sailscasts.com/migrating-your-sails-actions-to-actions2/) style actions and helpers follow the same authoring pattern, you can also access the running Sails app instance in the` fn` method of the helper via…


- ` this.sails`


### In legacy actions and policies


As you may have noticed when we discussed about accessing the Sails instance in actions2 style actions, Sails also exposes the Sails app instance in the incoming request stream aka` req` . So in legacy actions and[policies](https://blog.sailscasts.com/understanding-sails-policies-and-best-practices/) you get access to the Sails app instance via:


- ` req._sails`


## Conclusion


The` sails` global is comes in handy in a lot of business logic you may want to write in a Sails application. In this article we saw that it’s possible to disable it being automatically available as a global across a Sails app.


We also saw other ways to access the running Sails app instance exposed by the` sails` global in different places across a Sails application.
