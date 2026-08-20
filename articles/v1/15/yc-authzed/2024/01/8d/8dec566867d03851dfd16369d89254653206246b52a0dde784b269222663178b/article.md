---
schema_version: "1.0.0"
document_id: "8dec566867d03851dfd16369d89254653206246b52a0dde784b269222663178b"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/authentication-vs-authorization"
published_at: "2024-01-10T10:04:18.029+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T22:26:19.240569+00:00"
content_hash: "sha256:a1dcbac8e1679e6742511d3f6734ea3562612aa393f3dcc38b5c8a8ee796ba1d"
---

# Authentication vs Authorization

How two foundations of secure access have evolved and where they’re headed.


## Authn vs Authz, a primer


Imagine you want to go on a trip to another country. Among the essentials you’ll need are a passport and a visa. Your passport tells another country who you are and gives them the information they need to uniquely identify you. A visa, on the other hand, gives you permission to enter the country you’re visiting.


No, you haven’t mistakenly visited a travel blog – this analogy is useful when it comes to talking about the differences between authentication and authorization, two key principles of securing our possessions (and our information) that date back 6,000 years to when locks were invented. Put simply, **authentication is the passport, and authorization is the visa** . To break it down more technically, authentication is the way an entity verifies someone is who they say they are. Authorization, then, allows access to an asset once a person’s identity has been verified.


Let’s look at an example in computing. Users of a highly used app may be verified and authorized billions of times per day. And, if it’s done right, you probably don’t even think about it. Authentication happens after you’ve provided an app with something that verifies your identity, and authorization relies on the app to understand your relationship to the resources it can grant access to:


Authentication and authorization work together so seamlessly in the real world, that it’s easy to understand why they get confused so OFTEN – I mean, the words even share a common root, “auth” (from the Greek word for “self”). And you really can’t have one without the other:


## Authentication


### The three principles of authentication


To take this beyond the app example above, authentication in computing means verifying three different types of entities: user, process, or machine. To authenticate a user, we can use one or a combination of three key factors:


- **Knowledge** : Something a person knows – i.e. a password/username, a passcode or PIN, or a security question
- **Ownership** : Physical token or something a person owns – i.e. a Yubikey, hardware capable of an encrypted handshake (like ACME for Apple devices), or Google Authenticator
- **Inherence** : A vital piece of something a user is or does – i.e. biometrics or keystrokes. (These might be more familiar to you in how they translate into real-word use: Voice recognition and fingerprint identification are good examples.)


Whenever you sign into social media, your email, or any Web-based application, you are using authentication to prove you are who you say you are.


### The history of authentication


Authentication started in the 1960’s (60 years ago!) with the first passwords in databases. From there it moved to passwords with a hash, and eventually, encryption. Modern encryption has been around since the 1970’s with RSA asymmetric encryption. Today, we have complex authentication technologies that include multi-factor authentication, password managers, and web authentication, which have become a daily part of users’ experience across the internet.


### Timeline of authentication milestones


