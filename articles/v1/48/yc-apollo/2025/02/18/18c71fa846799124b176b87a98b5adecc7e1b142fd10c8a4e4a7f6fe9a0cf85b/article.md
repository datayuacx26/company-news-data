---
schema_version: "1.0.0"
document_id: "18c71fa846799124b176b87a98b5adecc7e1b142fd10c8a4e4a7f6fe9a0cf85b"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/api-orchestration-with-connectors-thinking-in-entities"
published_at: "2025-02-12T16:51:19+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T20:58:20.301648+00:00"
content_hash: "sha256:3688c0f913bdd154da5eb17e3e229208fc00139666d2e204dc2e82c620a1b7c7"
---

# API Orchestration with Connectors – Thinking in Entities

Since we first[announced Apollo Connectors for REST](https://www.apollographql.com/blog/apollo-connectors-for-rest-apis) at GraphQL Summit 2024, there has been an enormous amount of excitement from the community to dive in. I’ve been working with dozens of Enterprises on their use cases from breaking apart existing monoliths to integrating multiple production OpenAPI specification endpoints. In my work, there have been some patterns that have emerged and I wanted to start a new blog series that helps share the happy path to building faster. One of the most common questions I get is *how do I connect this thing to that thing.*


But before we begin connecting things together, let’s dive a bit into entities. We all have entities in our APIs that are associated with the domains of our business and identifying them at the start is a quick way to get on the right schema design pattern.


## Identifying Entities


Understanding which objects in our APIs are entities might seem trivial to some, but it can get complicated when you have a service catalog of 100s or 1000s of APIs. For the purposes of our schema, it’s great to start looking at endpoints that have unique identifiers for the type. Taking a look at the PetStore swagger from[editor.swagger.io](https://editor.swagger.io/) , we can see that` Pet` is an entity:


The hints we can see to help define entities are:


1. GET route
2. URL structure has unique identifier in it *(i.e. /pet/{petID})*


At the same time, you can see that` Store` doesn’t have a unique identifier available in any of the listed routes. So` Store` isn’t an entity that we would want to expose *(although that could just be an error in the OpenAPI spec because` Store`* *seems like it should be uniquely identifiable, but if I had a nickel for every time I found an incomplete OpenAPI spec I wouldn’t need to have a job* 😂 *)* .


You might also have entities that are uniquely identifiable by multiple parameters in the URL structure and that is perfectly fine. You’ll want to express these entities in your schema as they have already been defined. There is probably multiple good reasons for why that entity is defined in the given domain and we’re not here to rebuild, *we’re here to connect.*


You’ve already invested a lot in your existing REST APIs, they’re not technical debt, they’re technical assets and you just need a better way to unlock them for developers.


## Exposing an Entity in a Connector


Now that we’ve identified some entities, let’s look at how we can connect them. We’ll use a` Product` entity for this example:


```text
// GET /products/:product_id
{
"id": "prod_123",
"name": "OG Olive Putter - Blade",
"description": "The traditional Block in a blade shape is made from a solid block of Olive wood. The head weight is approximately 360 grams with the addition of pure tungsten weights. Paired with a walnut center-line and white accents colors.",
"image": "https://keynote-strapi-production.up.railway.app/uploads/thumbnail_IMG_9102_3119483fac.png",
"price_id": "price_123"
}
```


First we need to design the schema that should be our` Product` entity and we should keep that structure as is. The only exception is that we should avoid exposing anything that is an “entityId” in our schema – this is an anti-pattern. In our example, we shouldn’t expose` price_id` but express the` Price` as another object in our schema:


```text
type Product {
id: ID!
name: String
description: String
image: String
price: Price
}


type Price {
id: ID!
}
```


` Price` surely has additional fields, but we’ll get to that when we want to connect the two entities together. First we need to connect` Product` to our endpoint which we can do by exposing it on the root` Query` . This requires us to also create a` selection` from the JSON response of our API:


```text
type Query {
product(id:ID!): Product
@connect(
source: "products"
http: { GET: "/products/{$args.id}" }
selection: """
id
name
description
image
price: {
id: price_id
}
"""
entity: true
)
}
```


We can take the` id` argument from the` Query.product` field and append it to our URL structure using` $args` . Notice that we actually create a new object in our selection for the` price` field. This is done by adding the “:” to` price` because the` price_id` is returned in the flat structure of our JSON response. We then map the` id` field to the` price_id` .


Finally we marked the` @connect` with` entity: true` to mark this as an entity. This enables us to build other connectors (or GraphQL servers) that can use` Product` and our execution will utilize this root entry point automatically when additional fields of a` Product` are needed.


As a general rule of thumb, if you see “entityId” or “entity_id” in any of your JSON responses, you should expand that into another object or omit it from your schema and selection. Expanding it can be generalized to:


```text
entity: {
id: entityId
}
```


## Connecting` Price` to our` Product`


We already created our` Price` object, but we need to add the additional fields that are exposed in our pricing endpoint:


```text
// GET /prices/:price_id
{
"id": "price_123",
"currency": "usd",
"unit_amount": 499.99
}
```


The challenge here is that we have a price endpoint that requires us to use the id of the` Price` entity to get the details. This is where the magic 🪄of connectors comes in for me. If we simply expose our` Price` as an entity like we did` Product` , our orchestration will just work *automatically* . So let’s copy the same pattern we did, but we don’t need to necessarily make` Price` an entity. We don’t have a use case to connect another entity to the` Price` so we can omit` entity: true` for now:


```text
type Product {
id: ID!
name: String
description: String
image: String
price: Price
}


type Price {
id: ID!
unit: String
amount: Float
}


type Query {
price(id:ID!): Price
@connect(
source: "products"
http: { GET: "/prices/{$args.id}" }
selection: """
id
unit
amount: unit_amount
"""
)
product(id:ID!): Product
@connect(
source: "products"
http: { GET: "/products/{$args.id}" }
selection: """
id
name
description
image
price: {
id: price_id
}
"""
entity: true
)
}
```


Now if someone queries a` Product` , ***our graph router will be able to understand these two connections can be brought together because the` /products/:product_id` endpoint returns the` Price.id` in the selection set*** . *And it all just works together like magic* 🌈


Our operation is automatically translated to the necessary downstream calls 🤯


This is an extremely powerful orchestration of our API endpoints and a pattern you can repeat for any URL structure that follows:


- ` /foo/:foo_id` – where the API response contains` bar_id`
- ` /bar/:bar_id`


But you could also have a different API structure where that pricing information is actually contained in the product url:` /products/:product_id/price` . In this case,` Price` isn’t really exposed as an entity in our API but that doesn’t mean we can’t connect it!


In this case, we just want to connect the specific` Product.price` field. We won’t need the` price(id: ID!): Price` entity connector we previously had. We can use the` @connect` on the` Product.price` field and instead of using the` $args` to provide the product id, we can use` $this` (which means we want to use *this* *types id* ):


```text
type Product {
id: ID!
name: String
description: String
image: String
price: Price
@connect(
source: "products"
http: { GET: "/products/{$this.id}/price" }
selection: """
id
unit
amount: unit_amount
"""
)
}


type Price {
id: ID!
unit: String
amount: Float
}
```


The key here is that since we already spread the` Product.price` with the` price_id` coming from the product entity we defined, now an incoming request that selects the pricing fields will automatically be orchestrated for you in the query plan generated by the Apollo Router:


You can identify this orchestration pattern for any URL structure that follows:


- ` /foo/:foo_id`
- ` /foo/:foo_id/bar`


It really is that simple to make connections where a simple GraphQL operation can be translated to a series of downstream REST calls – *all without writing any additional code in something like a BFF!*


## Using multiple` @connect` directives


Another pattern that you can utilize is placing multiple` @connect` directives on a single field. Each` @connect` can satisfy a portion of the type being returned. In our previous example, we can use a similar` @connect` directive on the root` product` field. The main difference, is now we can have an` @connect` directive that provides the` price` fields information from our second API endpoint:


```text
type Query {
product(id:ID!): Product
@connect(
source: "products"
http: { GET: "/products/{$args.id}" }
selection: """
id
name
description
image
"""
entity: true
)
@connect(
source: "products"
http: { GET: "/products/{$args.id}/price" }
selection: """
price: {
id
unit
amount: unit_amount
}
"""
)
}
```


Now each connector will be only be used based on the fields selected by the incoming operation. This means that the` /products/:product_id/price` endpoint will be called only if the incoming operation selects the price field (i.e.` query { product(id:"1") { price { amount } } }` ). This is just another option in how you can design out your connections!


## Wrap up


In this post, we discussed how you can start identifying and exposing entities already defined in your REST APIs as connectors. Then we covered how we can follow a best practice of expanding “entityId” fields in our API responses into objects that enable us to connect other entities when you see this url pattern:


- ` /foo/:foo_id` – where the API response contains` bar_id`
- ` /bar/:bar_id`


We also covered how to connect an entity to other endpoints where the url pattern relies on the underlying entity id:


- ` /foo/:foo_id`
- ` /foo/:foo_id/bar`


Last, we covered how you can use multiple` @connect` directives as an example with the previous pattern:


```text
type Query {
foo(id:ID!): Foo
@connect(
source: "foo"
http: { GET: "/foo/{$args.id}" }
selection: """
id
...
"""
entity: true
)
@connect(
source: "foo"
http: { GET: "/foo/{$args.id}/bar" }
selection: """
bar: {
id
...
}
"""
)
}
```


This is just the beginning and we’ll be sharing more fun tips on how you can speed up your development and orchestrate your APIs at lightning fast pace. You can[try out connectors today](https://go.apollo.dev/connectors) by getting[started with GraphOS](https://studio.apollographql.com/signup?referrer=blog) for free. Send us your feedback and ask questions in our[community forums](https://community.apollographql.com/tag/connectors) . We can’t wait to see what you create!


Written by


Michael Watson


[Read more by Michael Watson](https://www.apollographql.com/blog/author/watson)
