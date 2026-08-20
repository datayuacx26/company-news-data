---
schema_version: "1.0.0"
document_id: "3d2359dae8a615a6eb2b47ac6fbe1ee8035092ade3234eb2aeb42f329c877c30"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/first-impressions-with-apollo-client-3"
published_at: "2020-03-13T04:48:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:05:25.747778+00:00"
content_hash: "sha256:9f1648ecec421cff2cef248fd5ff5b7ea012119478cb6f98acafa8cabaa08f14"
---

# First Impressions with Apollo Client 3

I’ve been using the Apollo platform for almost two years now. I’ve used it on several commercial applications, and it’s become one of my all-time favorite frameworks.


With the upcoming release of Apollo Client 3, I thought it would be an excellent time to test out the latest beta and write about some of the exciting improvements I’m most excited about.


## What’s New


You’ve probably heard the old mocked phrase attributed to the Netscape engineer Phil Karlton: “ *There are only two hard things in Computer Science: cache invalidation and naming things.”* The phrase is getting a bit old, there are many hard things in Computer Science, but cache invalidation is indeed very challenging.


Apollo Client has had a powerful normalized caching system for years, but previous versions had virtually no cache invalidation (which is very useful in some scenarios). In the absence of that, users resorted to hacky solutions, like resetting the entire store or even manipulating the cache directly, which could lead to any number of unexpected problems


