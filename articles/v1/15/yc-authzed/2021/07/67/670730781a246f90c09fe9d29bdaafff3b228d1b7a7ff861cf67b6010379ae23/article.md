---
schema_version: "1.0.0"
document_id: "670730781a246f90c09fe9d29bdaafff3b228d1b7a7ff861cf67b6010379ae23"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/defining-systems-lucidly"
published_at: "2021-07-14T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:04:52.597445+00:00"
content_hash: "sha256:7dbd93bd4bad389e17e2386eec8ad7034ca944ad4858ebd4b23484be820a219f"
---

# Defining Systems Lucidly

Any complex system is only useful if it can be configured and maintained in a reasonable manner by those using it. At Authzed, we are building the permission system as a service for the world, modeled on the[Zanzibar](https://authzed.com/blog/what-is-zanzibar/) service as originally described by Google in the[Zanzibar paper](https://research.google/pubs/pub48190/) . While Zanzibar and Authzed are simple to **use** , the complexity of configuring the[object type definitions](https://docs.authzed.com/concepts/terminology#object) (called "namespaces" in Zanzibar) associated with a permissions system has left something to be desired. This is primarily due to the configuration being based on[Protocol Buffers](https://developers.google.com/protocol-buffers) , with the configuration "language" merely being the text form of the underlying proto messages. Recognizing that this experience for developers has shown room for improvement, we are happy today to announce the release of the new Authzed configuration language.


## Reducing complexity


The primary goal of the new Authzed configuration language is to reduce complexity of configuration. As an example, imagine a simple permissions system consisting of` document` s, which have roles` reader` and` writer` , permissions` read` and` write` , and a type of` user` .


In the new configuration language, this can be represented concisely and clearly:


In contrast, in the previous Zanzibar-inspired configuration, this would consist of two configuration "files":


proto


1


```text
name: "user"


```


proto


1


```text
name: "user"


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


18


19


20


21


22


23


24


25


26


27


28


29


30


31


32


33


34


35


36


37


38


39


40


```text
name: "document"


relation {
name: "reader"
}


relation {
name: "writer"
}


relation {
name: "write"
userset_rewrite {
union {
child {
computed_userset {
relation: "writer"
}
}
}
}
}


relation {
name: "read"
userset_rewrite {
union {
child {
computed_userset {
relation: "writer"
}
}
child {
computed_userset {
relation: "reader"
}
}
}
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


18


19


20


21


22


23


24


25


26


27


28


29


30


31


32


33


34


35


36


37


38


39


40


```text
name: "document"


relation {
name: "reader"
}


relation {
name: "writer"
}


relation {
name: "write"
userset_rewrite {
union {
child {
computed_userset {
relation: "writer"
}
}
}
}
}


relation {
name: "read"
userset_rewrite {
union {
child {
computed_userset {
relation: "writer"
}
}
child {
computed_userset {
relation: "reader"
}
}
}
}
}


```


As we can see, the above configuration is verbose, confusing (what even is a` computed_userset` ?), and broken into two distinct blocks, despite them being related and, despite being **far** more concise, the config language has exactly reproduced the above configuration (in fact, it is translated directly into it!).


The new language allows for easy and readable configuration, without any loss of power or expression.


## Permitting a relationship


Two interesting items to note in the config language are the separation of concerns between` relation` and` permission` and the addition of allowed subject types.


We've had a lot of developer feedback that issuing[Check](https://docs.authzed.com/v0/api#aclservicecheck) calls on a relation to represent a permission can be... somewhat confusing. To rectify this issue, the configuration now has a distinction between` relation` 's, which relate one Object to another (or a Subject), and` permission` s, which are *computed* from one or more relations (or permissions), and represent distinct authorization decisions. To reinforce this distinction, we recommend using verbs for permissions (` read` ,` write` ) and nouns for relations (` reader` ,` writer` ), to indicate that relations define a relationship (such as a **role** in[RBAC](https://docs.authzed.com/concepts/authz#what-is-rbac) ), while permissions represent, well, a permission!


The addition of allowed subject types, on the other hand, provides two major benefits: developer safety and data for driving new APIs.


Developer safety is enhanced by the API verifying that subjects written to a` relation` match those specified in the schema: If a subject does not match, the write call to the Authzed API will now fail at runtime, ensuring that relationships only exist between objects and subjects that are expected.


This data is also used by the[Lookup API](https://authzed.com/blog/acl-filtering-in-authzed/) to assist in its walk of the permissions graph, ensuring that Authzed can provide performant ACL filtering without having to infer the kinds of relationships that might exist between objects.


## Try it now


The configuration language is now fully supported in the[Authzed Playground](https://play.authzed.com/) , as well as the[Authzed API](https://docs.authzed.com/reference/api) . Try it today,[read the documentation](https://docs.authzed.com/reference/schema-lang) , or[discuss in Discord](https://authzed.com/discord) . We hope it proves a far better configuration experience!


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)
- [A Primer on Modern Enterprise Authorization (AuthZ) Systems](https://authzed.com/blog/authz-primer)
- [Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
- [Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization)


On this page


- Reducing complexity
- Permitting a relationship
- Try it now
- Additional Reading


## Related


[Engineering SpiceDB v1.39 Released The SpiceDB v1.39 release delivers enhanced monitoring through native histograms, smarter health checks, and transaction metadata improvement. Dec 20, 2024 · 3 min](https://authzed.com/blog/spicedb-v1-39-released)[Engineering SpiceDB v1.39 Released The SpiceDB v1.39 release delivers enhanced monitoring through native histograms, smarter health checks, and transaction metadata improvement. Sam Kim · Dec 20, 2024 · 3 min](https://authzed.com/blog/spicedb-v1-39-released)


[Engineering LookupSubjects and SpiceDB v1.12.0 Product Updates for July & August Sep 20, 2022 · 3 min](https://authzed.com/blog/lookup-subjects)[Engineering LookupSubjects and SpiceDB v1.12.0 Product Updates for July & August Joey Schorr · Sep 20, 2022 · 3 min](https://authzed.com/blog/lookup-subjects)


[Engineering Automatic release notification in SpiceDB and zed The engineering behind notifying users about updates to SpiceDB and zed May 18, 2022 · 9 min](https://authzed.com/blog/automatic-release-notification)[Engineering Automatic release notification in SpiceDB and zed The engineering behind notifying users about updates to SpiceDB and zed Joey Schorr · May 18, 2022 · 9 min](https://authzed.com/blog/automatic-release-notification)
