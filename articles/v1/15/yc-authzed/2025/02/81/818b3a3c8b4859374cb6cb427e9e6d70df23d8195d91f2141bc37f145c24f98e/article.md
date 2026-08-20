---
schema_version: "1.0.0"
document_id: "818b3a3c8b4859374cb6cb427e9e6d70df23d8195d91f2141bc37f145c24f98e"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/the-evolution-of-expiration"
published_at: "2025-02-13T10:16:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T20:58:20.301648+00:00"
content_hash: "sha256:1438f014a797915c351fd21ca1692b00332c9306a305b58305bc86be26ab4a94"
---

# The Evolution of Expiration

# Feature Highlight: Relationship Expiration


We are excited to[announce](https://authzed.com/blog/build-time-bound-permissions-with-relationship-expiration-in-spicedb) that as of the[SpiceDB v1.40](https://github.com/authzed/spicedb/releases/tag/v1.40.0) release, users now have access to a new experimental feature: Relationship Expiration. When writing relationships, requests can now include an optional expiration time, after which a relationship will be treated as removed, and eventually automatically cleaned up.


## The evolution of expiration


Even when first setting out to create SpiceDB, there was never any doubt whether or not users would want time-bound access control to their resources. However, the inspiration for SpiceDB, Google's Zanzibar system, has no public documentation for how this functionality is built. As our initial goals for the SpiceDB project were to be as faithful to Google's design as possible, we initially left expiration as an exercise to the user.


Without explicit support within SpiceDB, users could still use external systems like workflow engines (e.g.[Temporal](http://temporal.io/) ) to schedule calls to the SpiceDB DeleteRelationships or WriteRelationships APIs in order to solve this problem. This is a perfectly valid way to solve this problem, but it has a major tradeoff: users must adopt yet another system to coordinate their usage of the SpiceDB API.


After we had successfully reached our goal of being the premier implementation of the concepts expressed in the Google Zanzibar paper, we turned our focus to improving developer experience and more real-world requirements outside of the walls of Google. This led us to[collaborating with Netflix](https://authzed.com/customers/netflix) on a system for supporting lightweight policies to more effectively model ABAC-style use cases. This design came to be known as Relationship Caveats. Caveats allow SpiceDB users to write conditional relationships that exist depending on whether a CEL expression evaluates to true while their request is being processed. With the introduction of Caveats, SpiceDB had its first way to create time-bounding without relying on any external system. The use case was so obvious, even our first examples of Caveats demonstrated how to implement time-bounded relationship expiration.


As more SpiceDB users adopted Caveats, we began to acknowledge some trends in its usage. Many folks didn't actually need or want the full expressiveness of policy; instead they cared solely about modelling expiration itself. Eventually it became obvious that expiration was its own fully-fledged use case. If we could craft an experience specifically for expiration, we could steer many folks away from some of the tradeoffs associated with caveats. If you still need caveats for reasons other than expiration and you're wondering if relationships support both caveats and expiration simultaneously, they do!


## What's going on under-the-hood for Relationship Expiration?


If you've spent time reading some of the deeper discussions on SpiceDB internals or studying other systems, you might be familiar with the fact that time is incredibly nebulous in distributed systems. Distributed systems typically eschew "wall clocks" altogether. Instead, for correctness they need to model time based on the[ordering of events that occur in the system](https://lamport.azurewebsites.net/pubs/time-clocks.pdf) . This observation, among others, ultimately led Leslie Lamport to win a Turing Award. SpiceDB is no exception to this research: the opaque values encoded into SpiceDB's ZedTokens act as logical clocks used to provide consistency guarantees throughout the system.


If the problem here isn’t already clear: fundamentally, relationship expiration is tied to wall clock time, but distributed systems research proves this is a Bad Idea™. In order to avoid any inconsistencies caused by the skew in synchronization of clocks across machines, SpiceDB implements expiration by pushing as much logic into the underlying datastore as possible. For a datastore like PostgreSQL, there is no longer a synchronization problem because there's only one clock that matters: the one on the leader's machine. Some datastores even have their own first-class expiration primitives that SpiceDB can leverage in order to offload this logic entirely while ensuring that the removal of relationships are done as efficiently as possible. This strategy is only possible because of SpiceDB's unique architecture of reusing other existing databases for its storage layer rather than the typical disk-backed key-value store.


## Trying out Relationship Expiration


There are only a few steps required to try out expiration once you've upgraded to SpiceDB v1.40:


1. Enable the experimental flag when running SpiceDB


command


1


```text
spicedb serve --enable-experimental-relationship-expiration [...]


```


command


1


```text
spicedb serve --enable-experimental-relationship-expiration [...]


```


1. Annotate that you want to use first-class expiration in your schema.


zed


1


2


3


4


5


6


```text
use expiration


definition    folder    {  }
definition    resource    {
relation    parent  :  folder
}


```


zed


1


2


3


4


5


6


```text
use expiration


definition    folder    {  }
definition    resource    {
relation    parent  :  folder
}


```


1. Annotate the relations in your schema where you want to support expiration.


zed


1


2


3


4


5


6


```text
use expiration


definition    folder    {  }
definition    resource    {
relation    parent  :  folder     with   expiration
}


```


zed


1


2


3


4


5


6


```text
use expiration


definition    folder    {  }
definition    resource    {
relation    parent  :  folder     with   expiration
}


```


1. Provide a timestamp for the \`OptionalExpiresAt\` field when writing relationships.


proto


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
WriteRelationshipsRequest { Updates: [
RelationshipUpdate {
Operation: CREATE
Relationship: {
Resource: { ObjectType: "resource", ObjectId: "123", },
Relation: "parent",
Subject: { ObjectType: "folder", ObjectId: "456", },
OptionalExpiresAt: "2025-12-31T23:59:59Z"
}
}]
}


```


proto


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
WriteRelationshipsRequest { Updates: [
RelationshipUpdate {
Operation: CREATE
Relationship: {
Resource: { ObjectType: "resource", ObjectId: "123", },
Relation: "parent",
Subject: { ObjectType: "folder", ObjectId: "456", },
OptionalExpiresAt: "2025-12-31T23:59:59Z"
}
}]
}


```


## The journey for peak performance continues


Relationship Expiration is a great example of our never-ending journey to achieve the best possible performance for SpiceDB users. As SpiceDB is put to the test in an ever-increasing number of diverse enterprise use-cases, we learn new things about where optimizations should be made in order to deliver the best product for scaling authorization. Sometimes it requires going back to the drawing board on a problem we thought we had previously solved and totally reconsidering its design. With that, I encourage you to go out and experiment with Relationship Expiration so that we learn even more about the problemspace and continue refining our approach.


On this page


- Feature Highlight: Relationship Expiration
- The evolution of expiration
- What's going on under-the-hood for Relationship Expiration?
- Trying out Relationship Expiration
- The journey for peak performance continues


## Related


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)


[Engineering Build and Deploy a GitHub-Style Permission System in AuthZed Cloud Learn how to model a complex GitHub-style permission system with SpiceDB and deploy it to AuthZed Cloud, covering tiered roles, org ownership, team hierarchies, and granular repository permissions. May 26, 2026 · 9 min](https://authzed.com/blog/github-permission-system-authzed-cloud)[Engineering Build and Deploy a GitHub-Style Permission System in AuthZed Cloud Learn how to model a complex GitHub-style permission system with SpiceDB and deploy it to AuthZed Cloud, covering tiered roles, org ownership, team hierarchies, and granular repository permissions. Sohan Maheshwar · May 26, 2026 · 9 min](https://authzed.com/blog/github-permission-system-authzed-cloud)


[Engineering Feature Highlight: The AuthZed Cloud Datastore, Unlocked AuthZed Cloud customers can now inspect and scale the datastore powering their SpiceDB permission systems directly from the Datastore Overview page — no support ticket required. Mar 3, 2026 · 3 min](https://authzed.com/blog/cloud-datastore-unlocked)[Engineering Feature Highlight: The AuthZed Cloud Datastore, Unlocked AuthZed Cloud customers can now inspect and scale the datastore powering their SpiceDB permission systems directly from the Datastore Overview page — no support ticket required. Jimmy Zelinskie · Mar 3, 2026 · 3 min](https://authzed.com/blog/cloud-datastore-unlocked)
