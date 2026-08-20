---
schema_version: "1.0.0"
document_id: "e2af43aa00af7d59cbd7b14a8444776104e08798a5f0fdc02726acb5e5cb2635"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/manage-time-gated-product-launches-with-graphql"
published_at: "2022-12-15T13:29:30+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:02:37.132176+00:00"
content_hash: "sha256:f0010a777c5c0f900dc55879bf6881ca413a9aa144c7cf1c2fa9be269e4da99c"
---

# Manage time-gated product launches with GraphQL

This post is a part of our[“How to power modern retail apps with Apollo GraphOS”](https://www.apollographql.com/blog/platform/retail/how-to-power-modern-retail-apps-with-apollo-graphos) series. Also in this series:


- [Unify your e-commerce checkout with GraphQL](https://www.apollographql.com/blog/platform/retail/unify-your-ecommerce-checkout-with-graphql)
- [Personalizing the e-commerce shopping experience with GraphQL](https://www.apollographql.com/blog/platform/retail/personalizing-the-ecommerce-shopping-experience-with-graphql)
- [Creating an omnichannel shopping experience with GraphQL](https://www.apollographql.com/blog/platform/retail/create-an-omnichannel-shopping-experience-with-graphql)
- [How to aggregate and share data with third parties using GraphOS](https://www.apollographql.com/blog/platform/retail/how-to-aggregate-and-share-data-with-third-parties-using-graphos)


---


Whether launching a new collection of products or coordinating product drops in an app, there’s often a need to filter queryable product data from a GraphQL API based some time-related metadata.


Typically, there are two possible places to gate product data based on a specific time:


- The backend service that acts as a data source for for the GraphQL API
- The GraphQL resolvers


Ultimately, the net data result will be the same for the client, but which option you choose depends on whether the time-gating should be handled implicitly or explicitly through the GraphQL API.


## Two approaches to filtering


No additions are required to a GraphQL schema when implicitly filtering time-gated product data. Instead, the logic that determines what products are returned is handled by the domain service, and the team that owns that service will be responsible for filtering out any invalid or unavaiable items before the data reaches the resolvers.


Alternatively, product filtering may take place in the GraphQL layer itself when appropriate metadata is available, such as a release date. Inside of the field resolver, the release date can be checked against the current time to determine whether the product should be made available to the client.


For example:


```text
import { productAPI } from "../APIs";


async function product(id: String): Product {


return productAPI.getProductById(id).then(product => {
// Date comparison is tricky, this is for demonstration purposes only!
if (product.releaseDate) {
const now = Date.now();
const release = new Date(product.releaseDate).getTime();
if (release > now) {
return null;
}
}


return product;
});
}
```


## Displaying unavailable products


Sometimes you need to show unreleased products or display a countdown until their release in a client app. For these cases you can include a \`releaseDate\` field as part of the \`Product\` type:


For example:


```text
type Product {
description: String
id: ID!
mediaUrl: String
releaseDate: String
title: String
variants: [Variant]
}
```


You would need to have additional logic in cart-related mutations to prevent users from adding unreleased products to their carts. Again, this logic could be handled either in the mutations field resolvers or in the data source behind the GraphQL API.


## Implications of gating products


When gating product releases via a GraphQL API, there are a few more things that we need to keep in mind. Specifically, we need to think about error handling, connecting entity types across subgraphs, and paginating lists.


First, if a not-yet-available product is filtered from the result (either in the field resolver or the underlying data source) and the field expects an non-null value to be returned, then a GraphQL error will be thrown. This could lead to unexpected error states in a client application.


Additionally, if the` Product` type is defined as an entity in a federated graph, then we may run into similar issues. When the` Product` entity is extended in another subgraph and that subgraph knows about and returns an unavailable product’s ID in its representation of the` Product` from a field resolver, then that ID will leak in the query results even when the results of the product data is determined to be unavailable by the subgraphs that owns` Product` .


For example, this query:


```text
query GetRecommendedProducts {
user {
recommendedProducts {
id
releaseDate
variants {
id
}
}
}
}
```


May return this response:


```text
{
"data":{
“user”: {
"recommendedProducts": [
{
"id":"unreleased-product:1",
"releaseDate": null,
"variants": null
}
]
}
}
}
```


This result would occur when the subgraph that owns the` viewer` field is aware of the` unreleased-product:1` ID, but the products subgraph won’t return the rest of the field data due to time-gating in the \`Product\` resolver or the data source.


However, if either the` releaseDate` or` variant` field was defined as non-null in the schema, then a GraphQL error would be thrown for the previous query. These may be the intended outcomes of running this operation, but it’s important to always consider the edge cases for time-gated results and ensure that the client is prepared to handle various error states.


Lastly, if the results of a query that contains time-gated products can be paginated, then you’ll also want to consider what unexpected client states may result when time-gated products are removed from the list, particularly when the filtering happens within the field resolver.


For example, filtering products in the resolver may result in pages of results being returned to the client with varying numbers of items. And in the case where every item in a particular page of results is time-gated and subsequently filtered, then this will result in an empty list being returned to the client. For these cases, it may be better for the time-gated results to be filtered from the list within the data source to ensure predictable responses are sent to the client.


## Get started with a retail supergraph today


Beyond time-gating products, the best way to see the possibilities of a supergraph is to try one out.[You can explore a retail supergraph schema and run real queries against it here.](https://studio.apollographql.com/public/apollo-retail-supergraph/explorer?explorerURLState=N4IgzghgbgpgJgYQPYBsUwMYBcCWSB2AknCAFwgCccEADAEYQAsdAtAIwBMcbLjMdrABwUBLOvEZM4jAOwA2AGYKQAGnDR4yNJlwEAoviwAnAJ7EyIOBxqyMAVkEsqdCrw72nEGAGZ23uNxyMIzcbHYgAL5AA&referrer=operation_collections&variant=prod)


We also have additional posts in this[series of retail best practices](https://www.apollographql.com/blog/platform/retail/how-to-power-modern-retail-apps-with-apollo-graphos) that dive into different elements of this schema to illustrate how Apollo GraphOS help power essential features of modern retail applications.


If you’d like to talk to an Apollo expert about how a supergraph can power your retail experience,[please reach out to us](https://www.apollographql.com/contact-sales/?referrer=retail-solutions-page) .


Written by


Shane Myrick


[Read more by Shane Myrick](https://www.apollographql.com/blog/author/shane)
