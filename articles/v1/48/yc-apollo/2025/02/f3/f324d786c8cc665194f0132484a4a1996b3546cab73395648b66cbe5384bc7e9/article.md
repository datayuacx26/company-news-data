---
schema_version: "1.0.0"
document_id: "f324d786c8cc665194f0132484a4a1996b3546cab73395648b66cbe5384bc7e9"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/whats-new-in-graphos-apollo-winter-25-release-apollo-connectors-native-query-planner-improved-tools-and-more"
published_at: "2025-02-19T18:26:47+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T20:58:20.301648+00:00"
content_hash: "sha256:cfc2adf0b24f425de4eb50be2543d431f6702bb3585d60f2ef8e2513ddd823b8"
---

# What’s New in GraphOS: Apollo Winter ’25 Release – Apollo Connectors, Native Query Planner, Improved Tools, and More!

**This week, the Apollo team 🚀SHIPPED 🚀**


Our 2025 Winter Launch took place today, Wednesday, February 19, and was attended by hundreds of developers from across the GraphQL community. Our entire team had such a blast spending time with all of you and taking a deeper look at some of the powerful and exciting capabilities we’ve added to our[GraphOS Platform](https://www.apollographql.com/graphos) . We’re really proud of the work that went into this release, and it’s always such a privilege to share that with all of you.


I wanted to take a moment and briefly recap the event and all that we introduced and talked about at the event.


- [Apollo Connectors for REST APIs GA](https://www.apollographql.com/blog/whats-new-in-graphos-apollo-winter-25-release-apollo-connectors-native-query-planner-improved-tools-and-more#a-better-way-to-orchestrate-apis-apollo-connectors-for-rest-apis-is-now-ga)
- [Router 2.0 GA, with a blazing-fast native query planner](https://www.apollographql.com/blog/whats-new-in-graphos-apollo-winter-25-release-apollo-connectors-native-query-planner-improved-tools-and-more#rock-solid-blazing-fast-api-runtime-router-2-0-ga)
- [Visual Studio Extension enhancements](https://www.apollographql.com/blog/whats-new-in-graphos-apollo-winter-25-release-apollo-connectors-native-query-planner-improved-tools-and-more#dynamic-autocomplete-in-visual-studio-extension)
- [Improvements to our client libraries](https://www.apollographql.com/blog/whats-new-in-graphos-apollo-winter-25-release-apollo-connectors-native-query-planner-improved-tools-and-more#improvements-to-apollo-client-apollo-ios-and-apollo-kotlin)
- A new[free pricing plan](https://studio.apollographql.com/signup?referrer=blog) , making it easier than ever to get started building with Apollo.


Let’s take a closer look.


## A better way to orchestrate APIs: Apollo Connectors for REST APIs is now GA


We first introduced[Apollo Connectors for REST APIs](https://www.apollographql.com/graphos/apollo-connectors) at GraphQL Summit 2024, and the product has been in preview for the last few months. Throughout the public beta, we heard so many stories from[customers](https://www.apollographql.com/customers/cox-automotive) about how much easier Connectors makes it to stand up a new federated GraphQL API and orchestrate API requests across REST services.


*“Apollo Connectors really accelerated our API modernization and monetization roadmap,”* said Mark Meiller, Principal Platform Engineer at[Cox Automotive](https://www.apollographql.com/customers/cox-automotive) . “ *What would have been a yearlong rewrite of our foundational vehicle data layer has become a two-day implementation. By connecting our existing REST API into our federated graph, we avoided over a million dollars in development costs while improving performance. Complex vehicle data logic that would have required extensive rewriting, and frankly, was seen as an insurmountable task, is now transformed into a few lines of declarative code.”*


If you haven’t heard about the product yet, before Apollo Connectors, integrating data from a REST API in your GraphQL schema required writing custom resolver functions to handle API communication. These resolvers made HTTP calls to fetch and integrate data for the GraphQL schema. Once created, these functions needed to be integrated into a GraphQL server and deployed to either a monolithic backend or subgraph servers in a federated architecture. With[Apollo Connectors](https://www.apollographql.com/graphos/apollo-connectors) , all of that complexity goes away. Developers can now easily map REST API endpoints to their GraphQL schema using a straightforward declarative syntax.


## Example: Using an Apollo Connector to get mapping data onto the graph


To see how easy it is to integrate REST API data within the graph, consider the[Trimble Maps API](https://developer.trimblemaps.com/) . Trimble exposes a REST API to return routing, geocoding, and mapping information. But what if I don’t want to interact with this API using REST? What if I want to use GraphQL?


After registering for an API key, we can simply define the schema declaratively as follows:


```text
extend schema
…
@source(
name: "trimblemaps"
http: {
baseURL: "https://singlesearch.alk.com/"
headers: [{ name: "Authorization", value: "{$config.api}" }]
}
)


type Address {
shortFormatted: String
timezone: String!
}


type Geo {
lat: String!
long: String!
}


type Query {


addressByGeo(lat: String!, long: String!): Address
@connect(
source: "trimblemaps"
http: {
GET: "ww/api/search?query={$args.lat},{$args.long}&matchNamedRoadsOnly=true&maxCleanupMiles=0.2"
}
selection: """
shortFormatted:Locations->first.ShortString
timezone:Locations->first.TimeZone
"""
)
}
```


Okay, that’s it – we’ve got our data on the graph, no resolver required. Of course, we can query just like you would with any other GraphQL API–for example, calling` addressByGeo(“48.8584”,”2.2945”)` yields the following result:


```text
{
"data": {
"ByGeo": {
"Address": "2 Avenue Gustave Eiffel, 75007 Paris, Île-de-France, FR, Paris",
"Timezone": "GMT+1:00"
}
}
}
```


Apollo Connectors is the culmination of a huge amount of work by our team and the realization of a longtime vision for many of us here at Apollo.[Read Lenny’s blog on the creation of Connectors](https://www.apollographql.com/blog/our-journey-to-apollo-connectors) to learn more about our journey of bringing this product to life and some of the inside details on what makes Connectors so exciting.


Want to give Connectors a try for your next project? Get started right away by signing up for our new Free plan (more on that in a second) and running through our[quick start](https://www.apollographql.com/docs/graphos/get-started/guides/rest-quickstart) !


And once you’re up and running, make sure to check out the[Apollo Connectors Community](https://github.com/apollographql/connectors-community) GitHub repo, containing pre-built connectors, such as Stripe, Strapi, OData, and the Trimble Maps API example in this article. You can clone these Connectors, build your own, and share them with the community.


## Rock-solid, blazing-fast API runtime: Router 2.0 GA


If GraphQL federation and innovations like Apollo Connectors provide the raw capabilities for better orchestrating API workloads, Apollo Router gives you the rock-solid, enterprise-grade runtime that allows you to serve those workloads at scale. At the Winter Launch event, we were also proud to announce the general availability of version 2.0 of Apollo Router, which supports Connectors and also ships with a new, native implementation of the query planner.


If you’re not familiar, Router is the GraphQL gateway that receives and orchestrates GraphQL operations based on the data model defined in your federated schema – the screenshot below shows an example of a simple query plan generated by the Router.


This process of determining how to orchestrate API calls is called query planning. For dynamic queries, the Router must generate a query plan at runtime–and because the Router is in the hot path of returning API responses, speed and efficiency in query planning are critical to delivering a responsive, reliable API.


We began validating the new, native query planner implementation with some incredible partner customers last summer, and brought the new query planner to the broader market at[GraphQL Summit](https://www.apollographql.com/events/series/graphql-summit-2024) last year. Since then, Router has been pushed through some major load tests in production environments–the US election news cycle at our media company customers, Black Friday and the holiday sales rush with our retail customers, and more.


We’re consistently seeing the following results or better across our customer base:


- 10x faster query planning (median improvement)
- 1.9x faster execution time
- 5.5x lower CPU consumption
- 2.7x lower heap consumption


If you’re already running Apollo Router, what this means for you is that simply upgrading to Router 2.0 will translate to cost savings: a more efficient, performant, and scalable runtime for your API workload.[Upgrade today.](https://www.apollographql.com/docs/graphos/reference/upgrade/from-router-v1)


## Dynamic autocomplete in Visual Studio extension


We’re always looking for ways to make the experience of developing with GraphQL more productive and delightful. To that end, we introduced an improved[Visual Studio Code extension](https://www.apollographql.com/docs/vs-code-extension) at Summit last year. The first release included syntax highlighting for all aspects of GraphQL, including .graphql schema files, GraphQL operations in TypeScript, and client-only schema extensions.


Now, we’ve also added dynamic autocomplete support for fields, arguments, types, and variables in real time as operations are written.


The extension can also highlight composition errors, as shown in the screenshot.


If you’re not a VSCode user, have no fear: We’ve got similar capabilities available in our extension for[IntelliJ IDE](https://plugins.jetbrains.com/plugin/20645-apollo-graphql) and[Vim/NeoVim](https://www.apollographql.com/docs/graphos/schema-design/connectors/vim) .


## Improvements to Apollo Client, Apollo iOS, and Apollo Kotlin


And that’s not all – we also made some important enhancements to our client libraries:


- Apollo Client introduced new suspense-enabled React hooks for enhanced control over rendering, allowing developers to create more efficient UIs.
- Apollo iOS added declarative cache management with the @typePolicy directive.
- Apollo Kotlin released an experimental next-generation caching library with new pagination and eviction APIs.


## Get started building today using the Apollo GraphOS free plan


We’re so excited to see what Apollo Connectors, Router 2.0, and our other product improvements can do to accelerate API development for every developer and every organization.


For that reason, one of the things we’re most excited about is our new[GraphOS Free pricing plan](https://studio.apollographql.com/signup?referrer=blog) – intended to make it even easier for everyone to take advantage of Apollo GraphOS and build with our platform.


GraphOS is Apollo’s integrated platform that includes everything you need to build, orchestrate, and operate APIs with Apollo. GraphOS is built on Apollo’s open-source foundation, but includes enhanced capabilities at every layer of our stack intended to help your team deliver a robust API platform:


- Apollo’s full developer toolchain, so every developer in your organization can build with Apollo – this includes Apollo Sandbox, Explorer, Rover CLI, our IDE and browser extensions, and much more.
- GraphOS Platform, Apollo’s fully-managed platform designed to support a modern API software development lifecycle, including schema registry, checks and insights, SSO integration with advanced roles and permissions, schema collaboration tools, audit logging, and more.
- GraphOS Router, our Industry-leading, enterprise-grade GraphQL runtime – which includes enhancements for AuthN/AuthZ, Federated subscriptions, traffic shaping, demand control, entity caching, query safelisting, extensibility, and more.


Our new GraphOS Free plan gives you access to all the GraphOS capabilities you need to learn and build your API with Apollo for up to three users. Some of the features include:


- Register any existing GraphQL API with GraphOS Platform for schema registry, insights, and checks for up to one day of operations history.
- One self-hosted GraphOS Router instance per graph variant, supporting up to 60 requests per minute, so you can deploy GraphOS Router in your own environment as you prototype your new API.
- Advanced runtime capabilities, like Apollo Connectors, entity caching and subscriptions.


There are no monthly operation limits and time restrictions in our new Free plan, so you’ve got complete freedom to prototype risk-free.[Sign up now](https://studio.apollographql.com/signup) .


## Wrapping up


If you’re new to GraphQL you can learn everything you need to know with[Apollo Odyssey Tutorials and Certification](https://www.apollographql.com/tutorials/) . And if you want to learn more about the broader vision for where we’re taking Apollo, read our[CTO and cofounder Matt Debergalis’s blog here](https://www.apollographql.com/blog/api-orchestration-with-graphql/) . If you missed the live launch event, you can view the Apollo Winter ‘25 Release Launch Webinar recording here:


Whew! That’s a lot of new stuff we’ve introduced. And that’s a wrap – let us know what you think about everything we’ve introduced in this release. We can’t wait to see what you build with Apollo!


Written by


Rob Brazier


[Read more by Rob Brazier](https://www.apollographql.com/blog/author/rbrazier)
