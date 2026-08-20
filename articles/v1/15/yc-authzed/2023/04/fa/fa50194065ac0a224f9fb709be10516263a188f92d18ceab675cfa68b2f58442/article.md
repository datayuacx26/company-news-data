---
schema_version: "1.0.0"
document_id: "fa50194065ac0a224f9fb709be10516263a188f92d18ceab675cfa68b2f58442"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/introducing-fine-grained-access-management"
published_at: "2023-04-27T15:26:44.860+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:01:55.089772+00:00"
content_hash: "sha256:3d0a4101541cf6c5413025b2fd6ef135f3c0fef05131a4773d4b369638ff183f"
---

# Introducing: Fine-Grained Access Management

The systems we build at AuthZed are the direct result of feedback from our community and customers. Because security is the core of our flagship product, SpiceDB, we take feedback on this topic very seriously. We’ve heard you, and today we’re proud to introduce a better way to secure AuthZed customers’ client applications accessing the SpiceDB API: **Fine-Grained Access Management** (FGAM).


It’s part art and part science when deciding to stamp a commercial product as ready for production. We initially launched our commercial products with very basic API key management to gather market feedback and prioritize future development. Thanks to our customers’ input ❤️, it became clear that they needed far more control over what exactly API keys could access in SpiceDB. FGAM is available now for SpiceDB Dedicated and SpiceDB Self-Hosted customers creating Permissions Systems with SpiceDB 1.19.0 or newer. 🚀


## Why Fine-Grained Access Management is Important


Mature engineering organizations typically enforce **the principle of least privilege** : each client will have access to the minimum set of APIs needed for it to perform its operations. For example, a client updating a SpiceDB Schema as part of a continuous integration and delivery pipeline would have the minimum required permission of` WriteSchema` while other production services are restricted to common read-only APIs such as` CheckPermission` ,` LookupResources` , and` LookupSubjects` .


The benefit here is huge for security: in the event that a bad actor gains access to one of your systems, Fine-Grained Access Management to the SpiceDB API will limit their capabilities.


## FGAM for your FGAM—SpiceDB! 😎


Because SpiceDB itself is designed to provide Fine-Grained Access Management, it was our first choice for securing the SpiceDB API. That’s right, we[eat our own dog food](https://en.wikipedia.org/wiki/Eating_your_own_dog_food) : SpiceDB powers **Fine-Grained Access Management (FGAM)** for our customers’ SpiceDB Permissions Systems! 🤯


Using SpiceDB lets us take the principle of least privilege a step further. Fine-Grained Access Management protecting the SpiceDB API would've been enough for most users, but that alone is insufficient for scenarios where users want to also enforce policy based on runtime context. For example, allow API calls only during specific time-frames or from specific IP ranges. You can further limit access by pairing permissions with context. Here are some examples:


- Only allow` CheckPermission` API calls for a specific subject and/or resource type
- Only allow` WriteRelationship` permissions over a subset of Schema relations
- Filter certain elements from a streaming API
- Only allow changes to a restricted subset of a SpiceDB Schema
- Only allow an API call if a` CheckPermission` for a SpiceDB instance returns` allowed` (API meta-permissions! 🤯)


Our solution offers a familiar RBAC-like paradigm seen in cloud providers' IAM products. You can create **Service Accounts** to represent your workloads, **Roles** with permission and conditions to access one or more API methods, and the **Policy** that binds a role to a principal.


The star of the show is SpiceDB, which allowed us to create a schema that captures all those[IAM concepts](https://authzed.com/blog/google-cloud-iam-modeling/) seamlessly and supports policy conditions with[SpiceDB Caveats](https://authzed.com/blog/caveats/) . Caveats are type-safe and compiled at runtime for blazing-fast dynamic enforcement! ⚡


## Try out FGAM today! ⭐


You can enable FGAM today for new SpiceDB Permissions Systems using SpiceDB v1.19.0 and up. For help getting started, check out the[Fine-Grained Access Management Documentation](https://authzed.com/docs/spicedb-dedicated/fgam) . For support, reach out to us on the[AuthZed Discord](http://authzed.com/discord) , via a shared Slack channel, orsupport@authzed.com .


## Additional Reading


-


[Fine-Grained Access Management Documentation](https://authzed.com/docs/spicedb-dedicated/fgam)


-


[Caveats: A Scalable Solution for Policy - Jimmy Zelinskie](https://authzed.com/blog/caveats/)


-


[Top-3 Most Used SpiceDB Caveat Patterns - Víctor Roldán Betancort](https://authzed.com/blog/top-three-caveat-use-cases/)


-


[Modeling Google Cloud IAM in SpiceDB - Jake Moshenko](https://authzed.com/blog/google-cloud-iam-modeling/)


-


[Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)


-


[A Primer on Modern Enterprise Authorization (AuthZ) Systems](https://authzed.com/blog/authz-primer)


-


[Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)


-


[Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)


-


[Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization)


On this page


- Why Fine-Grained Access Management is Important
- FGAM for your FGAM—SpiceDB! 😎
- Try out FGAM today! ⭐
- Additional Reading


## Related


[Engineering LookupSubjects and SpiceDB v1.12.0 Product Updates for July & August Sep 20, 2022 · 3 min](https://authzed.com/blog/lookup-subjects)[Engineering LookupSubjects and SpiceDB v1.12.0 Product Updates for July & August Joey Schorr · Sep 20, 2022 · 3 min](https://authzed.com/blog/lookup-subjects)


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)


[Engineering Build and Deploy a GitHub-Style Permission System in AuthZed Cloud Learn how to model a complex GitHub-style permission system with SpiceDB and deploy it to AuthZed Cloud, covering tiered roles, org ownership, team hierarchies, and granular repository permissions. May 26, 2026 · 9 min](https://authzed.com/blog/github-permission-system-authzed-cloud)[Engineering Build and Deploy a GitHub-Style Permission System in AuthZed Cloud Learn how to model a complex GitHub-style permission system with SpiceDB and deploy it to AuthZed Cloud, covering tiered roles, org ownership, team hierarchies, and granular repository permissions. Sohan Maheshwar · May 26, 2026 · 9 min](https://authzed.com/blog/github-permission-system-authzed-cloud)