*\[Timeline is based on an excellent piece - “[Digital authentication: The past, present and uncertain future of the keys to online identity](https://www.geekwire.com/2018/digital-authentication-human-beings-history-trust/) ” on GeekWire, with a few new additions by the authors\]*


We don’t know where “the passport” of authentication will take us in the future but as technology continues to advance, the ability to identify an individual user will get more complicated. This rings true for all Web applications and services, but even more so for highly secure or zero trust environments such as government platforms and healthcare data. These platforms, coincidentally, also require accurate authorization technology that gets users what they need, when they need it, in an efficient way. Enter Authorization, a.k.a. the visa.


## Authorization


As our digital footprint has grown, experts have developed new access control and permissions management models to meet the needs of feature hungry teams. Since the advent of the original Access Control List Model (ACL) in the 1960s – similar to an “invite only” guest list – we have designed increasingly abstract authorization systems.


In computing, authorization is when one entity grants permission to another entity to engage with a resource within a set of boundaries. Authorization has become increasingly difficult for application developers, often causing performance issues when deployed at scale and blocking feature development.


### Timeline of authorization milestones


Today, online experiences are ubiquitous and plentiful, and collaboration can take place just about anywhere and anytime, using any number of tools. That means that authorization needs to advance quickly to keep up with all of the ways that people are collaborating online together around the world. Moving forward, interest will likely trend toward dynamic, context-aware systems that can adjust permissions in real time. The exploration of decentralized models for authorization that integrate concepts from ReBAC and systems like Zanzibar suggests a future with more secure, efficient, and flexible access control mechanisms.


Authentication vs Authorization comparison


Authentication (authn) Authorization (authz)


Verifying user identity Determining user permissions (the level of access to a resource)


Passwords, 2FA, Multi-factor authentication, Captcha, Biometrics


Role-based Access Control (RBAC), Attribute-based Access Control (ABAC), Relationship-based Access Control (ReBAC)


Dex, LDAP, AD, Ping, Okta SpiceDB, Google Zanzibar, OPA, authZ libraries


As technology develops, so will both authentication and authorization. We at AuthZed are excited to be on the forefront of the authorization space, bringing concepts from Google’s Zanzibar to the masses with SpiceDB.


SpiceDB is a database for your applications permissions that combines the two things that have traditionally been incongruous: flexible authorization modeling and scalable, high performance. It can unlock product features and usage growth that was not previously possible because it allows your product to incorporate complex permissions while also accommodating high usage.


You can[try modeling a permissions system here](https://play.authzed.com/) and get started with fine-grained access control across all of your environments and applications.


To explore more about SpiceDB, checkout these resources:


- [SpiceDB Documentation](https://authzed.com/docs)
- [Customer Stories](https://authzed.com/customers)


## FAQ


### What are the basics of AuthN versus AuthZ?


Authentication (AuthN) answers "who is this?" while authorization (AuthZ) answers "what can they do?" AuthN verifies identity through mechanisms like passwords, passkeys, or FIDO2, typically via OpenID Connect. AuthZ evaluates whether a verified identity may perform a specific action on a specific resource. Conflating the two is a common source of security vulnerabilities, which is why[keeping them architecturally separate](https://authzed.com/blog/authentication-vs-authorization) matters.


Once a user authenticates, the application takes that stable identity and asks an authorization system whether access is permitted.[SpiceDB](https://authzed.com/spicedb) handles that second step using[relationship-based access control](https://authzed.com/docs/spicedb/concepts/relationships) , where permissions are computed by evaluating graph connections between objects. You define those rules in a[schema](https://authzed.com/docs/spicedb/concepts/schema) , and SpiceDB resolves them at query time.


For teams that need contextual rules on top of relationships,[Caveats](https://authzed.com/docs/spicedb/concepts/caveats) support attribute-based conditions without abandoning the ReBAC model.[Consistency guarantees](https://authzed.com/docs/spicedb/concepts/consistency) via ZedTokens prevent stale permission checks, and the[Watch API](https://authzed.com/docs/spicedb/concepts/watch) streams relationship changes for audit and cache invalidation.


### What is the difference between AuthN and AuthZ in access control?


Authentication confirms who someone is. Authorization decides what they can do. These sound related, and they are, but conflating them creates real security gaps. A stolen access token, for instance, doesn't prove identity — it only grants delegated API access under OAuth 2.0. Identity comes from an OIDC ID token. Getting this wrong at the design stage tends to produce systems where UI-layer checks substitute for real enforcement, or where role assignments do too much work.


Once identity is established, the harder problem is modeling permissions accurately. RBAC works for simple role structures, but breaks down when permissions depend on relationships between objects. That's where[relationship-based access control](https://authzed.com/docs) becomes relevant.[SpiceDB](https://authzed.com/docs/spicedb/concepts/schema) , built by AuthZed, lets teams define object types, relations, and computed permissions explicitly, then query them at request time through a consistent API.


In practice, AuthZed handles authorization after your identity provider handles authentication. The subject ID from your IdP becomes the principal in[CheckPermission](https://authzed.com/docs/spicedb/modeling/protecting-a-list-endpoint) calls. For list endpoints,[LookupResources](https://authzed.com/docs/spicedb/modeling/protecting-a-list-endpoint) pre-filters results to what a user can actually see.[ZedTokens](https://authzed.com/docs/spicedb/concepts/consistency) handle read-after-write consistency so permission changes take effect immediately where it matters.


### What roles do AuthN and AuthZ play in determining access within a system?


Authentication confirms who is making a request, while authorization determines what that identity is permitted to do. These are distinct responsibilities that should never be collapsed into one mechanism. OIDC builds an identity layer on top of OAuth 2.0, producing a stable subject identifier (the` sub` claim) that flows into your authorization system. OAuth 2.0 itself is strictly an authorization framework and does not authenticate users on its own.


Once identity is established, the PDP/PEP pattern governs runtime decisions. The Policy Enforcement Point intercepts the request; the Policy Decision Point evaluates policy and returns allow or deny.[AuthZed](https://authzed.com/spicedb) operates as that PDP, using[SpiceDB's schema language](https://authzed.com/docs/spicedb/concepts/schema) to model object types, relations, and computed permissions through set operations and graph traversal.[Relationship tuples](https://authzed.com/docs/spicedb/concepts/zanzibar) store subject-relation-object facts, and[caveats](https://authzed.com/docs/spicedb/concepts/caveats) layer in contextual attributes without sacrificing performance.


At runtime, your service extracts the OIDC` sub` , maps it to a[SpiceDB subject](https://authzed.com/docs/spicedb/modeling/representing-users) , and calls CheckPermission or LookupResources.[ZedTokens](https://authzed.com/docs/spicedb/concepts/consistency) ensure decisions stay time-consistent and avoid stale-read problems. Avoid embedding permissions in JWTs; that approach creates revocation gaps and policy drift that real-time checks against SpiceDB eliminate.


Originally published January 10, 2024: Added TL;DR, converted timeline images to accessible text tables, added FAQs section and internal links


On this page


- Authn vs Authz, a primer
- Authentication
- The three principles of authentication
- The history of authentication
- Timeline of authentication milestones
- Authorization
- Timeline of authorization milestones
- FAQ
- What are the basics of AuthN versus AuthZ?
- What is the difference between AuthN and AuthZ in access control?
- What roles do AuthN and AuthZ play in determining access within a system?


## Related


[Industry Financial Services and RAG: Join us at Open Source in Finance Forum! The financial industry is leading the way on RAG—and for good reason. Join us at OSFF London on June 25 to see fine-grained authorization for RAG embeddings in action. Jun 17, 2026 · 2 min](https://authzed.com/blog/financial-services-rag-osff-london)[Industry Financial Services and RAG: Join us at Open Source in Finance Forum! The financial industry is leading the way on RAG—and for good reason. Join us at OSFF London on June 25 to see fine-grained authorization for RAG embeddings in action. Cormac Foster · Jun 17, 2026 · 2 min](https://authzed.com/blog/financial-services-rag-osff-london)


[Company Identiverse 2026: The Conference about Identity's Authorization Problem AuthZed is sponsoring Identiverse 2026, where three of the four featured summits point to the same underlying issue: the assumptions that underpinned IAM for two decades have collapsed, exposing authorization as the missing layer. Jun 10, 2026 · 3 min](https://authzed.com/blog/identiverse-2026-identity-authorization-problem)[Company Identiverse 2026: The Conference about Identity's Authorization Problem AuthZed is sponsoring Identiverse 2026, where three of the four featured summits point to the same underlying issue: the assumptions that underpinned IAM for two decades have collapsed, exposing authorization as the missing layer. Cormac Foster · Jun 10, 2026 · 3 min](https://authzed.com/blog/identiverse-2026-identity-authorization-problem)


[Industry Calculate the Authorization Tax: Quantifying the Costs of Home-Grown AuthZ Everyone is paying for authorization. Here are five inputs to help you quantify your Authorization Tax—and two paths to reduce it. May 18, 2026 · 6 min](https://authzed.com/blog/calculate-the-authorization-tax)[Industry Calculate the Authorization Tax: Quantifying the Costs of Home-Grown AuthZ Everyone is paying for authorization. Here are five inputs to help you quantify your Authorization Tax—and two paths to reduce it. Cormac Foster · May 18, 2026 · 6 min](https://authzed.com/blog/calculate-the-authorization-tax)
