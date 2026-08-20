---
schema_version: "1.0.0"
document_id: "2da86d8eee10efa656913a8f13e4389ce97c2b75182d43bd797e83009859a184"
company_key: "cloudflare-inc-class-a-common-stock"
company: "Cloudflare Inc."
source_id: "cloudflare-inc-class-a-common-stock-rss-72eb55cabac1"
canonical_url: "https://blog.cloudflare.com/r2-ga/"
published_at: "2026-07-21T14:45:23+00:00"
first_seen_at: "2026-07-21T14:50:50.156325+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:5ea07630c14cba312c689d7b2a71747340df3e660644a7edb1609e4abb596ed3"
---

# R2 is now Generally Available

[R2](https://www.cloudflare.com/developer-platform/r2/) gives developers[object storage](https://www.cloudflare.com/learning/cloud/what-is-object-storage/) , without the egress fees. Before R2, cloud providers taught us to expect a data transfer tax every time we actually used the data we stored with them. Who stores data with the goal of never reading it? No one. Yet, every time you read data, the egress tax is applied. R2 gives developers the ability to access data freely, breaking the ecosystem lock-in that has long tied the hands of application builders.


In May 2022, we launched R2 into open beta. In just four short months we’ve been overwhelmed with over 12k developers (and rapidly growing) getting started with R2. Those developers came to us with a wide range of use cases from podcast applications to video platforms to[ecommerce websites](https://www.cloudflare.com/ecommerce/) , and users like[Vecteezy](https://www.vecteezy.com/) who was spending six figures in egress fees. We’ve learned quickly, gotten great feedback, and today we’re excited to announce R2 is now generally available.


We wouldn’t ask you to bet on tech we weren’t willing to bet on ourselves. While in open beta, we spent time moving our own products to R2. One such example, Cloudflare Images, proudly serving thousands of customers in production, is now powered by R2.


## What can you expect from R2?


### S3 Compatibility


[R2](https://www.cloudflare.com/developer-platform/products/r2/) gives developers a familiar interface for object storage, the S3 API. With[S3 Compatibility](https://www.cloudflare.com/developer-platform/solutions/s3-compatible-object-storage/) , you can easily migrate your applications and start taking advantage of what R2 has to offer right out of the gate.


Let’s take a look at some basic data operations in javascript. To try this out on your own, you’ll need to[generate an Access Key](https://developers.cloudflare.com/r2/platform/s3-compatibility/tokens/) .


```text
// First we import our bindings as usual
import   {
S3Client,
ListBucketsCommand,
}   from   "@aws-sdk/client-s3"  ;


// Then we create a new client. Note that while R2 requires a region for S3 compatibility, only “auto” is supported
const   S3   =   new   S3Client  ({
region:   "auto"  ,
endpoint:   `https://${  ACCOUNT_ID  }.r2.cloudflarestorage.com`  ,
credentials: {
accessKeyId:   ACCESS_KEY_ID  ,   //  fill in your own
secretAccessKey:   SECRET_ACCESS_KEY  ,   // fill in your own
},
});


// And now we can use our client to list associated buckets just like we would with any other S3 compatible object storage
console.  log  (
await   S3  .  send  (
new   ListBucketsCommand  (  ''  )
)
);
```


Regardless of the language, the S3 API offers familiarity. We have examples in[Go](https://developers.cloudflare.com/r2/examples/aws-sdk-go/) ,[Java](https://developers.cloudflare.com/r2/examples/boto3/) ,[PHP](https://developers.cloudflare.com/r2/examples/aws-sdk-php/) , and[Ruby](https://developers.cloudflare.com/r2/examples/aws-sdk-ruby/) .


### Region: Automatic


We don’t want to live in a world where developers are spending time looking into a crystal ball and predicting where application traffic might come from. Choosing a region as the first step in application development forces optimization decisions long before the first users show up.


While S3 compatibility requires you to specify a region, the only region we support is ‘auto’. Today, R2 automatically selects a bucket location in the closest available region to the create bucket request. If I create a bucket from my home in Austin, that bucket will live in the closest available R2 region to Austin.


In the future, R2 will use data access patterns to automatically optimize where data is stored for the best user experience.


### Cloudflare Workers Integration


The Workers platform offers developers powerful compute across Cloudflare’s network. When you deploy on Workers, your code is deployed to Cloudflare’s[more than 275 locations](https://www.cloudflare.com/network/) across the globe, automatically. When paired with R2, Workers allows developers to add custom logic around their data without any performance overhead. Workers is built on isolates and not containers, and as a result you don’t have to deal with lengthy cold starts.


Let’s try creating a simple REST API for an R2 bucket! First,[create](https://developers.cloudflare.com/r2/data-access/workers-api/workers-api-usage/#3-create-your-bucket) your bucket and then add an R2[binding](https://developers.cloudflare.com/r2/data-access/workers-api/workers-api-usage/#4-bind-your-bucket-to-a-worker) to your worker.


```text
export   default   {
async   fetch  (  request  ,   env  ) {
const   url   =   new   URL  (request.url);
const   key   =   url.pathname.  slice  (  1  );   // we’ll derive a key from the url path


switch   (request.method) {
// For writes, we capture the request body and write that out to our bucket under the associated key
case   'PUT'  :
await   env.  MY_BUCKET  .  put  (key, request.body);
return   new   Response  (  `Put ${  key  } successfully!`  );


// For reads, we’ll use our key to perform a lookup
case   'GET'  :
const   object   =   await   env.  MY_BUCKET  .  get  (key);


// if we don’t find the given key we’ll return a 404 error
if   (object   ===   null  ) {
return   new   Response  (  'Object Not Found'  , { status:   404   });
}


const   headers   =   new   Headers  ();
object.  writeHttpMetadata  (headers);
headers.  set  (  'etag'  , object.httpEtag);


return   new   Response  (object.body, {
headers,
});
}
},
};
```


Through this Workers API, we can add all sorts of useful logic to the hot path of a R2 request.


### Presigned URLs


Sometimes you’ll want to give your users permissions to specific objects in R2 without requiring them to jump through hoops. Through pre-signed URLs you can delegate your permissions to your users for any unique combination of object and action. Mint a pre-signed URL to let a user upload a file or share a file without giving access to the entire bucket.


```text
import   {
S3Client,
PutObjectCommand
}   from   "@aws-sdk/client-s3"  ;


import   { getSignedUrl }   from   "@aws-sdk/s3-request-presigner"  ;


const   S3   =   new   S3Client  ({
region:   "auto"  ,
endpoint:   `https://${  ACCOUNT_ID  }.r2.cloudflarestorage.com`  ,
credentials: {
accessKeyId:   ACCESS_KEY_ID  ,
secretAccessKey:   SECRET_ACCESS_KEY  ,
},
});


// With getSignedUrl we can produce a custom url with a one hour expiration which will allow our end user to upload their dog pic
console.  log  (
await   getSignedUrl  (  S3  ,   new   PutObjectCommand  ({Bucket:   'my-bucket-name'  , Key:   'dog.png'  }), { expiresIn:   3600   })
)
```


Presigned URLs make it easy for developers to build applications that let end users safely access R2 directly.


### Public buckets


Enabling[public access for a R2 bucket](https://developers.cloudflare.com/r2/data-access/public-buckets/) allows you to expose that bucket to unauthenticated requests. While doing so on its own is of limited use, when those buckets are linked to a domain under your account on Cloudflare you can enable other Cloudflare features such as Access, Cache and bot management seamlessly on top of your data in R2.


Bottom line: public buckets help to bridge the gap between domain oriented Cloudflare features and the buckets you have in R2.


### Transparent Pricing


R2 will never charge for egress. The[pricing model](https://r2-calculator.cloudflare.com/) depends on three factors alone: storage volume,[Class A operations](https://developers.cloudflare.com/r2/platform/pricing/#class-a-operations) (writes, lists) and[Class B operations](https://developers.cloudflare.com/r2/platform/pricing/#class-b-operations) (reads).


- Storage is priced at $0.015 / GB, per month.
- Class A operations cost $4.50 / million.
- Class B operations cost $0.36 / million.


But before you’re ready to start paying for R2, we allow you to get up and running at absolutely no cost. The included usage is as follows:


- 10 GB-months of stored data
- 1,000,000 Class A operations, per month
- 10,000,000 Class B operations, per month


## What’s next?


Making R2 generally available is just the beginning of our object storage journey. We’re excited to share what we plan to build next.


### Object Lifecycles


In the future R2 will allow developers to set policies on objects. For example, setting a policy that deletes an object 60 days after it was last accessed. Object Lifecycles pushes object management down to the object store.


### Jurisdictional Restrictions


While we don’t have plans to support regions explicitly, we know that data locality is important for a good deal of compliance use cases. Jurisdictional restrictions will allow developers to set a jurisdiction like the ‘EU’ that would prevent data from leaving the jurisdiction.


### Live Migration without Downtime


For large datasets, migrations are live and ongoing, as it takes time to move data over. Cache reserve is an easy way to quickly migrate your assets into a managed R2 instance to reduce your egress costs at the touch of a button. In the future, we'll be extending this mechanism so that you can migrate any of your existing S3 object storage buckets to R2.


We invite everyone to sign up and get started with R2 today. Join the growing community of developers building on Cloudflare. If you have any feedback or questions, find us on our Discord server[here](https://discord.gg/V3GEduuBjP) ! We can’t wait to see what you build.


### Watch on Cloudflare TV
