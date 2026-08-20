---
schema_version: "1.0.0"
document_id: "3c121dc53f3c97da7715871f10d05f15dbf5b307f579e38ca50fd50327b743ce"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-server-2-0-performance-and-error-reporting-built-in-baa6158f3c09"
published_at: "2018-06-26T17:54:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:a6f5747b7859817239621e40c19dd3f3fad546d9c765987d183d914ab227ccc7"
---

# Apollo Server 2.0: Performance and error reporting built in

Over the last few weeks, you might have heard a lot about[Apollo Server 2.0](https://www.apollographql.com/docs/apollo-server/v2/whats-new.html) , which is currently in the RC stage with the final release coming soon. It’s the biggest open source release we’ve worked on since Apollo Client 2.0 last year. Here’s our focus for this major new version:


Apollo Server 2 will come with everything you need to take your GraphQL API from prototype to production.


No more pulling in several libraries, wiring them together, and figuring out best practices on your own. We heard it loud and clear from the community: you should just be able to install one open source tool for GraphQL API development and use it throughout your whole development lifecycle.


We’ve also heard from our customers that the features provided by[Apollo Engine](https://www.apollographql.com/engine/) around performance tracing, error tracking, CDN integration, caching, and more are critical to their production success. Historically, those features have relied on a closed-source component implemented in Go that we called the “Engine Proxy”. This was in conflict with our goal above — that Apollo Server should be the only tool you need. So how did we solve this problem? We picked the most straightforward option:


**Apollo Server 2.0 will contain all of the features of the Engine Proxy, reimplemented with 100% open-source JavaScript.**


This took a lot of work over the past few months, but it means you will be able to run all of the features of Engine anywhere you can run JavaScript. You don’t need to install a separate package anymore, although you can still use the standalone proxy component with non-JavaScript servers.


Features like[Automatic Persisted Queries](https://www.apollographql.com/docs/apollo-server/v2/features/apq.html) and[CDN integration](https://www.apollographql.com/docs/apollo-server/v2/features/cdn.html) now work out of the box, and we’ll talk more about how to use them soon.


Today, we’re going to focus on cloud-based features like performance tracing and error tracking, which you can now set up just by providing an API key. Let’s take a look!


*Learn more about Apollo Server 2.0 in*[the new docs](https://www.apollographql.com/docs/apollo-server/v2/) *.*


---


## Setting up performance tracing and error tracking


Because all of the machinery to collect and report resolver performance data and errors is built right into Apollo Server 2, setting everything up is now extremely simple:


1. Get an API key by[signing into Engine](https://engine.apollographql.com/) and creating a service.
2. Pass the API key in an` ENGINE_API_KEY` environment variable.


That’s all!


### Try it yourself


If you want to see how easy it can be, let’s go through running a simple example to see data show up in Engine.


First, go download the Apollo Server 2.0 Pupstagram example app from here:[https://github.com/apollographql/pupstagram-api](https://github.com/apollographql/pupstagram-api)


Then, log into Engine and create a service to get an API key:[https://engine.apollographql.com](https://engine.apollographql.com/)


Then, run it with the key passed as an environment variable:


```text
ENGINE_API_KEY=<key here> npm start
```


### Running queries to see traces


Open the GraphQL Playground, and run the query below a few times:


```text
query GetCorgiImage {
dog(breed: "corgi") {
displayImage
subbreeds
}
}
```


In the sidebar of the Engine UI, click on the` GetCorgiImage` operation that should show up. Click on the “Traces” tab. Now you can see the per-request traces in Engine:


Now, run the query a few dozen times to get several samples of data. In the “Performance” tab, you’ll can also see the data aggregated over time:


Congratulations! Engine is all set up and ready to go, and you didn’t have to do anything except pass the API key with an environment variable!


### Deploying to production


When you deploy to production, don’t forget to set the environment variable. You can host your Apollo Server app anywhere you like! You can also set up separate services in Engine for production vs. development, so that you can avoid mixing up data from different environments.


## Run everywhere, get performance and error insights


There’s one more focus area for Apollo Server 2.0 that we haven’t talked about that much yet: You should be able to run your GraphQL API anywhere JavaScript runs. And that’s a lot of places:


1. Container-based hosting providers, like[Heroku](https://www.apollographql.com/docs/apollo-server/v2/deployment/heroku.html) and Zeit Now.
2. Serverless environments, like[AWS Lambda](https://www.apollographql.com/docs/apollo-server/v2/deployment/lambda.html) , Google Cloud Functions, and Azure Functions.
3. Edge environments, like Cloudflare Workers and Fly.io.


You heard me right — with Apollo Server 2.0, you’ll be able to deploy your GraphQL API to 150+ edge locations, responding to your users’ GraphQL requests with lower latency than ever before. Learn more in[Matt](https://medium.com/u/f88cf847c7fc?source=post_page-----baa6158f3c09----------------------) ’s opening keynote from Apollo Day:


This is another important implication of moving the performance and error tracking that used to be in the Go-based Engine Proxy to JavaScript inside Apollo Server. Now, you can get the production insights from Engine even when you’re running in a serverless environment like Lambda or an edge environment like a Cloudflare worker.


If running GraphQL on the edge is exciting to you, we’ve set up a form where you can sign up to get news about this concept and become an early tester. Check it out at:[https://www.apollographql.com/edge](https://www.apollographql.com/edge)


---


## Learn more about Apollo Server 2.0 and try the RC


I’m really excited that Apollo Server 2.0 is going to make lots of aspects of GraphQL API development a lot easier, and make it simpler for product developers to stand up a GraphQL layer over their existing backends.


We still often hear that while it’s super exciting on the frontend, adding a new type of API feels like a daunting task, and we’re dedicated to breaking down that barrier. Read more about what we’re doing in Apollo Server 2.0 to make that possible:


- [The new Apollo Server 2.0 getting started guide in the docs](https://www.apollographql.com/docs/apollo-server/v2/getting-started.html) .
- [Data Sources, the new simple way to call REST APIs from GraphQL](https://dev-blog.apollodata.com/easy-and-performant-graphql-over-rest-e02796993b2b) .
- [New best practices guides about topics like schema design, versioning, access control, and more](https://www.apollographql.com/docs/) .


And more coming up soon! We’re just putting the finishing touches on the release, but we’re already using the RC in our own production apps. Join us and send any feedback you have on GitHub!


---


I hope you’ll also[join us at the 3rd annual GraphQL Summit on Nov 7–8 in San Francisco](https://summit.graphql.com/) . With over 800 attendees expected, it’s the largest GraphQL developer event in the world. Super early bird tickets are selling fast, so[register today](https://www.eventbrite.com/e/graphql-summit-2018-tickets-46601841362) to reserve your spot!


Written by


Sashko Stubailo


[Read more by Sashko Stubailo](https://www.apollographql.com/blog/author/sashko)
