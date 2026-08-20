---
schema_version: "1.0.0"
document_id: "3faea08a0857da0b47d320a2adb27e736d379cd00e20acdb67969745e11b9024"
company_key: "shutterstock-inc-common-stock"
company: "Shutterstock Inc."
source_id: "shutterstock-inc-common-stock-rss-4c794ed87dc5"
canonical_url: "https://tech.shutterstock.com/2019/04/09/reduce-integration-time-javascript-sdk-shutterstock-api"
published_at: "2019-04-09T04:00:00+00:00"
first_seen_at: "2026-07-20T23:18:13.615263+00:00"
fetched_at: "2026-07-28T22:26:51.183990+00:00"
content_hash: "sha256:e5564ba190c9d77fd898dfcc261ed53e81980fe247f6233191df220a4a60c0de"
---

# Reducing Integration Time with Our Javascript SDK for the Shutterstock API

Today the Shutterstock Developer Platform team is excited to release an updated JavaScript SDK for Shutterstock’s API. The SDK includes methods for all endpoints in the API, so you can do anything with the SDK that you can do with the API by itself, like searching and downloading media. We’ve published it as an[NPM package](https://www.npmjs.com/package/shutterstock-api) , so you can install it in your JavaScript, ECMAScript, TypeScript, and Node.JS projects.


The SDK is a little friendlier in JavaScript code than accessing the API directly:


- It’s more readable than direct REST calls to the API because the SDK provides methods like` searchImages()` and` licenseVideos()` to use instead of URLs to the API.
- Using the SDK makes it easier to integrate the Shutterstock API with your code because you don’t have to use a separate HTTP request library and spend time integrating other packages.
- Using the SDK keeps your code up to date with the API’s latest features and ensures that your code is accessing the API correctly.


We’ll be thrilled to hear what you build with our API and SDK, so give them a try and let us know how you use them! See the examples below for some ideas.


## Installation


To install the SDK, use NPM or Yarn:


```text
npm  install   shutterstock-api  --save


```


```text
yarn add shutterstock-api


```


## Authentication


As with the API, to use the SDK you must create an application at[https://developers.shutterstock.com/applications](https://developers.shutterstock.com/applications) and note the client ID and client secret. Then, you can use that client ID and client secret for basic authentication or to request a token for OAuth authentication. For information about authenticating with the SDK, see the[GitHub repository](https://github.com/shutterstock/public-api-javascript-sdk) .


The SDK has the same limitations that are listed in the[Accounts and limitations](https://developers.shutterstock.com/documentation/authentication#accounts-and-limitations) section of the API documentation.


## Using the SDK


After you have installed the SDK and authenticated to the API, you can use the SDK’s classes and methods to access the Shutterstock public API. For example, to search for images with OAuth authentication, pass your token to the SDK, create a` shutterstock-api.ImagesApi` object, and pass your search parameters to its` searchImages()` method. The API returns the results in a promise that resolves to a JavaScript object with the same information as the` GET /v2/images/search` endpoint API response:


```text
const    sstk    =    require  (  '  shutterstock-api  '  );


sstk  .  setAccessToken  (  process  .  env  .  SHUTTERSTOCK_API_TOKEN  );


const    api    =    new    sstk  .  ImagesApi  ();


const    queryParams    =    {
query  :    '  New York  '  ,
sort  :    '  popular  '  ,
orientation  :    '  horizontal  '
};


api  .  searchImages  (  queryParams  )
.  then  (({  data  })    =>    {
console  .  log  (  data  );
})
.  catch  ((  error  )    =>    {
console  .  error  (  error  );
});


```


When you’re searching for media with the SDK, you’ve got access to all of the search options in the Shutterstock API. For example, you can search for images in portrait mode, for images of a certain type, or for images with certain numbers or types of people in them, as in these examples:


```text
const    sstk    =    require  (  '  shutterstock-api  '  );


sstk  .  setAccessToken  (  process  .  env  .  SHUTTERSTOCK_API_TOKEN  );


const    api    =    new    sstk  .  ImagesApi  ();


const    puppyPortraitParams    =    {
query  :    '  puppies  '  ,
image_type  :    '  photo  '  ,
orientation  :    '  vertical  '
};


api  .  searchImages  (  puppyPortraitParams  )
.  then  (({  data  })    =>    {
console  .  log  (  "  Images of puppies in portrait mode:   "  );
console  .  log  (  data  );
})
.  catch  ((  error  )    =>    {
console  .  error  (  error  );
});


const    twoPeopleIn50sParams    =    {
query  :    "  beach  "  ,
people_age  :    "  50s  "  ,
people_number  :    "  2  "
};


api  .  searchImages  (  twoPeopleIn50sParams  )
.  then  (({  data  }    =>  )    {
console  .  log  (  "  Images of two people in their 50s on the beach:   "  );
console  .  log  (  data  );
})
.  catch  ((  error  )    =>    {
console  .  error  (  error  );
});


```


For complete information about the search parameters, see the[API reference](http://api-reference.shutterstock.com/) .


If your account has full API access, you can request licenses for images and download them, as in this example:


```text
const    sstk    =    require  (  '  shutterstock-api  '  );


sstk  .  setAccessToken  (  process  .  env  .  SHUTTERSTOCK_API_TOKEN  );


const    api    =    new    sstk  .  ImagesApi  ();


const    body    =    {
images  :    [
{
image_id  :    "  419235589  "  ,
format  :    "  jpg  "
},
{
image_id  :    "  1079756147  "  ,
format  :    "  jpg  "  ,
}
]
};


const    queryParams    =    {
subscription_id  :    process  .  env  .  SHUTTERSTOCK_SUBSCRIPTION_ID  ,
format  :    '  jpg  '  ,
size  :    '  huge  '
};


api  .  licenseImages  (  body  ,    queryParams  )
.  then  (({  data  })    =>    {
console  .  log  (  data  );
})
.  catch  ((  error  )    =>    {
console  .  error  (  error  );
});


```


This example illustrates how much you can do with a small amount of code by getting information about each image in a collection (lightbox):


```text
const    sstk    =    require  (  '  shutterstock-api  '  );


sstk  .  setAccessToken  (  process  .  env  .  SHUTTERSTOCK_API_TOKEN  );


const    api    =    new    sstk  .  ImagesApi  ();


const    collectionId    =    ""  ;    // Collection ID


api  .  getLightboxItems  (  collectionId  )
.  then  (({  data  })    =>    {
Promise  .  all  (  data  .  map  (  function  (  image  )    {
return    api  .  getImage  (  image  .  id  ).  then  (  imageInfo    =>    imageInfo  );
}))
.  then  (  imagesInLightbox    =>    {
console  .  log  (  imagesInLightbox  )
});
})
.  catch  ((  error  )    =>    {
console  .  error  (  error  );
});


```


For more examples, see the documentation in the[GitHub repository](https://github.com/shutterstock/public-api-javascript-sdk) .
