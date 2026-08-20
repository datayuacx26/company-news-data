---
schema_version: "1.0.0"
document_id: "97476c34d01821498ad6d60b88c755adb9e357afe5d5f8fed6603cc1eaea9afc"
company_key: "yc-sails-co"
company: "Sails Co."
source_id: "yc-sails-co-rss-a888f7fb03bc"
canonical_url: "https://blog.sailscasts.com/how-to-paginate-your-sails-apis/"
published_at: "2021-01-12T00:00:00+00:00"
first_seen_at: "2026-07-25T22:05:07.405322+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:be0df46ef4ee2a9f64720ef1846fc3c6d20043c2012ffaa0627022d2d336419f"
---

# How to paginate your Sails APIs

Returning a large amount of data in a dataset(say 50,000 rows from your database) to a client at a time is not ideal as this will be overbearing on the user as he/she will have too much information to consume at a time. Doing so also results in a poor user experience as the user will have to wait for your API to return all 50,000 data(imagine your user has a slow connectivity at the time. That’s UX nightmare!)


## What is pagination


Imagine you are creating a Twitter API clone and you have an endpoint to return all tweets. Pagination happens when you return a subset of the tweets(lets say 20 tweets at a time) each time that endpoint is hit until you have returned the entire tweets. This subset, is called a **page** and for each page, we will skip tweets that was in previous pages.


## Benefits of paginating APIs


### Better user experience


Imagine Google returns all 500,000 results matching your search query at once! That’s definitely going to be unnecessary as you most surely won’t want to go through all 500,000 of them.


Pagination helps your content get to the client quicker which in turn allow your users to have a better experience with your application.


### Makes room for Client side pagination techniques and implementations


When you paginate your APIs, it makes it easy for the client to implement pagination techniques like infinite loading(The technique social websites like Twitter, Facebook and Instagram uses), a more traditional pagination technique with the **Next** and **Previous** links, or a numbered pagination implementation.


## Implementation


Now that we’ve gone over what pagination is and why it is important, let’s see how we can paginate our Sails APIs.


### Method 1 - Give the client the flexibility to determine page size


In this implementation, you let the client determine how many records it wants per requests. So here is how we will do it in Sails using the Waterline ORM. Still using our Twitter clone API analogy, say we have an API returning the tweets and we have a` Tweet` model. So in the action returning the tweets our database have, we will implement the action similar to the one below:


```text
// api/controllers/tweet/get-tweets.js
module  .  exports   =   {
friendlyName:   "Get Tweets"  ,


description:   "Returns all tweets"  ,


inputs: {
skip: {
type:   "number"  ,
description:   "Indicate the number of tweets to skip before returning the results."  ,
required:   true  ,
},
limit: {
type:   "number"  ,
description:   "Set the maximum number of tweets to retrieve"  ,
required:   true
},
},
exits: {
success: {
statusCode:   200  ,
},
}
fn:   async  (inputs, exits) {


// fetch paginated tweets from the database
const tweets   =   await   Tweet.  find  ()
.sort([{createdAt:   'DESC'  }])
.skip(inputs.skip)
.limit(inputs.limit)


// Construct the payload to send back to the user
const payload = {
data: {
tweets,
}
}


// Return payload as response
return exits.  success  (payload)
}
}
```


