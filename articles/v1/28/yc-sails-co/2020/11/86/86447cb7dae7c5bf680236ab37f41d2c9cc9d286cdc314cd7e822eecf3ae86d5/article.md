---
schema_version: "1.0.0"
document_id: "86447cb7dae7c5bf680236ab37f41d2c9cc9d286cdc314cd7e822eecf3ae86d5"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/understanding-sails-routes/"
published_at: "2020-11-11T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T21:05:18.773660+00:00"
content_hash: "sha256:88d157ae4b65a3a02a46f1d96527f6de394ce896ed50ad7ad01f213a8f1cb087"
---

# Understanding Sails routes

Routes are how a web application gives access to the outside world to it’s functionalities and pages. This is done by the ability of a web application to interprete a request sent to a URL and then send back a response. Every web development framework provides a mechanism to achieve this called a router - a mechanism for mapping URLs to actions or views.


## Types of Routes in Sails


In Sails routes are rules that tells Sails what to do with an incoming request. Sails recognizes two types of routes:


- Custom or explicit routes
- Automatic or implicit routes


## Custom routes


Sails gives you the flexibility to design how your app handles URLs in any approach you see fit i.e Sails does not restrict you in anyway when defining routes.


### config/routes.js


Every Sails project provides a` config/routes.js` file - a Node.js module that exports an object of custom or explicit route definitions. Let’s see an example of the content of a` config/routes.js` file:


```text
module  .  exports  .routes   =   {
'GET /user/register'  : { view:   'user/signup'   },
'GET /user/login'  : { view:   'user/login'   },
'/logout'  : { action:   'user/logout'   }
}
```


Each route definition in` config/routes.js` object consist of an **address** as the key and a **target** as the value.


### Route address


The address is mostly a URL path and optionally a specific HTTP method.


> Note the URL path of the address must be preceeded with a forward slash ’/’ or Sails won’t interprete it properly


In declaring an address, if you omit the HTTP method, Sails will match all HTTP methods requests to that specific route definition. So when you do` '/logout'` , you are telling Sails that this route definition should be mapped to GET, PUT, POST, DELETE or PATCH. Also if you define an address like` 'all /logout'` it will also match any HTTP method.


### Wildcards


Aside from specifying static path like **user/profile** , you can use` *` (asterisk) as a wildcard. For example:


```text
'/*'
```


will match all paths, and:


```text
'/user/*'
```


