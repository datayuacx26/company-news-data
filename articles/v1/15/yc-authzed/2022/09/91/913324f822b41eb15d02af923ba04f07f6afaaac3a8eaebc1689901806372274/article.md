---
schema_version: "1.0.0"
document_id: "913324f822b41eb15d02af923ba04f07f6afaaac3a8eaebc1689901806372274"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/lookup-subjects"
published_at: "2022-09-20T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:dc4741598973bdb75c9658c887dc89778f9bc334dc57ed4fe2c3e7f52d0710a3"
---

# LookupSubjects and SpiceDB v1.12.0

July and August have been busy months for everyone at Authzed!


In addition to the announcement of the[SpiceDB Operator](https://authzed.com/blog/open-source-spicedb-operator/) and the new[WASM based playground](https://authzed.com/blog/some-assembly-required/) ,[SpiceDB](https://github.com/authzed/spicedb) itself received a significant update in the form of[version 1.12](https://github.com/authzed/spicedb/releases/tag/v1.12.0)


Version 1.12 of SpiceDB introduces a number of improvements, the largest of which is a **new API** called[LookupSubjects](https://buf.build/authzed/api/docs/main:authzed.api.v1#authzed.api.v1.PermissionsService.LookupSubjects) .


## When to use LookupSubjects


The LookupSubjects API provides the ability to find all subjects reachable for a particular resource and permission, acting as a reverse of our previously released[LookupResources](https://buf.build/authzed/api/docs/main:authzed.api.v1#authzed.api.v1.PermissionsService.LookupResources) API.


LookupSubjects is most useful for any user interface or auditing code that needs to determine the full set of users, tokens, or other kinds of subjects which have a particular permission on a resource.


Unlike the[ExpandPermissionTree](https://buf.build/authzed/api/docs/main:authzed.api.v1#authzed.api.v1.PermissionsService.ExpandPermissionTree) API, LookupSubjects is automatically recursive: it will walk through other kinds of objects, such as groups, to determine the fully resolved set of subjects that have the requested permission. Also unlike the Expand API, LookupSubjects is **streaming** : it will return results as soon as it can to the caller.


For example, a "who can view this document" panel might use LookupSubjects to determine the full set of users that can view the document, including those who gain access through groups, or a parent organization:


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


```text
LookupSubjectsRequest {
Resource: {
ObjectType: "document"
ObjectId: "thedocument"
}


Permission: "view"


SubjectObjectType: "user"
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


```text
LookupSubjectsRequest {
Resource: {
ObjectType: "document"
ObjectId: "thedocument"
}


Permission: "view"


SubjectObjectType: "user"
}


```


## How LookupSubjects works


In my[ACL Filtering in Authzed](https://authzed.com/blog/acl-filtering-in-authzed/) blog post, I discussed how LookupResources (the inverse to LookupSubjects) functions by walking the permission graph in a "backwards" fashion.


LookupSubjects functions in much the same way, but in the same direction as a[Check](https://authzed.com/blog/check-it-out/) : starting at the permission requested and the resource type, SpiceDB walks **all** branches of the permission graph, outward, until it reaches any subjects of the requested type:


We chose to add the LookupSubjects API for two reasons:


1.


The ability to find all accessible subjects is incredibly important for auditing and other kinds of UI and was difficult to implement for users: previously, this kind of recursive walk and processing would have to be done by clients themselves via multiple Expand calls, which we recognized as a significant hurdle to adoption.


2.


LookupSubjects is one of the two APIs (the other being[ReachableResources](https://github.com/authzed/spicedb/blob/main/proto/internal/dispatch/v1/dispatch.proto#L81) ) necessary to support the[WatchResources API](https://github.com/authzed/spicedb/issues/207) , a new API currently being implemented which will stream a set of permission changes whenever a relationship is updated in SpiceDB. This new API will allow users of SpiceDB to store and update the set of accessible resources for a particular kind of subject, and use that set for filtering via RDBMSes, Elastic, and other external querying systems.


## Test it now


Want to test LookupSubjects now? Download[SpiceDB](https://github.com/authzed/spicedb) and then run a[zed](https://github.com/authzed/zed) call:


sh


1


```text
zed permission lookup-subjects document somedocument view user


```


sh


1


```text
zed permission lookup-subjects document somedocument view user


```


Have a question? Check out the[Discord](https://authzed.com/discord) , where we and the community are discussing all things SpiceDB


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)
- [A Primer on Modern Enterprise Authorization (AuthZ) Systems](https://authzed.com/blog/authz-primer)
- [Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
- [Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization)


On this page


- When to use LookupSubjects
- How LookupSubjects works
- Test it now
- Additional Reading


## Related


[Engineering SpiceDB v1.39 Released The SpiceDB v1.39 release delivers enhanced monitoring through native histograms, smarter health checks, and transaction metadata improvement. Dec 20, 2024 · 3 min](https://authzed.com/blog/spicedb-v1-39-released)[Engineering SpiceDB v1.39 Released The SpiceDB v1.39 release delivers enhanced monitoring through native histograms, smarter health checks, and transaction metadata improvement. Sam Kim · Dec 20, 2024 · 3 min](https://authzed.com/blog/spicedb-v1-39-released)


[Engineering Introducing: Fine-Grained Access Management The systems we build at AuthZed are the direct result of feedback from our community and customers. Because security is the core of our flagship product, SpiceDB, we take feedback on this topic very seriously. We’ve heard you, and today we’re proud to introduce a better way to secure AuthZed customers’ client applications accessing the SpiceDB API: **Fine-Grained Access Management** (FGAM). Apr 27, 2023 · 4 min](https://authzed.com/blog/introducing-fine-grained-access-management)[Engineering Introducing: Fine-Grained Access Management The systems we build at AuthZed are the direct result of feedback from our community and customers. Because security is the core of our flagship product, SpiceDB, we take feedback on this topic very seriously. We’ve heard you, and today we’re proud to introduce a better way to secure AuthZed customers’ client applications accessing the SpiceDB API: **Fine-Grained Access Management** (FGAM). Victor Roldan Betancort · Apr 27, 2023 · 4 min](https://authzed.com/blog/introducing-fine-grained-access-management)


[Engineering Automatic release notification in SpiceDB and zed The engineering behind notifying users about updates to SpiceDB and zed May 18, 2022 · 9 min](https://authzed.com/blog/automatic-release-notification)[Engineering Automatic release notification in SpiceDB and zed The engineering behind notifying users about updates to SpiceDB and zed Joey Schorr · May 18, 2022 · 9 min](https://authzed.com/blog/automatic-release-notification)
