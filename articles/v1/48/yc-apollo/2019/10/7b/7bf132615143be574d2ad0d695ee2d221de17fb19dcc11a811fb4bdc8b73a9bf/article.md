---
schema_version: "1.0.0"
document_id: "7bf132615143be574d2ad0d695ee2d221de17fb19dcc11a811fb4bdc8b73a9bf"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/previewing-the-apollo-client-3-cache-565fadd6a01e"
published_at: "2019-10-28T18:36:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:06:00.870812+00:00"
content_hash: "sha256:f96a06e6f89a09cdc939e280a912e773791d74922236751d7f23e6e0afb1e55e"
---

# Previewing the Apollo Client 3 Cache

Apollo Client’s caching system is core to both the user experience and the developer experience it provides. As one of Apollo Client’s most powerful features, the cache has been our biggest focus area while building Apollo Client 3. This major-version release will include plenty of other new features (such as a consolidated` @apollo/client` package that includes both React and Apollo Link functionality without requiring any additional imports), but the cache improvements are the most impactful and exciting.


Apollo Client 3 is available today as a beta. Let’s take a look at some of the caching features it includes!


## What is the Apollo Client cache?


Any library or framework can send a query to a GraphQL endpoint. Where Apollo Client shines is in its result caching. It uses a normalized cache to dramatically speed up the execution of GraphQL queries that don’t rely on real-time data. By reducing the number of network requests made to your GraphQL server, the cache improves the responsiveness of your application client. It also automatically keeps UI components up to date as new data is fetched and stored in the cache.


The Apollo Client caching system has worked well for a wide range of applications, but a few pain points have emerged over time, including a lack of cache eviction capabilities and inconsistent configuration APIs.


Apollo Client 3 aims to eliminate as many of these pain points as possible, while delivering a variety of useful features requested by the amazing GraphQL community.


## Working towards a better cache


Apollo Client 3 focuses on unifying its cache configuration strategy and providing fine-grained control over cache interactions. Let’s dive into a few features in detail:


### Garbage collection and cache eviction


