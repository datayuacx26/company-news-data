---
schema_version: "1.0.0"
document_id: "a159700d0f1151589b4841e73889353619ae21f1a075deee5d01a3dab802e7ed"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/building-trust-into-ai-how-graphql-architects-accelerated-developer-delivery"
published_at: "2025-11-10T12:25:03+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:00:58.612667+00:00"
content_hash: "sha256:48629fbeca0a5b90a826f0cbe0a8b28ecf9e62ca8d9af16f8b3d2e852dfe25dd"
---

# Building Trust into AI: How GraphQL Architects Accelerated Developer Delivery

## **How Capital One, Expedia, Intuit, and Wayfair are using AI to speed development**


---


At[GraphQL Summit 2025](https://graphqlsummit.com/) , platform leaders from Capital One, Expedia Group, Intuit, and Wayfair shared how they tackled the same challenge: manual loops that stretched reviews across days, governance bottlenecks that slowed every release, and developers waiting on humans for first feedback. Their solutions: agents handling first-pass reviews in seconds, AI-powered IDE plugins catching collisions before commits, and natural language queries replacing dashboards.


The breakthrough wasn’t just AI velocity. It was discovering where AI amplifies human judgment rather than replacing it.


What these companies share isn’t just scale or sophistication. It’s a federated GraphQL architecture that gave them the visibility and structure AI needed to work. Without a unified graph, each of these interventions would’ve required building the foundation first. With it, they could focus entirely on the leverage point: where does AI multiply human judgment?


For these platform leaders, left-shifting with GenAI unlocked speed they couldn’t achieve before: instant feedback, semantic understanding at scale, and runtime insights through conversation. Their journeys revealed the real bottleneck: turning guidelines and institutional knowledge into developer-facing feedback that accelerates delivery without eroding trust.


## **The Productivity Tax No One Budgets For**


Before GenAI, these companies were losing time in ways finance teams couldn’t easily see. Wayfair’s schema reviews stalled for 3.5 days waiting for a human’s first comment, then another 2.5 days to approval. The math added up: $700K annually in lost developer productivity. Capital One’s releases crawled through security reviews, cyber risk sign-offs, and owner approvals, often one to two weeks from data product to production GraphQL.


Intuit hit a different wall. Traditional linters caught maybe 50-60% of their API governance rules. But the semantically complex guidelines that required understanding intent? Those still needed human reviewers, creating bottlenecks every time a team wanted to onboard a subgraph.


The real cost wasn’t just delay. It was what didn’t get built. Partners waiting on integrations. Frontend teams blocked on backend schemas. New products stalled because the API foundation couldn’t keep pace.


## **Four Flavors of Left Shift**


### **Wayfair: Reviews That Start Themselves**


Rohit Gupta, Product Lead for API Platform at Wayfair, targeted the moment of maximum friction: when a developer creates a schema proposal and waits for someone to engage. The old pattern meant waiting 3.5 days for a human reviewer to notice and respond.


Wayfair embedded an agent directly into[Apollo schema proposals](https://www.apollographql.com/docs/graphos/platform/schema-management/proposals) . Now the first review happens instantly. The AI flags pagination anti-patterns, catches fields incorrectly tagged with public contracts, validates custom directive usage, all with deep links to Wayfair’s internal documentation. “We used Retrieval Augmented Generation to ground feedback in our actual standards,” Gupta said.


First-touch feedback dropped from 3.5 days to zero. Total approval time is tracking toward 4.5 days, with an estimated $200K in productivity gains over six months. Reviewers now start from a focused, well-grounded AI report instead of raw schema changes with no context.


### **Intuit: Governance Built into the IDE**


Alex Lin, Staff Software Engineer at Intuit, made a different bet: don’t wait for CI/CD to catch problems. Surface them in the IDE where developers are actively writing code.


His team built a custom plugin that auto-detects schema files and runs AI-powered linting in real time. Click on an error and it opens the file, jumps to the exact line, and links to Intuit’s internal “how to fix” documentation. The tooling also detects semantic collisions when a new type looks suspiciously similar to existing types in the supergraph. “Maybe there’s already a shared type they should conform to instead,” Lin explained.


The same governance checks appear again in CI/CD, producing automated summaries on the supergraph onboarding ticket. Reviewers see the full picture instantly. The final approval remains human, but the human is deciding with radically better context.


What Intuit unlocked wasn’t just speed. It was coverage beyond what static rules could achieve. The 30-40% of guidelines that required semantic understanding finally became automatable.


### **Capital One: Intelligence in the Approval Flow**


Krithika Prakash, Senior Distinguished Engineer for Data & AI at Capital One, took a different angle entirely: make the humans in the approval chain faster and smarter. With multiple review stages across subgraph owners, supergraph owners, security, and cyber risk, Capital One’s challenge wasn’t just speed. It was routing, prioritization, and confidence.


Their GenAI strategy targets three pressure points. First, summarizing changes with context so approvers understand what changed and why it matters. Prakash described opening a portal to find 70 approval requests: “Do I just keep clicking approve? What if the 59th change is the important one I missed?” AI can surface which changes require deep review.


Second, intelligent routing to the right reviewers. If GenAI can identify that a schema change falls into an area a particular reviewer has approved frequently, route it there. Reduce the “ask around and hope” problem that slows every approval cycle.


Third, suggesting merge order to avoid collisions. Multiple changes in flight often conflict downstream. If AI can analyze dependencies and propose sequencing, teams catch conflicts earlier or avoid them entirely. The goal: compress approval cycles from weeks to hours by making every human touchpoint more informed.


### **Expedia: Making Runtime Insights Conversational**


Samuel Vazquez, Principal Software Development Engineer at Expedia Group, took yet another direction. Instead of left-shifting development workflows, his team left-shifted operations.


Expedia built an MCP server connecting to Datadog via API, letting anyone ask natural language questions about subgraph health. “What are the top five most used subgraphs?” gets you request volumes. “Show me CPU and memory for this deployment” prints inline dashboards in your terminal.


Vazquez’s team discovered subgraphs running with excessive replicas and right-sized resources, seeing 50% cloud cost reductions in specific cases. The entire MCP took a week to build. But he also surfaced the strategic shift: “The idea is to have an LLM consuming this data permanently, without human interaction. Automate the process of finding improvement opportunities.


“The idea is to have an LLM consuming this data permanently, without human interaction. Automate the process of finding improvement opportunities.” – Samuel Vazquez, Principal Software Development Engineer at Expedia Group


## **How Teams Built Trust Into Their AI Systems**


Different use cases demanded different approaches. Capital One’s Krithika Prakash made the constraint explicit: “Human in the loop is a big thing for us. We’re not going to let AI run on its own.” Her team is architecting an MCP strategy for enterprise scale, thinking about identity propagation, deduplication of tools, governance standards that differ between internal experimentation and external production APIs. The platform strategy question matters: “How do you ensure you don’t have a proliferation of tools doing slightly different things?”


Contrast that with Wayfair’s approach to hallucinations. When asked about AI generating incorrect feedback, Gupta was pragmatic: “We’ve definitely seen hallucinations when generating schema descriptions without additional context. But for mock data? It’s okay for the model to hallucinate. We’re just looking for realistic test data.” Different use cases, different tolerance for error.


Intuit threaded the middle. They validated prompts against known good and bad examples and tiered their linting rules by confidence. Only the highest-certainty issues become blocking errors. “We still have the human in the loop for final approval,” Lin noted.


The pattern that emerged: AI accelerates delivery when developers trust what it produces. That meant different things to different companies, but everyone agreed: ship tools that hallucinate and developers route around your tooling.


“Human in the loop is a big thing for us. We’re not going to let AI run on its own.” – Krithika Prakash, Senior Distinguished Engineer for Data & AI at Capital One


## **What This Means for Platform Strategy**


Left-shifting with GenAI delivers results traditional tooling couldn’t: first reviews, semantic coverage beyond static rules, conversational access to runtime data. The teams that shipped this value designed for a specific pattern: AI handles volume, context, and pattern recognition while humans make high-stakes calls.


The teams that made this work followed the same playbook: start with measurable pain, keep humans in the loop for high-stakes calls, and treat prompt engineering and grounding as production concerns. Tactically, that means putting AI at your biggest bottleneck, grounding checks in internal examples, and keeping MCP servers thin.


What made this work wasn’t just better models. As Gupta reflected: “The key stage we spent the most time on was output quality. We’re not throwing garbage at developers.” These teams proved that AI can accelerate platform delivery dramatically when you invest in making it trustworthy from day one.


To see live demos of Wayfair’s AI-powered schema reviews, Intuit’s IDE plugin, and Expedia’s MCP server in action, watch the full session from GraphQL Summit 2025 with Rohit Gupta, Alex Lin, Krithika Prakash, and Samuel Vazquez.


Written by


Valeria Gomez


[Read more by Valeria Gomez](https://www.apollographql.com/blog/author/vgomez)
