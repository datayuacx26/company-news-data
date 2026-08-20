---
schema_version: "1.0.0"
document_id: "b9f0b1747fa526df95369f304da9534877cf10681cabc848aa875e143d15b449"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/how-caching-works-in-spicedb"
published_at: "2024-02-08T16:06:42.461+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T22:26:16.511012+00:00"
content_hash: "sha256:12408daa653af9163e219a719a443e27b4f76e426da87cfe23d165a67c4038da"
---

# How Caching Works in SpiceDB

## Background


Performance is one of the most important characteristics of an authorization system: authorization checks commonly appear in code paths that affect user experience, and any way to reduce this latency can have a major impact on reducing the overall time that real people are waiting for their requests to be served.


When Google embarked on the[Zanzibar project](https://authzed.com/zanzibar) to implement relationship based access control (ReBAC), performance was top of mind: Google processes millions of authorization requests per second, and these requests have to be answered as fast as possible. To effectively answer this many check requests and do so at a reasonable P95 latency, the Zanzibar paper introduced a novel way of caching that reuses authorization results.


In this blog post, we’ll discuss how caching works in SpiceDB, AuthZed’s open source implementation of Google Zanzibar, and answer the most commonly asked questions about caching in SpiceDB.


## Subproblems and caching


To understand how caching functions in SpiceDB to enable[super large scales](https://authzed.com/blog/google-scale-authorization) , it is important to first understand how check requests are handled.


In previous blog posts ([here](https://authzed.com/blog/check-it-out) and[here](https://authzed.com/blog/check-it-out-2) ), we discussed how check requests in SpiceDB are broken down into a series of “subproblems,” with each subproblem running in parallel to ensure solid performance.


In the above animation, we can see that the check request \`document:somedocument#view@user:jill\` was broken down into a number of subproblems:


- document:somedocument#reader@user:jill
- document:somedocument#owner@user:jill
- organization:theorg#can_admin@user:jill


Decomposing a single check problem into (potentially) numerous subproblems also has a major advantage when it comes to caching: **individual subproblems can be cached and reused while answering other check queries, if those other queries share the same (or a subset) of those subproblems** .


For example, let’s say the above document’s (“somedocument”) organization (“theorg”) was shared with another document (“someotherdocument”). If, after checking for \`document:somedocument#view@user:hannah\`, a check came in for \`document:someotherdocument#view@user:hannah\`, there would be no need to check again if “user:hannah” has “can_admin” on “organization:theorg”; we just answered that question (in the affirmative) moments ago for the other document:


In fact, if we compare the subproblems generated for each request, we can find the crossover between the two:


document:somedocument#view@user:hannah• document:somedocument#reader@user:hannah• document:somedocument#owner@user:hannah• organization:theorg#can_admin@user:hannah


document:someotherdocument#view@user:hannah• document:someotherdocument#reader@user:hannah• document:someotherdocument#owner@user:hannah• organization:theorg#can_admin@user:hannah


Thus, if two (or more) checks which share common subproblems are made in “rapid succession,” we can see how the cache can be used to answer them:


## What is “rapid succession”?


On its surface, the above caching mechanism seems pretty straightforward: simply cache the result of any check or subproblem forever, and, if the problem exists in the cache when the next check request comes in, answer it!


Sadly, this kind of caching introduces a new issue: it violates correctness. A cache entry is only usable so long as its underlying data has not changed; otherwise, the entry has become invalid and needs to no longer be trusted.


Imagine the same scenario as above, but if the relationship \`organization:theorg#admin@user:hannah\` was removed between the calls:


As we can see, the result of the second check command is wrong: \`user:hannah\` no longer has access to the organization, which means she should also no longer have access to the document.


You may be thinking, “This seems quite bad: we’ve returned the wrong permissions check!” In order to have an effective cache, we need to come up with rules to invalidate the cache entry when any of the data it relies upon has changed. Given the possible complexity of the schema, however, this seems quite a tall order and would likely be quite slow.


Fortunately, the authors of the Zanzibar paper realized two important facts about authorization checks: most checks occur with hotspots of shared subproblems and the majority of the time, a permission check being slightly out-of-date (i.e. slightly stale) is perfectly acceptable.


If a user previously had access to view a resource and just lost access, nothing is being leaked so long as the resource has not changed. For example, if a user only lost access to a particular GitHub repository within the last 30 seconds, allowing them to view the repository now isn’t a major issue so long as the contents of the repository have not changed: they could have already pulled the code 30 seconds ago and the outcome would be the same.


For the case where the resource has changed, however, this kind of staleness can be quite a problem. In fact, it was recognized as being such a problem that it received its own name:[The New Enemy Problem](https://authzed.com/blog/new-enemies) and its own solution[Zookies](https://authzed.com/docs/reference/zedtokens-and-zookies#what-is-a-zedtoken) . We won’t go into a discussion here as to the specifics of the new enemy problem (see the linked blog post for more information), but it is important to note this important caveat about caching!


Since we find some staleness acceptable in most instances, and because caching is often used for handling significant hotspots, Zanzibar settled on a fairly straightforward and easy to compute solution: define a temporal window for which cache entries will be kept and then simply throw them all out after that window closes!


In SpiceDB, this configurable window is set by default to \`5\` seconds, which for most cases is a reasonable amount of time to handle hotspots while also not being too stale. For cases where staleness must be avoided at all cost (such as checking whether the user can perform actions like \`edit\`), SpiceDB also provides[configurable consistency options](https://authzed.com/docs/reference/api-consistency) on its APIs. This allows the caller to choose whether to use the faster cached values or the slower, but guaranteed consistent values.


(To learn more about configuring the caching windows, see[https://authzed.com/blog/hotspot-caching-in-google-zanzibar-and-spicedb](https://authzed.com/blog/hotspot-caching-in-google-zanzibar-and-spicedb) )


## Distributing the cache


Even with the support of a configurable window and ZedTokens (our name for Zookies) to ensure correctness, another problem with caching comes up: how do we make sure that the correct cache is used (and updated) when deploying a multi-node distributed service like SpiceDB?


A naive solution is simply to keep the cache on all nodes, but this creates a problem: duplication. A (sub)problem that we may have already solved and cached may not be stored on the node for the incoming request, requiring that the problem be recomputed.


Thus, to ensure the maximal reuse of the cache, SpiceDB makes use of a token hashring to distribute the cache across nodes in a stable fashion. Rather than each node computing its own solutions and keeping its own cache, the cache is distributed across the nodes, with each set of subproblems being handled by a specific node (or nodes):


As we can see in the above example, each unique subproblem is assigned a node on the token hashring, ensuring only that portion of the cache is kept on that node. This allows for a much larger cache, and also makes it possible to deduplicate requests: if we get a request for the same subproblem (before it is cached), we can have all nodes wait on the result, rather than recomputing the solution


## Some numbers


As result of the above design, SpiceDB users can often see cache rates of up to 60% over all subproblems being computed. With request deduplication support, users can often see up to a 40% reduction in the number of requests as well.


When processing[1 million Queries Per Second](https://authzed.com/blog/google-scale-authorization) , this kind of caching adds up mighty quick!


## Future of SpiceDB caching


In this blog post we discussed the current caching strategy used by SpiceDB, modeled after the strategy defined in the Zanzibar paper. While this form of caching (called “L1” internally) is[super effective](https://tenor.com/view/pokemon-super-effective-gif-2194997937857907108) , it is not the only caching strategy that can be used in a distributed relationship-based permissions system.


Stay tuned to the blog for a future post discussing the next steps in SpiceDB’s caching strategy!


On this page


- Background
- Subproblems and caching
- What is “rapid succession”?
- Distributing the cache
- Some numbers
- Future of SpiceDB caching


## Related


[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Joey Schorr · Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)


[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Adora Nwodo · Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)
