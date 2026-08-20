---
schema_version: "1.0.0"
document_id: "1aa80da675d20265af8a2e7be50f698c7377688c5b241bf90f7d88fd0486c9eb"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/sending-graphql-metrics-to-datadog-with-apollo-engine-d7876b9dbcba"
published_at: "2018-04-17T19:56:18+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:7b4aa104fab678cd253537d3e1786d83c94881d7cbfaf0ce7b1dd3917920fa8c"
---

# Sending GraphQL metrics to Datadog with Apollo Engine

One of our core tenets on the Apollo team is that we want to enable you to use GraphQL in a way that works with your existing technology investments. It’s much easier to convince your coworkers that the benefits of GraphQL are worth the effort if you can bring your favorite tools with you.


There are tons of software-as-a-service platforms developers use every day to understand and manage their apps. We see[Apollo Engine](https://www.apollographql.com/engine/) , our service for GraphQL management and insights, as the natural nexus point between these services and your GraphQL API. Engine handles the GraphQL-specific parts, and can hand off the data to the other tools you use.


That’s why recently, we’ve been working hard on integrating Engine’s GraphQL-specific features with:


1. [CDNs, like CloudFlare, Fastly, and Akamai,](https://dev-blog.apollodata.com/caching-graphql-results-in-your-cdn-54299832b8e2)
2. [Slack, with daily overview reports of your API,](https://dev-blog.apollodata.com/daily-slack-reports-of-your-graphql-query-performance-and-errors-1749311f33e1) and
3. [PagerDuty, with custom triggered alerts](https://dev-blog.apollodata.com/introducing-alerts-in-apollo-engine-e32eeb03b7fa)


Today, we’re introducing another commonly-requested integration: The ability to pipe metrics into **Datadog** , a popular monitoring and analytics platform!


Seeing your GraphQL operation latency in Datadog


We’ve been using this feature ourselves and testing it with some early access customers, and it’s allowed us to easily integrate GraphQL-specific information from our API into our existing backend monitoring dashboard.


Datadog integration is one of the newest features of our Engine[Standard and Pro plans](https://www.apollographql.com/engine/#plans) , which also include benefits like much longer data retention and[proactive alerting](https://dev-blog.apollodata.com/introducing-alerts-in-apollo-engine-e32eeb03b7fa) . If you’re on Community edition today, you can try it out for two weeks with no credit card required!


*If you haven’t added Engine to your API yet, read the*[setup directions here](https://www.apollographql.com/docs/engine/setup-node.html) *or learn more*[on the website](https://www.apollographql.com/engine/) *.*


## Information provided to Datadog


Almost all of the performance and error statistics you can see in the Engine UI are exported through the integration. With today’s integration, you can get:


- Request rates,
- Error rates,
- Cache hit rates, and
- Latency histogram statistics


But it’s much better than just that, since Engine embeds extra information in the data it passes on.


### GraphQL operation tagging


The data sent to Datadog is tagged with GraphQL-specific information, like the name of the GraphQL operation. That way, you can easily filter down to a specific query or mutation by using the provided tags.


Here’s what it looks like in the Datadog UI:


This allows you to integrate any information you want to know about your GraphQL API’s specific operations into your existing Datadog dashboards.


Here’s what one of our operations looks like in a chart:


### Setting up composite alerts in Datadog


Integrating your GraphQL data into a sophisticated monitoring service lik Datadog has additional benefits beyond just being able to see charts. Even though you can get alerts directly from Engine, you also can get sophisticated alerting based on the data piped into Datadog. In particular, you can use[composite alerts](https://docs.datadoghq.com/monitors/monitor_types/composite/) to trigger an alert based on a combination of factors, for example to get notified when an operation’s error rate *and* latency go up.


## Setting it up


Setting up Datadog with Engine is easy.[Get the full details in the docs](https://www.apollographql.com/docs/engine/datadog.html) , but it’s just a few steps:


1. Get a new API key from the[Datadog API integrations page](https://app.datadoghq.com/account/settings#api)
2. In Engine, go to the settings for your service, and enable Datadog integration
3. Paste in your API key from Datadog


Since metrics are tagged with the service they came from, you can use the same key for all of your services.


Think of the satisfaction you’ll get when this button is green and you can see all your metrics coming in! 📈


## Engine: Bringing together all your services


When you go into production with a new GraphQL API, you don’t want to leave all of your existing tooling behind. We all rely on tools like Datadog to bring together our data and operate services, and GraphQL needs these just like any other technology.


But usually, these services don’t integrate directly into a GraphQL API, so you can’t filter by things like your operation name. Plus, once you identify a problem you might need to dive into specific traces and reports to see what needs to be changed.


That’s why I’m really excited about[Apollo Engine](https://www.apollographql.com/engine/) integrating with the world of developer services — Engine can handle all of the GraphQL-specific bits, and enable you to keep using the tooling you need to do your work. And this is just the beginning; stay tuned for more.


Let us know[on Twitter](https://twitter.com/apollographql) if you have other ideas for services you’d like to see integrated with GraphQL and Engine!


Written by


Sashko Stubailo


[Read more by Sashko Stubailo](https://www.apollographql.com/blog/author/sashko)
