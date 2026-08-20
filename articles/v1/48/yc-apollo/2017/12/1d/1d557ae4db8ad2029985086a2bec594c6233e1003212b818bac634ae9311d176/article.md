---
schema_version: "1.0.0"
document_id: "1d557ae4db8ad2029985086a2bec594c6233e1003212b818bac634ae9311d176"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/key-takeaways-from-graphql-summit-2017-b292d6fa121b"
published_at: "2017-12-07T22:52:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:26:54.348132+00:00"
content_hash: "sha256:c4dd648c1262fc33c72b6c4545aaf4b6d6efe969c60e19fdcab87b9c07029d31"
---

# Key Takeaways from GraphQL Summit 2017

*This is a guest post from*[Xavier Cazalot](https://medium.com/u/2c55ebc62328?source=post_page-----b292d6fa121b----------------------) *, engineer and GraphQL trainer at*[OK GROW!](https://www.okgrow.com/) *and was originally published at*[okgrow.com](https://www.okgrow.com/posts/graphql-summit-2017-wrap-up) *.*


GraphQL Summit wasn’t about advanced GraphQL: this was cutting-edge GraphQL! — a GraphQL Summit attendee


The conference stage and its +650 attendees


The above quote reflects two awesome aspects of the GraphQL ecosystem:


- Compared to last year, there were *significantly* more people at the conference already using GraphQL, including a large number of people using it in production systems.
- It sounds like people were pretty excited!


I’d like to share five things that stood out to me the most from my conference experience as both an attendee and a GraphQL trainer at the Advance GraphQL workshop.


## Schema stitching


There was a lot of excitement as GraphQL[schema stitching](https://dev-blog.apollodata.com/graphql-schema-stitching-8af23354ac37) , a method to combine multiple schemas into a unified API, was unveiled during both the workshop and the conference. Schema stitching shows promise as a way to structure microservices while getting the advantages of a manageable, central API. We had the opportunity to link an external API to the app’s schema built during the training, which was one of the training exercises that attendees were most enthused about.


If you’re interested in using microservices with GraphQL, you should check out this talk by Jason Lengstorf from IBM where he speaks about his solution,[GRAMPS](https://github.com/gramps-graphql/) (GraphQL Apollo Microservice Pattern Server):


## Schema stitching example from our training class


Here is an example of how we stitch our local schema with a remote one to create the final schema used by the app in our Advanced GraphQL workshop:


## Subscriptions


We’ve been hearing about[subscriptions](http://graphql.org/blog/subscriptions-in-graphql-and-relay/) , the latest feature to be added to the GraphQL specification, for a while, and they are starting to[get real](https://github.com/apollographql/graphql-subscriptions) . This talk by Robert Zhu of Facebook covered the subject really well.


There was also a[talk on live queries by Rodrigo Muñoz](https://www.youtube.com/watch?v=BSw05rJaCpA) . Facebook are experimenting with them internally — though live query functionality isn’t there yet, it’s very promising.


## Subscription example from our training class


Here is an example of how we handle subscriptions on the client (from a React Component) in our Advanced GraphQL workshop:


## Understanding what’s going in your API


With the increased focus on production GraphQL in complex architectures, we saw a lot more interest in understanding what was happening with the GraphQL stack and what it means GraphQL API management at scale:


- We noticed in the training that not everyone was using a batching/caching solution for their GraphQL backend. If you aren’t already, we highly recommend[dataloader](https://github.com/facebook/dataloader) (or a similar tool in your language of choice if you don’t use JavaScript).
- Apollo proposes a[GraphQL tracing extension](https://github.com/apollographql/apollo-tracing) that profiles all of your GraphQL operations, supported by many server implementations.
- [Apollo Engine](https://dev-blog.apollodata.com/introducing-apollo-engine-insights-error-reporting-and-caching-for-graphql-6a55147f63fc) is a new product tailored for production GraphQL users that offers GraphQL query performance monitoring, error tracking, schema management and data caching.


To see how some of these concepts come together, you should watch this talk by Sashko Stubailo of the Apollo team:


## Apollo Client 2.0


The release of[Apollo Client 2.0](https://dev-blog.apollodata.com/apollo-client-2-0-5c8d0affcec7) took the most popular client for GraphQL and made it smaller and more flexible. The network interface is now built from composable links that can not only control network connections, but also retries and errors.


Apollo Client can even store state data in the local cache, potentially even displacing Redux for some developers. It also allows a choice of client cache technologies, which should help for mobile adoption.


Peggy Rayzis dove into some of the details of the 2.0 release and the new projects it will enable in this talk:


### Apollo Link example from our training class


Xavier Cazalot (me! 👋) presenting Apollo Link during the training class


## Health of the ecosystem


GraphQL feels like the “real deal” now. We heard from speakers from the New York Times, Facebook, Twitter, Coursera, Khan Academy, and GitHub, and chatted with attendees from Atlassian, Circle CI, Nike and others.


Talk topics went beyond “what is GraphQL” to how companies are beginning to scale GraphQL:


## Conclusion


GraphQL Summit was an awesome event — full of great people, great discussions and great discoveries. Let’s continue sharing our passion for GraphQL, discover new best practices, and keep building great applications.


We’ve tailored the Advanced GraphQL workshop to this conference, and continue to thanks to attendee feedback. We couldn’t have asked for a better group to work with, and look forward to hopefully doing it again next year. In the meantime, if you’re interested in having a GraphQL Training at your office,please get in touch .


30 people learning advanced GraphQL at Apollo’s HQ


A big thank you to the Apollo team at Meteor Development Group for organizing this second edition of GraphQL Summit and to Lee Byron for doing such an amazing job at MCing the event.


We hope to see you all next year at GraphQL Summit 2018! 🎩


Written by


Xavier Cazalot


[Read more by Xavier Cazalot](https://www.apollographql.com/blog/author/xavxyz)
