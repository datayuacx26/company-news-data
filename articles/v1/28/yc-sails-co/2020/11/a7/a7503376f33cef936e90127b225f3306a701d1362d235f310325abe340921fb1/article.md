---
schema_version: "1.0.0"
document_id: "a7503376f33cef936e90127b225f3306a701d1362d235f310325abe340921fb1"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/one-way-to-organize-sails-routes/"
published_at: "2020-11-26T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:37ad45cb5f5b3a9e70c92ddff231baea901c26d5cbb8206caaff549533acba44"
---

# One way to organize Sails routes

If you are new to the Sails ecosystem you can check out[Understanding Sails routes](https://blog.sailscasts.com/understanding-sails-routes/) by[Kelvin Omereshone](https://twitter.com/dominus_kelvin) to understand how routes work in Sails before proceeding to read this article. Otherwise dive right in!


Currently I have a project that is built with Sails. The project has a website, admin panel and a main quiz flow with a payment gateway to get desired service for mothers or future mothers. Each part of the project has more than 20 routes to handle.


Sails provides you with a single` config/routes.js` file to define your application’s custom routes. To prevent complexity when you have more than a handful of routes, it is a good idea to split the routes definitions into sub routes files. Doing so, when I need to work on the quiz flow part of the app, I will be focused on routes concerning it.


## The Setup


Now that I have introduced you to the problem, let’s look at how to setup a simple pattern to better organize the custom routes definitions


We will be using the merge method in[Lodash](https://lodash.com/) library - a modern JavaScript utility library delivering modularity, performance & extras.


Sails by default, loads Lodash automatically into a global underscore` _` variable however, it won’t be available inside of config files because they will be loaded as part of Sails initialization. So we will have to require the already installed Lodash in` config/routes.js` when we need it.


We will create` routes/` directory within the` config/` driectory. After that we will create two files in routes folder called` admin.js` and` app.js` . Below is the content of both files:


```text
/**
* App Routes
* config/routes/app.js
*/


module  .  exports   =   {


'GET /'  : {
controller:   'HomeController'  ,
action:   'index'
},


}
```


```text
/**
* Admin Routes
* config/routes/admin.js
*/


module  .  exports   =   {


'GET /admin'  : {
controller:   'Admin/DashboardController'  ,
action:   'index'
},


}
```


The last step is merging those files into our main` config/routes.js` file. To do this, we will import the files in` config/routes/` folder and since the files are exporting objects of routes definition, we can call lodash’s` merge` function to merge the objects into one in` config/routes.js` . Here is the complete implementation for that:


```text
// config/routes.js


const   _   =   require  (  '@sailshq/lodash'  );
const   appRoutes   =   require  (  './routes/app'  );
const   adminRoutes   =   require  (  './routes/admin'  );


const   routes   =   _.  merge  (appRoutes, adminRoutes);


module  .  exports  .routes   =   routes;
```


That’s all the changes you need to make in order to set up this pattern in your Sails application! Going forward, if you need a new category of routes, you simply just add a new route file in` routes/` and define the associated routes inside. Then you import the file in` config/routes.js` and then pass it as one more argument to the` _.merge` method.


## Conclusion


You` config/routes.js` can easily get out of hand if your applications have a lot of custom routes. In this article I have introduced a simple pattern to help organize your routes to make them more easy to work with by you or other members of your team.