Cache invalidation is by far[the most requested feature](https://github.com/apollographql/apollo-feature-requests/issues/4) from the Apollo community. It’s easy to understand why, since the Apollo Client 2.x Cache API doesn’t allow specific parts of the cache to be purged.


To address this, Apollo Client 3 introduces new` InMemoryCache` methods to garbage-collect cache entries that are no longer reachable (` cache.gc()` ) and evict a specific entry by its ID (` cache.evict()` ). You can call either of these methods on an active` InMemoryCache` instance at any time:


```text
const cache = new InMemoryCache();// Remove all unreachable objects from the cache
cache.gc();// Remove a specific entity by ID
cache.evict(id);
```


We believe the[tracing garbage collection](https://en.wikipedia.org/wiki/Tracing_garbage_collection) implementation used by` cache.gc()` will serve the needs of most applications, but` cache.evict()` will come in handy for applications that need a little more direct control.[Check out the docs](https://www.apollographql.com/docs/react/v3.0-beta/caching/cache-interaction/#garbage-collection-and-cache-eviction) for details.


### Streamlined cache configuration


Previous versions of Apollo Client have split cache configuration functions across multiple, inconsistent APIs. Apollo Client 3 introduces a consolidated **type policies API** that lets you configure how the cache interacts with individual types in your schema. When you create a new` InMemoryCache` instance, you can pass its constructor an object that maps` __typename` strings to` TypePolicy` objects.


A` TypePolicy` object can specify the primary key fields for a type, along with a collection of` FieldPolicy` objects that customize the handling of individual fields. Let’s look at a few examples.


1. ` <strong>typePolicies</strong>` **can replace the**` <strong>dataIdFromObject</strong>` **function**


You can now specify which fields of an object represent its primary key(s), on a per-` __typename` basis. For example, let’s say a book can be uniquely identified by its ISBN:


```text
const cache = new InMemoryCache({
typePolicies: {
Book: {
keyFields: ["isbn"],
},
},
});
```


This API supports multiple primary keys and composite primary keys, without any risk of unstable JSON serialization, or forgetting to include the` __typename` in the ID.


**2.**` <strong>typePolicies</strong>` **can replace the**` <strong>@connection</strong>` **directive**


Instead of including the` @connection` directive in every query that interacts with a particular field, you can now specify the storage identity of fields and their arguments in one place.


In other words, this code:


```text
query Feed($type: FeedType!, $offset: Int, $limit: Int) {
feed(type: $type, offset: $offset, limit: $limit)  @connection  (
key: "feed",
filter: ["type"]
) {
...FeedEntry
}
}
```


becomes this code:


```text
const cache = new InMemoryCache({
typePolicies: {
Query: {
fields: {
feed: {
keyArgs: ["type"],
},
},
},
},
});
```


**3.**` <strong>typePolicies</strong>` **can replace**` <strong>cacheRedirects</strong>`


Instead of using a resolver-like approach for finding objects elsewhere in the cache (like this):


```text
const cache = new InMemoryCache({
cacheRedirects: {
Query: {
book: (_, args, { getCacheKey }) =>
getCacheKey({ __typename: 'Book', id: args.id })
},
},
});
```


You can now use a custom` FieldPolicy` read function (like this):


```text
const cache = new InMemoryCache({
typePolicies: {
Query: {
fields: {
book(existingData, { args, toReference }) {
return existingData ||
toReference({ __typename: 'Book', id: args.id });
}
}
},
},
});
```


The new type policy API provides a uniform approach for customizing your cache interactions, and it has plenty more great applications. In addition to the above examples, you can now:


- Implement arbitrary pagination logic (or other custom storage) without leaking implementation details outside of the cache
- Reuse generic pagination logic across different fields
- Disable the normalization of certain data to improve performance


### Disabling cache normalization


The Apollo Client cache uses data normalization to help track and manage changes to entities. Although normalization makes sense in most cases, normalizing certain types of data can negatively affect performance. This is especially true if you’re caching large quantities of transient data (such as metrics that are identified by a timestamp and never receive updates).


The type policy API in Apollo Client 3 lets you disable normalization for particular types, like so:


```text
const cache = new InMemoryCache({
typePolicies: {
Person: {
keyFields: false,
},
},
});
```


When normalization is disabled for a type, objects of that type are embedded within their parent object in the cache, and they can be accessed via that parent.


Check out the[disabling normalization](https://www.apollographql.com/docs/react/v3.0-beta/caching/cache-configuration/#disabling-normalization) section of the docs for more details.


### Additional cache improvements


The previous sections highlight the biggest upcoming cache improvements, but here’s a quick rundown of other notable changes.


**a) The**` <strong>InMemoryCache</strong>` **no longer generates local IDs**


Previous versions of the` InMemoryCache` would generate identifiers for entities that lack one in order to enable normalization. In Apollo Client 3, the` InMemoryCache` uses a new, reference-ID-based approach that only normalizes entities that have an ID. Entities *without* an ID are instead stored within their parent object in the cache. Initial tests with production data sets show a dramatic reduction in cache size, and an increase in overall cache performance.


**b) The**` <strong>InMemoryCache</strong>` **uses a new approach for fragment matching**


The` InMemoryCache` now supports using fragments with unions and interfaces via the` possibleTypes` option. By passing in an object that defines the polymorphic relationships between your schema’s types, you can then look up cached data by interface or union. See the updated[using fragments with unions and interfaces](https://www.apollographql.com/docs/react/v3.0-beta/data/fragments/#using-fragments-with-unions-and-interfaces) section of the docs for details.


**c)**` <strong>InMemoryCache</strong>` **results are frozen/immutable**


As promised in our[What’s new in Apollo Client 2.6 post](https://blog.apollographql.com/whats-new-in-apollo-client-2-6-b3acf28ecad1) , cache results are now immutable by default. The` freezeResults` cache option has been removed (` assumeImmutableResults` is still available for now, since cache implementations other than` InMemoryCache` might not return immutable results).


## Try Apollo Client 3 today!


You can try all of these great new cache features right now, using the latest Apollo Client 3 beta:


```text
npm install @apollo/client@beta
```


Please note that this beta also includes many other non-cache-related changes, which we’ll be covering in future posts. Also note that this is a major release that includes[backward-incompatible changes](https://github.com/apollographql/apollo-client/blob/release-3.0/CHANGELOG.md#breaking-changes) .


To learn more about what’s coming in Apollo Client 3, see the[release 3.0 PR](https://github.com/apollographql/apollo-client/pull/5116) . You can also refer to the[3.0 beta documentation](https://www.apollographql.com/docs/react/v3.0-beta/) . We hope to launch the final version of Apollo Client 3 shortly after Summit, but we still have a few great new features lined up that we’d like to get in place first.


A huge **THANK YOU** to all the Apollo community members who have helped shape the Apollo Client 3 release. If you have a chance, please test out` @apollo/client` and let us know what you think!


One final note — Apollo Client’s lead developer[Ben Newman](https://medium.com/u/540bbf1a0d2b?source=post_page-----565fadd6a01e----------------------) will be talking about all of the new upcoming cache changes in his GraphQL Summit talk, “Fine-Tuning Apollo Client Caching for Your Data Graph.” The talk will take place on October 30th at 12 pm Pacific and will be[livestreamed here](https://www.youtube.com/watch?v=LF6Ja9_zNrA) !


Written by


Hugh Willson


[Read more by Hugh Willson](https://www.apollographql.com/blog/author/hwillson)