will match paths that starts with **user/**


> Using a route with wildcard will also match requests to static assets and override them. To avoid this you can use the` skipAsset` route option and set it to` true` in the route definition


### Dynamic parameters


You can capture parts of an address in Sails by specifying dynamic parameters that will be added in` req.allParams` . Here is an example of how this is done:


```text
'user/:id'
```


The` :id` part of the URL path is the dynamic parameter. The route path above will match like using` 'user/*'` but will also provie the value of the dynamic portion to the route handler as a parameter value which you can access for example via` req.param('id')` .


In case you want the dynamic parameter to be optional, you can append a` ?` after the dynamic parameter name, like so:


```text
'user/:id?'
```


### Route target


While the address portion of a custom route definition specifies which URLs the route should match, the target portion tells Sails what it should do after that match is made is what handles the request coming to the address e.g` { action: 'user/profile' }` . In Sails the target can be defined in a number of ways. Let’s take a look at them:


### Controller/action target syntax


This syntax is used when you have your actions in a single controller file. The following route definitions are all equivalent using this syntax:


```text
'GET /bar/baz'  :   'BarController.myBazAction'  ,
'GET /bar/baz'  :   'bar.myGoAction'  ,
'GET /bar/baz'  : { controller:   'bar'  , action:   'myBazAction'   },
'GET /bar/baz'  : { controller:   'BarController'  , action:  'mBazoAction'   },
```


> Note the controller and actions names in the above definitions are case-insensitive.


### Standalone action target syntax


If your actions are defined using[actions2](https://blog.sailscasts.com/migrating-your-sails-actions-to-actions2) style, then this is the target syntax you probably would want to use. Here are a couple of ways you use:


```text
// Use the action in api/controllers/index.js
'GET /'  : { action:   'user/index'   },


// Use the action in api/controllers/baz/super-action.js OR
// the "super-action" action in api/controllers/BazController.js
'GET /baz/super'  : { action:   'baz/super-action'   }


// Binds to the same action as above, using shorthand notation
'GET /bar/super'  :   'baz/super-action'
```


> Since the shorthand notation involves more keystrokes it should be used most often.


## Routing to blueprint actions


The blueprint API adds several actions for each of your models, all of which are available for routing. For example, if you have a model defined in api/models/User.js, you’ll automatically be able to do:


```text
// Return a list of users
'GET /user/:id'  :   'user/find'
```


or


```text
// Same as above
'GET /user/:id'  :   'UserController.find'
```


> If you have a custom action in api/controllers/user/find.js or api/controllers/UserController.js, that action will be run instead of the default blueprint find.


### View target syntax


If you need to send a view when a URL is matched the you would use this syntax:


```text
'GET /about'  : { view:   'info/about'   }
```


This tells Sails that when it gets a “GET” request to` /about` it should sever the view template located at` views/info/about.ejs` (we are assuming you are using the default EJS template engine).


> One gotcha with using this syntax is that if you specified some policies for the route, it won’t be applied because you are binding a view directly to the route. If you need to add a policy then you should make an action return the view instead


You could specify the views layout within the route definition like so:


```text
'GET /terms'  : {
view:   'legal/terms'  ,
locals: {
layout:   'legals'
}
},
```


Apart form specifying the layout, you can pass other[locals](https://sailsjs.com/documentation/concepts/views/locals) for the view like so:


```text
'GET /profile'  : {
view:   'entrance/profile'  ,
locals: {
name:   'Mike McNeil'
}
}
```


### Redirect syntax


Sail router provides a convenient way to redirect one address to another. The address you are redirecting to can be within your Sails application or another server, it doesn’t matter, the redirect syntax will work according. Here is how to use it:


```text
'/terms'   :   '/legal/terms'  ,
'GET /sails'  :   'https://www.sailsjs.com'
```


> You should note that request methods and other data may not survive a redirect.


### Response target syntax


Sails allows you to write custom responses in` api/responses` directory. You can choose to handler request with these custom responses by simply specifying the name of the custom response file without the` .js` extension like so:


```text
'/bar'  : { response:   'notAuthorized'   }
```


> If you attempt to bind a route to a custom response that does not exist, Sails will output an error and ignore the route


### Policy target syntax


In a situation where you need to apply a policy to a view, you will use this target syntax along side the view target syntax like so:


```text
'/dashboard'  : [
{ policy:   'is-logged-in'   },
{ view:   'dashboard'   }
]
```


> You specify the policy name in` policies` directory without the` .js` file extension


### Function target syntax


This syntax is suitable for quick prototyping or testing needs. Here is how you use it:


```text
'/say-hello'  :   function  (  req  ,   res  ) {
return   res.  send  (  'hola!'  );
},
```


Another use of this syntax is to specify quick inline middleware by combining it with other syntaxes like so:


```text
'/user/:id'  : [
function  (  req  ,   res  ,   next  ) {
sails.  log  (  'Params:'  , req.  allParams  ());
return   next  ();
},
{ controller:   'user'  , action:   'find'   }
],
```


> For best practices, only use the function syntax only for temporary routes, since doing so goes against the structural conventions that make Sails useful!. Also avoid cluttering your routes.js file.


## Automatic routes


Sails binds many routes for you automatically in addition to your custom routes. If a URL doesn’t match a custom route, it may match one of the automatic or implicit routes and still returns a response. Here are the main types of custom routes in Sails:


- [blueprint routes](https://sailsjs.com/documentation/reference/blueprint-api?q=blueprint-routes) , which provide your controllers and models with a full REST API.
- [assets](https://sailsjs.com/documentation/concepts/assets) , such as images, Javascript and stylesheet files.


## Unhandled requests/routes


When Sails can not match incoming request to either a custom route or implicit route, it will respond with a 404. You can further customize this response by adding a` notFound.js` file in` api/responses` in your application.


## Unhandled errors in request handlers


If an unhandled error is thrown during the processing of a request (for example, in an action code), Sails will send back a default 500 response. This response can be customized by adding a` serverError.js` file in` api/responses` in your application.


## Conclusion


Routing is a core functionality of every web framework. In this article you see how Sails handles routes and the various ways you can define routes and use routes in your Sails application.
