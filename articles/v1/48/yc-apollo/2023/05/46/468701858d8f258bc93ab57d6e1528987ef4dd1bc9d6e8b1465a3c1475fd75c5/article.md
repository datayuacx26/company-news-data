---
schema_version: "1.0.0"
document_id: "468701858d8f258bc93ab57d6e1528987ef4dd1bc9d6e8b1465a3c1475fd75c5"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/prevent-graph-misuse-with-operation-size-and-complexity-limits"
published_at: "2023-05-04T11:29:42+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:497697c09d83306f04c5a2c6cb7d465ac1bd2bc086a26990a14e53d742976de0"
---

# Prevent graph misuse with operation size and complexity limits

A GraphQL API with the power to give app developers everything they need in a single query is excellent for delivery velocity, but it also could have the potential to enable improper access to data if not secured correctly.


With supergraph architecture, you can mitigate this risk by implementing the proper security controls in each subgraph, but Apollo GraphOS is helping platform teams add an additional layer of security in front of subgraphs by building controls into the graph router itself.


Router security is one of the key themes in our 2023 GraphOS roadmap, and today we’re excited to ship a preview of our newest router security feature for GraphOS Enterprise: operation size and complexity limiting.


## Centralized protection from misuse


For production graphs, preventing accidental or intentional misuse is a key requirement. If not protected properly, client apps can issue expensive queries that cause excessive load on the server potentially resulting in denial of service. Even in less extreme cases, poorly formed queries can still impact the performance of client apps and lead to a slower user experience.


There are a variety of packages designed to limit operation complexity and cost at the server level; however, as a federated graph grows, it can become increasingly difficult to set, standardize, and maintain these limits across many subgraphs owned by different teams. Luckily, supergraph architecture presents us with the unique opportunity to centralize control in a single point: the router.


Enforcing operation size and complexity limits in the graph router makes it far easier to standardize policies for a large graph and alleviates the burden of coordinating limits across every subgraph.


## Limit depth, height, root fields, and aliases


Just like it sounds! With GraphOS Enterprise, you can now configure your router to limit the depth, height, number of root fields, and number of aliased fields of any incoming GraphQL operation. If an operation exceeds any of the set limits, the router will reject it before attempting to resolve the request with a` 400 BAD_REQUEST` status code.


All of these limits can be configured in the` router.yaml` file for any router authenticated with your GraphOS Enterprise credentials:


```text
preview_operation_limits:
max_depth: 100
max_height: 200
max_root_fields: 40
max_aliases: 30
```


Optionally, you can also configure the router to only log a warning message for operations that exceed the set limits rather than rejecting the operation outright:


```text
preview_operation_limits:
# some limits
warn_only: TRUE
```


` warn_only` defaults to` FALSE` , but it can be useful to set it to` TRUE` in development environments for testing purposes.


## Getting started


The feature preview for operation size and complexity limits is available now for all users with a GraphOS Enterprise plan. To get started, make sure that your router is authenticated with your GraphOS Enterprise credentials and is running the[latest version of Apollo Router](https://github.com/apollographql/router/releases) . Then, head to[the documentation](https://www.apollographql.com/docs/router/configuration/operation-limits) for detailed information about configuring limits in your` router.yaml` file.


If you’re not a GraphOS Enterprise customer yet,[get in touch with us](https://www.apollographql.com/contact-sales) to learn more and start a trial!


We’re excited for you to start setting some limits and to hear what you think! If you encounter any problems or have feedback, feel free to[open an issue](https://github.com/apollographql/router/issues) . That being said, this feature is still in preview, so there may be some final adjustments before we mark it as GA. To stay up to date, be sure to bookmark the[Apollo Router changelog](https://github.com/apollographql/router/blob/dev/CHANGELOG.md) .


Happy querying!


Written by


Vivek Ravishankar


[Read more by Vivek Ravishankar](https://www.apollographql.com/blog/author/vivekravishankar)
