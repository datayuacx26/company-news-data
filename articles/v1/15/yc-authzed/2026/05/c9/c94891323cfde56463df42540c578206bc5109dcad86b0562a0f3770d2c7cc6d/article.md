---
schema_version: "1.0.0"
document_id: "c94891323cfde56463df42540c578206bc5109dcad86b0562a0f3770d2c7cc6d"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/query-planner-early-benchmarks"
published_at: "2026-05-20T14:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T20:50:36.289404+00:00"
content_hash: "sha256:3c98cdaed645c33a4f6f9b77bb82272a6676d12225daabbb6830325657133925"
---

# Query Planner Early Benchmarks

## Introduction


We've been hard at work on the[SpiceDB Query Planner](https://authzed.com/blog/introducing-spicedb-query-planner) and are excited to share some early progress and benchmarks. We even[talked about it a bit](https://www.youtube.com/watch?v=uzS2GBGe3sw) on our livestream of Authzed Office Hours.


ReBAC is usually represented as a permission graph, where we consider paths of relations between resources and subjects. Often, these are mediated by entities that are useful to the problem domain; if you're building Google Docs, for example, you're going to care about sharing documents with users, groups and mailing lists full of users, folder structures, and the like. These types form the *schema* the` CheckPermission` request is evaluated against.


