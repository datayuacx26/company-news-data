---
schema_version: "1.0.0"
document_id: "89d49723dc1ecbeecb8ea61c4139936372c751837b03146e22d32c4ff76df4e8"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/why-model-users"
published_at: "2022-01-05T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:04:10.558196+00:00"
content_hash: "sha256:7afb8261ff897d51a509bcd339680cf493f9033b3d5e3c89a10bc89a8e82f50c"
---

# Why model my users in SpiceDB?

SpiceDB is an open source database system for managing application permissions. It is inspired by Google's internal authorization as described by the[Zanzibar paper](https://authzed.com/blog/what-is-zanzibar/) . In both systems, relationships between resources and users are stored and queried by applications in order to determine whether or not a user should be allowed to access a resource. Newcomers to SpiceDB often pose the question: how does SpiceDB differ from the paper? We briefly cover each difference[in our documentation](https://docs.authzed.com/reference/glossary#notable-differences) , but today I'd like to talk about one specific difference: how users are modelled in SpiceDB.


Before jumping into how SpiceDB works, let's briefly run through how the system at Google functions. Google has their own user service,[GAIA](https://developers.google.com/issue-tracker/concepts/access-control#users) , which provides guarantees to their authorization service, Zanzibar. Most importantly, GAIA guarantees a globally unique, stable integer for every user or program that interacts with Google's software. As an aside, you can find GAIA IDs present in the data Google exchanges during an OAuth flow and occasionally in public-facing URLs for Google products. The assumption that users can always be represented by a unique integer is pervasive in the design of Zanzibar. As a result, Zanzibar treats users as a first-class concept. In their simplest case, relationships take the form of three values:` (resource, relation, user)` such as` (document:readme, viewer, 1234567)` .


When designing SpiceDB, we knew that organizations outside Google often do not have a ubiquitous user system like GAIA. Today, most organizations leverage various[Identity Providers](https://en.wikipedia.org/wiki/Identity_provider) that use protocols such as OIDC, SCIM, and SAML in order to bridge their user systems into their applications. However, each organization structures and interacts with their Identity Providers in different ways. This makes it infeasible to assume any details of the authentication system(s) that could simplify the design of SpiceDB.


Ultimately, there were three options we considered: take a hard stance on how users should be represented, build on the lowest common denominator between all of the possible ways that users could be represented, or allow developers to model their own users. Taking a hard stance would limit adoption and potentially require us to build our own Identity system. Building on the lowest common denominator would limit functionality and might not satisfy the requirements of those adopting SpiceDB. Allowing developers to model their own users pushes complexity to the developer, but makes it possible to satisfy any and all of their requirements.


We chose to move forward with the last option: users are not a special case built into SpiceDB, but rather Objects like everything else. This allows for relationships that take the form of` (resource, relation, subject)` , such as` (document:readme, viewer, user:1234567)` . This might appear very similar to Zanzibar, but allows for interesting ideas like having different types of subjects (e.g. users, personal access tokens, API keys).


What's more, the freedom to model your own users allows for schemas that are not only singular user types, but a graph of various interrelated types. This can be used implement all kinds of behavior, including making users agnostic to the identity provider or supporting various identity providers *simultaneously* .


In the example below, we create a top-level user type that owns accounts of various other types: GitHub and LDAP. By defining the` view` permission on documents as` viewer + viewer->owned_account` , it allows for checking access not only for the user type, but also directly with GitHub and LDAP accounts. You can see these checks being exercised in the Assert tab below.


By empowering developers to model their own user types, we've created a more flexible system. We believe that this is the proper trade-off for SpiceDB and the natural conclusion if your design goals are to not impose any restrictions on application design or build identity products. While we've been using our own custom subject types for the[AuthZed Serverless](https://app.authzed.com/) , we're still impressed by some of the designs that have been shared with the community. As more community collaboration occurs, it will be interesting to see what patterns emerge.


Want to learn more about the SpiceDB schema language? You can[read the reference documentation](https://docs.authzed.com/reference/schema-lang) ,[read our previous blog post](https://authzed.com/blog/unveiling-wildcard-permissions-in-spicedb/) on allowing all users to have access to a public resource simultaneously, or[join Discord](https://authzed.com/discord) to see and discuss what schemas folks are developing.


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)
- [A Primer on Modern Enterprise Authorization (AuthZ) Systems](https://authzed.com/blog/authz-primer)
- [Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
- [Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization)


On this page


- Additional Reading


## Related


[Engineering Consistency is the Key to Performance and Safety Both SpiceDB and Zanzibar combine performance, scalability, and correctness into one manageable, global authorization solution. Strong consistency is key to ensuring correctness, but caching is necessary for performance. Consistency and caching are often diametrically opposed so how do SpiceDB and Zanzibar solve this problem? With a few key realizations around staleness, when consistency is necessary and how the two interact. Sep 5, 2024 · 7 min](https://authzed.com/blog/consistency-is-the-key-to-performance-and-safety)[Engineering Consistency is the Key to Performance and Safety Both SpiceDB and Zanzibar combine performance, scalability, and correctness into one manageable, global authorization solution. Strong consistency is key to ensuring correctness, but caching is necessary for performance. Consistency and caching are often diametrically opposed so how do SpiceDB and Zanzibar solve this problem? With a few key realizations around staleness, when consistency is necessary and how the two interact. Joey Schorr · Sep 5, 2024 · 7 min](https://authzed.com/blog/consistency-is-the-key-to-performance-and-safety)


[Engineering Fine-Grained Access Control: Can You Go Too Fine? In this article, AuthZed's co-founder and CEO Jake Moshenko addresses trade-offs to consider when modeling your permissions, when to consider fine-grained access control, costs associated with fine-grained access control, how to strike a balance, and main takeaways. Mar 5, 2024 · 8 min](https://authzed.com/blog/fine-grained-access-control)[Engineering Fine-Grained Access Control: Can You Go Too Fine? In this article, AuthZed's co-founder and CEO Jake Moshenko addresses trade-offs to consider when modeling your permissions, when to consider fine-grained access control, costs associated with fine-grained access control, how to strike a balance, and main takeaways. Jake Moshenko · Mar 5, 2024 · 8 min](https://authzed.com/blog/fine-grained-access-control)


[Engineering Announcing AuthZed Materialize AuthZed Materialize has entered Early Access. AuthZed Materialize is a new service that provides two major benefits: accelerated SpiceDB API responses and the ability to efficiently stream access changes to other systems. Feb 21, 2024 · 3 min](https://authzed.com/blog/materialize-early-access)[Engineering Announcing AuthZed Materialize AuthZed Materialize has entered Early Access. AuthZed Materialize is a new service that provides two major benefits: accelerated SpiceDB API responses and the ability to efficiently stream access changes to other systems. Jimmy Zelinskie · Feb 21, 2024 · 3 min](https://authzed.com/blog/materialize-early-access)
