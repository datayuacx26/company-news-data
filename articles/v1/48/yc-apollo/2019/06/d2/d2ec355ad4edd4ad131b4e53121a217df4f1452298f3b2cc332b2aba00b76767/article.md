---
schema_version: "1.0.0"
document_id: "d2ec355ad4edd4ad131b4e53121a217df4f1452298f3b2cc332b2aba00b76767"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/whats-new-in-apollo-client-2-6"
published_at: "2019-06-11T22:01:40+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:06:00.870812+00:00"
content_hash: "sha256:744831e549b8f9e2eb5d33ae3d0067733e10b270fe49666cdab346c81f674825"
---

# What’s new in Apollo Client 2.6

Apollo Client 2.6 is a backwards-compatible update that fixes bugs and provides new options (namely,` assumeImmutableResults` ,` freezeResults` , and` returnPartialData` ) for rendering components faster and more smoothly.


These changes lay the groundwork for the next major version of Apollo Client, whose interrelated goals and guiding principles we think you will find both compelling and worth understanding:


- 🔏 Immutability of cache results, enabling` ===` equality where possible
- 🔂 Selective, configurable cache normalization
- ♻️ Cache eviction, invalidation, deletion, and garbage collection
- 🎣 Async iterators instead of` Observable` s
- 🎁 Smaller bundle sizes


Keep reading to find out what all of this means, how Apollo Client 2.6 fits into this vision, and what you can do today to align your code with this future.


## Rewarding immutability


Apollo’s official` InMemoryCache` optimizes cache reads by immediately returning the same result objects (including partial result objects nested within larger result trees) when the underlying data have not changed.


