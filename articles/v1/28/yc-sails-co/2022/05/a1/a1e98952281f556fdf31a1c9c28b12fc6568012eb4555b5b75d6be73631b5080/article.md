---
schema_version: "1.0.0"
document_id: "a1e98952281f556fdf31a1c9c28b12fc6568012eb4555b5b75d6be73631b5080"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/using-the-paginate-query-modifier-method-for-pagination/"
published_at: "2022-05-31T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:e131e3e0a15692578653efabda9d9bb80b36dce97da1f0f8440fe10ed3aac2af"
---

# Using the paginate query modifier method for pagination

One of the things I love about Sails, is how it seems to be equipped with practical usable features and nuances. I recently discovered one and I wanted to share it in this article.


I wrote about paginating Waterline query results a[previous](https://blog.sailscasts.com/how-to-paginate-your-sails-ap-is/) article and while I have used the method written in that article, it always feels good to see that Sails have that covered as well.


## The paginate query modifier


If you’ve used Sails for a while, you will know that you can chain methods like[sort](https://sailsjs.com/documentation/reference/waterline-orm/queries/sort) ,[meta](https://sailsjs.com/documentation/reference/waterline-orm/queries/meta) ,[select](https://blog.sailscasts.com/understanding-waterline-projections/) ,[omit](https://blog.sailscasts.com/understanding-waterline-projections/) ,[populate](https://sailsjs.com/documentation/reference/waterline-orm/queries/populate) , etc. on a Waterline query. These methods are called **query modifiers** as they modify the query you are making. Waterline also expose another of these so called query modifiers which is a syntactic sugar for getting paginated query results back from Waterline. This method is called` paginate` .


To use the` paginate` query modifier, you call it with a required page argument - which is the` page` you want Waterline to return and an optional` pageSize` second argument which will default to 30 if not set. Let’s see an example of using the paginate method.


```text
const   servers   =   await   Server.  find  ({ isRunning:   true   }).  paginate  (  0  )
```


You can also specify the page size:


```text
const   servers   =   await   Server.  find  ({ isRunning:   true   }).  paginate  (  0  ,   10  )
```


The paginate method also accepts an options object so you can call it like so:


```text
const   servers   =   await   Server.  find  ({ isRunning:   true   }).  paginate  ({ page:   0  , limit:   10   })
```


Note you don’t have to specify either the` limit` property or the positional` pageSize` argument if you want to use the default` pageSize` which is 30.


One thing to recall is the` page` value expected by the paginate query modifier is zero-based. What this means is that, to get the first page, you have to pass in` 0` to paginate as the page value. This makes sense as the paginate query modifier is essentially syntactic sugar and will pass the` page` value to the[skip](https://sailsjs.com/documentation/reference/waterline-orm/queries/skip) query modifier.


## Usage


So now that you know the general form of the paginate query modifier, let’s look at a practical usage of it as you don’t want to just get the first page every time(right?).


Generally, with a typical pagination flow, the` page` value should is passed via a` page` query string. For example say you have a route to return a list of servers e.g` /servers` .


The caller of route can pass in the` page` value as a query string so you can get requests like:` /servers?page=1` ,` /servers?page=2` , etc. You get the idea.


To implement the above logic, in your action just add a page input which is a number and set it’s default value to 1. This is because we want visits to the` /servers` route to return the first page even though the page value wasn’t set via the page query string. Also taking into account that the paginate query modifier expects a zero-based page value, we should remove` 1` from page before passing it to the paginate query modifier.


Here is the implementation


```text
// controllers/servers/get-servers.js
{
inputs  : {
page  : {
type  :   'number'  ,
defaultsTo  :   1
}
},
exits  : {},
fn  :   async   function  ({   page   }) {
const   servers   =   await   Server.  find  ({ isRunning:   true   }).  paginate  (page   -   1  )
return   servers
}
}
```


Isn’t that beautiful? So again we are decreasing the page value from the query string by` 1` because of aesthetic as we want the to client to call the route using **one-based** page values because it reads more naturally than saying “give me page 0”, saying “give me page 1” makes more sense in userland. If you don’t care about that in your app, you can always start the page with the value of` 0` .


## Conclusion


Discovering this nice little query modifier made me super happy with the thought of how much more of Sails I am yet to discover and be awed about. I will continue to share as always so you can build more amazing Sails applications.
