---
schema_version: "1.0.0"
document_id: "c9a903a395c2adb9220d82d7b57757a0044da215a1cf4f5b5ebd5f39ac93c9e7"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/the-new-trace-view-in-apollo-engine-566b25bdfdb0"
published_at: "2017-10-31T23:26:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:27:24.699163+00:00"
content_hash: "sha256:eea26f19b26bed285a46a7df7da486904408c4121ff05a934dbd712feffa7377"
---

# The new Trace view in Apollo Engine

Last week we announced our new GraphQL gateway product and successor to Apollo Optics,[Apollo Engine](https://www.apollographql.com/engine/) . It has the essential GraphQL monitoring features like request tracing and performance metrics over time that Optics did, and adds new functionality like error reporting and whole query caching.


It’s been very exciting to see Optics evolve into Engine over the last year, and in the calm after the storm of[an exciting product launch](https://dev-blog.apollodata.com/introducing-apollo-engine-insights-error-reporting-and-caching-for-graphql-6a55147f63fc) and a truly amazing[GraphQL Summit](https://summit.graphql.com/) last week, I wanted to take some time in this post to talk about the evolution of one of my favorite features in Engine: *the Trace view.*


The new Trace view in Engine!


## The Trace view


Engine collects tracing information on each of your server’s responses and can show you a distribution of request latencies for every query shape in your app. For each duration bucket in those distributions, Engine saves a sample trace of the operation that you can explore through the Trace view, helping you get a precise picture of each GraphQL operation sent to your server and its performance.


### What’s new in the Trace view


As part of the Engine launch, we made a significant upgrade to the Trace view from what it was in Optics. Here are some things you can do now that you couldn’t before:


1. **Immediately discover the critical path of your query** . On render, the Trace view automatically expands the path to the node with the latest “end time”, meaning that, by default, you’re looking at your query’s critical path. This lets you immediately answer the question: “Which fields are making my query take this long?”
2. **Understand how much time is spent outside your resolvers** . Executing a GraphQL query includes doing several things outside of what it takes to just resolve fields. Each query’s trace starts at the point at which it leaves the Engine Proxy and stops at the point at which it returns to the proxy. This means that a full trace could include the network latency between the Engine Proxy and your server, as well as any request queueing your server might do. Your server also needs to both parse and validate the query’s AST when it is received. In the new Trace view, you can now clearly see which part of your query’s overall execution time was spent in the resolvers, and which parts weren’t.
3. **Understand exactly when resolver execution starts and ends** . The trace view now has a cursor that tracks your mouse and clearly shows you which resolvers were active at the specific point you’re hovering over. This makes it easier than ever to identify exactly when resolvers were running and which ones were executing simultaneously.
4. **Explore the parent-child relationship between resolvers** . You can now expand/collapse resolvers that have children, allowing you to clearly identify relationships between resolvers and to see an overview of your query at a glance, all while still being able to dive into the details of how your query executed.


A close-up of the Trace view


## How we got here


One of the best hidden values of GraphQL is that it gives developers across the stack insight into their API that they never had before. Since a GraphQL query specifies exactly what it wants, you know exactly which fields are being used by exactly which clients and queries and when. You can figure out which fields are the most highly requested. You can confidently determine whether or not a field is still being actively used. And best of all, you can find bottlenecks in your queries, because you can trace their exact execution.


When we first started brainstorming what types of tools might be useful for GraphQL developers, building something that could show you a visual “trace” of a query immediately came to mind. We wanted to build a tool that gave developers more insight into how their GraphQL servers were running, and GraphQL’s execution model inspired us to build something offering resolver timing data for queries.


### Apollo Tracer


Our first prototype of what became Optics was a tool called Apollo Tracer. You would install the tracing agent on your server, and it wrapped your resolvers with timing functions that could measure the start and end times of each resolver’s execution. Your server would send batched reports of its query measurements to the Tracer service, which would render those measurements as bars on a chart that could show you how your query was executed visually.


To see if this would be valuable, we instrumented the API of our hosting service, Galaxy, with Tracer and found that one the most high-volume queries had a trace that looked like this:


A query for a list of apps and app metrics, requesting each app separately in series.


We knew we were onto something as soon as we saw this chart, because we immediately recognized that this is a case where a query was resolving a list of data, but it was asking for each item in the list in series instead of in parallel.


When we updated our resolvers to request the list items in parallel instead, we were able to cut the average query response time in half and the new query trace looked like this:


The same query, updated to resolve the apps in parallel instead. The average response time improved from 500ms to 300ms.


This type of insight into the order in which resolvers fire is something that’s hard to catch when looking at code, but is immediately obvious when drawn on a simple chart. So when we launched Optics, tracing was our flagship feature and the one that our customers valued the most.


### Apollo Optics


Optics collected timing information for each request, and would plot the distribution of request durations on a histogram for you. For each duration bucket on the histogram, you could see a sample trace of a query whose overall duration fit into that bucket.


Optics when it launched: the request distribution and a sample trace.


As customers of our own product, we used the insights we gained from these traces in Optics to make performance improvements in the GraphQL queries we wrote in Galaxy.


### Apollo Engine


Over the last year Optics has come a long way along every dimension to become Engine. We now have more features, an improved visual design, and a better and more performant underlying system. As part of building the proxy-based architecture for Engine, we designed a new[community-based tracing standard](https://github.com/apollographql/apollo-tracing) (Apollo Tracing) for server developers that is extensible and a foundation on which many great tools can be built.


In preparation for Engine’s launch, we completely redesigned the tracing feature and upgraded it from rendering a flat list of resolvers to rendering the the tree in which the resolvers actually resolve, maintaining the parent-child relationship between each of them.


The new Trace view as it exists in Engine today


The best part about the new Trace view is that it’s the evolution of a concept we’ve known would be valuable to GraphQL since the first prototype of our Apollo Tracer tool. I’m particularly excited because I believe we’ve finally landed on an interface and interaction model that lets you explore your data in the same way it actually works under the hood.


## What’s next


I look forward to seeing tracing available in every GraphQL server so that we can continue to build more tools like this one, and help developers take full control over what’s happening in their GraphQL layer.


Outside of Engine, I think there are a lot of opportunities to build more tooling for the GraphQL community. I’m particularly excited about ideas like adding a tracing view similar to this one to things like GraphQL DevTools or GraphiQL, which are other valuable tools that developers use to work with and gain insight into their GraphQL APIs.


Engine is free for 1 million queries a month, which is more than enough traces for development and most production applications, so you can[get started improving your server’s performance](https://www.apollographql.com/engine/) ! The new Trace view was extremely fun to build, and I look forward to hearing your stories and seeing what insights you can get into your server’s implementation and execution from both this feature and others.


Want to learn more or start a conversation? Find me and the rest of the Engine team in the #engine channel of the[Apollo Slack](https://www.apollographql.com/#slack) 🙂


Written by


Danielle Man


[Read more by Danielle Man](https://www.apollographql.com/blog/author/dman)
