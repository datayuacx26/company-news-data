---
schema_version: "1.0.0"
document_id: "2e517285f44022d57fad21205ccbad5262cd1e5b248a5303a448eb6ad0ff3f47"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/understanding-sails-helpers/"
published_at: "2020-12-09T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:5c55954e7a29ffc4f536135c65d264d0d76a34ed113ff9dace35e6694a88877e"
---

# Understanding Sails helpers

One of the core philosophies for Node.js is **small modules** - what this means is having blocks of codes that does one thing and one thing well and then building your program by composing these small modules together. This philosophy has bearings on the Unix philosophy:


- “Small is beautiful”
- “Make each program do one thing well.”


In the spirit of **small modules** , Sails as of v1.0 provides you out of the box with a mechanism for writing small, focused and reusable block of codes called **helpers** .


## What are helpers


Take for example you want to have 20 actions in your Sails application send an email, there are a couple of ways you can do this:


-


Have the send email implementation inline in the 20 actions definitions - this will prove inefficient and obviously not DRY( **D** on’t **R** epeat **Y** ourself).


-


Abstract the send email implementation in a Node.js module and require it in the 20 actions - this would be the traditional way to go in a Node.js applicatioin and it’s an improvement from having to have the send mail implementation inline in the actions.


Sails intelligently know that you will be needing a way to share code so it provides you with **helpers** - utilities that you can share across your Sails applications.


## Benefits of helpers


- Helpers help(pun intended) you avoid repeating yourself and keeping your Sails application DRY.
- Helpers reduces the surface area of bugs in your application and the surfaces areas of rewrites there by improving the maintainability of your application.
- Helpers are convenient as they are globally available within your Sails applications.
- Helpers follows the[Node-machine specification](https://node-machine.org/) like[actions2](https://blog.sailscasts.com/migrating-your-sails-actions-to-actions2) and as a result, they are self documenting.
- Helpers are self-validating as they will throw an error when provided with invalid or missing inputs.
- Helpers through the help of Sails hook pattern can be made into installable NPM packages to be conveniently shared across projects. Examples are[sails-hooks-organics](https://www.npmjs.com/package/sails-hook-organics) and[sails-hook-node-fetch](https://www.npmjs.com/package/@sailscasts/sails-hook-node-fetch)


## Creating helpers


Now you have seen the various benefits helpers provide you with, let’s move on to how to create them. Helpers are created within` api/helpers/` directory, Sails ships with several[generators](https://sailsjs.com/documentation/concepts/extending-sails/generators/available-generators) and one of them helps you conveniently scaffold a helper file. Here is how to use the built-in helper generator:


```text
sails   generate   helper   <  helperNam  e  >
```


Replace` <helperName>` with the name of your helper in camel-casing. For example, you want to create an helper to send email within your application. You will run:


```text
sails generate helper sendEmail
```


Note: you can name the helper whatever you want.


Sails will then go on to create the helper` api/helpers/send-email.js` with basic helper boilerplate. Let’s see that boilerplate.


## Structure of an helper


When you open` api/helpers/send-email.js` you will find the below code structure:


```text
module  .  exports   =   {
friendlyName:   'Send email'  ,
description:   ''  ,
inputs: {


},
exits: {
success: {
description:   'All done.'  ,
},
},


fn  :   async   function   (  inputs  ) {
// TODO
}
};
```


If the above structure don’t make sense to you, you can checkout[Understanding Machines](https://www.smashingmagazine.com/2020/05/understanding-machines-open-standard-javascript-functions/) and[Migrating your Sails actions to actions2](https://blog.sailscasts.com/migrating-your-sails-actions-to-actions2) articles. Let’s go over some notable part of the helper structure to get you started:


- ` inputs` - You define the inputs your helper will need to run(if any) with the` inputs` object. Inputs in an helper are similar to inputs in an actions2 action.
- ` exits` - In here you specify the possible exits(return states) of your helper (See the[Node-machine specification](https://node-machine.org/spec/machine) for more details).
- ` fn` : This is the function that will be called when your helper is invoked.


So following our example helper -` sendEmail` helper, we would have the following inputs:


```text
module  .  exports   =   {
friendlyName:   'Send email'  ,
description:   ''  ,
inputs: {
template: {
description:   'The relative path to an EJS template within our `views/emails/` folder -- WITHOUT the file extension.'  ,
example:   'confirm-password'  ,
type:   'string'  ,
required:   true
},


data: {
description:   'A dictionary of data which will be accessible in the EJS template.'  ,
type: {},
defaultsTo: {}
},


to: {
description:   'The email address of the recipient.'  ,
example:   ' [email protected]  '  ,
required:   true  ,
isEmail:   true  ,
},


subject: {
description:   'The subject of the email.'  ,
example:   'Hello there.'  ,
defaultsTo:   ''
},
},
exits: {
success: {
description:   'All done.'  ,
},
},


fn  :   async   function   (  inputs  ) {
// TODO
}
};
```


Implementing a complete` sendEmail` helper is not the intent for this article however, you should get the idea that the purpose of an helper is all left to you to decide so you can implement however you want for your use case.


## Calling an helper


In Sails you can consume helpers in two ways: the **default usage** and the **named parameters usage** . To call our sendEmail helper using the default syntax, in your actions file(or anywhere you need the helper), you write:


```text
// assuming you have a user to send email to
await   sails.helpers.  sendEmail  (  'confirm-email'  , {token:   'confirmTokenGoesHere'  }, user.email,   'Confirm Email'  );
```


We can write the above code using the named parameters syntax which should read better in identifying what each inputs are:


```text
await   sails.helpers.sendEmail.  with  ({
template:   'confirm-email'  ,
data: {
token:   'confirmTokenGoesHere'
} ,
to: user.email,
subject:   'Confirm Email'
});
```


## Exits


In the` exits` dictionary, you define the possible outcomes an helper may have both success and failure. By default, helpers automatically supports the` success` and` error` exits. When calling an helper, it will return normally if` fn` trigger the` success` exits. It will however throw an error if any other exit was return.


In Sails, it is good practice to provide other custom exits - known as Exceptions explicitly in your` exits` dictionary this helps guarantee maintainability and transparency in your codebase as a developer can simply know at a glance what errors has to be negotiated.


For example if we have an helper to` createNewUser` and a custom exit` emailAddressInUser` , we can negotiate this exit in an action liks so:


```text
// badRequest is defined with the action calling the createNewUser helper
var   newUser   =   sails.helpers.  createNewUser  (payload)
.  intercept  (  'emailAddressInUse'  ,   'badRequest'  );
```


Note:` .intercept` is a convenient shortcut provided by Sails so you are not forced to write custom` try/catch` blocks to negotiate errors all the time.


## Synchronous helpers


One thing to note is that helpers like all Node-machines, are asynchronous by default, so you have to call them with the` await` keyword. However if your helper doesn’t perform any asynchronous operation you can mark it as synchronous by giving it the property` sync` with the value of` true` . Don’t also forget to change` fn: async function` to` fn: function` . Note: calling an asynchronous helper without the` await` keyword will not work. Here is an example of a synchronous helper:


```text
module  .  exports   =   {
friendlyName:   'Synchronous helper'  ,
description:   ''  ,
sync:   true  ,
inputs: {


},
exits: {
success: {
description:   'All done.'  ,
},
},


fn  :    function   (  inputs  ,   exits  ) {
return   exits.  success  (  'All done'  )
}
};
```


## Conclusion


Helpers gives you the convenience of writing small, focused and reusable block of codes to be consumed severally within you Sails applications reducing the amount of code you write, increasing maintainability and improving development experience with Sails.
