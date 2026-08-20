---
schema_version: "1.0.0"
document_id: "eb2dcc8e58247cefb1950cf02d347b05f5c2b8f98aeb6095971cf1afdb4e7a2d"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/announcing-the-release-of-apollo-client-3-0"
published_at: "2020-07-14T12:41:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:26:46.047008+00:00"
content_hash: "sha256:0c2fafd899dd2281b200289e67c2fb873fca3af155777f4392834e26ec3e972c"
---

# Announcing the Release of Apollo Client 3.0

Today we’re thrilled to announce the **official release of Apollo Client 3.0** ! This release is the culmination of 55 betas, 14 release candidates, and hundreds of resolved issues and merged pull requests over the past eleven months. Phew!


To everyone who’s tried out AC3 during this extended beta period, *thank you* . We couldn’t have reached this milestone without your continued feedback and support. And to everyone who’s been waiting for the official launch, we appreciate your patience. Yes, it really is finally here!


**Here’s a recap of what’s new, with links to related documentation:**


- A single, consolidated` @apollo/client` package


- Includes entry points like` @apollo/client/utilities` for efficient use *without* the core library


- New` InMemoryCache` APIs:


- [Eviction of objects and fields](https://www.apollographql.com/docs/react/caching/garbage-collection/#cacheevict)
- [Garbage collection](https://www.apollographql.com/docs/react/caching/garbage-collection/#cachegc)
- Configurable policies for[types](https://www.apollographql.com/docs/react/caching/cache-configuration/#typepolicy-fields) and[fie](https://www.apollographql.com/docs/react/caching/cache-field-behavior/)[l](https://www.apollographql.com/docs/react/caching/cache-field-behavior/)[ds](https://www.apollographql.com/docs/react/caching/cache-field-behavior/)
- Pagination helpers


- Improved[local state management](https://www.apollographql.com/docs/react/local-state/local-state-management/)
- Expanded and refined UI reactivity:


- [Reactive variables](https://www.apollographql.com/docs/react/local-state/reactive-variables/)
- More reliable cache broadcast behavior
- More predictable` FetchPolicy` enforcement


- Extensive internal refactoring


Before diving into some of those features here, I’d like to talk a bit about Apollo Client’s cache-focused design philosophy, which informed just about everything that’s included in this release.


## The purpose of a GraphQL client library


You can consume GraphQL with anything that makes an HTTP request, such as` fetch` in the browser or` curl` on the command line. Whichever tool you use, you get all the classic GraphQL benefits: fetching exactly the data you need, in exactly the shape you need, with a single network request.


*But.* Modern client applications use **caching** extensively to improve performance and user experience. And generic HTTP caching just doesn’t work with GraphQL. Every time you request even slightly different data, your HTTP-cached value is invalidated, making it useless for an application of any complexity.


GraphQL data is inherently, well, *graphical* . And to support GraphQL data effectively, a cache needs to *reflect* that graphical structure. HTTP caching can’t do this, but a library like Apollo Client *can* . In our opinion, this is the most important functionality that a GraphQL client library can provide.


### The client-side data graph


When Apollo Client fetches data from your server, it caches that data using a[normalized structure](https://www.apollographql.com/docs/react/caching/cache-configuration/#data-normalization) that matches your GraphQL schema. By caching this data, *Apollo Client locally reconstructs a subset of your back-end data graph* . This means that the next time Apollo Client queries some of that same data, it can fetch it directly from the cache, *even if an entirely different query requests it* . The cache only falls back to contacting your remote server when local data is missing or invalidated. And like any other GraphQL server, the cache provides APIs to[define how types and fields are read and modified](https://www.apollographql.com/docs/react/caching/cache-configuration/#typepolicy-fields) .


Because the cache is integrated directly with Apollo Client, it knows exactly which queries use exactly which fields in your graph. Whenever a cached field’s value changes, Apollo Client automatically updates all of the queries that include that field. This makes the cache just as reactive as any other part of a modern web application.


It’s because of the cache-focused philosophy behind Apollo Client that we don’t think of it primarily as a library for executing GraphQL operations, but rather as **a library for interacting with a client-side data graph** .


## Feature Spotlight


This is far from everything that’s new in AC3, but it’s some of what we’re most excited for you to try out in your application!


### Reactive variables


A[reactive variable](https://www.apollographql.com/docs/react/local-state/reactive-variables/) is a value that registers a dependency when you read it, and later triggers re-reading whenever the value is updated. The concept has been around for decades, predating[Meteor’s ReactiveVar API](https://docs.meteor.com/api/reactive-var.html) back in 2014. Reactive variables are a staple of reactive programming, and now they enable flexible new ways of storing local state in AC3.


When you modify a reactive variable created with the` makeVar` function, Apollo Client automatically updates every active query that depends on that variable’s value. This is similar to what happens whenever a field in the cache changes, *however* : reactive variables *aren’t in the cache* . That means they can hold data of any type and structure, and you can interact with them throughout your application without using GraphQL syntax.


As a company with a long history of creating and consuming reactive variable APIs, we genuinely believe that this addition to Apollo Client will have a transformative effect on local state management.


Here’s an example:


```text
// We have consolidated all the most important imports into
// a single package, now called @apollo/client:
import {
InMemoryCache,
makeVar,
gql,
useQuery,
} from "@apollo/client"


// Create a reactive variable and initialize it to false,
// which also allows TypeScript to infer the type of
// the variable (boolean).
const darkModeVar = makeVar(false);


const cache = new InMemoryCache({
typePolicies: {
Query: {
fields: {
// Similar to AC2 local resolvers, this field policy
// controls how the Query.darkModeEnabled field
// gets read from the cache:
darkModeEnabled() {
return darkModeVar();
},
},
},
},
});


// Query the darkModeEnabled field using an @client
// directive, as in AC2:
function App() {
const { data, loading } = useQuery(
gql`query { darkModeEnabled @client }`,
);
return loading ? <Loading/> :
<div className={data.darkModeEnabled ? "dark" : "light"}>...</div>;
}


// Change the value of the variable at any time by
// calling it as a function with one argument (the new
// value). Call the function with zero arguments to
// get the current value.
function toggleDarkMode() {
darkModeVar(!darkModeVar());
}
```


### Cache field policies


You can define a[field policy](https://www.apollographql.com/docs/react/caching/cache-field-behavior/) for any and every GraphQL field that appears in your cache. A field policy can include a[read function](https://www.apollographql.com/docs/react/caching/cache-field-behavior/#the-read-function) that customizes what happens when the field is read from the cache, and a[merge function](https://www.apollographql.com/docs/react/caching/cache-field-behavior/#the-merge-function) that customizes what happens when it’s written.


```text
const cache = new InMemoryCache({
typePolicies: {
Person: {
fields: {
name: {
read(name) {
// Return the cached name, upper-cased:
return name.toUpperCase();
}
}
},
},
},
});
```


**You can even do this for fields that aren’t in your schema!** Such[local-only fields](https://www.apollographql.com/docs/react/local-state/managing-state-with-field-policies/) are the basis for using AC3 to query both local and remote data simultaneously.


By defining all of this custom field logic in one place (the constructor of` InMemoryCache` ), you avoid repeating code, and your teammates can interact with the types and fields you’ve configured *without* needing to understand how they’re stored or fetched.


After[reading the documentation](https://www.apollographql.com/docs/react/caching/cache-field-behavior/) , you’ll have the tools to write your own field policies to handle use cases like:


- Default field values
- Transforming or normalizing field values
- Sorting and slicing lists
- Exposing reactive variables as GraphQL fields
- Using references to redirect to data elsewhere in the cache
- Pagination (covered below as well)
- … and much more!


### Pagination helpers


One of the most compelling use cases for a custom field policy is to handle **paginated lists** of data without baking any specific pagination logic into Apollo Client.


Even with field policies, though, it can be tricky to get pagination exactly right. With so many details to digest, you might want to start with one of our prewritten helper functions.


Here’s how you can consume search results from a Relay-friendly GraphQL server, such as the[Artsy search API](https://metaphysics-production.artsy.net/) :


```text
import { ApolloClient, InMemoryCache } from "@apollo/client";
import { relayStylePagination } from "@apollo/client/utilities";


const client = new ApolloClient({
uri: "https://metaphysics-production.artsy.net/",
cache: new InMemoryCache({
typePolicies: {
Query: {
fields: {
// Reusable helper function to generate a field
// policy for the Query.search field, keyed by
// search query:
search: relayStylePagination(["query"]),
},
},
},
}),
});


function BasquiatSearchResults() {
const { data, loading, fetchMore } = useQuery(gql`
query BasquiatQuery($afterCursor: string) {
search(query: "basquiat", first: 10, after: $afterCursor) {
edges {
node {
displayLabel
}
}
pageInfo {
endCursor
}
}
}
`);


if (loading) return <Loading />;


return (
<div>
<ul>
{data.search.edges.map(edge => (
<li>{edge.node.displayLabel}</li>
))}
</ul>
<input
type="button"
value="load more"
onClick={() => fetchMore({
variables: {
afterCursor: data.search.pageInfo.endCursor,
},
// No need for an updateQuery function, since the
// field policy handles all Query.search updates.
})}
/>
</div>
);
}
```


If you’ve ever read Relay’s[GraphQL Cursor Connections specification](https://relay.dev/graphql/connections.htm) , you know how complex Relay pagination can be, so it’s a big help to capture that complexity in a single helper function.


If you find your own field policies becoming repetitive, don’t forget that you can reuse logic! Write a helper function that generates a generic field policy, and that takes parameters for customization.


Following this release, we’ll continue collecting useful cache policy helper functions like` offsetLimitPagination` and` relayStylePagination` in` @apollo/client/utilities` . Feel free to use them directly in your own code, or adapt them to your own specific needs!


## Release FAQ


### How do I get started?


If you have an existing application that uses Apollo Client 2.x, check out the[migration guide](https://www.apollographql.com/docs/react/migrating/apollo-client-3-migration/) and refreshed[documentation](https://www.apollographql.com/docs/react/) , as certain concepts and interfaces have changed. We’ve worked hard to ensure that every required change is a *positive* one that makes logical sense and leaves you feeling better about your application and its data.


If you’re brand new to Apollo Client,[get started here](https://www.apollographql.com/docs/react/get-started/) !


### Is AC3 a complete rewrite of Apollo Client?


No. We care deeply about providing a pleasant migration between software versions, and the majority of 2.x functionality remains in 3.0. You can[migrate to AC3](https://www.apollographql.com/docs/react/migrating/apollo-client-3-migration/) now and incrementally adopt its features on your own timeline. Note that some 2.x features (such as local resolvers) are now officially deprecated.


Even though it *isn’t* a rewrite, AC3 includes features that needed a while to bake. As one example, the new[cache.evict API](https://www.apollographql.com/docs/react/caching/garbage-collection/#cacheevict) ([#5310](https://github.com/apollographql/apollo-client/pull/5310) ) enables you to remove objects and individual fields from the cache. Initial versions of this feature made it possible to leave the cache in a broken state after evicting critical data. To address this problem, we needed to:


- change the way the cache uses fetch policies to respond to missing data ([#6221](https://github.com/apollographql/apollo-client/pull/6221) ),
- devise a strategy for gracefully handling dangling references ([#6454](https://github.com/apollographql/apollo-client/pull/6454) ), and
- prevent overlapping queries from clobbering each other’s data in the cache ([#6448](https://github.com/apollographql/apollo-client/pull/6448) ).


If those three PR descriptions inspire a more immediate solution to this problem that we missed, we’d absolutely love your input on future releases! 🙂


### What’s next?


Our top priority is a return to a steadier and more predictable release cadence. Here’s some of what we’re hoping to deliver in the coming weeks and months:


- Blog posts and tutorials for specific AC3 features
- A revamp of the Apollo Client DevTools
- Full deprecation of Apollo Client 2.x local resolvers
- Alignment of caching concepts with Apollo iOS and Android
- First-class support for persisting the cache to device storage


---


We’re over the moon to finally be sharing the full release of AC3 with the entire Apollo community. It’s your passion, your support, and the amazing things you make with Apollo that inspire us to build the very best developer tools that we can. As always,[we’d love to hear what you think](https://twitter.com/apollographql) .


Happy querying (and caching)!


Written by


Ben Newman


[Read more by Ben Newman](https://www.apollographql.com/blog/author/benjamn)
