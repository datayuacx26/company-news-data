---
schema_version: "1.0.0"
document_id: "3a61dbf9a148f2eb29aed6b2b5b8e58cba6d27936d454612504b386b48e2a54a"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/how-indeeds-bold-bet-on-parallel-api-platforms-paid-off"
published_at: "2025-09-02T08:21:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T20:56:25.546971+00:00"
content_hash: "sha256:955315a3b24dad246ce8aa25a43abd5926dfe6998961fa1d746c58147d73eefe"
---

# How Indeed’s Bold Bet on Parallel API Platforms Paid Off

This post is part of our GraphQL Summit 2024 customer series, highlighting how leading teams are using Apollo to accelerate development and build world-class applications. Join hundreds of your peers this October as we return for[GraphQL Summit 2025.](https://www.apollographql.com/graphql-summit-2025)


---


“Like all great software companies, we built it twice,” Mike Cohen shared with the GraphQL Summit audience, pausing for laughter. As Technical Fellow at Indeed, he was describing a decision that looked like organizational dysfunction turned into strategic advantage: running two competing GraphQL platforms simultaneously before unifying them into OneGraph, which now processes **100 billion requests per month** across **295 subgraphs** .


For business leaders wrestling with API strategy decisions, Indeed’s journey offers a counterintuitive lesson about when duplication becomes discovery.


## **The Cost of API Chaos**


Five years ago, Indeed’s API landscape was costing them market opportunities. With no central strategy, hundreds of APIs sprawled across teams with inconsistent protocols and security practices. Developers couldn’t find existing APIs, forcing them to “ask around and hope to find a team that would give you access,” as Cohen put it.


The business impact was tangible: engineering velocity slowed, critical partner integrations like Glassdoor faced delays, and new market initiatives stalled on integration complexity. For a company helping millions find jobs while serving employers and hundreds of partner organizations, API fragmentation was becoming a tax on growth.


Nina Mutty, Senior Technical Product Manager at Indeed, saw this firsthand in the employer-focused products: “Teams were building APIs in silos with limited reuse across the organization. It was hard to scale new initiatives efficiently.” Each team’s distinct approach was furthering silos.


## **The Parallel Platform Experiment**


But instead of forcing a single solution, Indeed’s leadership made an unconventional choice: fund API platform teams in two business units to solve the same problem differently and learn which might offer more for the company. The employer team built EmployerGraph using schema stitching, while the jobseeker team created Jobseeker Federator using Apollo Federation.


“The Jobseeker team reached out to the Employer team and tried to get them to combine forces because duplication seemed bad… but neither solution was proven,” Mutty recalls. “It was just untenable at the time.”


This parallel approach let Indeed test two technical strategies simultaneously while teams learned what worked in their specific contexts. Both platforms shared the same vision: centralized discovery, consistent standards, reusability, and strong developer experience.


Early wins from both the GraphQL implementations validated the overall strategy. API developers could reuse capabilities while maintaining autonomy and discovery problems disappeared. Most importantly for Indeed’s micro frontend architecture, data sharing became dramatically simpler, eliminating the “manually wrangling code and redux stores” that had plagued frontend development.


## **Choosing the Winner**


By fall 2021, the data was clear. Jobseeker Federator had proven itself at scale, serving **3.5 billion requests per month** and successfully onboarding Glassdoor as Indeed’s first major external customer. Apollo Federation’s schema registry provided better safety and validation, while its decentralized approach offered more flexibility for extending types across teams.


The employer organization made the strategic call: abandon EmployerGraph and migrate to Apollo Federation. This meant shutting down their schema stitching approach and moving all employer APIs to what would become OneGraph, with an aggressive two-year timeline to maintain momentum.


“We really only had about two years to make this migration happen or we’d risk losing that support and momentum,” Mutty explained. This urgency shaped their approach: focus on adoption over perfection.


## **Platform Results at Scale**


The migration succeeded, delivering impressive scale:


- **100 billion requests per month** globally
- **295 subgraphs** maintained by **200+ engineering teams**
- **9,000 types** in the unified graph


This scale was achieved through automated developer workflows that eliminated traditional bottlenecks. Indeed implemented push-button subgraph creation with built-in best practices, automated deployment pipelines, and integrated schema checks and lint rules into every merge request. All subgraphs automatically include observability through Datadog integration, reducing operational overhead as the platform grew.


The business impact extended beyond internal efficiency. Indeed successfully integrated Glassdoor for job search and matching capabilities, launched Indeed PLUS in Japan to expose APIs to third-party job boards, and reduced duplication and maintenance overhead across customer platforms. The platform survived and thrived through Indeed’s recent organizational shift, merging jobseeker and employer units into a unified marketplace. OneGraph already reflected this structure, validating the federated approach.


## **Platform Challenges and Evolution**


The migration succeeded, but success brought new complexities. Traffic surged from **5 billion to 100 billion requests per month** . The core platform team faced what Cohen calls “success overload,” buried in requests as hundreds of teams onboarded.


Despite establishing GraphQL guilds, automated tooling, and formal API reviews, the rapid pace of adoption created new challenges that would need to be addressed as the platform matured.


## **Strategic Investment in Partner Experience**


Indeed is now leveraging OneGraph’s proven scalability for strategic business expansion. Following the successful Japan launch, they’re investing heavily in partner capabilities, addressing challenges that emerged from rapid platform growth:


**Domain Model Refinement** : Updating internal concepts to align with industry-standard terminology, reducing friction for external partners while addressing some of the inconsistencies that developed during the rapid scaling phase.


**Graph Organization** : With over 9,000 types in the graph, Indeed has “sort of reintroduced a discoverability problem,” as Mutty noted. “Users really only care about a specific slice of the graph.” They’re exploring namespacing and experience-based variants to serve different user needs – job search and matching (sourcing) versus job posting and advertising (management)


The company is also implementing differentiated governance standards: flexible for internal experimentation, elevated quality for external production APIs. As Cohen reflects, while their GraphQL guilds and automated tooling helped during the migration, “they might not have been quite strong enough” for the scale they had reached.


## **Lessons for API Strategy Leaders**


*“Don’t underestimate what you can get done with stewardship, but recognize when you need stronger governance. Our graph wouldn’t be where it is today without very active stewards who care about the quality of the graph.”* *— Mike Cohen, Technical Fellow at Indeed*


Cohen’s insight challenges conventional wisdom about avoiding duplication. Indeed’s parallel platform approach worked because it generated real data about what mattered: decentralized ownership, proven scalability, external integration capabilities. The “failed” EmployerGraph wasn’t a waste – it was research that informed the winning choice.


For business leaders considering API platform investments, Indeed’s journey demonstrates that sometimes the fastest path to the right solution involves building competing approaches first. The key is having clear criteria for choosing the winner and the discipline to act on the data.


To learn more from Indeed, watch the full session from GraphQL Summit 2024.


Written by


Maylee Jacob


[Read more by Maylee Jacob](https://www.apollographql.com/blog/author/mjacob)