Apollo Client 3 has many cache improvements, including but not limited to invalidation,[garbage collection](https://www.apollographql.com/docs/react/v3.0-beta/caching/cache-interaction/#garbage-collection-and-cache-eviction) , and[cache eviction](https://www.apollographql.com/docs/react/v3.0-beta/caching/cache-interaction/#garbage-collection-and-cache-eviction) . In 3.0 all cache results are now frozen/immutable, and it introduces a new declarative API for managing type policies and field objects with tools for surgically manipulating the in-memory cache.


In this article, I’m going to share my experience with my favorite parts so far:


- The new Type & Field Policy APIs
- Modifying the cache in place with cache.modify
- Garbage collection and eviction with cache.evict / cache.gc


If you haven’t already, check out[Hugh Willson’s](https://medium.com/u/7c7515610c74) excellent article, “[Previewing the Apollo Client 3 Cache](https://blog.apollographql.com/previewing-the-apollo-client-3-cache-565fadd6a01e) ” for a great introduction to Apollo Client 3.0’s cache improvements.


## Type & Field Policy API’s


You can customize how individual fields in the Apollo Client cache are read and written. To do so, you define a` FieldPolicy` object for a given field. You nest a` FieldPolicy` object within whatever[TypePolicy object](https://www.apollographql.com/docs/react/v3.0-beta/caching/cache-configuration/#the-typepolicy-type) corresponds to the type that contains the field.


Type Policies can replace` dataIdFromObject` , the` @connection` directive, and` cacheRedirects.`


Here is an example of an` inMemoryCache` implementation with` typePolicies` ,` fieldPolicy` objects, and generated` possibleTypes` :


```text
export const cache = new InMemoryCache({
...possibleTypes,
typePolicies: {
Query: {
fields: {
card(existingData, { args, toReference }) {
return (
existingData || toReference({ __typename: 'Card', id: args?.id })
);
},
category(existingData, { args, toReference }) {
return (
existingData ||
toReference({ __typename: 'Category', id: args?.id })
);
},
},
},
Card: {
fields: {
categories: {
keyArgs: [],
},
},
},
Category: {
fields: {
cards: {
keyArgs: [],
merge(existing, incoming, { args }) {
return incoming; // this example is just returning the incoming
},
},
},
},
},
});
```


The beta documentation for Apollo Client 3 has a new section entitled “[Customizing the behavior of cached fields](https://www.apollographql.com/docs/react/v3.0-beta/caching/cache-field-behavior/) ” which explains some of the new Field Policy object functions:


The **Read** Function:


If you define a` read` function for a field, the cache calls that function whenever your client queries for the field. In the query response, the field is populated with the` read` function’s return value, *instead of the field’s cached value* .


**Read** is useful for manipulating values when they’re read from the cache, for example, things like formatting strings, dates, etc. You can also use it to implement virtual fields. It’s very powerful when you consider[local state management](https://www.apollographql.com/docs/react/v3.0-beta/data/local-state/) .


This feature could also be useful for schema-first development. Consider the scenario where you want to add a new field to your API that will not be ready for a few weeks — you could implement a temporary virtual field to unblock yourself.


The **Merge** Function:


If you define a` merge` function for a field, the cache calls that function whenever the field is about to be written with an incoming value (such as a response from a successful mutation via your GraphQL server). When the write occurs, the field’s new value is set to the` merge` function’s return value, *instead of the* original *incoming value* .


**Merge** can take incoming data and manipulate it before merging it with existing data. Suppose you want to merge arrays or non-normalized objects. You can also use it to handle logic for[pagination](https://www.apollographql.com/docs/react/v3.0-beta/caching/cache-field-behavior/#handling-pagination) .


These new Field Policy functions are more declarative, and allow you to centralize your logic. You can also create reusable Field Policies, for example, you could write reusable pagination policies.


Consider this example from the[Apollo Client 3 beta documentation](https://www.apollographql.com/docs/react/v3.0-beta/caching/cache-field-behavior/#handling-pagination) :


```text
function afterIdLimitPaginatedFieldPolicy<T>() {
return {
merge(existing: T[], incoming: T[], { args, readField }): T[] {
...
},
read(existing: T[], { args, readField }): T[] {
...
},
};
}


const cache = new InMemoryCache({
typePolicies: {
Agenda: {
fields: {
tasks: afterIdLimitPaginatedFieldPolicy<Reference>(),
},
},
},
});
```


**If you can separate your logic and only have to create it once, it should be much easier to maintain a complex graph of nodes.**


In Apollo Client 2.0, a combination of` FragmentMatcher` ,` HeuristicFragmentMatcher` , and` IntrospectionFragmentMatcher` were used to determine which object belongs to which fragment.


In AC3, they have all been removed. Today, in AC3,` InMemoryCache` contains the` possibleTypes` property that replaces all of that previous complexity.


```text
const cache = new InMemoryCache({
possibleTypes: {
Character: ["Jedi", "Droid"],
Test: ["PassingTest", "FailingTest", "SkippedTest"],
Snake: ["Viper", "Python"],
},
});
```


See the[possibleTypes](https://www.apollographql.com/docs/react/v3.0-beta/data/fragments/#defining-possibletypes-manually)[documentation](https://www.apollographql.com/docs/react/v3.0-beta/data/fragments/#defining-possibletypes-manually) for more info on how it works.


## cache.modify


I’ve been playing with this new method for a few days, and it’s probably my favorite feature in Apollo Client 3. It allows you to modify the cache in place rather than having to use` readData` to parse what you want to change, then writing it back to cache with` writeData` .


Because` cache.modify` can also be used to delete fields, it makes` writeData` largely redundant.` previousResult` is no longer a major consideration.


[Eliminating writeData](https://github.com/apollographql/apollo-client/pull/5923) is one of Apollo Client 3.0’s[release milestones](https://github.com/apollographql/apollo-client/milestone/14) .


I always found manipulating the cache with mutation updates to be a little cumbersome. It can be challenging to pluck out the data you need to rewrite, but on top of that, it can be hard to decide where to locate your logic.


It never felt right to co-locate my cache code inside containers, for example, putting a cache update into a button container. What if I need the same cache update policy in a different place? For instance, if I have a “remove category” button on a detail page, and I need the same remove category functionality on a category listing, this could mean introducing duplicate logic.


To avoid duplication, I had previously begun to wrap my` useQuery` hooks inside custom hooks. With the new Type & Field Policy API, the code to manipulate the cache lives in one place 🏆.


To illustrate what I mean, take a look at these examples.


**Before:**


```text
const removeCategory = (id: string) =>
removeCategoryMutation({
variables: {
id
},
update: (cache, { data }) => {
const { categories } =
cache.readQuery({
query: GetCategoriesDocument,
}) || {};


cache.writeQuery({
query: GetCategoriesDocument,
data: {
categories: categories.filter(
(category: CategoryPartsFragment) => category.id !== id,
),
},
});
},
});
```


**After:**


```text
const removeCategory = (id: string) =>
removeCategoryMutation({
variables: {
id,
},
update: cache => {
cache.modify('ROOT_QUERY', {
categories(categories: Reference[], { readField }) {
return categories.filter(
category => id !== readField('id', category),
);
},
});


// evict this item from the in memory cache
cache.evict(`Category:${id}`);
},
});
```


Notice how I don’t have to read the categories first?` cache.evict` removes the item from the cache completely, and using it in the mutation update function **broadcasts changes** so that my React components reflect the changes.


Here’s another example, this time using a reference to remove a card from a category.


```text
cache.modify(`Category:${categoryId}`, {
cards(cards: Reference, { readField }) {
const edges = readField<CardEdgeWithReference[]>('edges', cards).filter(
edge => edge.node.__ref !== `Card:${cardId}`,
);


let { totalCount } = readField<PageInfo>('pageInfo', cards);
const pageInfo = buildPageInfo<Edge<Card>>(edges, --totalCount, 'Card'); // rebuild pageinfo


return {
edges,
pageInfo,
};
},
});
```


In this example, I’m updating a connection and recalculating` pageInfo` , so evicting here is not appropriate, so using` modify` here is a big improvement over the original logic. In Apollo Client 2.0, you’d have to pluck out the original page info before you could modify it. This is much nicer.


Here is an example of using` cache.evict` to delete a field:


```text
cache.evict(id, 'fieldNameToDelete')
```


You could also use` cache.modify` to delete the field, but the` cache.evict` call is shorter and simpler.


` cache.identify` can be used to get the cache reference for a given object, so for example if you had the following object:


```text
const cuckoosCallingBook = {
__typename: 'Book',
isbn: '031648637X',
title: "The Cuckoo's Calling",
author: {
__typename: 'Author',
name: 'Robert Galbraith',
},
};
```


Calling` <strong>cache.identify(cuckoosCallingBook)</strong>` would return` <strong>Book:031648637x.</strong>`


You can use this together with` cache.modify` to identify cached records to manipulate.


## cache.evict / cache.gc


There are scenarios where you need better cache access- like when you need to clear cached data to avoid it being re-read by other functions. AC3 introduces the ability to evict data from the in-memory cache.


Using the` cache.evict` method, you can remove any normalized object from the cache (there are also` retain` and` release` methods for holding and releasing cache objects).


` cache.gc` removes all objects from the normalized cache that are not reachable (garbage collection). It’s not available on the mutation update function but you don’t need to call this function often.


## Conclusion


Apollo Client 3.0 is looking great. The cache has always been one of Apollo’s strongest features and version 3.0 is a significant improvement.


It simplifies and solves a lot of existing issues, especially cache invalidation, which has been a highly requested feature.


AC3s Type & Field policy APIs makes it easier to write reusable cache logic, makes modifying the cache easier, is more declarative, efficient, and requires less code.


Removing things from the cache is now easy with` evict` and there are some powerful new tools for cache manipulation (` modify` ,` identify` ,` gc` ,` read` and` merge` ).


I’m looking forward to the final release of Apollo Client 3.0 later this year. Many thanks to the Apollo team and[Khalil Stemmler](https://medium.com/u/8384d5944dde?source=post_page-----2ae2a069ab2f----------------------) for your help with the article!


## Resources


[Apollo Client 3.0 Change-log](https://github.com/apollographql/apollo-client/blob/master/CHANGELOG.md)


[Apollo Client 3.0 beta documentation](https://www.apollographql.com/docs/react/v3.0-beta/caching/cache-field-behavior/#handling-pagination)


[Apollo Client 3.0 release milestones](https://github.com/apollographql/apollo-client/milestone/14)


“[Previewing the Apollo Client 3 Cache](https://blog.apollographql.com/previewing-the-apollo-client-3-cache-565fadd6a01e) ” by[Hugh Willson](https://medium.com/u/7c7515610c74?source=post_page-----2ae2a069ab2f----------------------)


Written by


Sean Dearnaley


[Read more by Sean Dearnaley](https://www.apollographql.com/blog/author/sean)
