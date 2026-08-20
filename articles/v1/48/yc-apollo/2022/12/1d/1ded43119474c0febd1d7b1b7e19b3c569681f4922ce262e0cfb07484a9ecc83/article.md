---
schema_version: "1.0.0"
document_id: "1ded43119474c0febd1d7b1b7e19b3c569681f4922ce262e0cfb07484a9ecc83"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/unify-your-ecommerce-checkout-with-graphql"
published_at: "2022-12-12T15:27:26+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:02:37.132176+00:00"
content_hash: "sha256:328a24d709933721f2d0e5c8f6ea6cb1c6ed0f7185aea20ebfce611cb9753d77"
---

# Unify your e-commerce checkout with GraphQL

This post is a part of our[“How to power modern retail apps with Apollo GraphOS”](https://www.apollographql.com/blog/platform/retail/how-to-power-modern-retail-apps-with-apollo-graphos) series. Also in this series:


- [Personalizing the e-commerce shopping experience with GraphQL](https://www.apollographql.com/blog/platform/retail/personalizing-the-ecommerce-shopping-experience-with-graphql)
- [Creating an omnichannel shopping experience with GraphQL](https://www.apollographql.com/blog/platform/retail/create-an-omnichannel-shopping-experience-with-graphql)
- [Manage time-gated product launches with GraphQL](https://www.apollographql.com/blog/platform/retail/manage-time-gated-product-launches-with-graphql)
- [How to aggregate and share data with third parties using GraphOS](https://www.apollographql.com/blog/platform/retail/how-to-aggregate-and-share-data-with-third-parties-using-graphos)


---


17% of online shoppers abandon their carts due to long, complicated checkout experiences ([Baymard Institute](https://baymard.com/blog/perceived-security-of-payment-form) ). GraphQL can help you elevate, modernize, and improve your checkout functionality faster. As a result, you can ensure shoppers are experiencing a frictionless checkout process across all your applications, so you can capitalize on every revenue opportunity.


GraphQL provides a single endpoint that developers can query to get all of the information they need to render a view in a client app by connecting disparate backend data sources and standardizing functionality like authorization. In doing so, client teams can save time rewriting the same code across customer-facing apps and spend more time shipping product features, like enhanced checkout experiences.


## Implementing a digital cart with GraphOS


Apollo GraphOS—the only end-to-end supergraph platform that unifies all cloud data and services into a single, connected GraphQL schema—can provide a single platform with all the capabilities you need to iterate on your checkout.


When designing checkout functionality, the complexity of implementing this experience tends to land outside a single service. For example, it may require calls to additional services to process credit cards, send invoice orders to warehouses, and create shipping labels.


GraphOS can help your clients by giving them focused operations to perform, so each client team can focus on creating the best UX flow instead of independently writing duplicate logic to call the various services.


Below is a sample schema we created that helps shoppers to view a cart and the payments options they can use:


```text
type Query {
user: User
}


type User @key(fields: "id") {
id: ID!


"""
Saved payment methods that can be used to submit orders
"""
paymentMethods: [PaymentMethod]


"""
The user's active cart session. Once the cart items have been purchases, they transition to an Order
"""
cart: Cart
}


"""
An user's saved cart session. Only one cart can be active at a time
"""
type Cart {
"""
Items saved in the cart session
"""
items: [Variant]


"""
The current total of all the items in the cart, before taxes and shipping
"""
subtotal: Float
}


"""
A saved payment option for an user
"""
type PaymentMethod {
id: ID!
name: String
description: String
type: PaymentType!
}


"""
A fix set of payment types that we accept
"""
enum PaymentType {
CREDIT_CARD
DEBIT_CARD
BANK_ACCOUNT
}
```


Lucky for us, the cart is already set up for a few users in our example[retail supergraph](https://studio.apollographql.com/public/apollo-retail-supergraph/home?variant=prod) . If you look at the schema for the` user` query, the user ID is not supplied as a query variable. That’s by design—the user is pulled from a header. In a real production application, this would come from your auth provider and be extracted from a JWT or something similar. In our case, the user ID is pulled from the header` x-user-id` for this demo.


We have already created a few example operations that you can try, like[fetching all the items in a cart](https://studio.apollographql.com/public/apollo-retail-supergraph/explorer?explorerURLState=N4IgzghgbgpgJgYQPYBsUwMYBcCWSB2AknCAFwgCccEADAEYQAsdAtAIwBMcbLjMdrABwUBLOvEZM4jAOwA2AGYKQAGnDR4yNJlwEAoviwAnAJ7EyIOBxqyMAVkEsqdCrw72nEGAGZ23uNxyMIzcbHYgAL5AA&referrer=operation_collections&variant=prod) or[getting all the user’s payment methods](https://studio.apollographql.com/public/apollo-retail-supergraph/explorer?explorerURLState=N4IgzghgbgpgJgYQPYBsUwMYBcCWSB2AknCAFwgCccEADAEYQAsdAtAIwBMcbLjMdrABwUBLOvEZM4jAOwA2AGYKQAGnDR4yNJlwEAoviwAnAJ7EyIBQFYKjOHPEsrcAMwdeEDu5EUFvBYKCCg4uclZWjCAAvkA&referrer=operation_collections&variant=prod) .


With this data, we should be able to present the user with all of the items in their cart and create a checkout page, but we still need a way to perform our checkout flow actions.


## Modifying our cart and checking out


With a few schema enhancements, we can start describing a basic checkout workflow with[GraphQL mutations](https://www.apollographql.com/docs/apollo-server/schema/schema/#the-mutation-type) .


```text
type Mutation {
cart: CartMutaitons
}


type CartMutaitons {
checkout(paymentMethodId: ID!): CheckoutResult
addVariantToCart(variantId: ID!, quantity: Int = 1): ResultWithMessage
removeVariantFromCart(variantId: ID!, quantity: Int = 1): ResultWithMessage
}


type ResultWithMessage {
successful: Boolean
message: String
}


type CheckoutResult {
successful: Boolean
orderID: ID
}
```


Before we check out, we need to allow users to set up their own cart, including adding and removing cart items. Our schema uses the concepts of` Products` and` Variants` , where a` Product` is a root type that describes the item and it has many` Variants` which are different sizes, colors, or price options. Since` Variants` are the items that are actually purchasable, they will be the ones included in our cart. You can try an[example add-to-cart mutation](https://studio.apollographql.com/public/apollo-retail-supergraph/explorer?explorerURLState=N4IgzghgbgpgJgYQPYBsUwMYBcCWSB2AknCAFwgCccEADAEYQAsdAtAIwBMcbLjMdrABwUBLOvEZM4jAOwA2AGYKQAGnDR4yNJlwEAoviwAnAJ7EyIDhxkK4AZkY0WgmRju8ArHDgsqjDCw0ChQcdHIYdBT%2BJAC%2BQA&referrer=operation_collections&variant=prod) to add a variant to the cart in Apollo Explorer.


Note that all mutations in the example supergraph can be run but the example server does not store or modify any state, so it just returns a mock response.


We have designed our mutations to inform the end user of the result, so instead of returning status codes we have a generic` message` field that the server can populate with whatever message it wants to show to users. The same idea can be applied when we[remove a variant from the cart](https://studio.apollographql.com/public/apollo-retail-supergraph/explorer?explorerURLState=N4IgzghgbgpgJgYQPYBsUwMYBcCWSB2AknCAFwgCccEADAEYQAsdAtAIwBMcbLjMdrABwUBLOvEZM4jAOwA2AGYKQAGnDR4yNJlwEAoviwAnAJ7EyICB0F1BNOXHZsMNXmzauKNaiwCsEGF9BDAVGDAw5NhAAXyA&referrer=operation_collections&variant=prod) or when we go to[checkout with all the items in the cart](https://studio.apollographql.com/public/apollo-retail-supergraph/explorer?explorerURLState=N4IgzghgbgpgJgYQPYBsUwMYBcCWSB2AknCAFwgCccEADAEYQAsdAtAIwBMcbLjMdrABwUBLOvEZM4jAOwA2AGYKQAGnDR4yNJlwEAoviwAnAJ7EyIQRgEYOAZh6CYCmrziCZY6q5oBWCHZ0GBQwgnAygiAAvkA&referrer=operation_collections&variant=prod) . With this message, we can display a banner if the mutation was successful or display a warning if it failed, but we don’t need to update every client to have them use the same logic or message string.


## Get started with a retail supergraph today


Beyond unifying checkout experiences across client apps, the best way to see the possibilities of a supergraph is to try one out.[You can explore a retail supergraph schema and run real queries against it here.](https://studio.apollographql.com/public/apollo-retail-supergraph/explorer?explorerURLState=N4IgzghgbgpgJgYQPYBsUwMYBcCWSB2AknCAFwgCccEADAEYQAsdAtAIwBMcbLjMdrABwUBLOvEZM4jAOwA2AGYKQAGnDR4yNJlwEAoviwAnAJ7EyIOBxqyMAVkEsqdCrw72nEGAGZ23uNxyMIzcbHYgAL5AA&referrer=operation_collections&variant=prod)


We also have additional posts in this[series of retail best practices](https://www.apollographql.com/blog/platform/retail/how-to-power-modern-retail-apps-with-apollo-graphos) that dive into different elements of this schema to illustrate how Apollo GraphOS help power essential features of modern retail applications.


If you’d like to talk to an Apollo expert about how a supergraph can power your retail experience,[please reach out to us](https://www.apollographql.com/contact-sales/?referrer=retail-solutions-page) .


Written by


Shane Myrick


[Read more by Shane Myrick](https://www.apollographql.com/blog/author/shane)
