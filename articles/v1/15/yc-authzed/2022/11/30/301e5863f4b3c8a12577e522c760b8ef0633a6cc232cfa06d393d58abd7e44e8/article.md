---
schema_version: "1.0.0"
document_id: "301e5863f4b3c8a12577e522c760b8ef0633a6cc232cfa06d393d58abd7e44e8"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/caveats"
published_at: "2022-11-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:02:37.132176+00:00"
content_hash: "sha256:8adfe7d0a8fbd5407100dd092c56875cbbc2a3293d2c3f07e2180d7b70bcfa2f"
---

# Caveats: A Scalable Solution for Policy

Every day, thousands of developers design and build permissions systems into their software. Without knowing it, these developers risk creating a legacy of a bespoke system with siloed data and minimal tooling for maintenance or ensuring correctness. To reduce this risk, Authzed is building every developer's dream suite of authorization tooling. At the core of this toolchain is[SpiceDB](https://github.com/authzed/spicedb) , the premier solution for implementing fine-grained permissions. SpiceDB takes inspiration from state of the art internal systems at[Google](https://authzed.com/blog/what-is-zanzibar/) and[Facebook](https://www.usenix.org/conference/osdi20/presentation/shi) . But unlike these proprietary systems, SpiceDB is built as an open source collaboration with our community.


When we open-sourced SpiceDB in 2021, it had already seen a year of production usage. Our team has a strong background of operating critical systems at companies like Google, Amazon, and Red Hat, so we knew the importance running the software you build and aiming for a high level of maturity early in a project's development. While that meant SpiceDB launched as a phenomenal solution for a wide variety of use cases, it also meant that it couldn't handle all of the use cases our community would eventually throw at us.


## Taken ABAC


