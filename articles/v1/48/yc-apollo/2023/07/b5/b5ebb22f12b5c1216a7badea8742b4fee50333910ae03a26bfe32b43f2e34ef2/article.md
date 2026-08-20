---
schema_version: "1.0.0"
document_id: "b5ebb22f12b5c1216a7badea8742b4fee50333910ae03a26bfe32b43f2e34ef2"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/mitigate-scraping-and-bot-attacks-with-graphos"
published_at: "2023-07-14T13:19:39+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:f87afed09e997aa5f1a36ff3d43d4ecbb6f55e1a8b254e5f4c720e9b81bf9cec"
---

# Mitigate scraping and bot attacks with GraphOS

This post is a part of our “[How to build connected travel apps with Apollo GraphOS](https://www.apollographql.com/blog/platform/travel/how-to-build-connected-travel-apps-with-apollo-graphos/) ” series. Also in this series:


- [Delivering personalized travel experiences with GraphQL](https://www.apollographql.com/blog/platform/travel/delivering-personalized-travel-experiences-with-graphql/)
- [Ship products faster with SDUI](https://www.apollographql.com/blog/platform/travel/ship-products-faster-with-sdui/)
- [Seamlessly integrate with partners to enhance the travel experience](https://www.apollographql.com/blog/platform/travel/seamlessly-integrate-with-partners-to-enhance-the-travel-experience/)


---


In this digital age, where data and connectivity are a vital part of a company’s business, APIs have become essential tools for developers to enhance their applications. GraphQL is also becoming the leader for powering the API layer exposed to user-facing apps, providing access to all the data needed to power customer experiences.


However, this accessibility also invites malicious actors like bots to exploit these APIs, including data scraping, spamming, and executing custom queries. To strike a balance between security and utility, it is crucial to implement measures to block abusive bots from consuming your GraphQL API while allowing legitimate ones like search engines to scrape your websites for indexing purposes.


## Shielding a GraphQL API from bots


GraphQL is a powerful technology but its[specification](https://spec.graphql.org/) is intentionally limited to describe just the schema types and operation definitions to define how the API should be run. If you want to add additional security features, you will either need to implement these measures from scratch in your GraphQL server or use[Apollo GraphOS](https://www.apollographql.com/graphos) , an all-in-one solution to build, monitor, and secure your GraphQL API. GraphOS also provides a number of built-in runtime security features that can help travel companies from malicious bots as travel data is a primary target for scraping.


In order for your front end to use your API from a web browser or mobile app it needs to be exposed on the public internet, but if the API is accessible from the public internet that means anyone can view the requests and responses being sent over the wire. If you have not[turned off introspection](https://www.apollographql.com/docs/technotes/TN0008-production-readiness-checklist/#server-side) malicious actors can retrieve the full schema info from your public endpoint and use that knowledge to make additional custom requests to extract custom data sets. You can also add[authentication](https://www.apollographql.com/docs/router/configuration/authn-jwt) to your Apollo Router so that requests have to be made with a valid user tokens, but malicious bots can also simulate real users or use multiple accounts.


Ultimately, you need to strike the right balance between limiting the execution of random, authenticated requests while still allowing the developers of known client applications to craft their unique operations and iterate the shape of those operations over time to accommodate new features.


## Extracting your operations


Most of the time, front-end developers build their components with the queries already defined in the UI code. As a result, anyone navigating through a web app’s source code or network requests using a browser’s developer tools could parse your schema and perform the custom operations by tweaking the request made or implementing a custom call. If we could extract all the operations from our known clients in advance, then we could know ahead of time what operations are the “real” ones versus potentially malicious ones from other sources.


Using[@apollo/generate-persisted-queries](https://www.npmjs.com/package/@apollo/generate-persisted-query-manifest) enables your clients to extract their operations ahead of time. You can then assign a specific id to each operation and have an id to reference at runtime. This also gives you the ability to discuss with your client teams when they want to build new operations or change existing ones when they are building new features, if the change they want to make are appropiate


## Blocking Unknown Operations


Once you have your operations extracted, you can then register them in GraphOS with[Persisted Queries Safelisting](https://www.apollographql.com/blog/announcement/platform/secure-your-graphql-platform-by-safelisting-known-queries/) . The allow the Apollo Router to get the list of approved operations and run in a few different modes, like[auditing the unknown operations](https://www.apollographql.com/docs/graphos/operations/persisted-queries/#audit-mode-dry-run) :


```text
# router.yaml
preview_persisted_queries:
enabled: true
log_unknown: true
```


Or blocking any operations at all that are[not sent by their registered id](https://www.apollographql.com/docs/graphos/operations/persisted-queries/#safelisting-ids-only) :


```text
# router.yaml
preview_persisted_queries:
enabled: true
log_unknown: true
```


## Security in obscure environments


Safelisting works well in situations where you can communicate with your clients teams and provide them access to register their operations, but their are other scenarios where that might not be possible. Travel companies often provide a partner API so other businesses can extract the data they need or upload their company data to display on the travel apps. For these third-party uses cases you still want to protect your API, so using Apollo Router features like[Operation Limits](https://www.apollographql.com/docs/router/configuration/operation-limits) ,[Traffic Shaping](https://www.apollographql.com/docs/router/configuration/traffic-shaping) , and[Coprocessor](https://www.apollographql.com/docs/router/customizations/coprocessor) will allow you to limit the request rate per client. You can even introspect the incoming operation and dynamically calculate the[operation cost](https://ibm.github.io/graphql-specs/cost-spec.html) to have more granular control on how much data partners can query for in a given time-period.


## Get started with a travel supergraph today


Beyond protecting your API from scraping attacks and securing access to your data, the best way to see the possibilities of a supergraph is to try one out.[You can explore a travel supergraph schema and run real queries against it here](https://studio.apollographql.com/public/apollo-travel-supergraph/variant/prod/explorer?explorerURLState=N4IgzghgbgpgJgYQPYBsUwMYBcCWSB2AknCAFwgDMcAZgJwAsATBRALQQCsADB6-T7Va0OANhisAjBmqcA7ACNZMLhUYgANOGjxkaTLgIBRfFgBOAT2JkQMWRDhcMGCawwQYIvhAAcvCPW9PWThaWQ55DHlbRlkQAF8gA&referrer=operation_collections) .


We also have additional posts in this[series of travel best practices](https://www.apollographql.com/blog/platform/travel/how-to-build-connected-travel-apps-with-apollo-graphos/) that dive into different elements of this schema to illustrate how Apollo GraphOS help power essential features of modern travel applications.


If you’d like to talk to an Apollo expert about how a supergraph can power your travel experience,[please reach out to us](https://www.apollographql.com/contact-sales/?referrer=travel-solutions-page) .


Written by


Shane Myrick


[Read more by Shane Myrick](https://www.apollographql.com/blog/author/shane)
