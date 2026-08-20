---
schema_version: "1.0.0"
document_id: "b74087b65d6e6c334ddaaf2251e75d34a7921b7693eb8842c21033f0c6d4e15c"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/sails-for-express-developers-routing/"
published_at: "2022-11-01T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T21:02:37.132176+00:00"
content_hash: "sha256:1d2abfe868c2704eae536f82376cf2ba66aea50206560ecd35caa08544aae851"
---

# Sails for Express developers - Routing

Let’s look at how routing is done in both Express and Sails. I’m going to start with Express.


**[Watch](https://youtu.be/4rh0iPH7KFw) a screencast version of this article on YouTube.**


## Express


First create a folder called` express-app` in a desired location by running the following command in your terminal:


```text
mkdir   express-app
```


Then move into the folder:


```text
cd   express-app
```


Let’s install` express` by running the following command in the terminal as well:


```text
npm   i   express   --save
```


Once that’s done, create an` index.js` file in the root of the` express-app` folder and add the following code:


```text
const   express   =   require  (  'express'  )
const   app   =   express  ()
const   port   =   3000


app.  get  (  '/'  , (  req  ,   res  )   =>   {
res.  json  ({ message:   'Hello World'   })
})


app.  listen  (port, ()   =>   {
console.  log  (  `Example app listening on port ${  port  }`  )
})
```


Here is the bit of code handling the routing:


```text
app.  get  (  '/'  , (  req  ,   res  )   =>   {
res.  json  ({ message:   'Hello World'   })
})
```


From the above code, we can deduce the general form of routing in Express is as:


```text
app.  METHOD  (path, handler)
```


Where you can substitute` METHOD` for any of the[HTTP methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods) in lowercase e.g,` get, post, patch` .


Then a` path` is provided. We are providing` /` in the above code because we want to handle request to the home route.


Finally the` handler` is the function that is invoked to handle the request to the` path`


Now when you run the Express server by using the below command in your terminal:


```text
node   index.js
```


This will start the Express server on port` 3000` so you can visit[http://localhost:3000](https://localhost:3000/) to see the response to that request.


So that’s it for Express now let’s see how we can achieve this in Sails…


## Sails


First thing first let’s scaffold a new Sails project without a view to arrive at a similar state as the Express project above. To do this run:


```text
npx   sails   new   sails-app   --no-frontend
```


If you are on a NPM version earlier than 8.x, you will need to first install the` sails` CLI globally by running the below command:


```text
npm   i   -g   sails
```


Then you can proceed to scaffold the Sails project:


```text
sails   new   sails-app   --no-frontend
```


For the rest of this article and other articles in this series, we will assume a 8.x version of NPM is installed on your machine and we’ll proceed to use` npx` .


Now open` sails-app` in your editor then open` config/routes.js` - which is the file you define routes in Sails.


Inside this file you wil find an empty exported object, your route definition goes in here. So modify that file so it looks like this:


```text
module  .  exports  .routes   =   {
'GET /'  :   'home/index'
}
```


You will notice unlike Express, in Sails routing is done with Plain Old JavaScript Objects (POJO) object, where the` key` is the path to path with the HTTP method in uppercase, and the` value` is the path to a standalone action in the` api/controllers/` folder.


Now` home/index` don’t exists yet, let’s create it by running the below command in your terminal:


```text
npx   sails   generate   action   home/index
```


The above command will create a folder called` home` in` api/controllers/` and then a standalone action file called` index.js` .


In Sails you read` home/index` as: **the home controller with a standalone action called` index`**


Now open up` index.js` and locate the` fn` method and modify it to look like this:


```text
module  .  exports   =   {
friendlyName:   'Index'  ,
description:   'Index home.'  ,
inputs: {
},
exits: {
},
fn  :   async   function   (  inputs  ) {
// All done.
return   { message:   'Hello World'   }
}
};


```


Note in Sails to respond with` JSON` to a request we just return a JavaScript object in the action.


And that’s it!


When you start up the Sails server by running:


```text
npx   sails   lift
```


or


```text
node   app.js
```


And you visit[http://localhost:1337](http://localhost:1337/) , you will see similar output like we saw in the Express example above.


## Conclusion


Note that unlike Express where the route handler is passed inline, in Sails you have a file for every action which makes it maintainable for large projects or large actions(clearly you will be doing more in your actions than just returning JSON).


Sails routing also separates the route handling from the handler definition which makes for a great way to reason about your codebase as you know that your routes are in` config/routes.js` and your actions are in` api/controllers/`


That’s it for part 1 of **Sails for Express developers** . See you in part 2.
