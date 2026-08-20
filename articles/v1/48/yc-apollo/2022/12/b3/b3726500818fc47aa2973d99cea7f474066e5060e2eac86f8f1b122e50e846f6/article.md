---
schema_version: "1.0.0"
document_id: "b3726500818fc47aa2973d99cea7f474066e5060e2eac86f8f1b122e50e846f6"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/how-to-aggregate-and-share-data-with-third-parties-using-graphos"
published_at: "2022-12-16T13:56:39+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:02:37.132176+00:00"
content_hash: "sha256:c63694faf75eb0afc9a13bf1d6d8a53a0f35b4f22be937720786aea8c41c149f"
---

# How to aggregate and share data with third parties using GraphOS

This post is a part of our[“How to power modern retail apps with Apollo GraphOS”](https://www.apollographql.com/blog/platform/retail/how-to-power-modern-retail-apps-with-apollo-graphos) series. Also in this series:


- [Unify your e-commerce checkout with GraphQL](https://www.apollographql.com/blog/platform/retail/unify-your-ecommerce-checkout-with-graphql)
- [Personalizing the e-commerce shopping experience with GraphQL](https://www.apollographql.com/blog/platform/retail/personalizing-the-ecommerce-shopping-experience-with-graphql)
- [Creating an omnichannel shopping experience with GraphQL](https://www.apollographql.com/blog/platform/retail/create-an-omnichannel-shopping-experience-with-graphql)
- [Manage time-gated product launches with GraphQL](https://www.apollographql.com/blog/platform/retail/manage-time-gated-product-launches-with-graphql)


---


One of the great things about GraphQL is that it allows you to consolidate data across multiple data sources and present all of that data in a unified view to client developers. For example, in a supergraph for a retail business that might mean fetching product attribute data from an internal source while fetching related review data from a third party service.


Apollo Federation makes this easy—even when different teams need to manage portions of the supergraph schema that provide the product and reviews data in two different subgraphs. Let’s take a look at how that would work.


## Linking third-party reviews to products


Imagine that we have a products subgraph that contains the following type definition for` Product` :


```text
# Products subgraph


"""
A specific product sold by our store. This contains all the high level details but is not the purchasable item.
See Variant for more info.
"""
type Product @key(fields: "id") @key(fields: "upc") {
id: ID!
title: String
description: String
mediaUrl: String
upc: ID!


"""
Variants of the products to view specific size/color/price options
"""
variants(searchInput: VariantSearchInput): [Variant]
}
```


We need to add a` reviews` field to the` Product` type, but the reviews data is managed by another team via a third-party service. To accommodate this, that team can create a new reviews subgraph and add the following definition:


```text
# Review subgraph


type Review {
id: ID!


"The plain text version of the review"
body: String


"The person who authored the review"
author: String


"The product which this review is about"
product: Product
}
```


The reviews subgraph won’t need to know anything about products except the UPC of the product that the review is associated with (this is one of the key fields that has been identified for the` Product` type). For brevity, we’ll assume that it’s possible to configure this value in the third-party reviews service and we’ll also assume this value can be retrieved from that service’s API when querying review data.


The team that owns the new reviews subgraph will then need to write resolvers for this new portion of the schema, including a resolver for the` product` field that returns the product ID value so that Apollo Router can forward these values to the products subgraph to be fully resolved with all product details:


```text
const resolvers = {
Review: {
Product: (parent) => parent.product.upc
}
};
```


We can also make the relationship between products and reviews bidirectional. To associate a list of reviews with a product, we can extend the \`Product\` entity type from within the reviews subgraph as follows:


```text
type Product @key(fields: "id") @key(fields: "upc") {
id: ID!
reviews: [Review]
}
```


The reviews subgraph will then need to include a resolver that fetches a list of reviews based on a product’s UPC:


```text
const resolvers = {
Product: {
reviews: (parent) => getReviewsByProductUpc(parent.product.upc)
}
};
```


Once the reviews subgraph is composed into the supergraph, a client developer will now be able to all product and reviews data seamlessly and as though it all originated from the same data source.


## Sharing (some) data back with third parties


Consolidating data across multiple sources is a big win that can help speed up development times for internal teams that build online shopping apps for web and mobile devices, but what about external partners such as third-party sellers that also need access to product data to build their own client apps?


There would likely be some data in supergraph meant for internal use only, such as sensitive user data. So that could mean maintaining two different versions of the supergraph schema to hide this data from external partners, which would require significant development overhead.


Luckily, GraphOS allows you to have the best of both worlds and maintain a single, unified supergraph schema that exposes different views to different API consumers using a feature of Apollo Studio called[Contracts](https://www.apollographql.com/docs/graphos/delivery/contracts/) (currently available for Enterprise plans).


If we wanted to expose a limited partner schema that only revealed product information to partners, then we can use the special` @tag` directive to decorate the` Product` type:


```text
type Product @key(fields: "id") @key(fields: "upc") @tag(name: "partner") {
id: ID! @tag(“internal”)
title: String
description: String
mediaUrl: String
upc: ID!


"""
Variants of the products to view specific size/color/price options
"""
variants(searchInput: VariantSearchInput): [Variant]
}
```


All` Product` fields will inherit the` partner` tag set on the parent type definition. Keep in mind that this also applies to field extensions from other subgraphs (such as the` reviews` field that the reviews subgraph adds to` Product` ) so you will want to apply tags at the object type level with care, or alternatively, apply them at the field level only for finer-grained control.


We also explicitly set an` internal` tag for the product ID field because this value will be used for internal purposes only. Next, we can apply the same partner tag to some product` Query` fields:


```text
type Query {


"""
Get all available products to shop for. Optionally provide some search filters
"""
searchProducts(searchInput: ProductSearchInput! = {}): [Product] @tag(name: "partner")


"""
Get a specific product by id. Useful for the product details page or checkout page
"""
product(id: ID!): Product @tag(name: "partner")
}
```


Now we can create a[Contract variant](https://www.apollographql.com/docs/graphos/delivery/contracts/) in Apollo Studio that includes the` partner` tag and excludes the` internal` tag. And with a Contract variant available in Apollo Studio, we can now configure an Apollo Router to serve this supergraph schema only at an endpoint available for third-party partners to query.


## Get started with a retail supergraph today


Beyond onboarding third-party partners to supergraph, the best way to see the possibilities of a supergraph is to try one out.[You can explore a retail supergraph schema and run real queries against it here.](https://studio.apollographql.com/public/apollo-retail-supergraph/explorer?explorerURLState=N4IgzghgbgpgJgYQPYBsUwMYBcCWSB2AknCAFwgCccEADAEYQAsdAtAIwBMcbLjMdrABwUBLOvEZM4jAOwA2AGYKQAGnDR4yNJlwEAoviwAnAJ7EyIOBxqyMAVkEsqdCrw72nEGAGZ23uNxyMIzcbHYgAL5AA&referrer=operation_collections&variant=prod)


We also have additional posts in this[series of retail best practices](https://www.apollographql.com/blog/platform/retail/how-to-power-modern-retail-apps-with-apollo-graphos) that dive into different elements of this schema to illustrate how Apollo GraphOS help power essential features of modern retail applications.


If you’d like to talk to an Apollo expert about how a supergraph can power your retail experience,[please reach out to us](https://www.apollographql.com/contact-sales/?referrer=retail-solutions-page) .


Written by


Shane Myrick


[Read more by Shane Myrick](https://www.apollographql.com/blog/author/shane)
