---
schema_version: "1.0.0"
document_id: "5925007530be00f9cd2ee893266564314b71ca796096a32eb7ac1122e857e81c"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/create-an-omnichannel-shopping-experience-with-graphql"
published_at: "2022-12-14T15:12:09+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:02:37.132176+00:00"
content_hash: "sha256:dca984af097be750f637cd2e80f136759e5ffd3e81456d661ab3bedc9ee8ef80"
---

# Create an omnichannel shopping experience with GraphQL

This post is a part of our[“How to power modern retail apps with Apollo GraphOS”](https://www.apollographql.com/blog/platform/retail/how-to-power-modern-retail-apps-with-apollo-graphos) series. Also in this series:


- [Unify your e-commerce checkout with GraphQL](https://www.apollographql.com/blog/platform/retail/unify-your-ecommerce-checkout-with-graphql)
- [Personalizing the e-commerce shopping experience with GraphQL](https://www.apollographql.com/blog/platform/retail/personalizing-the-ecommerce-shopping-experience-with-graphql)
- [Manage time-gated product launches with GraphQL](https://www.apollographql.com/blog/platform/retail/manage-time-gated-product-launches-with-graphql)
- [How to aggregate and share data with third parties using GraphOS](https://www.apollographql.com/blog/platform/retail/how-to-aggregate-and-share-data-with-third-parties-using-graphos)


---


“Omnichannel” has become the go-to term to describe how data is shared across your company so customers will have a unified experience interacting with all parts of your business—from social marketing to the digital or physical marketplace.


Because Apollo GraphOS is the go-to platform for providing a unified API experience to clients apps by connecting all of your data sources in a supergraph, it’s a perfect fit for creating omnichannel experiences. Let’s dive into how you can accomplish that with GraphQL.


## Federating a supergraph schema


A[federated GraphQL architecture](https://www.apollographql.com/docs/federation/) splits services into subgraphs so that each team can own a smaller, more focused domain, while still providing a single, unified supergraph schema for all of your clients to consume. This means that your client teams—whether they work on the website, mobile apps, chatbots, or POS systems—can all use the same API to accomplish their specific goals.


Omnichannel revolves around connecting all data sources back to your user, so ideally, you have a user ID that can be used across all channels or a way of converting it in each channel. We can define that user in a subgraph schema as follows:


```text
# Users subgraph


type User @key(fields: "id") {
id: ID!
}
```


By setting the` id` as the key field using the` @key` directive, we have turned the` User` object type into an[entity](https://www.apollographql.com/docs/federation/entities/) so any other subgraph can now add fields to the` User` type or return the` User` from any of their fields, and all they need to know about a specific user is their ID.


For example, in the separate subgraph for a checkout service, we can add a` cart` field on the` User` by extending it like so:


```text
# Checkout subgraph


type User @key(fields: "id") {
id: ID!


"The user's active cart session"
cart: Cart
}
```


As more and more subgraphs extend additional fields on the` User` type, client developers will be able to see the full representation of the` User` type which will give them the ability to create intricate queries that span across many subgraphs, such as getting all the[items a user has ordered or put in their cart](https://studio.apollographql.com/graph/apollo-retail-supergraph/explorer?explorerURLState=N4IgJg9gxgrgtgUwHYBcQC4QGIAEBxBFHAQwBtScBLFBOAZxxQAsEcY6EAnHJ4hiTmC4IwOATgAOMIpSSMWlbrE6dkRKMU4oAOkiy7cAJQQBHGIoQNmrAAYAPALTsuDymBs8ExIZ11muAJ44APJwSJRQvEhICKQAonbEcBKkrMC6OGwc3OlymVRgGfkaWji5%2BfnUtAzlFZWFeXWSnBEIRU0SnBBgMFBEtU2ZKNSp7XUAvmOZk404MxUCPjVTVDT0ZSuZbpvNrTud3b39O5k78xNj5zPjIONAA&variant=prod) so we can craft a targeted marketing email.


## Loyalty across channels


An effective omnichannel strategy involves tracking when a user is moving across all your channels and which actions they take in each one. Creating a federated supergraph allows each channel to operate independently to report user actions and scale out based on their runtime or development needs.


A common strategy usually involves offering rewards or discounts to users for shopping in different channels or tracking where they have previously purchased products. We could try to calculate this information in each client by looking at previous orders, but with a supergraph architecture we can add this functionality to our existing API without breaking our current clients:


```text
# Users subgraph


type User @key(fields: "id") {
id: ID!
loyalty:LoyaltyProgram
}


type LoyaltyProgram {
activeDiscounts: [Discount]
joinedDate: Date
points: Int
}
```


By adding these new fields we can look at all of our active discounts when querying for user data across all channels. We can even use the graph itself to add more discounts to our users in an offline process, like querying for their loyalty start date and adding a discount on their anniversary or making sure our systems don’t offer overlapping or duplicate discounts to the users.


## Get started with a retail supergraph today


Beyond realizing the potential of an omnichannel shopping experience, the best way to see the possibilities of a supergraph is to try one out.[You can explore a retail supergraph schema and run real queries against it here.](https://studio.apollographql.com/public/apollo-retail-supergraph/explorer?explorerURLState=N4IgzghgbgpgJgYQPYBsUwMYBcCWSB2AknCAFwgCccEADAEYQAsdAtAIwBMcbLjMdrABwUBLOvEZM4jAOwA2AGYKQAGnDR4yNJlwEAoviwAnAJ7EyIOBxqyMAVkEsqdCrw72nEGAGZ23uNxyMIzcbHYgAL5AA&referrer=operation_collections&variant=prod)


We also have additional posts in this[series of retail best practices](https://www.apollographql.com/blog/platform/retail/how-to-power-modern-retail-apps-with-apollo-graphos) that dive into different elements of this schema to illustrate how Apollo GraphOS help power essential features of modern retail applications.


If you’d like to talk to an Apollo expert about how a supergraph can power your retail experience,[please reach out to us](https://www.apollographql.com/contact-sales/?referrer=retail-solutions-page) .


Written by


Shane Myrick


[Read more by Shane Myrick](https://www.apollographql.com/blog/author/shane)