In[Part 1](https://authzed.com/blog/introducing-spicedb-query-planner) of this series we framed permission checks as reachability in a graph. To talk about the query planning, it's useful to use an analogy with geography: the goal of the query planner is to build a route on a map, and it uses various "advisors" to pick a better route. The "static" advisor gives hints to the query planner on which path to take as if it has never driven before and making generic assumptions about "traffic" (really, about fanout and cardinality). The "count" advisor gives hints to the query planner based on real data, by keeping some statistics on past trips it has made.


Each "hop" we have to do in the graph can be thought of as a distinct subproblem. For example, if we know a document is shared with the engineering org, a subproblem could be to check if Alice or Bob are members of that org (perhaps through their team). Perhaps they're members of multiple teams, in a cross-functional sense, and could be in either. These subproblems can be evaluated (and cached) separately by individual nodes in a SpiceDB cluster. Much of the way SpiceDB can improve performance is by acting as this distributed cache.


So a natural step is to wire in query plans as subproblems for the SpiceDB cluster. We call this dispatch: instead of evaluating every hop locally, the query planner can send a subproblem to another node in the SpiceDB cluster, which runs it and caches the result. Dispatching trades a bit of network cost for a potentially large cache win.


We'd like to show you what we're seeing in early performance numbers for some of our different scenarios and backends, with vs. without the advisors, and with vs. without dispatching.


## Scenario Walkthroughs


There are two major types of benchmarks that we run, and four different scenarios we're looking at today. (If you are interested in the raw results across history, see[this](https://authzed.github.io/spicedb/dev/bench/) .)


The major types are **algorithmic** benchmarks and **datastore** benchmarks. This is analogous to unit tests vs integration tests — the algorithmic/unit tests test the simplest, memory-based backends but are aimed at measuring and profiling the performance of individual parts of the query code. The datastore benchmarks are there to measure end-to-end performance, including all the subproblem dispatch choices and latencies to various datastore backends.


Scenarios are there to represent different patterns of data, based on schema situations we've noticed from customers. These live in a registry in` pkg/benchmark` and can be used across the codebase. Each defines a schema, populates relationships, and provides a set of queries to assert on (CheckPermission, or LookupResources, or LookupSubjects, depending on the specific benchmark). They're meant to be corner cases that exercise different patterns in the relationships that can be problematic for certain query plans, but potentially simple with the right optimizations. Let's look at these four:


### DeepArrow — 30-Level Recursive Chain


zed


1


2


3


4


5


6


7


```text
definition    user    {  }


definition    document    {
relation    parent  :  document
relation    view  :  user
permission   viewer  =   view  +   parent -  >viewer
}


```


zed


1


2


3


4


5


6


7


```text
definition    user    {  }


definition    document    {
relation    parent  :  document
relation    view  :  user
permission   viewer  =   view  +   parent -  >viewer
}


```


Documents (or, often, folders) can have a recursive chain of parents — so being able to view a parent document often gives access to child documents. This scenario builds a chain of 30 nested document-folders and adds view permission at the top. How well can we handle this recursion and where do we dispatch?


### WideArrow — Single Arrow with Fan-Out


zed


1


2


3


4


5


6


7


8


9


10


11


```text
definition    user    {  }


definition    group    {
relation    member  :  user
}


definition    document    {
relation    group  :  group
relation    view  :  user
permission   viewer  =   view  +   group -  >member
}


```


zed


1


2


3


4


5


6


7


8


9


10


11


```text
definition    user    {  }


definition    group    {
relation    member  :  user
}


definition    document    {
relation    group  :  group
relation    view  :  user
permission   viewer  =   view  +   group -  >member
}


```


A pretty standard document/groups/users structure, similar in spirit to the group membership example in the[SpiceDB Playground](https://play.authzed.com/) .


But what we're really exercising here is what happens when there's what we'd call a "wide arrow" or "large fanout" — that is, an individual document is shared widely, and we're looking for a specific user.


Assume we have 10 documents, 100 groups (each document shared with 30 groups), and 1000 users (20 users per group). The naive approach tries to start at the document and find the user. But it's clearly more efficient to start at the user and find the document! If there's only 10 documents, at worst we check each from the user. But if there's 1000 users, or even 600 possible users (30x20) with some duplication, there are a lot more intermediate groups to check, many of which will be wasted effort.


### DoubleWideArrow — Two Consecutive Fan-Out Arrow Hops


zed


1


2


3


4


5


6


7


8


9


10


11


12


13


14


15


16


```text
definition    user    {  }


definition    group    {
relation    member  :  user
}


definition    org    {
relation    group  :  group
permission   member  =   group -  >member
}


definition    document    {
relation    org  :  org
relation    view  :  user
permission   viewer  =   view  +   org -  >member
}


```


zed


1


2


3


4


5


6


7


8


9


10


11


12


13


14


15


16


```text
definition    user    {  }


definition    group    {
relation    member  :  user
}


definition    org    {
relation    group  :  group
permission   member  =   group -  >member
}


definition    document    {
relation    org  :  org
relation    view  :  user
permission   viewer  =   view  +   org -  >member
}


```


Same as before, but now there are three hops instead of two. 5 documents across 100 orgs, to 300 groups within those orgs, and 500 users. Same issue; but now it's even worse because there are more hops involved. We run these sorts of tests to exercise how efficient we are at hops, and which directions we take them.


### LookupIntersection — Intersection + Exclusion


zed


1


2


3


4


5


6


7


8


9


10


11


12


13


14


15


```text
definition    user    {  }


definition    organization    {
relation    reader  :  user
relation    banned  :  user
permission   read  =   reader  -   banned
}


definition    file    {
relation    viewer  :  user
relation    organization  :  organization


permission   read  =   organization -  >read
permission   view  =   viewer  &   read
}


```


zed


1


2


3


4


5


6


7


8


9


10


11


12


13


14


15


```text
definition    user    {  }


definition    organization    {
relation    reader  :  user
relation    banned  :  user
permission   read  =   reader  -   banned
}


definition    file    {
relation    viewer  :  user
relation    organization  :  organization


permission   read  =   organization -  >read
permission   view  =   viewer  &   read
}


```


This is important for checking the performance of the various set operations we can perform on queries. For example, if the file is not viewable by a user (a direct relation), it's pointless to try to seek out if the user is the member of an organization that can read that file — we can short-circuit! This is similar for the` banned` exclusion — if we're not a reader, we can skip removing any banned users. Or we can hedge and check both in parallel. But importantly, we're exercising the` &` and` -` operators.


## Optimizations the Query Planner Performs


These are a few of the optimizations we have implemented:


### Arrow reversal


Three of our scenarios relate to arrows, so we will start there. We'd like to know which direction is the faster way to evaluate an arrow. This is a direct analogue of choosing a join order in a relational database — which side of a join to bring into the other, constrained by the semantics of the arrow itself.


In Part 1 we learned that arrows are essentially the question "do these two sets intersect?". The cheapest way to answer that is to enumerate the smaller side. The "count" advisor watches the results-per-call ratio on each side and tells the planner which side is smaller in practice. This is what flips an arrow evaluation from Left-to-Right (LTR) to Right-to-Left (RTL), the two strategies available.


### Branch reordering


When the permission is a union, e.g A + B + C, we want to try the branch most likely to grant permission first, so we can short-circuit as soon as anything *matches* . When it's an intersection (A & B & C), we want the most *selective* branch first, so we can short-circuit as soon as anything *misses* . The "static" advisor uses cost estimates to sort branches into the order that maximizes the chance of short-circuiting.


### Arrow rotation


Nested arrows like (A->B)->C are associative in their meaning but not in their cost. The planner can rewrite them as A->(B->C) and pick whichever version is cheaper.


### Reachability pruning


If a branch of the plan can never produce the type of subject the request is asking about (for example, a` group->member` branch that can only yield` users` , when the request is asking whether a` robot` has access), the planner replaces it with a no-op.


## The Numbers


So without further ado, let's look at how query planner is shaping up in our two domains — algorithmic and datastores.


### Algorithm Comparison (no I/O, no dispatch)


Most of our performance work up to this point has been looking at what potential savings we can possibly get from using a query planner — remove everything else and compare the performance of the two algorithms; our current Dispatch and the Query Plan. For the given scenarios (which you can run yourself! This lives in` pkg/query/benchmarks` ) here's a rough breakdown. We also show Query Plans that are "plain" or, statistics-less and unadvised, and one that takes observations into account, to optimize the plan, with an advisor.


Scenario Direction Dispatch (time) QP Plain (time) QP Advised (time) Dispatch (mem) QP Plain (mem) QP Advised (mem)


**WideArrow** LTR → RTL 72 µs 771 µs **57 µs** 43 KB 40 KB **4.2 KB**


**DoubleWideArrow** LTR → RTL **138 µs** 11,175 µs 182 µs 93 KB 208 KB **7.9 KB**


**DeepArrow** LTR 772 µs **70 µs** **70 µs** 715 KB **120 KB** **120 KB**


**LookupIntersection** LTR 59 µs **16 µs** **16 µs** 53 KB **5.6 KB** **5.6 KB**


The advisor is doing a lot of work in the first two scenarios. When we choose the wrong plan, as in the plain case of either arrow, we do a lot of extra work. Some of this extra work is absorbed in the current implementation, which was written and hand-tweaked over the years. But in most cases, we're competitive, if not much faster, and we're able to perform with less memory overhead.


### Datastore Comparison


It was only in v1.53 that we introduced Dispatch for query plans. It's our newest addition and it has yet to be optimized — we know that we're over-dispatching the subproblems! But as these are early numbers, these are the places we expect to work on improvement. This is why we're comparing current Dispatch (going through the full gRPC path) to Local (QP doing zero dispatches, caching only locally) and the early dispatch-based query plans. All of these are advised.


**In-Memory datastore**


Scenario Dispatch (time) QP Local (time) QP Dispatch (time) Dispatch (mem) QP Local (mem) QP Dispatch (mem)


WideArrow 72 µs 59 µs 99 µs 43 KB 5.3 KB 49 KB


DoubleWideArrow 138 µs 186 µs 420 µs 93 KB 9.2 KB 128 KB


DeepArrow 771 µs 75 µs 294 µs 715 KB 122 KB 519 KB


LookupIntersection 59 µs 18 µs 109 µs 53 KB 7.1 KB 164 KB


**Postgres datastore**


WideArrow 1.33 ms 0.50 ms 1.56 ms 128 KB 35 KB 148 KB


DoubleWideArrow 2.49 ms 1.07 ms 5.79 ms 239 KB 53 KB 515 KB


DeepArrow 19.55 ms 10.41 ms 20.15 ms 2,464 KB 993 KB 1,792 KB


LookupIntersection 1.34 ms 0.70 ms 2.49 ms 172 KB 64 KB 300 KB


**CockroachDB datastore**


WideArrow 1.69 ms 0.43 ms 1.98 ms 114 KB 30 KB 131 KB


DoubleWideArrow 4.02 ms 0.68 ms 7.51 ms 214 KB 46 KB 457 KB


DeepArrow 25.96 ms 12.78 ms 26.10 ms 2,130 KB 938 KB 1,634 KB


LookupIntersection 1.79 ms 0.85 ms 3.47 ms 149 KB 58 KB 274 KB


## Conclusion


In Part 1 we had set the goal of improving the speed of` CheckPermission` requests. The numbers above show that the query planner is doing just that. For single-node SpiceDB clusters, or ones where the problems are hard to cache, Query Planner is looking great against the current query implementation — often twice as fast or better! We will be continuing our work on dispatch in 1.54 and going forward: deciding *when* a subproblem is worth sending over the wire vs. keeping local.


If you prefer this content in video format:


On this page


- Introduction
- Scenario Walkthroughs
- DeepArrow — 30-Level Recursive Chain
- WideArrow — Single Arrow with Fan-Out
- DoubleWideArrow — Two Consecutive Fan-Out Arrow Hops
- LookupIntersection — Intersection + Exclusion
- Optimizations the Query Planner Performs
- Arrow reversal
- Branch reordering
- Arrow rotation
- Reachability pruning
- The Numbers
- Algorithm Comparison (no I/O, no dispatch)
- Datastore Comparison
- Conclusion


## Related


[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Joey Schorr · Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)


[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Adora Nwodo · Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)