If you don’t yet understand actions2, do go over[Migrating your Sails actions to actions2](https://blog.sailscasts.com/migrating-your-sails-actions-to-actions2)


Let’s look at what we are doing in the above actions:


1.


We are telling the action to expect two **inputs** : skip and limit. The skip input will be used to tell Waterline how many tweets to skip each time it is fetching tweets from the database. While the limit input will be used to tell Waterline the maximum number of tweets to return for each query.


2.


We are then using the Waterline` find` method to get all tweets in the database, and sorting them with the` sort` method so we get the newest tweets first(we are assuming the Tweet model has a createdAt attribute) and then passing down **inputs.skip** and **inputs.limit** to Waterline’s[skip](https://sailsjs.com/documentation/reference/waterline-orm/queries/skip) and[limit](https://sailsjs.com/documentation/reference/waterline-orm/queries/skip) methods respectively.


3.


Finally, we are constructing a` payload` and then returning it as the APIs response using` exits.success` .


So for a client that wants tweets, it will start by sending a GET request to the endpoint sending both skip and limit as query parameters. The first time it makes the request, it will start off with the skip parameter being set to **0** and subsequent requests will add the value of the limit to the current value of skip like so:


```text
// First page
skip   =   0
limit   =   10


// Second page
skip   =   0   +   10   =   10
limit   =   10


// Third page
skip   =   10   +   10   =   20
limit   =   10


// Fourth page
skip   =   20   +   10   =   30
limit   =   10
```


### Method 2 - Allow the API determine the page size


In this method, we hand over the responsibility of determining how many records will be returned per page to the client allowing the client to just ask for pages and not to worry about the size of each page.


So for this method, the client wil trust the API for the page size and just send the page number each time it wants a page.


Here is the implementation:


```text
// api/controllers/tweet/get-tweets.js
module  .  exports   =   {
friendlyName:   "Get Tweets"  ,


description:   "Returns all tweets"  ,


inputs: {
page: {
type:   "number"  ,
description:   "Indicate the page of tweets to be returned"  ,
required:   true  ,
},
},
exits: {
success: {
statusCode:   200  ,
},
}
fn:   async  (inputs, exits) {
// This value is same as inputs.limit in Method 1
const   PAGE_SIZE   =   10


// For example, for page 1 the skip is (1 - 1) * 20 = 0.
// This will pass a skip of 0 to waterline telling it not to skip any posts.
const skip   =   (inputs.page   -   1  )   *   PAGE_SIZE


const tweets   =   await   Tweet.  find  ()
.sort([{createdAt:   'DESC'  }])
.skip(skip)
.limit(PAGE_SIZE)


const payload = {
data: {
posts,
}
}


return exits.  success  (payload)
}
}
```


Again, lets go over what we are doing here:


1.


In this method we are only expecting on input *page* this value should start at one and be incremented by one each time the client wants the next page.


2.


We are setting a constant **PAGE_SIZE** to 20 which will serve as the value passed to Waterline’s limit method to determine the maximum posts returned per each page.


3.


We are figuring out the value to be passed to the` skip` method by subracting one from the value specified as the page number and then multiplying it by the total page size. Doing this make sure we skip the correct amount of posts for each page.


4.


> Do note for each implementation we are zooming in on the minimum amount of code the implementation needs. So we are deliberating ignoring error-handling and other logic you might want to do that is specific to your use case.


## Best practices


Now that you have seen in general the two ways you might want to implement pagination on your APIs let’s look at some best practices.


### Do not paginate on the code-level


What this means is that don’t use methods like` splice()` in JavaScript to paginate as this defeats the purpose of pagination. The reasoning here is, you are still going to be making that expensive query that will return 50,00 records from the database. So pagination should always be done at the database level.


### Always apply skip first before limit


A frequent error when paginating is to apply limit before skip this will produce erroneous results as in the case when you want the limit to be 30 and you want to skip 20, if you apply the limit first, you will only get 10 records back.


### Use the sort method for better pagination results


Based on your use case, you might want to sort the results coming back using some filters. For example in our` get-tweets` action, we want to return all tweets starting from the latest. So we applied` .sort(\[{createdAt: 'DESC'}\])`


### Send the count of total pages and document


Whenever possible send the count of the total number of pages and also the total number of data in the dataset. This will allow the client build out some fun pagination buttons and UIs like identifying the current page, showing how many data are in the dataset(think Google search query) or how many pages the user have to look at.


## Conclusion


Pagination is a tool you will want to keep in your arsenal as it is frequently requested for when you have a lot of data to be returned from an API. This article shows two methods of implementing pagination in Sails and also we discussed some best practices to keep in mind when doing pagination in your Sails API.


It is also worthy of note that these techniques are not unique to Sails, you can apply it to whatever framework or language or ORM you are using(Just strip down the implementation to its fundamentals ignoring the Sails-centric parts)
