---
schema_version: "1.0.0"
document_id: "4e14f9120941e901a6806137f48f4c540d59990f2a56f2207d5cf3febe40233f"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/personalizing-the-ecommerce-shopping-experience-with-graphql"
published_at: "2022-12-13T18:54:30+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:02:37.132176+00:00"
content_hash: "sha256:cc701708c9fc38850b7ff11540c77a8892c340378eea98076826d61b9eda04ce"
---

# Personalizing the e-commerce shopping experience with GraphQL

This post is a part of our[“How to power modern retail apps with Apollo GraphOS”](https://www.apollographql.com/blog/platform/retail/how-to-power-modern-retail-apps-with-apollo-graphos) series. Also in this series:


- [Unify your e-commerce checkout with GraphQL](https://www.apollographql.com/blog/platform/retail/unify-your-ecommerce-checkout-with-graphql)
- [Creating an omnichannel shopping experience with GraphQL](https://www.apollographql.com/blog/platform/retail/create-an-omnichannel-shopping-experience-with-graphql)
- [Manage time-gated product launches with GraphQL](https://www.apollographql.com/blog/platform/retail/manage-time-gated-product-launches-with-graphql)
- [How to aggregate and share data with third parties using GraphOS](https://www.apollographql.com/blog/platform/retail/how-to-aggregate-and-share-data-with-third-parties-using-graphos)


---


Only 42% of e-commerce stores provide a proper mix of alternative and supplementary product recommendations ([Baymard Institute](https://baymard.com/blog/product-page-suggestions) ). Implementing these product suggestions ultimately requires upgrades to both the backend services that can recommend related products based on metadata or even machine learning models as well as the frontend apps that display the data.


However, requiring teams to upgrade to a new API version or make separate calls to a new recommendation service can slow down the speed at which they can roll these features out in customer-facing apps. Luckily, GraphQL can help with this.


## Making product recommendations available


There are many ways you can provide personalized data but the most common is including a list of recommended products on a product page. In a REST-based microservice architecture, you will most likely have some service that takes in a product and user ID and returns a list of suggested product IDs.


It is then usually up to the client app to call the product service again for each recommendation in order to get those products’ details and prices. This makes sense when your primary goal is to maintain separation of concerns in your microservices—you don’t want to duplicate basic product details in the recommendation service because that service should instead be focused on providing the best suggestions possible.


However, with REST-based approach, each client app would need to duplicate this multi-request business logic and you may end up with the infamous “N+1 problem” where each call to resolve recommended product data is a unique call from each client. This is where[Apollo Federation](https://www.apollographql.com/docs/federation) can step in and[help solve our issues](https://www.apollographql.com/docs/technotes/TN0019-federation-n-plus-1/) . Using the federation directives we can link our services together in a new[discovery subgraph](https://studio.apollographql.com/public/apollo-retail-supergraph/schema/sdl?currentSubgraph=discovery&variant=prod) :


```text
extend schema
@link(url: "https://specs.apollo.dev/federation/v2.0", import: ["@key"])


type User @key(fields: "id") {
id: ID!


"""
Suggest products for this user
"""
recommendedProducts(productId: ID = null): [Product]
}


type Product @key(fields: "id") {
id: ID!


"""
Related products for this product
"""
recommendedProducts: [Product]
}
```


These type definitions in the new discovery schema may look minor, but let’s dive deeper to understand what’s going on here.


## Federation 2 type linking


The top of the schema is defining which directive from the[Federation spec](https://www.apollographql.com/docs/federation/subgraph-spec) we would like to use


```text
extend schema
@link(url: "https://specs.apollo.dev/federation/v2.0", import: ["@key"])
```


Next, we are adding a new field to the` User` type to get a list of recommended products. This field will be added to any location where a` User` is returned. That means that we can use the user id to provide more personalized results and we can provide product suggestions on any page we are fetching the user. In the cases where we want to also provide suggestions based on the current product the user is looking at, we will need to allow clients to optionally pass in the product id.


```text
type User @key(fields: "id") {
id: ID!


"""
Suggest products for this user
"""
recommendedProducts(productId: ID = null): [Product]
}
```


Finally, we will provide another way to get to the same data using the` Product` type. This is helpful for the queries where you are not selecting the` User` directly, (such as on the product details page) but still want to provide suggestions. We could have an optional argument here for the user id to support the same user-based suggestions but our user id is actually already passed to the server through the` x-user-id` header, so there is no need to include it in the schema.


```text
type Product @key(fields: "id") {
id: ID!


"""
Related products for this product
"""
recommendedProducts: [Product]
}
```


## Linking to Other Subgraphs


We have also just solved the N+1 problem for free without taking any additional steps. Apollo Federation will take care of figuring out which field is resolved from which subgraph, so when our clients want to make queries that[fetch all the suggested products and the price of each item](https://studio.apollographql.com/graph/apollo-retail-supergraph/explorer?explorerURLState=N4IgJg9gxgrgtgUwHYBcQC4QGIAEBxBFHAQxwBsBLAZyIgDMcqYBzZhGhMHABwCcIwMKCioAdJFnG4AKgAtqjCnBhliKdiRy8ExMloRQIcREjBqKEJDmTMKSBDgDuFFLMVxuZAJ76UMXkh2zJq8xKZG5NS0DBCuCLw8-ILCYkgAjjDxPgDKLGwcYAAKSUIiABQAJHwCpQCSYOg4tQAiAIQAlDjA4jiJNcJlFA04VSXC9Z3dVr04Qz0zKC5kCPO92obGyGCcxf0iXaszc9MzOIsoy4e9AG7EvBRh%2B1Onp1QUAF4rJy%2BGZBC8jmIXiuMz4FCgXxeOAAvodYdN4dCQNCgA&variant=prod) the query planner will optimize to use the fewest requests possible.


## What’s next?


With our schema in place, we can now focus our attention on optimizing the recommendation engine and evolving how we recommend products to our users without ever breaking our existing use cases.


We could add new optional arguments that allow clients to select what type of recommendations they want, like recently viewed or new deals, but the schema can still stay the same. We might even consider making that decision for clients based on who is calling or who the user is so all our business logic lives on the server.


## Get started with a retail supergraph today


Beyond powering better product recommendations, the best way to see the possibilities of a supergraph is to try one out.[You can explore a retail supergraph schema and run real queries against it here.](https://studio.apollographql.com/public/apollo-retail-supergraph/explorer?explorerURLState=N4IgzghgbgpgJgYQPYBsUwMYBcCWSB2AknCAFwgCccEADAEYQAsdAtAIwBMcbLjMdrABwUBLOvEZM4jAOwA2AGYKQAGnDR4yNJlwEAoviwAnAJ7EyIOBxqyMAVkEsqdCrw72nEGAGZ23uNxyMIzcbHYgAL5AA&referrer=operation_collections&variant=prod)


We also have additional posts in this[series of retail best practices](https://www.apollographql.com/blog/platform/retail/how-to-power-modern-retail-apps-with-apollo-graphos) that dive into different elements of this schema to illustrate how Apollo GraphOS help power essential features of modern retail applications.


If you’d like to talk to an Apollo expert about how a supergraph can power your retail experience,[please reach out to us](https://www.apollographql.com/contact-sales/?referrer=retail-solutions-page) .


Written by


Shane Myrick


[Read more by Shane Myrick](https://www.apollographql.com/blog/author/shane)
