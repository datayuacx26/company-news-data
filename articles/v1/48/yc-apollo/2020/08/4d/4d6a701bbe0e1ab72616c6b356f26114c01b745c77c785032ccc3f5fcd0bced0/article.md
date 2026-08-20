---
schema_version: "1.0.0"
document_id: "4d6a701bbe0e1ab72616c6b356f26114c01b745c77c785032ccc3f5fcd0bced0"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/how-apollo-studio-client-awareness-improved-our-lives"
published_at: "2020-08-19T11:00:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:05:18.773660+00:00"
content_hash: "sha256:da4e81ccb5d897e8070e2a4042d7878caafce07dbbe858670d5c174d32161988"
---

# Who’s Using My Graph?

We’re all used to celebrating big, sparkly successes, like major releases, new customer successes, and the end of a long saga of re-architecture. My favorite kind of success, however, often goes unnoticed: **the success of *avoiding failure*** *.*


In our day-to-day lives as engineers, sadly, these events come to pass without much recognition: the sweet quietude of an incident avoided, rolling out a new feature with only a few code changes thanks to a sound architecture, automation set up behind the scenes, keeping your monitors silent. One of my favorite features of Apollo Studio enables exactly this characteristic, and it’s probably not the one you’re thinking of! I’m talking about **GraphQL client awareness** — the ability to understand *which clients* are using *which fields* of your graph.


In this article, I’ll talk about what GraphQL client awareness is, the kinds of workflows it enables, and how we’ve used it for our own graph at Apollo (I know, I know — I’m biased) to avoid incidents and speed up development.


## Maintaining a single client


At Apollo, we started with one main client for our GraphQL API: Apollo Studio’s web UI.


With only a single client, it was easy to determine the context of fields in use. Whenever we wanted to perform a breaking change, like removing a field relied on by a page in the app, it was easy enough to accomplish: add the new field, change the client to use the new one, and remove the old field. Voila!


**Apollo Studio made it easy for us to determine* ***which fields*** *were in use, but sometimes it was important to know who was using them, and how!**


## Challenges maintaining multiple clients


However, as time went on, we added ad-hoc scripts, an admin application, partner integrations, a CLI tool, and a handful of other clients, only known to exist based on tribal knowledge.


With Apollo Studio, it’s easy to keep track of the fields currently being utilized, but that information is incomplete without **knowing which clients are using them** . If we want to remove a field that’s out of date or modify a default argument, just knowing that the field is in use is insufficient. We need to know **which clients are using the field** , **how to assess the cost of a breaking change** , and **how to move forward** .


The desire to have these kinds of insights led us to build client awareness at the end of 2018.


## Client awareness


**Client awareness** is a feature from Apollo Studio that enables you to view operation metrics for each of your application’s different clients (such as web and iOS) and client versions (such as 1.0 and 1.1). It helps you understand how each one interacts with your data graph.


For me, client awareness has always been one of the most valuable insights into our GraphQL layer. It’s saved our team cumulative weeks of engineering time by making usage information readily available as we’re making schema changes.


### Cleaning up old fields


Here’s a good example. As we added more functionality to one of the features in our CLI, we realized that a dependency in our legacy code was slowing us down quite a bit, causing us to over-abstract to maintain backward compatibility support. This legacy code supported a deprecated field in our schema, but Apollo Studio showed that field still had usage! However, just one click of investigation revealed that **the only client using the field was a version of our CLI we had deprecated months before** . Usage was meager enough to justify removing it, and we rejoiced to discard the legacy code we’d been dragging along with every commit 🎉.


Guess we won’t be making any breaking changes to this field any time soon!


### Discovering use cases


Beyond the ability to know **who will be affected** by schema changes, we’ve found another use case for client awareness on the other side of the stack: **understanding the schema when building out operations** . Of course, in an ideal world, all fields would have a thorough description and notes on usage, but we have all run into confusingly named and under-specified parts of our API, and scratched our heads as to what they *actually* do!


The other day, when I was trying to understand *what a field did* , I got a rush from deducing its nature by figuring out which client used it & diving into that client’s source code to understand its use case.


### Managing graph access with client awareness


Client awareness has been so useful to us that we’re considering adopting an[Apollo Server plugin](https://gist.github.com/zionts/dfbd24145d41ed751a6856401c6d59e1) to restrict all operations from unidentified clients. We’re thinking of doing this just to ensure we can track any operation sent to our servers to a specific use case and have a person at the other end of the “client” to talk to.


With partner integrations, we’ve asked users to set their client names to an email address, and we’ve reached out via email whenever we want to make breaking changes. The ability to signal the **exact right audience** with the relevant changes has given our team incredible confidence to clean up pieces of our API without worrying about affecting customers.


When making changes to our` AffectedQueryMetrics` type, we were able to broadcast these changes directly to partners before rolling them out!


## Getting started with Apollo Studio


If you’re not using client awareness in Apollo Studio today, it’s really simple to[get set up](https://www.apollographql.com/docs/studio/client-awareness/) (and impeccably documented, thanks to our documentation lead, Stephen Barlow!):


1. Make sure your Apollo Server (v2+) is reporting usage metrics
2. Add the` name` argument and an optional` version` number to your Apollo Client instance.


```text
const client = new ApolloClient({
name: "YOUR_NAME_HERE",
// optionally, add a version number
version: 'YOUR_VERSION_HERE'
})
```


(There *is* the ability to try to automatically deduce the client name/version, using the[generateClientInfo option](https://www.apollographql.com/docs/studio/client-awareness/#advanced-apollo-server-configuration) in Apollo Server, but we’ve found things usually end up a lot cleaner if you set the` name` option directly) If you’re not using Apollo Client, all you need to do is pass the` apollographql-client-name` and` apollographql-client-version` headers instead, which we’ve done for a number of different scripts.


## We’d love your feedback


If you **are using client awareness** in Apollo Studio, we’d love to hear your story about how it’s changed the way you’ve worked, not to mention how we can improve the feature to make your lives better! We’ve just started a new[GitHub repo](https://github.com/apollographql/apollo-studio-community) to share a change log, expose preview documentation, and collect feedback, so please open an issue on that repo with any requests! If you want to showcase how you’re using Apollo Studio on our blog, reach out tostudio@apollographql.com !


Written by


Adam Zionts


[Read more by Adam Zionts](https://www.apollographql.com/blog/author/adamzionts)
