---
schema_version: "1.0.0"
document_id: "401634a020bed0a91d19e8d6870b3234680c666c1a7ed818d48decc935abb98e"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/fullstack-graphql-tutorial-defer-and-apollo-graphos"
published_at: "2022-11-10T11:46:40+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:02:37.132176+00:00"
content_hash: "sha256:01c02fec6f979e68a6cd465455d173714b5e2ce03262b13340869709282265a8"
---

# Fullstack GraphQL tutorial: @defer and Apollo GraphOS

With Apollo GraphOS, you can improve your client app’s time-to-interactive (TTI) and reduce latency in a few lines of code. Just plug your GraphQL API into the supergraph and add the` @defer` directive to the slow parts of your GraphQL query. When you mark slow fields with` @defer` , the supergraph knows it can deliver those parts of the result asynchronously.


The` @defer` directive is a[proposal](https://github.com/graphql/graphql-spec/pull/742) to the GraphQL specification that has been in development since July 2020. We know the GraphQL community has been eager to try it, so[we announced full end-to-end support for @defer across Apollo GraphOS](https://www.apollographql.com/events/virtual-event/thank-you/opening-keynote-with-matt-debergalis-cto-cofounder/) at this year’s GraphQL Summit. Even if your GraphQL API doesn’t support` @defer` yet, you can still use it with GraphOS.


In this tutorial, we’ll show you how to use` @defer` with[our e-commerce example](https://github.com/apollographql/hack-the-supergraph) . If you like what you see, you can follow the same steps to plug your GraphQL API into the supergraph and start using` @defer` to craft a snappier user experience.


## Get started with GraphOS


First, we need to connect our GraphQL API to GraphOS, Apollo’s supergraph platform. GraphOS includes a global supergraph runtime, powered by Apollo Router. It’s free to use with 10MM queries/month included.[Create an account](https://studio.apollographql.com/signup?referrer=blog) and add your GraphQL endpoint to set up your supergraph.


Here, we’re using[https://hts-products-production.up.railway.app/](https://hts-products-production.up.railway.app/) as our GraphQL endpoint. When we connect it to the supergraph, our GraphQL API becomes a subgraph.


Creating a new Supergraph in GraphOS using our existing GraphQL endpoint. Make sure to write down your Supergraph ID, we’ll use it later.


## Define our first entities


Now that we’re connected to the supergraph, let’s dive into our subgraph’s schema to define our first entities. An **entity** is a thing in the graph you can identify. Like a user with an account number, or a product in your catalog. You already have some entities in your schema today!


Why are entities important? Under the hood, the supergraph uses a query plan to determine how to fetch the deferred portions of a query. Apollo Federation builds that query plan with entities. Think of entities as the building blocks for the supergraph. We’re using them to power cool features coming soon to GraphOS, like[live queries and caching](https://www.apollographql.com/events/virtual-event/graphql-summit-october-2022/thank-you/the-future-of-real-time-data-in-graphql/) .


To define your first entities:


1. [Install the Apollo Federation package](https://www.apollographql.com/docs/federation/building-supergraphs/supported-subgraphs/) for your subgraph. Ensure your schema is being built with the installed package.
2. Add the` @key` directive to any type and define the key fields (typically the` id` field).
3. Create the reference resolver for your entities.


Here are two examples showing how to add the` @key` directive to a schema whether you’re using Schema Definition Language (SDL) or Code-first Schema Development:


On the left, we’re defining` Product` as an entity with[Apollo Server](https://www.apollographql.com/docs/apollo-server) using SDL. On the right, we’re defining a Product entity in[Strawberry](https://strawberry.rocks/) , a code-first Python framework. Over 20 languages and frameworks are supported.


Here are two examples showing how to add the reference resolver for the` Product` entity we defined in step 2:


On the left, we’re defining our` Product.__resolveReference` resolver using Typescript. On the right, we’re defining our` Product.resolve_reference` with` @classmethod` inside the class using Python.


We’ve already completed those steps for you in our products subgraph[schema](https://github.com/apollographql/hack-the-supergraph/blob/main/subgraphs/products/schema.graphql) and[resolvers](https://github.com/apollographql/hack-the-supergraph/blob/main/subgraphs/products/src/resolvers/Product.js) . Now try it in your graph! The more entities you define, the more places you can start using` @defer` .


*Remember that` @defer` still works even if you have only one subgraph. A deferred query could be re-entrant within the same subgraph* ! 🤘


## Add another subgraph


If you have several subgraphs, they can be added to a supergraph from the Subgraphs tab in GraphOS. While this step is optional, it’s a best practice to think about separation of concerns when designing your GraphQL schemas. We’ve already split our reviews entity into a separate subgraph for this example, but if you want to follow along, you can follow the instructions displayed in GraphOS and use the reviews deployment located at[https://hts-reviews-production.up.railway.app/](https://hts-reviews-production.up.railway.app/) .


## Test` @defer` with Explorer


We’ve connected our subgraphs to the supergraph and defined our entities. Let’s test out` @defer` with Explorer, our GraphQL IDE. You can test out ours[here](https://studio.apollographql.com/public/hack-the-e-commerce/explorer?variant=main) .


1. Write a query that includes an entity (like products or reviews).
2. Select the part you want to defer. Right-click to extract a fragment and add` @defer` .
3. Execute the query and you can see the data load dynamically


## Use` @defer` in a React app


Since` @defer` returns multiple responses, the client must support a multipart format.[Apollo Client](https://www.apollographql.com/docs/react/data/defer) and[Apollo Kotlin](https://www.apollographql.com/docs/kotlin/fetching/defer/) both support` @defer` today.[Apollo iOS support is coming soon](https://github.com/apollographql/apollo-ios/issues/2395) .


Here’s how to use` @defer` in your React application:


1. Install the latest version of Apollo Client (v3.7.0 or later):


```text
npm install @apollo/client@latest
```


1. Add` @defer` to your query with a fragment.
2. Ensure your React component expects the deferred data to be undefined since it might not be available right away.


[Configure your router](https://www.apollographql.com/docs/graphos/cloud-routing/configuration) to secure your subgraph and set up CORS rules for your website. Below is an example of a configuration we used:


```text
cors:
origins:
- https://studio.apollographql.com
- https://hack-the-supergraph.netlify.app
headers:
all:
request:
- insert:
name: "subgraph-token"
value: "${env.SUBGRAPH_SECRET}"
- propagate:
named: "authorization"
```


You can see` @defer` in action in our[e-commerce site](https://hack-the-supergraph.netlify.app/product/product:1) example. It’s a product details page where we’re deferring the product’s reviews. To increase/decrease the latency for the deferred views, adjust the slider in “Demo Settings.”


Notice the product details load first and provide a quicker TTI for the user while the reviews load when the data is available (and it’s all from one query!)


## @defer + GraphOS = 😍


Let’s recap what we learned! By implementing` @defer` with[Apollo GraphOS](https://studio.apollographql.com/signup?referrer=blog) , we can improve our app’s TTI and reduce latency in 3 steps:


1. Connect our GraphQL API to the supergraph.
2. Define entities in our supergraph using` @key` .
3. Mark slow fields in our query with` @defer` .


To check out the code powering our example, here’s the[frontend](https://github.com/apollographql/hack-the-supergraph/tree/main/website) , the[subgraphs](https://github.com/apollographql/hack-the-supergraph/tree/main/subgraphs) , and[Explorer](https://studio.apollographql.com/public/hack-the-e-commerce) . We’d love to hear your feedback in our community forums.


Happy querying!


Written by


Michael Watson


[Read more by Michael Watson](https://www.apollographql.com/blog/author/watson)
