---
schema_version: "1.0.0"
document_id: "33c6480c2ea58f0dbb2c3bd9a31055ba44d8a048416a7e8199a607dc8db3e569"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/rethinking-react-components-with-defer"
published_at: "2023-01-17T15:29:47+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:02:31.747135+00:00"
content_hash: "sha256:d3c13bc7be572907a9914e26d2502472a1d10c1eaa5bb8fcc6d2c0732e7ab45b"
---

# Rethinking React components with @defer

Since the rise of the frontend framework and the framework wars of the 2010s, we’ve all gotten pretty used to the idea of building single-page apps that load a JavaScript bundle and then send queries to a server dynamically as a user clicks around. The move to single-page apps was driven by the need to create faster, more interactive user experiences.


As with every great innovation, solving one problem just opens your eyes to the next one. As we all settled into the new normal of building apps with UI frameworks, the next major challenge presented itself: data fetching. Over the last 6 years, GraphQL has once again innovated on the frontend developers’ experience, bringing UI-driven API design, automatic state management, strong types, and much more to the world of building interfaces.


And as someone who’s been building apps with GraphQL for a while, there’s one major ergonomic thing that has bothered me all that time: the relationship between queries and component structure. It’s common to create symmetry between the shape of a GraphQL query and the data that’s rendered in a UI component, but up until now, there has been no way to decouple the query itself from the importance of each field it holds. If you want some fields to load faster than others, or you know there’s a slow field and you don’t want your whole query’s response to be held up, you have to split those into multiple queries. This creates a lot of spaghetti code in components; something simple from a UI and schema design perspective just got complicated due to server implementation.


This isn’t a GraphQL problem–we had it with REST too–but GraphQL is uniquely positioned to solve it because we can annotate each field with metadata!


**Introducing,` @defer`**


` @defer` is a directive you can add to queries that will tell your server it can respond to this query in multiple parts. Do you know a particular field is slow? Defer it! This means your server can respond with an initial response to your end-user much faster and fill in the rest when it gets there.


For the last several months, we’ve been working to support` @defer` across the Apollo stack, and today we’re excited to announce full supergraph support for` @defer` in GraphOS. ** You don’t even need to make any changes to your existing GraphQL API to use it, *it just works* .


## **Using` @defer` with your existing GraphQL API**


To use` @defer` , you just need to have a supergraph router between your GraphQL server and your client (even if you only have one GraphQL server that isn’t using Apollo Federation). The easiest way to create a router is to set up a free serverless instance of Apollo Router with[Apollo GraphOS](https://apollographql.com/graphos/) . You can also use[Rover](https://www.apollographql.com/docs/rover) , the GraphOS CLI, to start up a local instance of the router.` @defer` can be used on the root` Query` field and the router will break the query into multiple GraphQL requests. We can test this out using **any** GraphQL endpoint and` rover dev` .


In this post, we will use the SpaceX API to see how this works. Remember, the SpaceX example *does not* use Apollo Federation. First, we use the` rover dev` command to start up a local instance of the router pointing at[https://spacex-production.up.railway.app/](https://spacex-production.up.railway.app/) :


```text
rover dev --name=spacex --url=https://spacex-production.up.railway.app/
```


Now you can open up[http://localhost:3000](http://localhost:3000/) and try executing a query using` @defer` on the root` Query` :


```text
query Launches {
launches {
id
details
rocket {
rocket_name
}
}
... @defer {
deferredLaunchDetails: launches {
id
upcoming
}
}
}
```


Notice we’re aliasing the root query field to isolate a single field. An upgraded query planner instructs the router to fetch the deferred portions of a query:


We can see that the generated query plan shows the router breaking our query into two requests to the SpaceX GraphQL API. Now you can` @defer` any field in your current API without making any changes 🚀


## **Using` @defer` on any Entity in the supergraph**


When[entities](https://www.apollographql.com/docs/federation/entities/) are used in the supergraph, they define an additional execution boundary for the deferred portions of a query with additional benefits like being able to` @defer` on nested fragments within a single root query.


Entities in the supergraph are traditionally used to split the implementation of an entity into multiple subgraph modules for a better[separation of concerns](https://www.apollographql.com/docs/federation/#separation-of-concerns) . The supergraph router can then perform API-side joins by fetching the entity data from individual subgraphs and flattening the results into the requested query shape.


Since the Router uses the existing[subgraph specification](https://www.apollographql.com/docs/federation/subgraph-spec/) supported in[over 24 languages and frameworks](https://www.apollographql.com/docs/federation/supported-subgraphs/) , app developers can start using` @defer` in their client queries without any backend changes to existing subgraphs.


In this post, we’re going to use the[Apollo Hack the Supergraph demo](https://github.com/apollographql/hack-the-supergraph) for the hosted subgraphs. We’ll still use` rover dev` to try this out. In separate terminal windows, start a rover dev session for these subgraphs:


```text
rover dev --name=products --url=https://hts-products-production.up.railway.app/


# In a new terminal window
rover dev --name=reviews --url=https://hts-reviews-production.up.railway.app/
```


Now you can open up[http://localhost:3000](http://localhost:3000/) and try executing a query using` @defer` on the product reviews:


```text
fragment ProductReviewsFragment on Product {
reviews {
content
}
}


query ExampleQuery {
products {
id
title
description
price {
amount
}
...ProductReviewsFragment @defer
}
}
```


You should see the` data.products.reviews` be deferred in the response and query plan:


## **Start using` @defer` today**


The easiest way to get started with the Apollo Router is to[create a serverless instance](https://www.apollographql.com/docs/graphos/quickstart/cloud) with[GraphOS](https://studio.apollographql.com/signup?referrer=blog) . Once you’ve added your existing GraphQL API in GraphOS, you’ll need to be on the latest version of Apollo Client ([React](https://www.apollographql.com/docs/react/data/defer/) ,[Kotlin](https://www.apollographql.com/docs/kotlin/fetching/defer/) ,[iOS support coming soon](https://github.com/apollographql/apollo-ios/issues/2395) ). That’s it, you can start using` @defer` in your apps 🎉.


Want to dig deeper into` @defer` and GraphOS?[Join our Discord](https://discord.gg/graphos) to continue the discussion! We’ll be live streaming about this topic and more!


Written by


Michael Watson


[Read more by Michael Watson](https://www.apollographql.com/blog/author/watson)
