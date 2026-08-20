---
schema_version: "1.0.0"
document_id: "933609ea571e940f935543e95b6f610f16d1f41ccb67436e9f0d2e2fd2af52d5"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/acl-filtering-in-authzed"
published_at: "2021-06-23T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:04:52.597445+00:00"
content_hash: "sha256:ffe0e0b5fb8eb736e9e29929c598a1287e1b0120f50893eaddbdaefa3d1a5004"
---

# ACL Filtering in Authzed

## A high-level "view" of the problem


In our[previous blog post](https://authzed.com/blog/six-month-profile-page/) we discussed a scenario that is common when building modern multi-user or multi-tenant applications: How to provide each user with their own "view" of the resources found within the system in a performant and safe manner. Such views are often found on landing pages, application entry points, and other kinds of "start" pages, where a user desires to see their most recently accessed or available resources. Providing the *correct* resources to display for a particular user from the set of all resources is known as[ACL Filtering](https://docs.authzed.com/authz/acl-filtering/) , where correct is defined as ensuring that the current user has permission to view (or edit, etc), with those that fail being excluded from view.


On the surface this problem seems fairly straightforward: simply[check](https://docs.authzed.com/concepts/check) every resource to be displayed against the current user’s set of permissions. Yet despite the apparent simplicity, it has been our experience that solving this problem in a performant and secure manner is one of the larger challenges facing application developers today.


Why is performing ACL or permission based filtering so very hard?


## A massive undertaking


In a word: *scale* .


In order to properly and *securely* filter the resources a user is capable of viewing, the permissions check must be performed for *each* (relevant) resource found within the application. For some applications, the number of resources to check may be small: if your application is showing the last 10 opened documents for a user, then checking these 10 documents may be very quick. For other queries, however, the numbers of resources to check could be in the millions (or more): imagine trying to show a paginated list of *all* documents which the current user can edit; even if the page size is merely 25 documents, there is no guarantee how many documents will need to be checked before that limit is met.


In applications making use of database-defined permissions models, this problem is traditionally handled by joining the resource table being displayed with the permissions tables necessary for filtering. At first glance, this seems a reasonable solution: Simply add as many necessary` JOIN` statements to the` select` in order to properly filter the results, and the database will handle the rest! Unfortunately, in practice, each additional` JOIN` statement often adds significant overhead to the query, and results in massive increases in query time as each of the joined tables grows in size. In our own experience, we have had databases where the *majority* of CPU time and resources were spent on such permissions filtering, forcing hand written brittle optimization and manual pagination (or limited subqueries) as workarounds.


In applications making use of[Policy Engines](https://docs.authzed.com/authz/policy-engine) , the situation is even worse: all potential resources first have to be fetched from the database and then filtered by the policy engine. Because Policy Engines are almost always executed alongside an application, this takes the inefficiencies of the database` JOIN` and multiplies them with networking overhead.


Both these examples demonstrate the underlying problem: trying to filter all the resources down to those visible or accessible to the user will quickly hit scaling limits once more than perhaps a few hundred or thousands of resources exist (let alone millions or billions).


What if there was another way?


## Checking in with Authzed


In another previous blog post, we described how[Authzed computes permissions answers](https://authzed.com/blog/check-it-out/) (if you have not read that blog post, we recommend doing so now and then returning here).


As shown, permissions in Authzed are computed by walking the *graph* formed by combining object definitions and the relationships between data, searching for a path from the Object (say, a specific document) to the Subject (for example, a user) via an Action (e.g.` reader` ):


*Jill is the owner of` somedocument` , so she is also a reader*


This structure of answering permissions allows for computing complex permissions hierarchies, including inferred or implicit permissions, as well as nesting such as groups. It *also* allows for use of graph-based optimizations, such as walking subgraphs in parallel, to allow for faster computation where possible.


We thought to ourselves if we could take advantage of the graph nature to compute which Subjects could perform an Action on an Object, could we also use it to do the reverse: find out which Objects could have an Action performed on them by a specific Subject?


## Reversing a permissions check


The answer, as it turned out, is **yes** ! Because all Checks in Authzed are answered by walking a graph, we realized we could perform a similar operation (but in "reverse").


As an example, let’s take our sample Check from above:


> Is` Jill` \[Subject\] a` reader` of \[Action\] document` somedocument` \[Object\]?


As we saw above, this answer to this Check is computed by walking the graph from` somedocument` , through` reader` , through` owner` and finally reaching` Jill` , proving that she has access to read the document.


Now let’s imagine we wanted to find *all* documents in the system in which Jill is the` reader` . We can compute this information by *reversing* the question we’re asking:


Is` Jill` \[Subject\] a` reader` of \[Action\] a document \[Object\]?


➡️


On which documents \[Object\] is` Jill` \[Subject\] a` reader` \[Action\]?


To compute the answer to this question, we can therefore walk the graph "backward", starting at the Subject (` Jill` ) and walking until we find the correct Action, returning any Objects found there:


At first glance, this seemed a simple enough solution: Start at the Subject (` Jill` ) and walk backward from her across any path that can potentially reach the Action on the type of Object for which we are querying.


However, we quickly hit a road bump: in addition to the standard *union* operation for computing permissions, Authzed also supports *intersection* and *exclusion* . Intersection and exclusion, however, cannot be computed without *all* child paths themselves having been computed, meaning that we’d have to perform the equivalent of a "reduce" operation at any intersection or union. Fortunately, this led to a natural solution: map, then reduce.


## The great filter


We therefore landed on our current solution for ACL-aware indexing: the[Lookup API](https://buf.build/authzed/api/docs/main:authzed.api.v1#authzed.api.v1.PermissionsService.LookupResources) .


The Lookup API, when given a specific Subject, and a class of Object and Action, performs a map reduce operation over the Authzed permissions graph, starting at the Object+Action, towards reaching the Subject wherever it can be found.


Taking our previous example of all the documents which` Jill` is a` reader` :


Each path is mapped over the objects found, and then reduced as we go along, until only those objects accessible to the Subject via the specified Action remain.


Thus, by taking advantage of the graph nature of Authzed’s permission system, we can now elegantly and performantly compute an ACL-aware filter of objects!


## Trying the API


The beta[Lookup API](https://buf.build/authzed/api/docs/main:authzed.api.v1#authzed.api.v1.PermissionsService.LookupResources) is available today for testing and use.


Have any questions, comments or just want to chat?[Visit the Discord](https://authzed.com/discord) .


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)
- [A Primer on Modern Enterprise Authorization (AuthZ) Systems](https://authzed.com/blog/authz-primer)
- [Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
- [Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization)


On this page


- A high-level "view" of the problem
- A massive undertaking
- Checking in with Authzed
- Reversing a permissions check
- The great filter
- Trying the API
- Additional Reading


## Related


[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Joey Schorr · Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)


[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Adora Nwodo · Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)
