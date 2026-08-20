---
schema_version: "1.0.0"
document_id: "a8fe6ad22f5fa13ab3965ad1a218d819d83bbb1a15b9d2aaa25ead734a1b18c2"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/zanzibar-implementations"
published_at: "2021-07-06T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:04:52.597445+00:00"
content_hash: "sha256:febf0e953e27bf88fb1bbbc0d52217ee6b8c8d50e6e9fe032fa4071ca64c12a9"
---

# Zanzibar Implementations

*Update Jul 29, 2021* : The Papers We Love[video](https://www.youtube.com/watch?v=1nbSbe3kw2U) is available and included in this post.


Google's Zanzibar paper was the initial inspiration for Authzed and continues to provide insight as we build our product. But we're not the only ones interested in this technology: the broader software community has also taken an interest in activity around Zanzibar-inspired permissions systems.


(A recent[Google trends search](https://trends.google.com/trends/explore?geo=US&q=google%20zanzibar) corroborates this observation.)


Coincidentally, our CEO Jake recently presented the Zanzibar paper for[Papers We Love](https://paperswelove.org/chapter/newyork/) , and his talk included a survey of the current landscape of (known) Zanzibar-based implementations. In this talk, he collected the following implementations and reviewed each of them against a rubric.


- [Zanzibar](https://research.google/pubs/pub48190/)
- [ory/keto](https://www.ory.sh/keto/docs/)
- [Auth0 Sandcastle](https://sandman-86.herokuapp.com/)
- [Airbnb Himeji](https://medium.com/airbnb-engineering/himeji-a-scalable-centralized-system-for-authorization-at-airbnb-341664924574)
- [Carta AuthZ](https://medium.com/building-carta/authz-cartas-highly-scalable-permissions-system-782a7f2c840f)
- [Authzed](https://authzed.com/)


The rubric comprises a set of subjective measures intended to accentuate the design decision differences between the services.


## Zanzibar Scorecard


- **Paper faithfulness** : How closely does the implementation follow the original paper?
- **Scalability** : Is the implementation distributed?
- **CAP Compromise** : What CAP trade-offs are taken?
- **New enemy?** : Does the implementation support zookies?
- **Dev UX** : Are supporting developer tools provided?


The video of his talk includes the detailed discussion of each of these services and we'll post a link as soon as it becomes available is available to view now:


Can you guess how each service did according to Jake's scorecard?


Here's the scorecard for Airbnb's Himeji as a preview.


**Airbnb Himeji**


Criteria Score


Paper faithfulness Medium


Scalability High


CAP Compromise AP


New enemy? No zookies


Dev UX Average


Share your thoughts in the[Zanzibar discord](https://authzed.com/discord) or introduce any new Zanzibar-inspired implementations not covered yet.


*Image credit:[dschenkelman](https://twitter.com/dschenkelman/status/1408396389298626561_)*


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)
- [A Primer on Modern Enterprise Authorization (AuthZ) Systems](https://authzed.com/blog/authz-primer)
- [Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
- [Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization)


## FAQ


### What are the differences between Permify and SpiceDB, another popular open source implementation of Zanzibar?


SpiceDB and Permify both draw from Zanzibar's design, but differ meaningfully in licensing. SpiceDB ships under[Apache-2.0](https://authzed.com/docs) , making it straightforward to embed in closed-source products. Permify uses AGPL-3.0, which can trigger source-availability obligations for networked deployments of modified versions. Permify was also acquired by FusionAuth in November 2025, shifting its governance trajectory.


On datastores,[SpiceDB supports CockroachDB, Spanner, PostgreSQL, and MySQL](https://authzed.com/docs/spicedb/concepts/consistency) , giving teams real flexibility around consistency and geography. Permify centers on PostgreSQL. Both use snapshot tokens for cache freshness, though SpiceDB calls these[ZedTokens](https://authzed.com/docs/spicedb/concepts/consistency) while Permify uses Snap Tokens.


Schema modeling diverges too.[SpiceDB caveats](https://authzed.com/docs/spicedb/concepts/schema) attach CEL conditions directly to relationships, while Permify bakes ABAC into the schema via attributes and rules. These approaches aren't interchangeable. AuthZed also offers managed services and[Materialize](https://authzed.com/docs/authzed/concepts/authzed-materialize) for accelerating large-scale lookup queries, plus[validation tooling](https://authzed.com/docs/spicedb/modeling/validation-testing-debugging) and a Kubernetes operator for production lifecycle management.


### Can you provide a comparison of SpiceDB with other Google Zanzibar inspired projects?


SpiceDB stands out among Zanzibar-inspired engines primarily through its[ZedToken consistency model](https://authzed.com/docs/spicedb/concepts/consistency) , which provides genuine per-request bounded-staleness semantics and "New Enemy" problem mitigation. OpenFGA offers per-request consistency hints today with tokenized snapshots listed as future work, while Ory Keto's snaptokens were documented as unimplemented at publication time.


On conditional logic, SpiceDB[Caveats](https://authzed.com/docs/spicedb/concepts/caveats) and OpenFGA Conditions both use Google CEL, but SpiceDB's[schema DSL](https://authzed.com/docs/spicedb/concepts/schema) offers stronger typing with union, intersection, and arrow traversals. Permify adds CEL-based ABAC rules but now sits within FusionAuth's roadmap, introducing acquisition-related uncertainty.


Operationally, SpiceDB ships a[first-party Kubernetes Operator](https://authzed.com/docs/spicedb/ops/operator) for Day-2 lifecycle management, alongside[schema validation and testing tooling](https://authzed.com/docs/spicedb/modeling/validation-testing-debugging) and[reverse-index queries](https://authzed.com/docs/spicedb/concepts/querying-data) like LookupResources and LookupSubjects. Teams evaluating these engines should review[SpiceDB best practices](https://authzed.com/docs/best-practices) and AuthZed's documentation to assess fit against their consistency and operational requirements.


On this page


- Zanzibar Scorecard
- Additional Reading
- FAQ
- What are the differences between Permify and SpiceDB, another popular open source implementation of Zanzibar?
- Can you provide a comparison of SpiceDB with other Google Zanzibar inspired projects?


## Related


[Engineering Consistency is the Key to Performance and Safety Both SpiceDB and Zanzibar combine performance, scalability, and correctness into one manageable, global authorization solution. Strong consistency is key to ensuring correctness, but caching is necessary for performance. Consistency and caching are often diametrically opposed so how do SpiceDB and Zanzibar solve this problem? With a few key realizations around staleness, when consistency is necessary and how the two interact. Sep 5, 2024 · 7 min](https://authzed.com/blog/consistency-is-the-key-to-performance-and-safety)[Engineering Consistency is the Key to Performance and Safety Both SpiceDB and Zanzibar combine performance, scalability, and correctness into one manageable, global authorization solution. Strong consistency is key to ensuring correctness, but caching is necessary for performance. Consistency and caching are often diametrically opposed so how do SpiceDB and Zanzibar solve this problem? With a few key realizations around staleness, when consistency is necessary and how the two interact. Joey Schorr · Sep 5, 2024 · 7 min](https://authzed.com/blog/consistency-is-the-key-to-performance-and-safety)


[Engineering Fine-Grained Access Control: Can You Go Too Fine? In this article, AuthZed's co-founder and CEO Jake Moshenko addresses trade-offs to consider when modeling your permissions, when to consider fine-grained access control, costs associated with fine-grained access control, how to strike a balance, and main takeaways. Mar 5, 2024 · 8 min](https://authzed.com/blog/fine-grained-access-control)[Engineering Fine-Grained Access Control: Can You Go Too Fine? In this article, AuthZed's co-founder and CEO Jake Moshenko addresses trade-offs to consider when modeling your permissions, when to consider fine-grained access control, costs associated with fine-grained access control, how to strike a balance, and main takeaways. Jake Moshenko · Mar 5, 2024 · 8 min](https://authzed.com/blog/fine-grained-access-control)


[Engineering Announcing AuthZed Materialize AuthZed Materialize has entered Early Access. AuthZed Materialize is a new service that provides two major benefits: accelerated SpiceDB API responses and the ability to efficiently stream access changes to other systems. Feb 21, 2024 · 3 min](https://authzed.com/blog/materialize-early-access)[Engineering Announcing AuthZed Materialize AuthZed Materialize has entered Early Access. AuthZed Materialize is a new service that provides two major benefits: accelerated SpiceDB API responses and the ability to efficiently stream access changes to other systems. Jimmy Zelinskie · Feb 21, 2024 · 3 min](https://authzed.com/blog/materialize-early-access)