After engaging with the community for a few months, we began researching the ideal way to represent permission systems modeling[ABAC](https://en.wikipedia.org/wiki/Attribute-based_access_control) in SpiceDB. Attribute-Based Access Control is a design pattern for permission systems that gates access by excluding or including entities based on the presence of particular label known as an attribute. Because SpiceDB is a relationship-based system, we could try to represent attributes as relationships using a schema such as this:


zed


1


2


3


4


```text
definition    user    {  }
definition    attribute    {
relation    haver  :  user
}


```


zed


1


2


3


4


```text
definition    user    {  }
definition    attribute    {
relation    haver  :  user
}


```


With these definitions, you could write a relationship that assigns` user:emilia` to be a` haver` of` attribute:superadmin` . An example of using these relationships could look like the following:


zed


1


2


3


4


5


```text
definition    document    {
relation    shared_with  :  user
relation    required_admin_attrs  :  attribute
permission   admin  =   shared_with  &   required_admin_attrs -  >haver
}


```


zed


1


2


3


4


5


```text
definition    document    {
relation    shared_with  :  user
relation    required_admin_attrs  :  attribute
permission   admin  =   shared_with  &   required_admin_attrs -  >haver
}


```


This definition of the` admin` permission performs an[intersection](https://docs.authzed.com/reference/schema-lang#-intersection) between the users a document has been shared with and the users that have the attributes required. But there's a problem here and it's not just that intersections are slower than other operations: we've actually written an expression that returns the users that have *any* of the attributes and not *all* of the attributes. This limitation led to some interesting discussion (and hilarious syntax bike-shedding on the[SpiceDB Discord](https://authzed.com/discord) ) and[a proposal on GitHub](https://github.com/authzed/spicedb/issues/597) to add new syntax. As of the writing of this blog post, this proposal has not been implemented in SpiceDB. While we're exploring to implementing this proposal, we realized that this functionality alone isn't sufficient to completely implement all ABAC designs.


## What's our policy on Policy?


Those diligent few that have already read Wikipedia's article on ABAC before proceeding may have already spoiled the big reveal. While assigning labels to objects is a convenient way to conceptualize ABAC, in actuality, attributes are formally defined as *boolean functions* . And, in practice, there are a variety of systems that need these boolean functions to be *dynamic* policies. Examples of dynamic policies include things like "during these hours", "with an IP addresses in this subnet", or "while the account balance is greater than 0".


If you're familiar with existing authorization tooling, your first intuition might be to see if you could leverage an existing[policy engine](https://docs.authzed.com/reference/glossary#policy-engine) alongside SpiceDB. While it is trivial to call SpiceDB's HTTP API from most policy engines, they are not a viable solution for every SpiceDB user. SpiceDB has a philosophy of being able to enforce data consistency when necessary in a truly horizontally scalable fashion. While not all applications require consistency or scalability, for the users that discover they have these requirements, SpiceDB's philosophy allows to avoid re-architecting their entire security critical authorization toolchain and instead simply adopt[zedtokens](https://docs.authzed.com/reference/zedtokens-and-zookies#what-is-a-zedtoken) only where appropriate.


In January of 2022, our co-founder Jake hit upon an idea: what if we could combine the best aspects of policies with the power of relationship-based access control. Shortly after, he[opened a proposal](https://github.com/authzed/spicedb/issues/386) for a scalable implementation of policy called "caveated relationships".


## What's the caveat?


Caveats are functions that are defined in a SpiceDB schema using[Google's CEL](https://github.com/google/cel-spec) . CEL is a powerful and familiar-looking expression language that is, importantly, not Turing-complete. This limitation is actually a very desirable feature for not only our use case but others such as[validating resources in Kubernetes](https://kubernetes.io/docs/tasks/extend-kubernetes/custom-resources/custom-resource-definitions/#validation) . By using CEL, SpiceDB tooling can guarantee performance by statically evaluating a caveat definition and bounding its execution time. This avoids a common pitfall for large deployments of policy engines where it's difficult to discover and avoid introducing slow policies. I can almost already hear someone shouting "What about WebAssembly?" off in the distance... but while it's a promising emerging technology, it actually suffers from the same issues plaguing policy languages: too much rope with which to[hang yourself](https://idioms.thefreedictionary.com/give+someone+just+enough+rope+to+hang+themselves) .


A caveat is defined in a SpiceDB schema:


zed


1


2


3


```text
caveat   only_on_tuesday (  day_of_week string )    {
day_of_week  =  =   'tuesday'
}


```


zed


1


2


3


```text
caveat   only_on_tuesday (  day_of_week string )    {
day_of_week  =  =   'tuesday'
}


```


Requests that check permissions (e.g.[CheckPermission](https://buf.build/authzed/api/docs/main:authzed.api.v1#authzed.api.v1.PermissionsService.CheckPermission) ,[LookupResources](https://buf.build/authzed/api/docs/main:authzed.api.v1#authzed.api.v1.PermissionsService.LookupResources) ,[LookupSubjects](https://buf.build/authzed/api/docs/main:authzed.api.v1#authzed.api.v1.PermissionsService.LookupSubjects) ) and[write relationships](https://buf.build/authzed/api/docs/main:authzed.api.v1#authzed.api.v1.PermissionsService.WriteRelationships) can provide a context which is a set of key-value pairs that are the named parameters passed to the caveats encountered while processing the request. Here's an example of a[CheckPermissionRequest](https://buf.build/authzed/api/docs/main:authzed.api.v1#authzed.api.v1.CheckPermissionRequest) providing such a context.


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


12


13


14


15


16


17


```text
CheckPermissionRequest {
resource: ObjectReference {
object_type: 'document'
object_id: 'somedocument'
}
permission: 'view'
subject: SubjectReference{
object: ObjectReference {
object_type: 'user'
object_id: 'fred'
}
}


context: google.protobuf.Struct {
'day_of_the_week': 'thursday'
}
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


12


13


14


15


16


17


```text
CheckPermissionRequest {
resource: ObjectReference {
object_type: 'document'
object_id: 'somedocument'
}
permission: 'view'
subject: SubjectReference{
object: ObjectReference {
object_type: 'user'
object_id: 'fred'
}
}


context: google.protobuf.Struct {
'day_of_the_week': 'thursday'
}
}


```


Requests that do not provide the required context yield a response that details which fields are missing to fully compute the permission. Admitting when a permission is unable to be determined is a testament to our dedication to safety by default -- remember we're here to develop security critical systems.


What's best of all is that caveats are designed leverage SpiceDB's distributed caching layer. SpiceDB caches the caveat expressions themselves, so that it can instantly determine what needs to be executed for a particular request. With this, our goal to maintaining properties of scale and latency is met for schemas using caveats.


## Boldly Go


The team at Authzed couldn't be happier about where the community has landed with caveats. There's truly nothing else out there like it. We've got a bunch of ideas for how to use them to power our own services like[SpiceDB Dedicated](https://authzed.com/pricing) , which we, of course, secure by using SpiceDB.


I want to especially thank all those that collaborated in the design of caveats. We had a ton of fun conversations where folks described their use cases and requirements and each and perspective was carefully considered throughout the process of designing caveats. Without you there would not be caveats.


After so much discussion and design, we're crazy excited to get this functionality into the hands of our users. Our next step is to add support for caveats into all of our development tooling, so keep your eyes out for an updated[playground](https://play.authzed.com/) . Caveats are a game-changer for those building secure, expressive permission systems that scale.


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)
- [A Primer on Modern Enterprise Authorization (AuthZ) Systems](https://authzed.com/blog/authz-primer)
- [Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
- [Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization)


On this page


- Taken ABAC
- What's our policy on Policy?
- What's the caveat?
- Boldly Go
- Additional Reading


## Related


[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Joey Schorr · Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)


[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Adora Nwodo · Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)
