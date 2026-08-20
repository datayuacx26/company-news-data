---
schema_version: "1.0.0"
document_id: "9727331751e65f62d38ab6c9ff579820ca8b943c32aa586cfa415b676d0af0bc"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/apollo-3-9-beta-feature-spotlight-the-memory-story"
published_at: "2023-12-20T16:02:32+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:00:19.733210+00:00"
content_hash: "sha256:cdabe96dbb634f79c323180277fe7a02b363b0e31ae1f77f9c77c658f0490b5d"
---

# Apollo Client 3.9 Feature Spotlight – The memory story

Apollo Client 3.9 is out in beta now, and it fixes various memory leaks, adds granular control over internal cache sizes, and deprecates the canonizeResults option due to a conceptual and non-fixable memory leak associated with it.


We want your help to find the correct default cache sizes – see the last paragraph for details on that.


## **The memory story**


Sometimes, in an open source project, you have that issue that sticks around for a long time because it’s not clear how you would tackle it.


[Apollo Client Issue #7942](https://github.com/apollographql/apollo-client/issues/7942) was undoubtedly one of those tickets.


A user had reported a memory leak, and the maintainer at the time put out a fix. This likely saved someone else from a memory leak but, unfortunately, did not help that specific user.


As maintainers, oftentimes, our hands are bound to figure out how to proceed. We can only stare at heap dumps so long before we would need access to the user’s Kubernetes cluster and “play” with the live production system that is seeing the issue.


Of course, that’s not feasible.


As a result,[issue #7942](https://github.com/apollographql/apollo-client/issues/7942) stayed around for a while. Because it was an issue that described a memory leak, other users started reporting memory leaks there, too. Because a memory leak is a memory leak, right? In most cases, we’ll never know if those users saw the same memory leak. It’s not very likely.


Eventually, the original user reported that upgrading to Apollo Client 3.6.0 in combination with following another user’s recommendation to call` cache.gc()` fixed the memory leak.


This would have been a good moment to close the issue since the original problem had been fixed – but because the issue now contained a lot of reports of different memory leaks, it stayed open.


Some users found new memory leaks and gave each other tips on avoiding them – which was great for those users, but unfortunately, we didn’t get a reproduction for any of those issues so they were really hard to investigate.


This issue was a constant reminder that something might be wrong somewhere, but we had no way of reproducing the issue and no access to any of those users’ code bases.


This fall, we set time aside to start a memory-themed spike – hunting for memory leaks that we could not locally reproduce previously. With artificially scaling benchmarks, hundreds of megabytes of heap dumps, and Facebook’s handy[memlab](https://facebook.github.io/memlab/) tool, we finally found and fixed many of them. But even when these tools wouldn’t produce any more signs of memory leaks, we had that lingering feeling that we were not done yet.


So we did the next reasonable thing: We read the whole source code, with a particular focus on any usage of internal data structures we frequently used for memoized results. That uncovered some more potential memory leaks.


Last Monday (December 18, 2023), we shipped Apollo Client 3.9 beta. This release contains over 20 memory-related pull requests, which not only fixes memory leaks, but also deprecates a feature, adds new configuration for tight control over Apollo Client’s internal memoization caches, and provides the necessary preparations to get insights into these caches from our DevTools.


## **Overview of changes**


Here’s an overview of some of the things we did:


### **Granular configuration for Apollo Client’s internal memoization**


Apollo Client does a lot of expensive work internally. To speed things up, it memoizes results for intense calculations.


Memoization is always a trade-off between processing time and memory. In the past, Apollo Client erred on the side of “using more memory is better than using more CPU time” and had either very generous cache limits or used a` WeakMap` approach that kept memoized values around for as long as some user-defined variable kept the computation source value in memory.


While the` WeakMap` approach should have been fine in most cases, we can’t deny that it can cause a memory leak if user code does not follow our expectations. That, or we make a mistake ourselves, like using one of the source objects we use as memoization keys as keys in a strong` Map` somewhere else!


We also believe that our cache limits were too generous, so we revisited every internal memoization cache, double-checked our key usage, replaced all` WeakMaps` with a new Weak LRU Cache implementation, and made sure that each cache has a more reasonable maximum size that better represents the results it stores.


While those defaults should be sufficient in 90% of applications, we know that some applications will need to make different trade-offs, so we made each cache size individually configurable. You can read up on all the new configuration options[in this new documentation page on memory management](https://www.apollographql.com/docs/react/caching/memory-management) .


### **Reflecting on the role of inMemoryCache.gc**


If you read through that issue, you will notice that many people regularly started calling` inMemoryCache.gc()` to eliminate their memory problems. With the changes we made in 3.9, you should have less of a reason to do that.


At the same time, we realize there is a time in life when you just need that hammer, so for those moments, we want to make sure you hit your target.` inMemoryCache.gc()` now will clear many more caches than before, so if you encounter any memory weirdness and have exhausted all the other options, the chances of this working out for you will be much higher.


### **Deprecating canonizeResults.**


You might have seen Apollo Client’s` canonizeResults` option – it’s a very cool feature: it ensures that if you encounter two objects of the same shape and contents, they are references to the same in-memory object. Since all results returned by Apollo Client are frozen, you also can’t accidentally modify these objects, so it is a clever way to save memory and computation time for deep equality checks.


Unfortunately, to offer this functionality, the underlying` ObjectCanon` class has to keep a reference to all these one-of-a-kind objects. Although the` ObjectCanon` uses a lot of weakly referenced data structures, we noticed that it just doesn’t let them go. Objects that end up in an` ObjectCanon` are very likely to stay in there forever.


This behavior might be acceptable (and still very beneficial) in a shorter-lived application or an application that doesn’t see a lot of data. Still, it can lead to serious memory problems in very long-lived applications that see a lot of data.


No matter what we tried – we can’t find a way around this problem, and we believe that keeping the` ObjectCanon` around as an option is too much of a foot gun for our users, especially as applications grow.


So, with Apollo Client 3.9 we are deprecating the option. The majority of our users won’t notice this change, as it was disabled by default. For the users that are currently using it (and happy with it), we will keep it around until the next major version. We will look into alternatives that enable similar behavior with a lower memory footprint.


### **Other memory-related changes in 3.9**


While these are the biggest changes we’re shipping, they are not the only ones.


- A potential memory leak in` RetryLink` was fixed, which also greatly reduced the bundle size for this link.
- ` PersistedQueryLink` will now clear its internal cache if the server signals that it cannot work with persisted queries.
- In React Native with the Hermes engine we now use` WeakMap` s/` WeakSet` s and the Weak LRU cache. In previous React Native engines, these were not supported, so we would have to fall back strong` Map` instances, which could cause memory issues. Unfortunately, Hermes does not support` WeakRef` s and` FinalizationRegistry` yet, so in some situations we have to fall back to strongly referenced data – but in every case it will have a maximum cache size.
- We fixed a potential memory leak if you changed variables in` useBackgroundQuery` or` useSuspenseQuery` very frequently.
- We fixed a potential memory leak that could occur in` Concast` if you directly used that in your code. This leak could never occur inside of Apollo Client due to the way we used` Concast` ourselves. We don’t assume that a lot of our users are using` Concast` directly, so this likely had a very low impact.
- ` canonicalStringify` is now decoupled from` ObjectCanon` and will use a lot less memory.
- All APIs that use internal memoization caches now expose the ability to reset the cache.


## **Your Help Wanted**


A lot of work has gone into all of this, and we have done our best to find good defaults for all the new cache sizes. Unfortunately, as library authors, we don’t have access to a lot of “real” apps that we can measure to validate our assumptions.


This is where we need your help!


Please try our beta in your application, run our development builds in your dev or staging environments, and after using your app for a while, run` __APOLLO_CLIENT__.getMemoryInternals()` , and submit the results to[our feedback collection issue](https://github.com/apollographql/apollo-client/issues/11444) . The more measurements we get from real-life apps, the better we can choose those default cache sizes!


The current plan is to wait until January 15 for measurements, at which point we will decide on our final cache sizes and put out a first release candidate.


Your help would be greatly appreciated!


Written by


Lenz Weber-Tronic


[Read more by Lenz Weber-Tronic](https://www.apollographql.com/blog/author/lenz)
