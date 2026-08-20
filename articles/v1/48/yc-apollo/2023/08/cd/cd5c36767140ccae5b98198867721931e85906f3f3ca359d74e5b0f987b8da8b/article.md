---
schema_version: "1.0.0"
document_id: "cd5c36767140ccae5b98198867721931e85906f3f3ca359d74e5b0f987b8da8b"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/secure-your-graphql-platform-by-safelisting-known-queries"
published_at: "2023-08-10T15:32:45+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:9f93e1a2268ffb81b59557b5f9e40d0d44214ea42a5c5e1af33489d4df48bd06"
---

# Secure your GraphQL platform by safelisting known queries

GraphQL is designed to offer a more flexible, self-serve developer experience, but its open-ended approach often raises questions about its security model. The most common objection is that GraphQL APIs allow unbounded operations — a trait that can degrade performance or result in denial-of-service if too many complex queries are sent to the endpoint. A[supergraph architecture](https://www.apollographql.com/blog/the-supergraph-a-new-way-to-think-about-graphql/) can compound this risk since it generally composes many services across an organization, significantly increasing the potential complexity of executing queries.


In order to deliver performance and security at all times, API platform teams should have the means to eliminate exposure to the potential performance impacts of excessively complex queries. In the spirit of a true platform, teams should be able to **proactively define safelisted queries upfront in a way that encourages iteration in development but provides mission-critical security in production** *.*


## Proactively safelist with GraphOS


Today, Apollo is introducing a new approach for preventing unbounded access to GraphQL APIs:[GraphOS persisted query safelisting](https://www.apollographql.com/docs/graphos/operations/persisted-queries) . With persisted query safelisting, API teams can prevent service degradation from complex queries by centrally safelisting known operations in[Apollo Router](https://www.apollographql.com/docs/router/) — Apollo’s blazing-fast GraphQL gateway and query planner.


GraphOS offers varying levels of safelist restrictions that enable incremental adoption and flexibility in development environments without sacrificing security in production. The *audit-only* configuration gives teams a frictionless way to get started with safelisting by logging unknown operation shapes without terminating the request. Meanwhile, production environments demanding maximum security and performance can use an *ID-only* configuration to accept only requests with a known hash, terminating all others.


Persisted query safelisting is available in preview today for Apollo customers on the GraphOS Enterprise plan – read the docs and try it out! We would love to hear your thoughts and learn how we can make safelisting work better for your use case. Over the coming weeks, we’ll be using your feedback to polish up the user experience ahead of the GA release.[Head to the docs](https://www.apollographql.com/docs/graphos/operations/persisted-queries) to get started or read on to learn more about how you can configure safelisting for your development lifecycle.


## How safelisting works in GraphOS


Nearly all APIs are designed to serve a finite set of first-party client apps, and, by their very nature, GraphQL services are aware of the shape of data requested by those applications. In this closed development ecosystem, we have the tools to implement a GraphQL gateway that significantly limits surface area by *only* allowing known operations from our first-party apps.


### Register operations from first-party apps


GraphOS gives client teams a self-serve experience for registering queries from their first-party apps to a central graph registry. Simply generate a persisted query manifest file for any app using Apollo Client (web, iOS, or Kotlin) and register it to a graph in GraphOS using the Rover CLI. The persisted query manifest includes a unique *persisted query ID* hash for each operation which can be used in place of the query string to improve app performance once persisted queries are configured in the router.


### Configure safelisting incrementally


After client teams register known queries to a graph in GraphOS, API platform owners can enable persisted queries for connected instances of Apollo Router. Once enabled, the router pulls the list of persisted query strings and IDs from the graph registry and stores them in memory to be used for safelisting and ID-only operations.


Enforcing severe safelisting rules in the router is as simple as updating a few lines of router config, but to prevent any breaking changes in your apps, it is best to start with a flexible configuration and incrementally move to a more restrictive approach in production. In the remainder of this post, we’ll walk through the various ways you can adapt safelisting to your use case.


## Flexible safelisting for every stage of development


Generally, you should configure your router to be more restrictive in production environments and less restrictive in development environments, but your particular use case and GraphQL client may limit how restrictive your router can be. GraphOS offers the ability to enforce more severe or less severe safelisting restrictions depending on your use case.


### Audit-only for agile development and adoption


In development environments or when you’re just getting started with safelisting, it is generally best to simply log any unregistered operations without terminating them. The audit-only configuration provides this flexibility, helping you determine if any operations are missing from the registry in GraphOS. Here’s how you can configure your router for audit-only:


```text
# router.yaml


preview_persisted_queries:
enabled: true
log_unknown: true
```


### Lock down and optimize production endpoints


When you’re confident that all supported operations are registered in GraphOS, you can enable more restrictive safelisting in production environments.


#### Maximize security and performance with ID-only operations


ID-only operations are the most restrictive, but they also are the most efficient since the router can avoid processing any complex, freeform query strings (similar to[automatic persisted query caching](https://www.apollographql.com/docs/router/configuration/distributed-caching/) ). In this mode, **clients are required to send requests to the router with *only* a unique persisted query ID** which the router can use to identify the requested operation. The router will reject any requests with an invalid ID or a freeform query string.


```text
# router.yaml


preview_persisted_queries:
enabled: true
safelist:
enabled: true
require_id: true
```


ID-only operations are supported by all Apollo Clients — web, iOS, and Kotlin — but support in other libraries may vary.


#### Allow any known shapes for more flexible enforcement


In the case that your GraphQL client does not support ID-only operations, you can use less stringent shape-based safelisting. Shape-based safelisting is also useful in cases where ID-only operations are excessively restrictive. For example, it may be useful for pre-production apps to query a staging graph using the full query string for easier debugging. In these cases, you can use the default option for safelisting which will accept operations without a persisted query ID as long as they match the shape of a registered operation:


```text
# router.yaml


preview_persisted_queries:
enabled: true
safelist:
enabled: true
```


## Defense-in-depth


Safelisting known operations in your router contributes yet another layer to your defense-in-depth strategy for your API platform. However, like any security measure, is most effective when layered with additional checks.


### Govern operation shape


One of the best checks to combine with safelisting is a general[limit on operation complexity](https://www.apollographql.com/docs/router/configuration/operation-limits/) . Setting a maximum height, depth, number of aliases, and number of root fields for any given operation creates a safety net to prevent accidentally or intentionally malformed queries from impacting the performance of your API.


These limits are table-stakes for public endpoints where safelisting isn’t an option but are also helpful in-house to prevent excessively complex operations from accidentally being safelisted.


### Limit scope of access


Safelisting restricts incoming requests to preregistered operations, but for compliance purposes, you may also need to limit the scope of data available in a particular endpoint. For example, if you have a supergraph that contains PCI, you may need a separate endpoint for client apps that removes that information.


With GraphOS you can use[contracts](https://www.apollographql.com/docs/graphos/delivery/contracts/) to systematically create filtered endpoints that remove any sensitive or unnecessary information. For each of your filtered endpoints, you can create a tailored query safelist specific to its clients.


## Get started with safelisting


Regardless of where you are in your journey with GraphQL, persisted query safelisting is a great way to reduce your exposure to malformed or malicious requests. Customers on the Apollo GraphOS Enterprise plan or[Enterprise Trial](https://studio.apollographql.com/signup?type=enterprise-trial&referrer=blog) can start using safelisting today. To get started, head over to the[GraphOS docs](https://www.apollographql.com/docs/graphos/operations/persisted-queries) !


Written by


Vivek Ravishankar


[Read more by Vivek Ravishankar](https://www.apollographql.com/blog/author/vivekravishankar)
