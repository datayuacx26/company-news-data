---
schema_version: "1.0.0"
document_id: "87cbb34eae27e6d2d2d8e7744d0bfc8308fc09e9cdf8c1b2fa756b4f5a1e2ca1"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/exploring-rebac"
published_at: "2021-03-03T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:c262fc9b776a1fec9ffc175680af94db18c0551d84117c934d7b8cd3fd67ee05"
---

# Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System

## Setting the scene: AuthZ vs AuthN


Have you ever chatted with a fellow developer about an application's permission system and quickly realized you're also talking about its login system? It's rather unfortunate, but these two entirely distinct systems often get merged together simply because their formal names start with the same four letters: **AUTH** .


> Authentication ("authN" or "identity") is who you are
>
>
> Authorization ("authZ" or "permissions" or "access control") is what you're allowed to do


This is no amateur mistake. Even[major web frameworks](https://docs.djangoproject.com/en/3.1/ref/contrib/auth/) bundle these concepts together out of convenience.


Because so many applications need to support users from inception, identity becomes vital for developers to understand on day one. However, building a robust permission system can usually be deferred until users start demanding it. When requests for[fine grained access control](https://authzed.com/blog/fine-grained-access-control) inevitably start pouring in, they often come alongside feature requests for integrations with various[Identity Providers](https://en.wikipedia.org/wiki/Identity_provider) . This makes it seem natural to assume that the permission systems should be direct integrations with the primitives that the Identity Providers expose. However, the *authorization* functionality that is often found in most *authentication* systems is generally overly simplistic and the resulting permissions systems that are built on top are usually fragile and error prone.


This is the last thing you want to hear when discussing software that determines whether or not a user has access to sensitive content. If you're thinking "that's only if you work in a domain, like healthcare or government, where you know sophisticated access control is required", you should consider that even in simple use cases you'll likely be iterating on your design, which gives you ample opportunities to introduce bugs that manifest themselves as security vulnerabilities.


## LDAP flips Conway's Law on its head


> Any organization that designs a system (defined broadly) will produce a design whose structure is a copy of the organization's communication structure.
>
>
> — Melvin E. Conway


Conway's Law describes how the architecture of software is a reflection of the organization of the people who built it. For example, software components that are decoupled, but belong to the same application are often separate only because they were built by separate teams.


When permission systems are based on Identity Providers, this law is entirely reversed.


If an organization of people cannot use any software because everything, for example, only supports a structure that can be modeled by LDAP Groups, it forces the people to reorganize into something that can be modeled by LDAP Groups.


This may be viable for organizations like businesses (the reorgs will continue until morale improves!), but obviously not all software can demand that their users reorganize just to use their product.


## Groups, Scopes, Claims don't answer the question


LDAP Groups, OAuth Scopes, SAML Claims,[JWT Claims](https://authzed.com/blog/pitfalls-of-jwt-authorization) : all a rose by any other name. These concepts all represent the same kind of data: an *[attribute](https://docs.authzed.com/authz/abac)* that is stored on a user, indicating something about that user. Attributes are useful: they can provide context about a user (such as an object they can access, or a role that the user has), which makes such a system, in theory, a reasonable solution to determining what a user can access. However, in practice, developers realize that this isn't quite so obvious once they have started working with this data.


Software that relies on these concepts for permissions all struggle in the same core principle: how they choose to interpret and apply significance to the presence of an attribute:


- If a user has both the "admin" and the "banned" attribute, what is the correct action?
- Should admins be able to ignore bans or was the employee just fired from the company?
- What about attributes that imply other attributes?
- Does being "admin" also imply "write" access to this resource?


You can see how this quickly gets out of hand. And once it's been decided how attributes should be properly interpreted, it's time to audit every other application and make sure they interpret the attribute the exact same way or else you might have a security problem!


Now, there is nothing fundamentally wrong with attribute-based permission systems. In fact, mature permission systems are almost always a fusion of ideas from \[various models\] based on the requirements at hand. In this case, how the attributes from *authentication* systems manifest themselves when they become the foundation of a permission system is the problem. This is because attributes can only state facts about an identity. But what we really need is the answer to the question "can this subject take this action on this resource?".


## Intro to Relationship-Based Access Control (ReBAC)


While identity is required to ask the question "Does this` subject` have permission to do this` action` to this` object` ?", it is not the *only* variable. Identity sits alongside the *action* and the *object* . That's all well and good, but how *should* one arrive at the answer to one of these questions?


A great place to start is to crack open a social network like Facebook. Go review (and probably update) your privacy settings; you'll find a variety of configurations for sharing your content like friends-only or friends-of-friends. When you change that setting, you'll find that it applies instantaneously; there is no migration happening in their backend where thousands of users are granted the attribute to view your content. This is because Facebook is designed to store and query *relationships* between their users. Facebook is powered by a social *graph* .


If we lean on the idea of modeling relationships, like Facebook does, you can change the question from "Does X have permission to do Y to Z?" into "Does X have the relationship Y with Z?". For example, "Does User #123 have the Write relationship with Document #456?". Recall the example from reading attributes where the application has to decide if the "admin" attribute also implies the "write" attribute. In a relationship-based access control model, this problem disappears because these are just more relationships:


The beauty of a relationship-based access control is that the application doesn't care *how* the user got to the Write relationship. Maybe they're the user that created the document. Maybe the user had some kind of admin relationship that gives them the ability to do everything. It doesn't matter as long as there is some path through our relationship graph from the user to the Write relationship on the document.


This means that relationships can change (like changing your privacy settings) and applications will not need to have their code rewritten because there was no longer anything left open to interpretation.


## So how does Relationship-Based Access Control (ReBAC) work?


ReBAC, or Relationship-Based Access Control, is a modern approach to access control that focuses on the relationships between users or services, aka subjects, and resources. Instead of relying solely on traditional role-based access control (RBAC) or attribute-based access control (ABAC), ReBAC takes into account the dynamic nature of relationships within an organization. In ReBAC, access control is not assigned based on predefined roles or attributes. Instead, they are determined by the relationships between subjects and resources. This means that access to certain resources can be granted or revoked based on the specific relationships a user has with those resources.


For example, let's say you have a project management application. In a traditional RBAC system, you might assign defined roles such as "project manager" or "team member" to users, and those roles would determine their access to various project resources. However, in a ReBAC system, permissions would be determined by the specific relationships between users and projects, and not the explicit roles assigned to them. That is one the main differences between RBAC and ReBAC.


This relationship-based approach offers several advantages:


It allows you to build fine-grained access control systems. Instead of relying on broad roles, you can define specific relationships and their associated permissions. This leads to a more flexible and scalable permission system that can adapt to changes in the organization.


ReBAC enables a more intuitive and natural way of managing access control. Instead of trying to fit users into predefined roles or attributes, you can simply define the relationships they have with resources. This makes it easier to understand and manage permissions, especially in complex organizational structures. ReBAC promotes a more collaborative and dynamic approach to access control. As relationships between users and resources evolve, access permissions can be easily adjusted to reflect those changes. This ensures that the right people have the right level of access at any given time.


[SpiceDB](https://authzed.com/products/spicedb) , our open-source Google Zanzibar-inspired authorization authorization service, provides a solid foundation for implementing ReBAC. It offers a flexible authorization model and powerful query capabilities, allowing you to efficiently manage and enforce access control policies based on relationships. With SpiceDB, you can easily define fine-grained permissions and dynamically update them as relationships evolve. Following its muse, Google Zanzibar, SpiceDB is meant to be distributed which ensures high performance and fault tolerance, making it suitable for large-scale applications.


## Restoring Conway's Law


By realizing that permissions systems are fundamentally coupled to the *relationships* between people and objects in our software, we can build systems that mimic the way people naturally organize their world to be most effective. This not only empowers the people consuming the software but leads developers to arrive at more robust permission systems that can withstand changes to the organization.


If you're left wondering what a permission system based on relationships looks like in practice,[Authzed](https://authzed.com/) is exactly that! We're currently working hands-on with customers to help them understand and migrate to a better permission system. If adding or refactoring permissions to support new functionality in your app is next on your roadmap,[reach out to us](https://authzed.com/contact) .


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)
- [A Primer on Modern Enterprise Authorization (AuthZ) Systems](https://authzed.com/blog/authz-primer)
- [Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization)


## FAQ


### What is Relationship-based Access Control (ReBAC)?


ReBAC grants or denies access based on the relationships between subjects and resources, evaluated as graph reachability across typed relationship links. Formalized academically in 2011 and popularized by Google's 2019 Zanzibar paper, it moves authorization logic out of application code and into a dedicated system that can reason over ownership, membership, and delegation chains.[SpiceDB](https://authzed.com/spicedb) , AuthZed's open-source permissions database, implements this model directly.


In SpiceDB, a[schema](https://authzed.com/docs/spicedb/concepts/schema) separates raw relations from computed permissions, and[relationships](https://authzed.com/docs/spicedb/concepts/relationships) are the tuples that link specific subjects to resources. Access checks reduce to graph traversal across those tuples. The[query API](https://authzed.com/docs/spicedb/concepts/querying-data) covers point checks, resource listing, and subject listing. For time-bounded grants,[expiring relationships](https://authzed.com/docs/spicedb/concepts/expiring-relationships) handle revocation cleanly without caveat workarounds.


ReBAC also subsumes RBAC and most ABAC patterns within one model, which simplifies auditing and reduces drift across services.[Consistency controls](https://authzed.com/docs/spicedb/concepts/consistency) via ZedTokens prevent the "new enemy" problem where stale reads follow recent writes. AuthZed's[best practices documentation](https://authzed.com/docs/best-practices) covers schema design, caveat usage, and operational guidance for running this at scale.


Originally published March 3, 2021: Added intro, TL;DR, RBAC comparison table, FAQs section, and "What is ReBAC?" section


On this page


- Setting the scene: AuthZ vs AuthN
- LDAP flips Conway's Law on its head
- Groups, Scopes, Claims don't answer the question
- Intro to Relationship-Based Access Control (ReBAC)
- So how does Relationship-Based Access Control (ReBAC) work?
- Restoring Conway's Law
- Additional Reading
- FAQ
- What is Relationship-based Access Control (ReBAC)?


## Related


[Industry Financial Services and RAG: Join us at Open Source in Finance Forum! The financial industry is leading the way on RAG—and for good reason. Join us at OSFF London on June 25 to see fine-grained authorization for RAG embeddings in action. Jun 17, 2026 · 2 min](https://authzed.com/blog/financial-services-rag-osff-london)[Industry Financial Services and RAG: Join us at Open Source in Finance Forum! The financial industry is leading the way on RAG—and for good reason. Join us at OSFF London on June 25 to see fine-grained authorization for RAG embeddings in action. Cormac Foster · Jun 17, 2026 · 2 min](https://authzed.com/blog/financial-services-rag-osff-london)


[Company Identiverse 2026: The Conference about Identity's Authorization Problem AuthZed is sponsoring Identiverse 2026, where three of the four featured summits point to the same underlying issue: the assumptions that underpinned IAM for two decades have collapsed, exposing authorization as the missing layer. Jun 10, 2026 · 3 min](https://authzed.com/blog/identiverse-2026-identity-authorization-problem)[Company Identiverse 2026: The Conference about Identity's Authorization Problem AuthZed is sponsoring Identiverse 2026, where three of the four featured summits point to the same underlying issue: the assumptions that underpinned IAM for two decades have collapsed, exposing authorization as the missing layer. Cormac Foster · Jun 10, 2026 · 3 min](https://authzed.com/blog/identiverse-2026-identity-authorization-problem)


[Industry Calculate the Authorization Tax: Quantifying the Costs of Home-Grown AuthZ Everyone is paying for authorization. Here are five inputs to help you quantify your Authorization Tax—and two paths to reduce it. May 18, 2026 · 6 min](https://authzed.com/blog/calculate-the-authorization-tax)[Industry Calculate the Authorization Tax: Quantifying the Costs of Home-Grown AuthZ Everyone is paying for authorization. Here are five inputs to help you quantify your Authorization Tax—and two paths to reduce it. Cormac Foster · May 18, 2026 · 6 min](https://authzed.com/blog/calculate-the-authorization-tax)
