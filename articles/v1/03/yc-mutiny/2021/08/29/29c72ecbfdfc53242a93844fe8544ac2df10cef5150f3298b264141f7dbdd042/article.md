---
schema_version: "1.0.0"
document_id: "29c72ecbfdfc53242a93844fe8544ac2df10cef5150f3298b264141f7dbdd042"
company_key: "yc-mutiny"
company: "Mutiny"
source_id: "yc-mutiny-rss-ceed54e2b595"
canonical_url: "https://engineering.mutinyhq.com/event-driven-cdn-caching/"
published_at: "2021-08-18T15:00:00+00:00"
first_seen_at: "2026-07-24T11:48:32.930778+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:77d575cc3666f4fd9f4534e77af3147ad85d9e79d4e4020ff65ea98b4502e7c5"
---

# Event Driven CDN Caching

Here at Mutiny, we are building a no-code conversion platform that enables companies to personalize their customer journey and close more revenue. Fast data delivery is a critical part of our product and is at the core of engineering at Mutiny. We make real-time decisions about what a user should see on customer websites and need an API to serve this information reliably — we call this service "user data". Today, our user data service handles up to 100M requests per month and is rapidly growing as we acquire new customers.


Let's look at an example of this in action. Imagine a Mutiny customer deployed a banner on their homepage to promote a set of new premium features to paying customers who have not yet upgraded. When a user clicks through this banner and switches to the premium plan, our events API processes this information. Now when the user returns to the homepage just a few clicks later, Mutiny must return the latest data about the user and hide the banner as the user no longer fits the criteria for the experience.


In traditional web applications, this API would be served directly by web servers that can pull the most up to date information from the database. This approach lends itself to the real-time nature of our system, where data changes in response to user actions or customer configuration changes such as adding a new data source or deploying a new experience. However, we serve these requests on public websites where traffic is globally distributed at a much higher scale and consistently fetching from our web servers would be costly and lack performance. As a result, we use a CDN in front of our user data service to maintain delivery speeds as our product continues to scale.


## Cache Invalidation with CDNs


