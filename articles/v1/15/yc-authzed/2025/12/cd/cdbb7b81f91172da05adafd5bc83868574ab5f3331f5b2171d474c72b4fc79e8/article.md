---
schema_version: "1.0.0"
document_id: "cdbb7b81f91172da05adafd5bc83868574ab5f3331f5b2171d474c72b4fc79e8"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/authzed-2025-year-in-review"
published_at: "2025-12-19T07:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T20:55:00.595584+00:00"
content_hash: "sha256:bdc91fb4aef9a5103bff4b744f680906c7e7a40c9f8aaee38ebea70931b82189"
---

# AuthZed 2025 Year in Review

Five years in, our mission remains the same, fixing access control. 2025 was about making our authorization infrastructure available to more teams in more ways.


## AI and Authorization


If AI isn't eating the world, it's certainly consuming the collective attention of the tech community. We started 2025 with generative AI still perceived largely as a novel tool, with developers and non-developers alike searching for the right use cases. By year's end, users had graduated from experimentation to optimizing their AI workflows, and enterprises found themselves navigating a familiar tension: the urgency to deploy AI applications balanced against the need to observe and secure them.


At AuthZed, we've been tracking this progression through the lens of authorization. We've worked directly with companies like[OpenAI](https://authzed.com/customers/openai) , where we provide the authorization infrastructure supporting ChatGPT connectors, and[Workday](https://authzed.com/customers/workday) , where we secure their AI-powered contract lifecycle management platform. These partnerships have shown us that AI magnifies authorization challenges: more actors, more checks, more velocity.


The Model Context Protocol (MCP) captured significant attention this year. We tracked its rapid development, analyzed the spec through an authorization lens, and[identified that the authorization model is lacking](https://authzed.com/blog/mcp-is-not-secure) for many critical use cases. We then built our own MCP servers and published resources for adding authorization to MCP implementations. We're continuing to track related efforts including ACP, A2A, and the newly formed[Agentic AI Foundation](https://aaif.io/) .


This work culminated in our announcement of[Authorization Infrastructure for AI](https://authzed.com/ai-authorization) , providing official support for Retrieval-Augmented Generation pipelines and agentic AI systems. Teams building AI applications can now enforce consistent fine-grained permissions across every stage of their RAG, in their MCP tools and AI agent workflows.


## Progress Toward Our Mission


Beyond AI, we made progress toward our core mission: fixing access control.


We launched[AuthZed Cloud](https://authzed.com/products/authzed-cloud) , our self-service, usage-billed permissions system. AuthZed Cloud makes authorization infrastructure more accessible. Teams can start quickly, manage their own deployments, and pay based on usage. For enterprises requiring dedicated infrastructure, we added[Microsoft Azure support](https://authzed.com/blog/authzed-adds-microsoft-azure-support) , giving customers the flexibility to choose from AWS, Google Cloud, or Azure.


We also focused on making authorization infrastructure fit naturally into existing operations. The[AuthZed Cloud API](https://authzed.com/blog/introducing-the-authzed-cloud-api) lets teams manage authorization resources alongside their other cloud infrastructure. Our[Terraform and OpenTofu provider](https://authzed.com/blog/terraform-and-opentofu-provider-for-authzed-dedicated) brings declarative infrastructure-as-code to authorization management. And with our[Datadog integration](https://authzed.com/blog/authzed-brings-additional-observability-to-authorization-via-the-datadog-integration) , engineering teams gain visibility into authorization performance without custom tooling.


We closed out our development year with the[December 2025 release](https://authzed.com/changelog/december-2025-release) , which includes Materialize support on Azure.


## Five Years of AuthZed


This year marked[AuthZed's fifth anniversary](https://authzed.com/authzed-is-5) . We celebrated by hosting the authorization infrastructure event where Jake, our CEO, reflected on our journey and announced AuthZed Cloud. Customers including[Canva](https://authzed.com/blog/the-dual-write-problem-in-spicedb-a-deep-dive-from-google-and-canva-experience) and[Turo](https://authzed.com/blog/turos-spicedb-success-story-how-the-leading-car-sharing-platform-transformed-authorization) joined us to share their experiences. We also explored the growing importance of authorization infrastructure as AI use cases emerge, with[Andy Pavlo from Carnegie Mellon](https://authzed.com/blog/authzed-is-5-event-recap-authorization-infrastructure-insights#database-philosophy-and-spicy-takes) contributing his perspective on databases.


## SpiceDB Community Showcase


Separately, we hosted a[Community Showcase](https://www.youtube.com/watch?v=tiq8OGCH_qc) to highlight SpiceDB adopters. Kelsey Hightower walked us through the history of Google research-inspired projects, which led to the release of our[annotated Google Zanzibar white paper with his foreword](https://authzed.com/z/google-zanzibar-annotated-paper) .


> What makes SpiceDB special isn’t just that it’s based on great research—it’s that it addresses a real-world problem that most developers don’t even realize is holding them back.
>
>
> - Kelsey Hightower


We also heard from teams at[Reddit](https://authzed.com/spicedb/showcase/reddit) and ReliON about their implementations.


## SpiceDB Development


SpiceDB continued its steady development throughout the year:


- **20 releases** published, from v1.40.0 (January) to v1.48.0 (December)
- **231,296 insertions** and **80,869 deletions** across the codebase
- **4,905 files changed**
- **29 contributors** participated


## Meet Dibs


In March, we[introduced Dibs](https://authzed.com/blog/meet-dibs-the-mascot-bringing-spicedb-to-life) , the SpiceDB mascot. Continuing our tradition of Dune references in the SpiceDB ecosystem, Dibs represents the resilience and adaptability we aim for in SpiceDB itself.


We wanted to end the year with a gift for our community, something that shares what we're passionate about. So we wrote[a story featuring Dibs](https://authzed.com/resources/dibs-and-the-magic-library) that explores authorization concepts. Our note in the book expresses it best:


> *We're on a mission to fix access control, the invisible layer that keeps systems safe and running smoothly. We want to share that mission with you, and we believe stories are one of the best ways to explore new ideas together.*
>
>
> *Just like in the Magic Library, when authorization is accurate and fast, the right doors open.*
>
>
> *Thank you for reading. We hope it sparks curiosity.*
>
>
> *With gratitude, The AuthZed Team*


## Looking Ahead


There's more to do in 2026. AI systems need better authorization and more teams need onramps to authorization infrastructure to secure and build their applications.


If you've been with us as a customer or user or community member, thank you. If you're new here, welcome. We hope you'll join us in the work ahead.


On this page


- AI and Authorization
- Progress Toward Our Mission
- Five Years of AuthZed
- SpiceDB Community Showcase
- SpiceDB Development
- Meet Dibs
- Looking Ahead


## Related


[Community Agentic AI Foundation Names AuthZed's Adora Nwodo and Sohan Maheshwar as Ambassadors Adora Nwodo and Sohan Maheshwar from AuthZed have been selected as official ambassadors for the Agentic AI Foundation (AAIF), joining the program's inaugural cohort. Jul 9, 2026 · 4 min](https://authzed.com/blog/authzed-agentic-ai-foundation-ambassadors)[Community Agentic AI Foundation Names AuthZed's Adora Nwodo and Sohan Maheshwar as Ambassadors Adora Nwodo and Sohan Maheshwar from AuthZed have been selected as official ambassadors for the Agentic AI Foundation (AAIF), joining the program's inaugural cohort. Melissa Smolensky · Jul 9, 2026 · 4 min](https://authzed.com/blog/authzed-agentic-ai-foundation-ambassadors)


[Community The Importance of Off-Sites for a Remote Company While many companies push return-to-office, AuthZed stays remote-first. Our secret is regular off-sites where bonding and business coincide. When we prioritize being human together, we return with more empathy, better communication, and renewed drive to solve hard problems. Dec 30, 2025 · 3 min](https://authzed.com/blog/the-importance-of-off-sites-for-a-remote-company)[Community The Importance of Off-Sites for a Remote Company While many companies push return-to-office, AuthZed stays remote-first. Our secret is regular off-sites where bonding and business coincide. When we prioritize being human together, we return with more empathy, better communication, and renewed drive to solve hard problems. Jenessa Petersen · Dec 30, 2025 · 3 min](https://authzed.com/blog/the-importance-of-off-sites-for-a-remote-company)


[Engineering AuthZed is 5: What We Learned from Our First Authorization Infrastructure Event We celebrated our 5th birthday with talks from Canva, Turo, and Carnegie Mellon. Here's what we learned about the dual-write problem, scaling authorization in production, and why everyone keeps reimplementing the PostgreSQL wire protocol. Sep 2, 2025 · 9 min](https://authzed.com/blog/authzed-is-5-event-recap-authorization-infrastructure-insights)[Engineering AuthZed is 5: What We Learned from Our First Authorization Infrastructure Event We celebrated our 5th birthday with talks from Canva, Turo, and Carnegie Mellon. Here's what we learned about the dual-write problem, scaling authorization in production, and why everyone keeps reimplementing the PostgreSQL wire protocol. Corey Thomas · Sep 2, 2025 · 9 min](https://authzed.com/blog/authzed-is-5-event-recap-authorization-infrastructure-insights)
