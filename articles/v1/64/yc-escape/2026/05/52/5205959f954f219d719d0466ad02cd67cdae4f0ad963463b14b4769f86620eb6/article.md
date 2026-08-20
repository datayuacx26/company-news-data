---
schema_version: "1.0.0"
document_id: "5205959f954f219d719d0466ad02cd67cdae4f0ad963463b14b4769f86620eb6"
company_key: "yc-escape"
company: "Escape"
source_id: "yc-escape-rss-d1370620bfa7"
canonical_url: "https://escape.tech/blog/fintech-platform-ai-pentesting-customer-story/"
published_at: "2026-05-08T09:45:20+00:00"
first_seen_at: "2026-07-20T23:23:55.281376+00:00"
fetched_at: "2026-07-28T21:14:05.751806+00:00"
content_hash: "sha256:bd1664124f75733804ccffd57ee66ada0f261c9838151f1b077d4d56098cf6c9"
---

# How one of the leading FinTech platforms used Escape to automate business logic security testing at scale

This company processes financial transactions for thousands of clients worldwide. Every API endpoint encodes financial logic: initiating transfers, approving transactions, and managing accounts across isolated tenants. The key challenge was how to test it at the speed and depth the platform demands.


The solution wasn't delivered as an off-the-shelf product. This company **became our design partner on the AI pentesting product and directly shaped how it was built.** It was co-designed through a series of intensive workshops, bi-weekly sessions where Escape's engineers worked directly alongside the offensive security team, iterating on scan configurations, debugging flows, extending the exploration framework, and building toward a shared definition of what "working" actually means.


## Use Cases


Use cases covered:


- **Automated Business Logic Testing**


Replacing manual, repetitive pentest cycles with continuous AI-driven scanning of multi-step API flows, basically catching what rule-based DAST can't.


- **Multi-Tenant Isolation Verification**


Verifying that User A can never touch User B's transactions automatically, across every endpoint, on every sprint cycle.


- **Privilege Escalation Detection**


Mapping role-based access across initiators, approvers, and admin users, then probing every handoff for exploitable gaps.


- **Attack Path Reasoning**


Understanding the sequence of requests, not just individual endpoints, to surface transaction-level abuse chains before attackers do.


## The Problem


#### Manual pentesting doesn't scale to the environment complexity


> *"We're looking for something that can aid the overall security testing, primarily the dynamic application testing on our APIs. We would like to see if there is a party in the world that can assist us with automating the way we test our APIs, trying to see what kind of automations and logical explorations can be integrated." - Team Lead - Offensive Security*


The offensive security team runs a rigorous program: manual penetration testing, red teaming, purple teaming, code reviews. They have deep expertise and precise documentation. The team had seen every flavor of API scanner on the market. Their interest wasn't in discovery, they already had comprehensive API documentation and Postman collections. What they needed was a tool that could reason about sequences: if User A initiates a transfer, gets a transaction ID, and hands it to User B for approval, can that flow be abused? Can User A cancel User B's transaction? Can User A reach User B's balances?


#### Repetitive work, finite team


With hundreds of APIs across the platform, the manual coverage gap was significant. The team's goal was clear: automate the low-hanging fruit so human testers could focus on what machines can't replicate.


#### The strict infrastructure constraint


Adding further complexity: the company's infrastructure philosophy. No third-party SaaS gets remote access into the network. No reverse tunnels. Either you scan what's publicly exposed, or you build a solution that meets their security bar. This ruled out most tooling options on the market entirely.


## The Solution


The partnership began with a shared insight: before you can test bad flows, you need to understand the good ones. The main goal was to ensure that everything is interpreted in the correct way and that all the input is valid, otherwise, invalid input leads to poor results. Escape introduced the concept of the "Attack Path Visualization", a structured representation of how an API is intended to work, built from the team's own Postman collections and OpenAPI specs. Once Escape could reliably walk through the intended flow (initiate → approve → verify), the AI could start asking the interesting questions.


#### Multi-user attack graph reasoning


The company's transaction API is a two-actor system: one user initiates, another approves. A third company should never be able to touch either. Escape built multi-user exploration to model this explicitly, mapping the full graph of which user has access to which endpoint, which resources they can reach, and what data flows between them.


#### AI pentesting agents for business logic


> *"What we were interested in is reasoning, attack paths, and unexpected behaviors from the apps." - Team Lead - Offensive Security*


As the relationship evolved, Escape introduced its AI pentesting agents, reasoning engines that don't just probe endpoints, but think about what the API is trying to do and how that intent could be subverted. Rather than a checklist of vulnerability types, the agent reasons through attack paths: what sequences, what role combinations, what data manipulations could produce unexpected behavior.


## The Impact


Results here aren't measured in CVE counts. They're measured in how the team works, what they no longer have to do manually, what they can now see that was invisible before, and what confidence they carry into the next sprint.


#### From reactive to continuous


The team's ambition was always clear: move from annual reviews and ad-hoc pentesting toward continuous automated coverage. Escape is now the engine that makes that possible, running against publicly-exposed APIs on a weekly cadence, surfacing regressions before they reach production.


> *"The results from the AI pentesting actually caught something interesting… Even through my own testing when I was initially trying to validate how transfers operated, that was something I was running into as well. It's nice to know the scan is recognizing this kind of gap in business logic."*


#### A product shaped by the problem


Perhaps the most significant outcome isn't something you can measure in a dashboard. The company's feedback has directly influenced Escape's AI pentesting product roadmap, from the rebuild of the exploration framework around Attack Path Visualization concepts, to the development of multi-user attack graphs, to the AI pentesting agent architecture now. The challenges of testing financial business logic at scale have made Escape sharper for every enterprise customer that comes after.


## What the future holds


**The north star the team defined early and has held to throughout: 99% accuracy in results.** Not because perfection is achievable, but because at that level, developers can trust the output enough to act on it directly. Scans they can run themselves. Results they don't need to triage.


That's the mission Escape and the offensive security team are building toward together.