This optimization works amazingly well not just because it speeds up repeated cache reads, but also because rendering frameworks like[React](https://reactjs.org/docs/react-api.html#reactpurecomponent) can take advantage of` ===` equality to avoid re-rendering unchanged data, without having to examine the structure of the data.


To make the most of these optimizations, it’s important that the objects in question are not mutated by application code. If you destructively modify cache results when you’re updating the cache, you run the risk of tampering with data stored and/or depended on by other consumers of the cache. You might think you can get away with mutating cache results as long as you immediately write them back to the cache—and you can, almost:


```text
const data = client.readQuery({
query: CommentQuery,
}); // Destructive!
data.comments.push(newComment);  client.writeQuery({
query: CommentQuery,
data,
});
```


For better or worse, Apollo Client supports this usage pattern by recording snapshots of previously delivered results, so it can decide if new results are truly different from previous results. Snapshots protect against the mistake of mutating a result object and then forgetting to update the cache appropriately, which can cause the new result to appear` ===` to the previous result object, even though its contents have meaningfully changed. While the snapshots guarantee safety, they are costly, and they exemplify the kind of unnecessary work our optimizations were supposed to eliminate.


If you’ve heard the gospel of **immutability** , you already know there’s a better way to update the cache: by creating a fresh` data` object that shares most of the memory of the original, without modifying the original object:


```text
const data = client.readQuery({
query: CommentQuery
});client.writeQuery({
query: CommentQuery,
data: {
...data,
comments: [
...data.comments,
newComment,
],
}  ,
});
```


While Apollo Client cannot mandate immutable data processing for all application code that consumes cache results, we can *reward* you for writing your code in an immutable style. In Apollo Client 2.6, if you’re confident that your application code does not modify cache result objects, you can unlock substantial performance improvements by communicating this assumption to the` ApolloClient` constructor:


```text
const client = new ApolloClient({
link: ...,
cache: ...,
assumeImmutableResults: true,
});
```


In short, this new` assumeImmutableResults` option allows the client to avoid recording defensive snapshots of past results.


How significant are the benefits of this optimization? It all depends on the size of your queries and their results, but one of the issues that inspired us to address these problems was titled[Huge performance regressions in recent versions](https://github.com/apollographql/apollo-client/issues/4464) . In other words, your mileage will certainly vary, but the performance improvements could be huge!


## Enforcing immutability


The` assumeImmutableResults` option is a shiny new power tool, but how can you be sure your application is free from destructive modifications of cache results? Even if you hunt down and eliminate all violations of immutability today, how can you be sure accidental mutations won’t creep back into your code in the future?


If you’re going to *assume* something (such as immutability), you’d better be *enforcing* it somehow, and that’s where another new option comes in handy:


```text
const client = new ApolloClient({
link: ...,
cache: new InMemoryCache({
freezeResults: true,
}),
assumeImmutableResults: true,
});
```


Passing` freezeResults: true` to the` InMemoryCache` constructor causes all cache results to be[frozen](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze) in development, so you can more easily detect accidental mutations. Since the freezing happens only in non-production environments, there is no runtime cost to using` freezeResults: true` in production. Please keep this in mind when benchmarking!


Note: mutating frozen objects throws a helpful exception in strict mode, whereas it fails silently in non-strict code. Be sure to add` “use strict”` to the top of any scripts where you handle cache results, if strict mode is not already enforced by your module system or your build tools.


Now, you could manually call` Object.freeze` recursively on any results you read from the cache, but the beauty of this implementation is that it does not need to repeatedly deep-freeze entire results, because it can shallow-freeze the root of each subtree when that object is first created. Thanks to result caching, those frozen objects can be shared between multiple different result trees without any additional freezing, so the entire result tree always ends up deeply frozen.


In Apollo Client 3.0, we intend to make both` freezeResults` and` assumeImmutableResults` true by default, in addition to using the TypeScript` Readonly<T>` type to discourage destructive modifications. In other words, if you embrace immutability now, you won’t have to do anything when it becomes mandatory.


## Improving local state


In Apollo Client 2.5 we integrated the functionality of` <a href="https://github.com/apollographql/apollo-link-state" target="_blank" rel="noreferrer noopener">apollo-link-state</a>` into the core` apollo-client` package, using the familiar mechanism of` @client` directives and resolvers. After that release, thanks to increased usage by the community, we discovered and fixed a number of bugs related to the timing and predictability of` @client` resolvers, especially the forced` @client(always: true)` variety.


Client state management is an open and active design space, and we will no doubt revisit this` apollo-link-state` -inspired API in future versions of Apollo Client, but for now we wanted to stabilize the current functionality, so developers can use it as it was designed, and give us feedback on its design and ergonomics, in addition to reporting bugs.


## The return of returnPartialData


All the way back in March 2017, in an effort to simplify the Apollo Client API, we[removed](https://github.com/apollographql/apollo-client/pull/1370) an option called` returnPartialData` from the` client.watchQuery` options. This option allowed consumers to opt into receiving partial results from the cache for queries not fully satisfied by the cache.


A lot has changed since 2017, to say the least. While` returnPartialData` was never a safe default behavior (unexpected missing data is bad news!), the option was actually pretty useful when used appropriately—especially in conjunction with[React Apollo](https://github.com/apollographql/react-apollo/) (which[uses](https://github.com/apollographql/react-apollo/blob/0adeb79e886003f57d0f77df875923cc75b4c332/src/Query.tsx#L159-L162)` <a href="https://github.com/apollographql/react-apollo/blob/0adeb79e886003f57d0f77df875923cc75b4c332/src/Query.tsx#L159-L162" target="_blank" rel="noreferrer noopener">client.watchQuery</a>` behind the scenes), since it helps prevent re-rendering` undefined` data just before fetching a larger query over the network.


In Apollo Client 2.6 (and React Apollo 2.5.6), we decided to[reintroduce](https://github.com/apollographql/apollo-client/pull/4743) the` returnPartialData` option. Concretely, this means you can run a smaller query in one component:


```text
query GetMember($id: ID!) {
member(id: $id) {
id
name
}
}
```


and cache the results, then run a larger query like the following in another component:


```text
query GetMemberWithPlans($id: ID!) {
member(id: $id) {
id
name
plans {
id
title
duration
}
}
}
```


If you enable` returnPartialData` and use a` fetchPolicy` such as` cache-and-network` , the larger query will come back as multiple results, first with any available data from the cache, and finally with the complete result data.


As long as you can make your application code resilient to the possibility of missing data, passing` returnPartialData: true` to` client.watchQuery` (or setting` returnPartialData={true}` on your` <Query/>` component) may be just the trick you need to avoid flickery re-renderings.


## 🌒 What’s next 🔭


Our vision for the next few versions of Apollo Client revolves around consolidating and simplifying the API in creative ways to pave the way for some important new features:


- **We believe**[async](http://2ality.com/2016/10/asynchronous-iteration.html)[iterators](https://jakearchibald.com/2017/async-iterators-and-generators/) **are a better primitive for delivering multiple query results than**` <strong>Observable</strong>` **s** , because they are reliably asynchronous (rather than sometimes synchronous), they do not require any additional runtime library, they support[back-pressure](https://nodejs.org/en/docs/guides/backpressuring-in-streams/) because they are[pull- rather than push-based](https://stackoverflow.com/a/47214496/128454) , and they can be trivially converted to` Observable` s (whereas the other direction is[not so trivial](https://github.com/tc39/proposal-observable/issues/74) ). Our short-term plan is to augment the return type of` client.query` to conform to[the](http://2ality.com/2016/10/asynchronous-iteration.html#the-interfaces-for-async-iteration)` <a href="http://2ality.com/2016/10/asynchronous-iteration.html#the-interfaces-for-async-iteration" target="_blank" rel="noreferrer noopener">AsyncIterator</a>`[interface](http://2ality.com/2016/10/asynchronous-iteration.html#the-interfaces-for-async-iteration) . Once we validate this approach, we hope to deprecate` client.watchQuery` , leaving only one method for issuing queries, no matter how many results are expected.
- This unified query API will make it possible for developers to signal when they no longer care about receiving results for a particular query, or about keeping the associated data in the cache, laying the foundations for **cache invalidation, eviction, deletion, and garbage collection** , which are some of our longest-standing[feature requests](https://github.com/apollographql/apollo-feature-requests/issues/4) .
- The standard` <a href="https://www.npmjs.com/package/apollo-cache-inmemory" target="_blank" rel="noreferrer noopener">apollo-cache-inmemory</a>` cache implementation promises to[normalize](https://www.apollographql.com/docs/react/advanced/caching/#normalization) your data, to enable efficient cache updates and repeated cache reads. However, not all data benefits from normalization, and the logic for deciding whether separate objects should be normalized together varies across use cases. **Future versions of the Apollo Client cache will unify several related features—**` <a href="https://www.apollographql.com/docs/react/advanced/caching/#normalization" target="_blank" rel="noreferrer noopener"><strong>dataIdFromObject</strong></a>` **,**[the](https://www.apollographql.com/docs/react/advanced/caching/#the-connection-directive)` <a href="https://www.apollographql.com/docs/react/advanced/caching/#the-connection-directive" target="_blank" rel="noreferrer noopener"><strong>@connection</strong></a>`[directive](https://www.apollographql.com/docs/react/advanced/caching/#the-connection-directive) **, and**` <a href="https://www.apollographql.com/docs/react/advanced/caching/#cache-redirects-with-cacheredirects" target="_blank" rel="noreferrer noopener"><strong>cacheRedirects</strong></a>` —so Apollo developers can implement specialized normalization logic, or even disable normalization for certain queries.
- **GraphQL errors** will be cached along with successful results. The wisdom of this behavior becomes more obvious in a` Promise` -oriented system, since promises can encapsulate either successful results or errors, so any system that caches promises tends to cache their errors, too.
- **Immutability of cache results** will become the default in Apollo Client 3.0, making the` ApolloClient({ assumeImmutableResults: true })` and` InMemoryCache({ freezeResults: true })` options fully automatic. We will also use TypeScript’s` Readonly<T>` type to help enforce immutability.
- A smaller API with fewer dependencies means **smaller bundle sizes** .


While this list is by no means complete, we hope it gives you a sense of our current priorities. If we’re missing something important to you, feel free to comment below, and with any luck the answer will be “Oh, right, we’re working on that too!”


Written by


Ben Newman


[Read more by Ben Newman](https://www.apollographql.com/blog/author/benjamn)
