---
schema_version: "1.0.0"
document_id: "e072f0e290b176e3f2614bf9e180b22a25b0560bfdaab689c0c206ac9b764264"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/migrating-your-sails-actions-to-actions2/"
published_at: "2020-10-21T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T21:05:18.773660+00:00"
content_hash: "sha256:51b5183b156e7dad2ad4c707c4af1902383905627b8667a8225d1a780c85d164"
---

# Migrating your Sails actions to actions2

When writing Sails applications, your application’s business logic will pretty much be encapsulated in actions - functions that are mapped to routes in your application so when a request is made to a particular route, the action mapped to it is executed. This methodology is not unique to Sails alone by the way as it is common with most web frameworks in different languages.


## Classic action form


Prior to Sails v1.0, actions where typically defined as a function that takes in the` req`[incoming request object](https://sailsjs.com/documentation/reference/request-req) and` res`[outgoing response object](https://sailsjs.com/documentation/reference/response-res) as the first and second arguments respectively and then you return a response from the function which will be sent back to the caller of that action. Below is a typical classic action definition:


```text
module  .  exports   =   async   function   viewDashboard   (  req  ,   res  ) {
const   userId   =   req.  param  (  'userId'  );
if   (  !  _.  isNumeric  (userId)) {
return   res.  badRequest  (  new   Error  (  'The user id is required!'  ));
}
const   user   =   await   User.  findOne  ({ id: userId });
if   (  !  user) {
return   res.  redirect  (  '/register'  );
}
return   res.  view  (  'dashboard'  , {username: user.username});
}
```


The above style of defining actions might be familiar if you’ve written some Express Route handlers before. However, there are some drawbacks to writing actions this way. Let’s look at a few of these drawbacks:


- There is no declarative way to tell what the action does without dissecting the code.
- There is no way to specify what request parameters an action expects to receive.
- Validation logic is mixed with business logic.
- The action is tightly coupled to the` req` and` res` argument making it not reusable.
- The possible outputs of the action can’t be known without traversing the code.


These drawbacks/limitations could be improved on or totally removed by using the **actions2** syntax for writing Sails actions.


## Introducing actions2


Since Sails v1.0, actions2 is a more modern and declarative syntax for writing actions. This syntax is powered by the[node machine specification](https://node-machine.org/) .


To better understand what machines are, check out[this](https://www.smashingmagazine.com/2020/05/understanding-machines-open-standard-javascript-functions/) article.


## How actions2 work


As mentioned earlier, the actions2 syntax is powered by the node machine specification. When you write your actions in actions2, Sails will use[machine-as-action](https://github.com/sailshq/machine-as-action) module to build a modified version of a machine that proxies its inputs from request parameters and proxies its exits through the response.


What this means is that the inputs you specify in your action will be populated when the action is called from the request parameters and the exits you return will be sent out as a response from the action. Let’s see how the previous action we defined above will look like using the actions2 syntax:


```text
module  .  exports   =   {
friendlyName:   'View Dashboard'  ,
description:   'Look up the specified user and sends the user to the dashboard, or redirect to a registration page if no user was found.'  ,


inputs: {
userId: {
description:   'The ID of the user to look up.'  ,
type:   'number'  ,
required:   true
}
},
exits: {
success: {
description:   'User was found and sent to the dashboard'
responseType:   'view'  ,
viewTemplatePath:   'pages/dashboard'
},
notFound: {
responseType:   'redirect'  ,
description:   'No user with the specified ID was found in the database.'  ,
}
},


fn  :   async   function   (  inputs  ,   exits  ) {
var   user   =   await   User.  findOne  ({ id: inputs.userId });


if   (  !  user) {   throw   { notFound:   '/register'  } }


return   exits.  success  ({username: user.username})
}
};
```


Let’s look at some advantages of writing actions in this form.


### actions2 syntax provides a declarative way to tell what the action does without dissecting the code


At a single glance, the actions2 syntax gives you metadata that tells you what this action is all about thanks to the machine specification. We know what parameters this action expects and the possible response it can give.


### Request parameters expected by action is declared explicitly


Using the actions2 syntax, you explicitly declared what parameters the action cares about. This guarantee that you’ll be able to ascertain the names and types of the request parameters the action expects.


### Validation logic is pulled out of the action’s body


The validation for each request parameter is now defined inside the` inputs` object. This removes the clutter from the action body leaving it to focus mostly on the business logic of the application. What Sails does is to automatically ensure those validation declarations are held true in each input and respond with a` res.badRequest()` when any of those constraints is not met by any of the incoming request parameters even before the action runs.


### The action is no longer tightly coupled to the` req` and` res` objects


With this new syntax, actions are no longer dependent on the` req` and` res` object which makes it easier to re-use or abstract into a[helper](https://sailsjs.com/documentation/concepts/helpers)


### The possible outputs of the action can be known without traversing the code


With actions2, we have an` exits` object which holds all the possible exits of an action(See the machine specification for more details). In machine terminology, these are called signals. So without looking at the code, you can see what possible ways an action can exit(return a response).


### Using exits in actions2


In an action throwing anything will trigger the error exit by default. If you want to trigger any other exit, you can do so by throwing a “special exit signal”. This will either be a string (the name of the exit), or an object with the name of the exit as the key and the output data as the value. For example instead of the usual exit syntax:


```text
return   exits.  hasNotSubscribed  ();
```


We can use the shorthand:


```text
throw   'hasNotSubscribed'  ;
```


Or if you want to pass output data you can use:


```text
throw   { hasNotSubscribed: {courseName:   'Developing production-ready Sails Applications'  };
```


## Customizing Response


For each exit in an actions2 action, you can optionally specify the following properties:


- responseType
- statusCode
- viewTemplatePath


## responseType


The responseType property can be one of the following values:


-


"" (an empty string): This is the standard responseType value and it signals sails to intelligently determine the responseType of the exit based on context: this might send plain text, download a file, transmit data as JSON, or send no response body at all. Note: This is the default responseType and you don’t need to specify this value as Sails will infer it if a responseType is not set in an exit.


-


“view”: This causes an exit to render and respond with a view; Any output provided to an exit with a responseType of view be provided as view[locals](https://sailsjs.com/documentation/concepts/views/locals) .


-


“redirect”: This will redirect to a URL provided by the exit output. We used this in our example action above


## statusCode


This is the statusCode to respond with. By default, Sails will set the success exit to a status code of 200, you can override this with the` statusCode` option if you need to. Also,` responseType` of` redirect` will have a status code of` 304` while every other exit signal will have a` 500` status code except specified otherwise.


## viewTemplatePath


This is the relative path (from the` views/` directory) of the view to render. It is only relevant if responseType is set to “view”.


## Migrating your codebase to use actions2


So let’s say you have a login and signup action in a controller called` UserController` which looks like this:


```text
// api/controllers/UserController.js
var   Passwords   =   require  (  'machinepack-passwords'  );
login  :   function  (  req  ,   res  ) {
if   (  !  req.  params  (  'email'  ))   return   res.  badRequest  ()
if   (  !  req.  params  (  'password'  ))   return   res.  badRequest  ()
try   {
const   foundUser   =   await   User.  findOne  ({email:req.  param  (  'email'  )})
if   (  !  foundUser)   return   res.  notFound  ();
Passwords.  checkPassword  ({
passwordAttempt: req.  param  (  'password'  ),
encryptedPassword: foundUser.encryptedPassword
}).  exec  ({
error  :   function  (  error  ) {
return   res.  serverError  (error)
}
incorrect:   function  () {
return   res.  notFound  ();
},
success  :   function  () {
req.session.userId   =   foundUser.id;
return   res.  redirect  (  '/dasboard'  );
}
}   catch  (error) {
return res.serverError(error)
}
},
```


and in your` routes.js` file you map the actions like so:


```text
"PUT /user/login"  :   "UserController.login"  ,
```


Let’s begin migrating these sections to use actions2 but first, let’s look at a concept introduce in Sails v1.0


## Standalone actions


It’s recommended to use standalone actions in your Sails applications for a couple of reasons:


-


It’s easier to keep track of the actions that your app contains by looking at the files contained in a folder than by scanning through the code in a controller file.


-


each action file is small and easy to maintain, whereas controller files tend to grow as your app grows


-


blueprint index routes apply to top-level standalone actions, so you can create an api/controllers/index.js file and have it automatically bound to your app’s / route (as opposed to having to create an arbitrary controller file to hold the root action)


So instead of having a controller file that looks like this:


```text
module  .  exports   =   {
login  :   function   (  req  ,   res  ) {   ...   },
logout  :   function   (  req  ,   res  ) {   ...   },
signup  :   function   (  req  ,   res  ) {   ...   },
};
```


With standalone actions, you have the following folder structure:


```text
api/
controllers/
user/
login.js
logout.js
signup.js
```


With that out of the way let’s migrate the` login` classic action to actions2.


### Step 1 - Generate the action file


Sails make it easy to write actions2 action by providing a CLI command to scaffold a boilerplate for the action. To do running the following command in your terminal


```text
sails   generate   user/login
```


Sails will look for a user directory in the controllers directory and if it finds one it will generate a` login.js` file with a scaffold for the actions2 syntax or it will create the` user` directory and then generate the` login.js` file inside it.


### Step 2 - declare the expected inputs


Looking at the login action we will see it’s expecting an email and password request parameters. Let’s add that to the` inputs` of` user/login.js` :


```text
inputs  : {
email  : {},
password  : {}
}
```


## Step 3 - Move validation logic into input declaration


We can now move the validation logic in the action body into the individual input declaration. And since we are using actions2 syntax we can do so much more:


```text
inputs  : {
email  : {
description  :   'An email address linked to a user in the database'  ,
example  :   ' [email protected]  '
type  :   'string'  ,
isEmail  :   true  ,
required  :   true
},
password  : {
description  :   'Password linked to a use record'  ,
example  :   'abc123'  ,
type  :   'string'  ,
required  :   true
}
}
```


## Step 4 - Specifying exit signals


From the classic login action we identify the following lines:


- return` res.notFound()`
- return` res.serverError()`
- return` res.redirect()`


These are all possible exits for the action so let’s declare these exits using the actions2 syntax:


```text
exits  : {
success  : {
responseType  :   'redirect'
},
notFound  : {
description  :   'User not found with the email address provided or password do not match email provided'
statusCode  :   404
}
}
```


Note we did not define an exit corresponding to` res.serverError` as this will be inferred by Sails when we throw an error in the action.


Finally, we will write the business logic of the action in the` fn` function:


```text
{
// ...
fn  :   async   function  (  inputs  ,   exits  ) {
try   {
const   foundUser   =   await   User.  findOne  ({email: inputs.email})
if   (  !  foundUser)   throw   'notFound'  ;


Passwords.  checkPassword  ({
passwordAttempt: inputs.password,
encryptedPassword: foundUser.encryptedPassword
}).  exec  ({
error  :   function  (  error  ) {
throw   error
}
incorrect:   function  () {
throw   'notFound'
},
success  :   function  () {
this  .req.session.userId   =   foundUser.id;
return   exit.  success  (  '/dasboard'  );
}
})
}   catch  (error) {
throw   error
}
}
}
```


And we are done!. Oh one last thing, we have to update the route definition in` routes.js` to this:


```text
"PUT /user/login"   :   "user/login"  ;
```


You can take this approach to migrate your classic actions in your project to use the recommended actions2 style of authoring actions in Sails and start enjoying the benefits stated above. Here are a few more things worthy of note:


- we still have access to the` res` and` req` objects when we need it by using` this.req` and` this.res` respectively.
- We did not need to validate parameters in the action body as that is taking care of in the definitions of the input.
- You can using object destructuring on the` inputs` argument so you don’t have to use dot notation to access your inputs. like so:


```text
fn  :   async   function  ({  email  ,   password  },   exits  ) {   ...   }
```


Below is the complete login action definition using actions2:


```text
// api/controllers/user/login.js
var   Passwords   =   require  (  'machinepack-passwords'  );
module  .  exports   =   {
friendlyName:   'Login'  ,
inputs: {
email: {
description:   'An email address linked to a user in the database'  ,
example:   ' [email protected]  '
type:   'string'  ,
isEmail:   true  ,
required:   true
},
password: {
description:   'Password linked to a use record'  ,
example:   'abc123'  ,
type:   'string'  ,
required:   true
}
},
exits: {
success: {
responseType:   'redirect'
},
notFound: {
description:   'User not found with the email address provided or password do not match email provided'
statusCode:   404
}
},
fn  :   async   function  (  inputs  ,   exits  ) {
try   {
const   foundUser   =   await   User.  findOne  ({email:inputs.email})
if   (  !  foundUser)   throw   'notFound'  ;


Passwords.  checkPassword  ({
passwordAttempt: inputs.password,
encryptedPassword: foundUser.encryptedPassword
}).  exec  ({
error  :   function  (  error  ) {
throw   error
},
incorrect  :   function  () {
throw   'notFound'
},
success  :   function  () {
this  .req.session.userId   =   foundUser.id;
return   exit.  success  (  '/dashboard'  );
}
})
}   catch  (error) {
throw   error
}
}
}
```


## Conclusion


**actions2** is the recommended way of writing actions in Sails and it provides a standardized modern style for writing actions in Sails.js offering you and your team the ability to see at a glance what an action is expecting as request parameters, the possible way an action can exit and other metadata that makes the action self-documenting. Here are further resources:


- [https://github.com/sailshq/machine-as-action](https://github.com/sailshq/machine-as-action)
- [https://sailsjs.com/documentation/concepts/actions-and-controllers](https://sailsjs.com/documentation/conceptsactions-and-controllers)
