---
schema_version: "1.0.0"
document_id: "689c809f1c8f26a226b8d083c252d777664020c699246911fd37ba3bdd74ce40"
company_key: "palantir-technologies-inc-class-a-common-stock"
company: "Palantir Technologies Inc."
source_id: "palantir-technologies-inc-class-a-common-stock-rss-f87a89a6619a"
canonical_url: "https://blog.palantir.com/enterprise-business-software-and-the-mixed-up-chameleon-problem-f16df333bc71"
published_at: "2026-05-29T15:06:52+00:00"
first_seen_at: "2026-07-20T03:31:10.454667+00:00"
fetched_at: "2026-07-28T21:11:49.157117+00:00"
content_hash: "sha256:d9f7e7fb8977070eb18efc69121163b76d98589bb0a7ed919f7333312e83b726"
---

# Enterprise Business Software and the Mixed-Up Chameleon Problem

# **Enterprise Business Software and the Mixed-Up Chameleon Problem**


[Palantir](https://palantir.medium.com/?source=post_page---byline--f16df333bc71---------------------------------------)


7 min read


·


May 29, 2026


--


Press enter or click to view image in full size


*Editor’s Note: This blog post was written by Greg Little, Senior Counselor at Palantir* , *with Aaron Jaffe, Senior Vice President at Palantir.*


Over 10 years of implementing Enterprise Resource Planning (ERP) systems, I remember one project where the CFO stopped the room cold. It was 11:30 at night during a mock cutover. People were exhausted manually fixing mapping issues that “should” have been automated. The CFO looked up, rubbed his face, and said, “Tell me again why we’re changing everything we do just to keep this system happy?”


It was a fair question. From the very first ERP project I ever worked on, the expectation was always the same: *fit your processes to the standards defined by the ERP core. Keep the core pristine. Never customize the underlying software. Follow the process exactly as delivered. Don’t deviate. Don’t change anything that might jeopardize upgradeability or introduce delays, cost overruns, or fragility.* And to be fair, there were good reasons behind this.


The ERP core was engineered to be standardized, stable, and reliable. It became the backbone of financials, HR, procurement, and supply chain — the place where compliance and accountability lived. The ERP approached every business process as something that could be standardized for every organization in a consistent structure. If the ERP core fails, everything fails.


But somewhere along the way, protecting that core increasingly meant sacrificing far more important things: the organization’s identity, its differentiated processes, and the ways it actually creates value. The deeper problem is that traditional enterprise software was built on rigid logic that treats every organization as if it were the same. The traditional ERP approach solved for standardization, but in doing so stifled uniqueness. It’s in that uniqueness — whether it’s a distinctive business model, a supply chain advantage, or a mission reality — that competitive edge lives. Uniqueness isn’t a bug software should erase; it’s the feature that keeps you ahead. When software dictates rigid structure, difference becomes weakness. When software adapts to the fluidity of the organization, difference becomes strength.


That realization hit even harder when reading one of my daughter’s favorite books, *The Mixed-Up Chameleon* by Eric Carle. In the story, the chameleon keeps trying to become different things — a flamingo, an elephant, a giraffe, a seal — in the hope that each new form will make it better. But the more it tries to be everything else, the more it loses its identity. By the end, it’s so mixed up it can’t even catch a fly to eat. I’ve watched organizations do the same in the name of “standard foundation,” twisting themselves so deeply to match the structure dictated by an ERP that they lose what once made them effective.


The old adage — the business must adapt to the software — made sense when code, data scale, and infrastructure were barriers. Rigid software paired with consistent implementation through consultants and systems integrators was the playbook to ensure successful digitization. But we don’t live in that world anymore. Today, reliability and adaptability no longer compete with each other; they *can* coexist — organizations can rapidly and flexibly evolve capabilities while ensuring stability.


To be clear, there is enormous value in the standard processes that ERP providers have spent decades refining. Those “best practices” weren’t invented in a vacuum — they were shaped by thousands of implementations, regulatory requirements, and hard-earned lessons across industries. In fact, in the vast majority of cases, following the standardized process *is* the right move. It brings discipline, consistency, auditability, and financial integrity. But no ERP provider can anticipate the precise way every organization creates value. Every business has a handful of processes where its uniqueness *is* the advantage. Those areas should never be forced into conformity simply because the system cannot adapt. In the past, organizations had no choice; today, with an adaptive architecture, they finally do.


Palantir provides a new technical model that makes this possible. Instead of customizing the ERP core, an interoperability layer sits above it, connecting legacy and modern systems. A shared[ontology](https://www.palantir.com/docs/foundry/architecture-center/ontology-system) defines the meaning of the enterprise — its concepts, relationships, business logic, events, and security guardrails — so that when the ERP calls something a “material,” and a supply-chain system calls it an “item,” and a warehouse system calls it a “stock number,” the architecture understands that they refer to the same thing. The Ontology becomes the Rosetta Stone of enterprise systems, ensuring that the ERP doesn’t have to be the single source of meaning in order to remain the single source of record.


The core Ontology system powers an adaptability and extension layer where business logic, workflows, automation, and user experiences live. This is where agentic AI and human engineers operate — building applications, updating data models, resolving inconsistencies, and automating entire chains of work — all atop a shared security and governance layer. This extends the data in the ERP core to the environments where the organization operates — the assembly line, the motorpool, the battlefield — enabling real-time bi-directional use on any form factor at the point of need. Because this system of agility lives outside the ERP core, organizations can migrate cleanly to the latest ERP while retaining the processes that make them unique. This is also the first architecture where data migration is a software and ontology-driven exercise. Instead of armies of business process consultants manually mapping fields, resolving duplicates, and cleansing data over the course of years, agentic systems can align structures, reconcile discrepancies, test transactions, and validate results continuously as the business runs. In this model, migration is not a one-time trauma — it becomes a living, iterative capability.


## Get Palantir’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


Press enter or click to view image in full size


This is also why the traditional systems integrator model is falling apart. It wasn’t built for this world. It was built for a time when systems were rigid and changed slowly. Complexity was managed through using labor to design, establish, and run serialized processes. The result was predictable: a vast majority of ERP program budgets were labor ([at a minimum 60%!](https://www.paxerp.com/blog/manufacturing-erp-implementation-cost) ) Software was the smallest piece of the cost structure. And too often, after all that labor, organizations didn’t get transformation; they got translation. The same complexity wasn’t managed, it was just repackaged.


The root problem wasn’t that consultants were bad. It was that the entire model charged by the hour to manually integrate systems. It optimized for headcount, not outcomes. It solved the problems of yesterday’s architecture, rather than enabling the possibilities of today’s. That world is gone. Modern enterprises can’t afford multiyear deployments and consultant-first operating models when software-first approaches can migrate, integrate, adapt to deliver outcomes that are better, faster, and cheaper.


Nothing exposed the limits of the labor-first model more clearly than data migration. Integrators billed thousands of hours to map and cleanse data manually — work that modern AI can now perform in minutes with greater accuracy and traceability. Migration used to be the barrier to modernization — the part of the program everyone feared. But when migration itself becomes automated, the entire equation changes.


The before and after is dramatic. Before, a contracting officer had to navigate multiple systems and re-enter the same data repeatedly just to initiate a simple purchase. Humans became caretakers of the ERP. After, the contracting officer simply says, “Procure 200 spare parts under this contract using this line of accounting,” and the system validates funding, generates documents, routes approvals, and flags exceptions automatically. No screens. No codes. No chasing emails. Often not even a login. The same transformation unfolds in HR onboarding, in logistics, in financial close, and in every business function that once had to mutate itself to fit a rigid system. The deeper point is that uniqueness and adaptability does not create chaos. The clean ERP core is the bedrock — it must remain stable, standardized, and dependable. But on top of that bedrock, a shared ontology, strong security, and adaptability layer ensure that agents and human operators work from the same truth even as workflows flex and evolve. This is structured adaptability, not anarchy.


After working over 15 large enterprise implementations in enterprise software, I’ve come to believe the old adage must be rewritten. The question is no longer whether the business can adapt to the software — but whether the software is capable of adapting to the business. That distinction matters, because flexibility is every CTO’s nightmare as much as it is their ambition. Full customization is a trap — it breaks upgrades, multiplies technical debt, and trades one form of rigidity for another. The right model isn’t custom; it’s malleable. You buy the platform for what it does best — stability, compliance, financial integrity — and you build on top of it for what makes your organization distinct. The ERP core never changes. What changes is the layer above it: the workflows, the semantic and kinetic models, the experiences. That’s the architecture that lets enterprises evolve without starting over.


In the age of agentic AI, the organizations that adapt their software as fast as the world changes will dominate the next decade. This isn’t a technology choice; it’s a strategic one. It determines whether you continue reshaping yourself like the mixed-up chameleon in my daughter’s book — or whether you finally build systems confident enough to reflect who you truly are.


Chameleons survive by blending in. Enterprises win by standing out. Keep the ERP core clean. Extend it with adaptability. Migrate faster with automation. Interoperate everywhere via ontology. And make the software adapt to you — not the other way around. That means flipping the equation entirely. Stop putting everything into your ERP and hoping to get what you need back out. Instead, get everything you need out of your data first — then put into the ERP only what Finance, your auditor, or a regulator refuses to accept from anywhere else. The ERP becomes what it always should have been: the system of record for accountability, not the system of constraint for operations.


Press enter or click to view image in full size
