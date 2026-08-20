---
schema_version: "1.0.0"
document_id: "fd9b224de6b2a6cc52a17b48fddfa6c0224c9da28f3e16bb01af2b3c847e064e"
company_key: "yc-artillery"
company: "Artillery"
source_id: "yc-artillery-news-import-69b75325e6e9"
canonical_url: "https://www.artillery.io/blog/boost-graphql-with-stellate-graphcdn"
published_at: "2022-06-09T00:00:00+00:00"
first_seen_at: "2026-07-21T07:55:08.009846+00:00"
fetched_at: "2026-07-28T21:33:49.818370+00:00"
content_hash: "sha256:66e8ff46c88d8bfce50312806987ed4e410941d73196eff5a2a6ba834cd08e5f"
---

# Load testing Stellate, the CDN for your GraphQL API

June 9th, 2022[How to](https://www.artillery.io/blog/tag/howto)


# Load testing Stellate, the CDN for your GraphQL API


Dennis Martinez


Hassy Veldstra


## GraphQL is everywhere


GraphQL is becoming one of the standards among the different types of APIs available, thanks to their flexibility in allowing consumers to choose the data they need. Artillery can help you regularly test your GraphQL APIs to detect and eliminate potential problems, as shown in the article[Using Artillery to Load Test GraphQL APIs](https://www.artillery.io/blog/using-artillery-to-load-test-graphql-apis) . However, as seen in the article’s examples, GraphQL’s flexibility also makes it easy to have a less-performant API.


Site reliability engineers and developers can spend weeks hunting down and fixing GraphQL performance issues, trying to squeeze as much efficiency as possible. Some problems like improperly fetching data associations or optimizing inefficient database queries can take you a long way to improve the performance of your APIs. However, minor incremental performance updates can quickly become a time suck.


## Can we cache GraphQL?


On most GraphQL APIs, the bulk of the work they perform is fetching data from a data source. Usually, it’s the same data fetched repeatedly. If your API is primarily read-heavy, an alternative for performance is to use a Content Delivery Network (CDN). A CDN temporarily caches your API responses and serves them much quicker to consumers since the data gets retrieved and sent through globally distributed servers much closer than your servers.


Caching in GraphQL is less straightforward than with RESTful HTTP because GraphQL lacks an URL-like primitive that can serve as a unique ID for caching (see[Caching](https://graphql.org/learn/caching/) in the official docs,[GraphQL & Caching: The Elephant in the Room](https://www.apollographql.com/blog/backend/caching/graphql-caching-the-elephant-in-the-room/) and[HTTP caching in GraphQL](https://blog.logrocket.com/http-caching-graphql/) for more details on the challenges of caching in GraphQL)


However, there’s now a CDN specifically built for GraphQL services called[Stellate](https://stellate.co/) (formerly known as GraphCDN). In this article, we’ll check out[Stellate](https://stellate.co/) by setting it up for an existing GraphQL API and load testing it with Artillery to verify whether it can boost performance of our API without any changes to the backend service.


## Setting our baseline


### Setting up our test


Our example GraphQL API allows consumers to fetch a list of users in a database or retrieve a single user based on their ID. For this example, we’ll use two GraphQL queries to get this information from the backend service and include a couple of fields related to the data we find. These queries are read-only, a good use case for load testing the API through a CDN since the underlying data won’t change between requests.


We’ll use the following Artillery test script to go through these queries:


```text
config  :
target  :   'https://graphql.test-app.dev'
phases  :
-   duration  :   600
arrivalRate  :   25
processor  :   './helper-functions.js'


scenarios  :
-   name  :   'Fetch user data'
flow  :
-   post  :
url  :   '/'
json  :
query  :   |
query UsersQuery {
users(limit: 100) {
id
}
}
capture  :
json  :   '$.data.users'
as  :   'users'


-   loop  :
-   post  :
url  :   '/'
beforeRequest  :   'selectRandomUserId'
json  :
query  :   |
query UserQuery($userId: ID!) {
user(id: $userId) {
id
username
email
}
}
variables  :
userId  :   '{{ selectedUser.id }}'
count  :   10
```


The test script will generate 25 virtual users per second for 10 minutes. Each VU will first make a query to retrieve 100 users from the API and get their IDs using the` users` GraphQL query. Next, it will go through a[loop](https://www.artillery.io/docs/guides/guides/http-reference#loops) ten times to retrieve a single user using the` user` GraphQL query. Inside our loop, we’ll use the` selectRandomUserId` function to take a random ID from the previous data retrieval to make the API query.


The` selectRandomUserId` function comes from[custom JavaScript code](https://www.artillery.io/docs/guides/guides/http-reference#loading-custom-js-code) that we load to use in our Artillery tests. we’ll invoke the function in a` beforeRequest` hook. Our custom JavaScript code inside the` helper-functions.js` file contains the following:


```text
module  .  exports   =   {
selectRandomUserId: selectRandomUserId,
};


function   selectRandomUserId  (  requestParams  ,   context  ,   ee  ,   next  ) {
// Select a random user from the `users` variable set in a prior request.
const   users   =   context.vars[  'users'  ];
context.vars[  'selectedUser'  ]   =
users[Math.  floor  (Math.  random  ()   *   users.  length  )];


return   next  ();
}
```


### Load testing the origin


This basic test will give us a good idea of how much read-only traffic our GraphQL server can withstand. Assuming the test script is under` users-test.yaml` , we will run an Artillery load testing from the` eu-west-2` AWS region with the following command:


```text
artillery   run-test   --region   eu-west-2   users-test.yaml
```


:::info We’re using[Artillery Pro](https://www.artillery.io/product) to run the test from our own AWS account rather than from a local machine for more realistic traffic generation. We’re using[AWS Fargate](https://aws.amazon.com/fargate/) for a serverless experience to avoid needing to set up any infrastructure for load testing from the cloud. :::


The test will send virtual user traffic directly to our GraphQL endpoint for ten minutes. When the test wraps up, we’ll see our results:


```text
All VUs finished. Total time: 12 minutes, 0 seconds


--------------------------------
Summary report @ 08:31:17(+0100)
--------------------------------


vusers.created_by_name.Fetch user data: ..................... 15000
vusers.created.total: ....................................... 15000
vusers.completed: ........................................... 14999
vusers.failed: .............................................. 1
http.request_rate: .......................................... 274/sec
http.requests: .............................................. 164990
http.codes.200: ............................................. 164988
http.responses: ............................................. 164989
http.codes.502: ............................................. 1
http.response_time:
min: ...................................................... 66
max: ...................................................... 750
median: ................................................... 80.6
p95: ...................................................... 89.1
p99: ...................................................... 96.6
errors.ETIMEDOUT: ........................................... 1
```


For read-only queries, these results **aren’t great** . With a median response time of >80 milliseconds for each virtual user’s flow and the 99th percentile hitting >96 milliseconds, the GraphQL API looks to struggle a bit under load. A few VUs also hit some timeouts, which doesn’t give us the confidence that we can scale this service any further. Remember that we’re testing a read-only endpoint, i.e. even though our queries vary the underlying data hasn’t really changed. Let’s see how Stellate can help here without spending too much engineering time fixing the issue.


## Setting up Stellate


Setting up Stellate for an existing GraphQL API is a straightforward process. At a high level, Stellate will set up an edge cache between your GraphQL service and any consumers, caching all the query results that go through. Consumers will communicate with your API through Stellate instead of directly to your server. When a query has a cache hit, Stellate serves it up without going to your service — much faster than a direct query. How much faster? We’ll see in a bit.


First, we’ll sign up for a Stellate account. Stellate provides a generous free tier supporting up to 5 million CDN requests per month so that you can get started quickly. After signing up and setting up your organization, Stellate asks you to create a new service by entering the URL of your GraphQL API. It will then attempt to fetch your GraphQL backend schema. Don’t worry if you don’t, as you can proceed without it. If your GraphQL service requires authentication, you’ll need to enter the information for Stellate to work correctly. You’ll also need to enter your desired subdomain that will serve as your service URL.


Once you finish this single step, Stellate will create a new edge cache with your service URL ready for you to use immediately. It can’t get any simpler than that. The only thing you need to do is replace your service’s URL with the provided Stellate service URL for any consumers of your API, and you can begin using the CDN instantly.


### Load testing Stellate


Let’s see how the CDN helps with performance by running the same Artillery test script pointing at our new Stellate edge cache. The only change we need to make is to switch the` config.target` URL in our Artillery test script.


Let’s go back to running the tests, but this time we’ll run the tests on ten workers (up from one that we ran the first test with) to increase the amount of traffic to the service by 10x:


```text
artillery   run-test   --region   eu-west-2   --count   users-test.yaml
```


```text
All VUs finished. Total time: 12 minutes, 3 seconds


--------------------------------
Summary report @ 08:44:21(+0100)
--------------------------------


vusers.created_by_name.Fetch user data: ..................... 150000
vusers.created.total: ....................................... 150000
vusers.completed: ........................................... 150000
http.request_rate: .......................................... 2773/sec
http.requests: .............................................. 1650000
http.codes.200: ............................................. 1650000
http.responses: ............................................. 1650000
http.response_time:
min: ...................................................... 1
max: ...................................................... 81
median: ................................................... 2
p95: ...................................................... 4
p99: ...................................................... 7
```


How about that? the median and 99th percentile response times are **an order of magnitude lower when using Stellate, despite throwing ten times more traffic at it** .


## Conclusion


If you’re a developer and you’re worried about running into performance issues with your GraphQL APIs, it sure looks like Stellate can help with intelligent GraphQL-specific caching to reduce load on your origin server and improve the experience of users of your service.


If you’re an SRE, you know that testing critical dependencies of the services you’re looking after is part of[Testing For Reliability](https://sre.google/sre-book/testing-reliability/) . *“If you haven’t tried it, assume it’s broken.”* is a a great guiding principle, which often extends to *“Trust but verify”* when it comes to performance & reliability claims by third-party services. Artillery is there for you for those scenarios. In this case, we’ve verified that Stellate is doing a stellar job.


(It goes without saying that you should always make sure to check and comply with ToS of any hosted services when it comes to load testing.)
