---
schema_version: "1.0.0"
document_id: "dea5e10348cd69572722f1abc02bd4943b76efcb4d867be105448153914cdd3b"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/authz-must-scale"
published_at: "2023-07-17T12:05:53.554+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:ec69db4aa2029c4de7ff5ee2e0f339d06694848f28ceb23112b39e0a37e951b4"
---

# Authorization Must Scale

As someone working at a YC startup, we are constantly reminded of Paul Graham’s classic advice "do things that don't scale". While we take this advice to heart for operating our business, we deliberately ignore it for the development of SpiceDB, the open source foundation of our company's products. Developing software is expensive, so why wouldn't you want to follow this advice? As it turns out, scalability is a requirement for authorization systems. There really hadn't been designs for scalable authorization solutions until Google published a paper documenting their internal system, Zanzibar. The Zanzibar paper is dense and includes many details that often cause the reader to miss the forest for the trees, but at its core it describes a system that scales. Rightfully, this breakthrough has gotten a lot of people excited—some of us are excited enough that we quit our jobs and started open source projects and a company 😉.


## Your MVP's authorization doesn't scale and that's okay


Experienced software developers and entrepreneurs have probably raised an eyebrow: before making anything scale, you must first test your assumptions by building the minimum viable product. I wholeheartedly agree. I'm not going to sit here and try to sell you software complexity if your MVP can prove its viability with limited authorization functionality; MVPs are about building as little as possible to validate your idea. There are a few industries (e.g. healthcare, finance) that require complex access models on day one, but many MVPs need very little to serve their initial users. As long as there's technology, balancing complexity will always be the name of the game.


## Scaling isn't exclusively a measure of traffic


It's easy to forget that organizations scale on various dimensions. While scale has become almost synonymous with software handling user traffic, it's more likely that you will experience the need to scale in one or more of the other dimensions first. Here's the typical order in which we see some dimensions of scale crop up in organizations:


1. Feature Development: amount of feature requests you can satisfy
2. Development Velocity: the speed at which teams can deliver complex features
3. Geographies: the localities of users you can serve
4. Traffic: the number of users you can serve


When the feature requests start coming in, that's the ideal time to consider your foundation for authorization. You might be tempted to extend your MVP authorization logic to add support for the new requirement, but doing so means setting yourself up an endless loop where implementing the next feature first requires refactoring security-critical authorization code to support it.


While building SpiceDB, we've interviewed thousands of software developers across industries. What we've concluded is that developers are struggling to keep their head above water when implementing anything beyond the basics of authorization. For the few that have gotten past the basics, they never have the time and focus needed to build tooling around their solution that empowers developers with the confidence to iterate on their designs.


The downstream effect of what we've observed is that most teams are limited in their ability to scale in all dimensions. Eventually this pain builds up until a breaking point has been met. At best, a new authorization system must be built and all feature development is paused while each individual product integrates with this new system. But the question is, who will build the new solution and how will you know it has any chance of being an improvement over the previous?


## Friends don't let friends build authorization


Despite being a pillar of computer science with research beginning equally as early as other topics like database systems, authorization experts in the industry are an order of magnitude more rare to employ than database experts. Today, the developer zeitgeist acknowledges that it'd be foolish in most cases to implement your own database for an application, but has yet to internalize that this is equally if not more true for authorization systems. Meta and Google can afford to staff a team of experts to build an authorization platform for their product teams to consume, but pretty much everyone else cannot.


## SpiceDB: the community for scalable authorization


Because there are so few authorization experts and everyone needs scalable authorization, it can only make sense to share expertise. The beauty of open source is that it's not only a great venue for sharing code, but also knowledge. That's why we created SpiceDB and are continuously investing in the community around it. By fostering an ecosystem that connects the folks solving everyday problems with the experts, we can build secure systems that scale with our needs. For real time discussion, you can join us on[Discord](https://authzed.com/discord) or those that prefer asynchronous communication can ask questions and raise topics on[SpiceDB's GitHub issue tracker](http://github.com/authzed/spicedb/issues) .


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)
- [A Primer on Modern Enterprise Authorization (AuthZ) Systems](https://authzed.com/blog/authz-primer)
- [Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
- [Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization)
- [Google's Zanzibar White Paper, Annotated by AuthZed](https://authzed.com/zanzibar)
- [SpiceDB Dedicated](https://authzed.com/products/spicedb-dedicated?utm_source=blog&utm_medium=website)


On this page


- Your MVP's authorization doesn't scale and that's okay
- Scaling isn't exclusively a measure of traffic
- Friends don't let friends build authorization
- SpiceDB: the community for scalable authorization
- Additional Reading


## Related


[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)[Product Why Large Organizations Need Materialize Search, analytics, entitlement management, and AI retrieval increasingly need continuous access to large, constantly updated sets of denormalized permissions. Materialize keeps computed permissions in sync with your SpiceDB permission graph. Irit Goihman · Jul 20, 2026 · 8 min](https://authzed.com/blog/why-large-organizations-need-materialize)


[Company Production-grade permissions, half off, exclusively for YC founders AuthZed Cloud is now 50% off for two years for YC-funded companies and companies founded by YC alumni. Here's how to claim it. Jun 25, 2026 · 2 min](https://authzed.com/blog/yc-authzed-cloud-discount)[Company Production-grade permissions, half off, exclusively for YC founders AuthZed Cloud is now 50% off for two years for YC-funded companies and companies founded by YC alumni. Here's how to claim it. Jimmy Zelinskie · Jun 25, 2026 · 2 min](https://authzed.com/blog/yc-authzed-cloud-discount)


[Engineering Build and Deploy a GitHub-Style Permission System in AuthZed Cloud Learn how to model a complex GitHub-style permission system with SpiceDB and deploy it to AuthZed Cloud, covering tiered roles, org ownership, team hierarchies, and granular repository permissions. May 26, 2026 · 9 min](https://authzed.com/blog/github-permission-system-authzed-cloud)[Engineering Build and Deploy a GitHub-Style Permission System in AuthZed Cloud Learn how to model a complex GitHub-style permission system with SpiceDB and deploy it to AuthZed Cloud, covering tiered roles, org ownership, team hierarchies, and granular repository permissions. Sohan Maheshwar · May 26, 2026 · 9 min](https://authzed.com/blog/github-permission-system-authzed-cloud)
