---
schema_version: "1.0.0"
document_id: "b8070d37afb0c750088272fbc6a329d1fa1bc2637cd81dce45467aec39d8962f"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/expedia-improved-performance-by-moving-from-schema-stitching-to-apollo-federation"
published_at: "2021-07-27T10:16:04+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:b73210da44050c6ce1e96d1421fa71c2639beef3223c33456a8d786486854993"
---

# Moving from Schema Stitching to Federation: How Expedia improved performance

##


Two weeks ago, we hosted a[webinar with Shane Myrick](https://www.apollographql.com/events/tech-talk/from-schema-stitching-to-federation-expedias-journey/) , Senior Software Developer at Expedia Group entitled, “From Schema Stitching to Federation: Expedia’s Journey.” Shane’s talk covered Expedia’s motivation for adopting GraphQL and why they needed a distributed approach for managing their graph.


Prior to adopting GraphQL, the Expedia Group faced the challenge of delivering a wide array of travel experiences across a growing number of platforms and clients. Expedia adopted GraphQL as a way to simplify the connection layer between their backend capabilities and front-end clients. The graph provides a single point for clients to connect to and for service teams to expose to clients. Adopting the graph helped Expedia reduce the friction and the number of connections and coordination points across their boundary layers. They also saw benefits from adopting a strongly typed language and the performance benefits of adopting a declarative language that allows clients to request just the data they need. Lastly, they saw benefits in being able to extend and merge common types.


*Building a graph across Expedia’s suite of travel products*


**Scaling their graph with schema stitching**


As Expedia began to add more capabilities to their graph, they were concerned about it becoming a single point of failure and preventing conflicts as the number of contributors to their graph increased.


A core requirement was that they wanted to have one schema and one endpoint for their clients while allowing individual teams to own their services and update those services on their cadence. Expedia started using a gateway as a layer between their GraphQL services and their clients to achieve this. They started off using schema stitching as a way to merge types.


*Using a gateway to provide a single endpoint for developers across services*


However, one downside of schema stitching is that Expedia needed to write a significant amount of gateway code to link types across different services. Shane walked through an example of how Expedia wrote code in their gateway to extend a type from their property service to include another type from their reviews service so that clients could query reviews by property. This code enabled the gateway to understand what types existed in different services and merged and extended the schema.


*Writing custom stitching code in the gateway.*


**Challenges emerge with schema stitching**


As they introduced more stitching code in their gateway, challenges emerged. Their gateway code was becoming more complex, and there was no way to determine the “true schema” without running the gateway. Schema changes required a change to the underlying service and changes to the gateway code and a redeploy of all gateways. This complexity started to cause issues with managing their infrastructure. Additionally, Expedia began to be concerned about preventing conflicts when managing updates to their gateway code.


**Moving to Apollo Federation for their graph architecture**


When Apollo Federation was[announced](https://www.apollographql.com/blog/announcement/apollo-federation-f260cf525d21/) in 2019, Expedia began investigating it as an alternative to schema stitching. They liked that[Federation](https://www.apollographql.com/docs/federation/) was published as an open-source[specification](https://www.apollographql.com/docs/federation/federation-spec/) and its declarative, directive-based approach to merging types. Federation’s support for all GraphQL servers was compelling as Expedia had invested a lot in Kotlin and even open-sourced their own[GraphQL Kotlin](https://github.com/ExpediaGroup/graphql-kotlin) server.


With Federation, Expedia could use GraphQL directives to extend services without writing any custom gateway code. They did have to write resolver code in the underlying service to extend the type, but this was easier than the stitching approach as each team could own the evolution of its service.


Expedia saw the following benefits from adopting federation:


1. Standardized the implementation as developers could follow the declarative approach to writing schemas.
2. Able to do offline composition and validation of the schema without spinning up a gateway.
3. Changed the coordination across teams to review gateway code to have a more strategic conversation about the schema architecture and design. The federated approach made reviewing changes to the schema easier as people reviewed the schema files vs. reviewing many lines of code.


*Example of service level resolver code to extend a type across services*


**Making the switch**


To safely migrate from stitching to Federation, Expedia first worked with the individual service teams to add the necessary federated directives, schemas, and resolver code to their GraphQL servers. Then Expedia set up two separate Apollo servers: one using schema stitching and one set up for Apollo Federation. They set up two variants in Apollo Studio’s schema registry to reflect the two different setups. They used a traffic router in front of the gateways to route traffic between the two graphs, eventually ramping up to a 50/50 split between the stitching and federated gateways.


**Benefits from moving from schema stitching to federation**


By running schema stitching versus Apollo Federation as an A/B test, Expedia was able to quantify the impact of moving to Federation, and they were pleased to see some immediate benefits. First, the federated implementation improved gateway processing latency which improved application performance compared to schema stitching. As Shane explained, “With Federation, we saw a reduced latency compared to schema stitching. Because there was a reduced latency, we were able to reduce our compute quite significantly by about 50%” Lastly, Expedia was able to delete many lines of gateway code which simplified their gateway deployments. Now, they can use almost a stock version of the gateway, making it easy for them to scale the gateway across their many brands since they no longer have much custom code in the gateway.


**Key takeaways from moving to Federation**


After moving to Federation, Shane says Expedia solved key customer and client problems by providing a single endpoint while ensuring that the service teams could independently deploy their changes. They also were able to reduce the potential impact of the gateway going down. They could remove the friction of having to work with multiple code bases to modify the schema. Additionally, they found the declarative approach of the Federation was more straightforward, which made it easier for teams to understand what’s happening and see the overall architecture, which elevated the discussions around the graph and reduced the coordination and complexity of scaling the graph.


To learn more about how Expedia’s journey from Schema Stitching to Federation, you can watch Shane’s talk[on-demand](https://www.apollographql.com/events/tech-talk/from-schema-stitching-to-federation-expedias-journey/) now. If you are looking to move from stitching to Apollo Federation, we’ve built a[migration guide](https://www.apollographql.com/docs/federation/migrating-from-stitching/) .


Written by


David Isquick


[Read more by David Isquick](https://www.apollographql.com/blog/author/isquick)