Traditional CDNs, like[Amazon Cloudfront](https://aws.amazon.com/cloudfront/?ref=engineering.mutinyhq.com) , are great for caching static content. This setup is typically used for assets that rarely change, like JavaScript, CSS, and images, and can deliver files globally to ensure fast delivery from the edge. However, our user data service returns information that is highly dynamic with a steep penalty on the user experience for using stale data. We must strike a balance with cache invalidation that maintains high cache hit ratios while also ensuring our data is accurate.


Let's take a look at the typical methods of cache invalidation with CDNs.


### Digest Caching


When delivering static assets, most websites will include a "digest" in the filename that is updated whenever the file content is modified. This usually happens as a part of the build process, such as when a Webpack website is bundled. The HTML page requesting these assets will be updated to get the latest files and will be deployed with the new assets. A typical asset requested in this fashion would look like` https://www.mutinyhq.com/_next/static/runtime/webpack-4b444dab214c6491079c.js` . This works well when you can update the consumer of the asset upon change.


Mutiny customers install a JavaScript client on their public website in order to start delivering personalization. This JavaScript calls our user data service to deliver personalization, but these clients don't know when the data on the server has been updated and cannot change each request's digest to take advantage of this caching strategy.


### Cache-Control Header


The[Cache-Control header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control?ref=engineering.mutinyhq.com) is a standard HTTP header that is used to tell clients and proxies how long a given request should be cached. This header supports a high degree of control on when the request should be revalidated on the server, supporting separate directives for CDNs using` s-maxage` and clients using` max-age` . Both of these can be set with a value in seconds that will force a refetch from the origin to get the most up to date information.


One option for us is to set a low` max-age` in the response headers from our user data service and have the CDN update from the origin on that cadence. However, we would have to set this value to just a few seconds in order to drive a dynamic experience . This would negate all the load management benefits of using a CDN and cause the vast majority of the requests to hit the origin, even if a given user's data hadn't changed for several hours.


### API Invalidation


Many CDNs support[cache invalidation](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Invalidation.html?ref=engineering.mutinyhq.com) by sending an API request to the CDN, forcing it to invalidate an entry. For example, with Cloudfront we could send an API call to invalidate all CDN entries for a particular company and require requests after flushing completes to pull the latest data. This works great in event driven systems like ours where we know exactly when data has changed and can maximize how long data is cached for. Unfortunately, most CDNs are not built to process these events quickly and can take up to 24 hours to fully flush the cache across the network. We needed a CDN solution that would allow us to invalidate entries in under a few seconds.


## Caching with Fastly


[Fastly](https://www.fastly.com/?ref=engineering.mutinyhq.com) is a hosted CDN that uses a customized version of the open source HTTP cache[Varnish](https://varnish-cache.org/?ref=engineering.mutinyhq.com) . Varnish is a high performance proxy that has been used for over 15 years and has been a staple of distributing content on the internet. Fastly's hosted solution allows anyone to get a CDN up and running in minutes and provides a number of features on top of Varnish to support applications on the modern web.


### Instant Purge


We use Fastly's[Instant Purge](https://www.fastly.com/blog/building-fast-and-reliable-purging-system?ref=engineering.mutinyhq.com) to invalidate CDN entries using an API request with an average of ~150ms. We also provide custom tags to invalidate groups of entries together using[Surrogate Keys](https://docs.fastly.com/en/guides/getting-started-with-surrogate-keys?ref=engineering.mutinyhq.com) . Every response from our user data service includes a` Surrogate-Key` response header that Fastly processes and uses to label entries to be invalidated together, such as the company the request was issued for and the data sources that were used.


We process about 50M events a day through our Events API and on an update to a user's profile, we can then quickly compute if their CDN entry is stale and use Instant Purge to force an update. We can also do more advanced invalidation in response to customer configuration changes like importing a new field from Salesforce that they want to use in personalization. Rather than purging their entire cache, we can pass the company ID and "salesforce" to the Fastly's Purge API and selectively bust only the entries that were dependent on those fields. These features allow us to keep a high cache hit ratio while guaranteeing that we can support real-time personalized experiences as users navigate around our customer's website.


### Custom Cache Keys


The[Vary header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Vary?ref=engineering.mutinyhq.com) is an HTTP response header that can tell intermediate proxies what should be used for caching. The` Vary` response header lists a set of *request headers* to be used as a cache key and if there is a matching cached object in the CDN when a new request comes in, it will be returned. Fastly allows you to take this a step further through custom[Varnish Configuration Language](https://varnish-cache.org/docs/trunk/users-guide/vcl.html?ref=engineering.mutinyhq.com) (VCL) that can be run at the edge of the CDN.


We often use geography (such as the continent) to determine if objects should be cached separately. Geography is not typically included as a header on requests coming into the CDN, but by writing some custom VCL, we are able to use the continent as a cache key.


```text
sub vcl_recv {
#FASTLY recv
set req.http.X-Continent-Code = client.geo.continent_code;
return(lookup);
}


...


sub vcl_fetch {
#FASTLY fetch
set beresp.http.Vary = "X-Continent-Code";
set beresp.http.X-Continent-Code = req.http.X-Continent-Code;
...
return(deliver);
}


```


A custom VCL file that can deployed with Fastly.


In this example, we add an` X-Continent-Code` header when the request is received using the geography provided by Fastly through the` vcl_recv` function. After the response is fetched from the origin, we add the` Vary` header to indicate that the CDN should cache the response base on the value of` X-Continent-Code` through the` vcl_fetch` function. Now, when requests coming from North America are cached independently from requests coming from Europe — all handled at the CDN layer. This can be extended by using dynamic response headers from the origin server for even more granular caching.


Fast data delivery is critical to building a great personalization product. Whenever we build new features on engineering that touch our user data service, we deeply consider the impacts on speed and how the data can be cached. Fastly has enabled us to maintain a high cache hit rate with user data service, maximize performance, and free up more time to build impactful features for customers. If you are interested in delivering highly dynamic content at scale and working on architectural solutions such as these, we'd love to[hear from you](https://apply.workable.com/mutiny/?ref=engineering.mutinyhq.com) .
