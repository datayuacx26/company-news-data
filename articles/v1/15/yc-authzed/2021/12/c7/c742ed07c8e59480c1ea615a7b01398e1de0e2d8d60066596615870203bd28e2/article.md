---
schema_version: "1.0.0"
document_id: "c742ed07c8e59480c1ea615a7b01398e1de0e2d8d60066596615870203bd28e2"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/schema-language-patterns"
published_at: "2021-12-07T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:04:10.558196+00:00"
content_hash: "sha256:d7e9b2d7fbe61f66e41c38cefa51ae2eea1801e486492ee7f4be4355ffef6c70"
---

# Schema Language Patterns

Five months ago, we[launched](https://authzed.com/blog/defining-systems-lucidly/) a new[schema language](https://docs.authzed.com/reference/schema-lang) with the goal of reducing the complexity of designing applications' permissions systems and with the open sourcing of[SpiceDB](https://github.com/authzed/spicedb) , the number of users working with the schema language is larger than ever. It's an exciting time to participate in the community building around SpiceDB and to see the applications that people have been capable of modelling. With that growth, we've observed patterns emerging in schemas and compiled some useful techniques for other users to use as they begin configuring their own permission systems.


## Group membership


**Scenario** : Apply specific users or members of a group to a permission on an object type.


In this example, a group can have users as admins and as members. Both admins and members are considered to have membership in the group. A role can be applied to individual users and groups. All individually applied users as well as members for applied groups will have the` allowed` permission.


## Super-admin / site-wide permissions


**Scenario** : Given an organizational hierarchy of objects where (regular) admin users may exist for a single level of the hierarchy, apply permissions for a set of super-admin users that span across all levels of the hierarchy.


In lieu of adding a` super_admin` relation on every object that can be administered, add a root object to the hierarchy, in this example` platform` . Super admin users can be applied to` platform` and a relation to` platform` on top level objects. Admin permission on resources is then defined as the direct owner of the resource as well as through a traversal of the object hierarchy to the platform super admin.


## Synthetic relations


The[-> operator](https://docs.authzed.com/reference/schema-lang#--arrow) allows for walking a relation hierarchy in order to compute permissions. But what if a permission is intuitively thought of as multiple relation traversals? Synthetic relations can simulate multiple walks across permissions and relations.


Given the example hierarchy, portfolio can have folders, folders can have documents, we’d like a reader of a portfolio to also be able to read documents contained in its folders. The read on documents could be thought of as:


reader + parent_folder->reader + parent_folder->parent_portfolio->reader


The relation traversals can be modeled using intermediate, synthetic relations:


## Recursive permissions


**Scenario** : Given a nested set of objects, apply a permission on the ancestors to its descendant objects.


In this example, a folder can have users with read permission. Additionally, users that can read the parent folder can also read the current folder. Checking read permission on a folder will recursively consider these relations as the answer is computed.


## Get involved


We’re always looking to the community for ways to improve both the developer experience for writing schema configs as well as making the config language more powerful. In fact, several of these patterns can be credited to insightful questions initially asked by users in our[Discord channel](https://authzed.com/discord) .


If you’re just getting started, here are some interesting existing issues to help get acquainted with schemas and of course you can[create a new issue](https://github.com/authzed/spicedb/issues/new/choose) for an improvement you’d like to see.


- Add 'public' keyword/type:[https://github.com/authzed/spicedb/issues/1](https://github.com/authzed/spicedb/issues/1)
- Implement the Language Server Protocol:[https://github.com/authzed/spicedb/issues/179](https://github.com/authzed/spicedb/issues/179)
- Add support for mixins/templates/inheritance in schema:[https://github.com/authzed/spicedb/issues/224](https://github.com/authzed/spicedb/issues/224)
- CLI command for validating schemas:[https://github.com/authzed/spicedb/issues/290](https://github.com/authzed/spicedb/issues/290)


Join us on[Discord](https://authzed.com/discord) to get feedback on your schema config and to discuss the ongoing development of SpiceDB.


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)
- [A Primer on Modern Enterprise Authorization (AuthZ) Systems](https://authzed.com/blog/authz-primer)
- [Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
- [Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization)


On this page


- Group membership
- Super-admin / site-wide permissions
- Synthetic relations
- Recursive permissions
- Get involved
- Additional Reading


## Related


[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)[Engineering Introducing the SpiceDB Playground Assistant We've added an AI assistant to the SpiceDB Playground. It writes and edits your schema, generates relationship data and assertions to test it, runs permission checks, and explains exactly why a check was granted or denied. Joey Schorr · Jul 27, 2026 · 5 min](https://authzed.com/blog/introducing-spicedb-playground-ai-assistant)


[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)[AI How SpiceDB Secures Authorization for AI AI agents don't make authorization decisions. SpiceDB does. Here's how relationship graphs, consistency guarantees, caveats, and explicit delegation keep every agent action provably scoped. Adora Nwodo · Jul 27, 2026 · 8 min](https://authzed.com/blog/spicedb-secures-authorization-for-ai)


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)
