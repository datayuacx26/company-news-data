---
schema_version: "1.0.0"
document_id: "376c0bb659864396b16afb5c184cec61b797a8379747edb9cd456f66cf161b0b"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/safer-migrations-of-fields-using-progressive-rollouts"
published_at: "2024-04-16T09:21:08+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:00:24.623123+00:00"
content_hash: "sha256:648701ec8a1be00a74c605d8634d9076f6fbdf74754a0105a516c1e000933d27"
---

# Safer migrations of fields using progressive rollouts

In the realm of software development, evolution is not just desirable; it’s inevitable. Technologies advance, methodologies shift, and systems need to adapt to meet changing demands. In this ever-evolving landscape, Apollo Federation 2.7 introduces a groundbreaking feature: progressive` @override` ✨


Rolling out any change to a production subgraph, including field migration, risks degrading the performance of your graph. Progressive` @override` enables you to migrate any part of your graph to a new service and customize how traffic is gradually rolled over to the new service.


## Understanding` @override` : A Primer


Before delving into the nuances of progressive` @override` , let’s take a moment to revisit the concept of` @override` .` @override` provide a mechanism for migrating fields from one subgraph to another. By applying an` @override` directive to a specific field, traffic for that field is rerouted to the designated service, facilitating the breakdown of monolithic architectures or the migration of fields between subgraphs.


```text
# Inventory subgraph
type Product @key(fields: "id") {
id: ID!
inStock: Boolean! @override(from: "Products")
}
```


In the example above, we’re migrating the` Product.inStock` field from the` Products` subgraph to the` Inventory` subgraph. Rerouting all traffic from one subgraph to another all at once could overload the overriding subgraph.


## Override only a portion of traffic


Anything that ships to production, ideally, has a gradual rollout with a certain percentage of traffic. This gives us the ability to roll back any bad deployments with a smaller impact in case everything is just broken (we’ve all been there 😅).


Progressive` @override` makes this simple for our Apollo Router to handle. Simply label your` @override` directive with the supported` percent(x)` syntax.


```text
# Inventory subgraph
type Product @key(fields: "id") {
id: ID!
inStock: Boolean! @override(from: "Products", label: "percent(1)")
}
```


Now our router will begin routing approximately 1% of all traffic to our new Inventory subgraph for the` Product.inStock` field. When we want to ramp that up, we can simply update our schema and once we reach 100%, we can remove the` inStock` field from the original` Product` subgraph 🎉


## Customize progressive` @override` with a feature flag service


It’s great that we can define a given percentage of traffic, but maybe we want to enable some items to bypass this percentage. You may want your beta application to always route to the new faster service 🏃


The router provides an interface for coprocessors and rhai scripts to resolve arbitrary labels. This means you can provide anything as the label and have the router interact with your external coprocessor to resolve the label. For an example implementation of a coprocessor that resolves labels using LaunchDarkly, see[the example](https://github.com/apollographql/router/tree/main/examples/coprocessor-override-launchdarkly/README.md) in the router repo.


## Championing the Monolith Breakup


A common use case we see with progressive` @override` is breaking down a GraphQL monolith. Everyone starts their GraphQL adoption journey with a monolith and although that can take you far, there are challenges as the usage of the graph increases. Eventually there is a desire to break a service off of the monolith, whether it’s to address a performance issue or maybe to utilize a new 3rd party service.


The easiest way to evaluate breaking up the monolith is to start with a known view that has some meaningful value in the overall application. You’ll want to keep the shape of the operation the same for the client so that you can just use a new URL for that single view’s GraphQL query. Let’s take this screen as an example:


```text
query ProductDetails {
product(id: "space-boots") {
sku
name
details
rating
price # The field we want to migrate
}
}
```


We want to migrate the` Product.price` field from our existing monolith to a new microservice. Ideally, this would require no changes to our client or monolith codebase. This is possible with progressive override and the architecture federation provides. Most commonly, teams will begin sending the traffic of the single client view to the hosted Apollo Router instance.


The Apollo Router can handle bridging traffic between your monolith and the new GraphQL service. Some teams will even opt to switch all of a single clients traffic to the router as a step of their progressive rollout (ie. switching the URL in` @apollo/client` to use the newly hosted router). Even though the router acts as a proxy for the rest of the clients traffic, it paves a golden path for adding new services that further break down the monolith.


For our example, we need to create a new subgraph that resolves the` Product.price` field. This will require us defining` Product` as an[entity](https://www.apollographql.com/docs/federation/entities) using the` @key` directive and adding the` @override` directive to` Product.price` . This requires no changes to our existing monolith or it’s schema:


```text
# Existing Monolith
type Product {
sku: ID!
name: String
details: String
rating: Float
price: Float
}
type Query {
products: [Product]
product(id: ID!): Product
}
```


*There are a lot of*[supported servers](https://www.apollographql.com/docs/federation/building-supergraphs/supported-subgraphs) *you can use to build your GraphQL subgraph in your language of choice like Java, C#, Python and more.*


Once our router re-configures with the composed schema, it will have the metadata of the` label` we added (` percent(25)` ). The Apollo Router will then use this information to change the generated query plan based on the percentage label 🎉


Now the` Product.price` field of our graph can resolve from a new microservice in our infrastructure and we can progressively rollout this change with simple updates to the schema. Once we’ve moved to 100%, we can eventually ***delete*** that code from our monolith 🔥


## Wrapping up


Progressive override brings a powerful new tool for better flexibility and control in service migration. Embracing incremental adoption in our migrations helps developers navigate the complexities of the organization’s constantly evolving stack. This also enables platform engineering efforts to tackle self-service migrations for APIs to production 🤯. What would you migrate if you could?


Progressive` @override` is an[Enterprise feature](https://www.apollographql.com/docs/router/enterprise-features) of the Apollo Router and requires an organization with a[GraphOS Enterprise plan](https://www.apollographql.com/pricing/?referrer=docs-content) . If your organization doesn’t have an Enterprise plan, you can test it out by signing up for a free[Enterprise trial](https://www.apollographql.com/docs/graphos/org/plans/#enterprise-trials) .


Written by


Michael Watson


[Read more by Michael Watson](https://www.apollographql.com/blog/author/watson)
