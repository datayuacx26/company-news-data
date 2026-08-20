---
schema_version: "1.0.0"
document_id: "27c671b1b10ed438f10927ecf4ea5b8b813c44e79e2e3c99b619e55259385ea5"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/return-or-throw-your-response/"
published_at: "2021-04-14T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T21:04:52.597445+00:00"
content_hash: "sha256:25d75120f387e4f84184c734ec3102c5ae68818b33c2cb78e088448f94676c8e"
---

# Return or throw your response

As of Sails v1.0,[actions2](https://blog.sailscasts.com/migrating-your-sails-actions-to-actions2) have been the de facto way for writing controller actions.


Learn how to migrate your legacy actions to this more modern style in[this](https://blog.sailscasts.com/migrating-your-sails-actions-to-actions2) Sailscasts article.


When using the actions2 style of authoring controller actions, you’d normally return an` exit` . Take this simplistic controller action below as an example:


```text
// api/controllers/admin/get-users.js
module  .  exports   =   {
friendlyName:   'Get users'  ,
description:   'Get all users'  ,
inputs: {
},
exits: {
success: {
description:   'Successfully retrieved all users'
},
forbidden: {
statusCode:   401  ,
description:   'User not permitted for this action'
}
},
fn  :   async   function   (  inputs  ,   exits  ) {
if   (  this  .req.me.role   !=   'ADMIN'  ) {
return   exits.  forbidden  ({
success:   false  ,
message:   'You are not authorized for this action'
})
}
const   users   =   await   User.  find  ({role:   'user'  })
return   exits.  success  ({
success:   true  ,
message:   'Successfully fetched users'  ,
data: users
})
}
}
}
```


You can see we have two exits for this action: the` success` and the` forbidden` exits and we are explicitly returning them as required.


With actions2 we can simplify things a bit by returning the response to implicitly trigger the success exit or throwing a value for any other kind of exits. Let’s see this in action:


```text
fn  :   async   function   () {
if   (  this  .req.me.role   !=   'ADMIN'  ) {
throw   {
forbidden: {
success:   false  , message:   'You are not authorized for this action'
}
}
}
const   users   =   await   User.  find  ({role:   'user'  })
return   {
success:   true  ,
message:   'Successfully fetched users'  ,
data: users
}
}
}
```


Notice we didn’t have to pass in the` exits` object as an argument to the` fn` function anymore and since we are not having any` inputs` in this action we can safely remove the` inputs` argument as well.


Also for the` forbidden` exits, we are simply throwing an object with the key being the exit name we want to return.


Implicitly Sails knows you want to return JSON as the response anytime you` exit` with an object so using this shorthand will save you some keystrokes as well as make your actions read more like a regular JavaScript function.


## A little word on throw


For the use case above, we passed in some output data for the` forbidden` exit hence the reason we call it like so:


```text
throw   {
forbidden: {
success:   false  , message:   'You are not authorized for this action'
}
}
```


However if for some reason your exit don’t have any data to be sent back, you can simply throw a string which will be the` exit` name, like so:


```text
throw   'forbidden'
```


How much shorter is that as to the following equivalent:


```text
return   exits.  forbidden  ()
```


You can even throw a JavaScript error object to trigger the default` error` exit which will make your server respond with a status code of` 500` . Like so:


```text
throw   new   Error  (  'Something went wrong :)'  )
```


## Conclusion


The shorthand shown in this article is an example of how Sails focuses on developer experience by providing you options to improve productivity.


Remember we use the` return` keyword for a` success` response or exit and we use the` throw` keyword for an error response.
