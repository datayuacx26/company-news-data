---
schema_version: "1.0.0"
document_id: "c6d5d66506efc368fbe4f6261b17179e1ede7fb622776c07329c1d3d0ba6c284"
company_key: "salesforce-inc-common-stock"
company: "Salesforce Inc."
source_id: "salesforce-inc-common-stock-news-import-e12c13bbd360"
canonical_url: "https://www.salesforce.com/blog/new-ai-trust-architecture/"
published_at: "2026-07-10T16:40:00+00:00"
first_seen_at: "2026-07-22T12:38:45.747221+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:9c02007c3a8eb369e077f366e833065beca7ad754a39d575af17f0f706c47d0a"
---

# The New AI Trust Architecture: 5 Requirements for Agent-to-Agent Communication

In London in 1832, clerks from thirty-one competing banks gathered every afternoon on Lombard Street to settle accounts. Each bank had already mastered the basics — tracking debits, crediting accounts, settling balances. The unsolved problem was architectural: How do you enable daily transactions between competitors who fundamentally don’t trust each other?


What they invented together was a “trust architecture” that allowed competitors to transact reliably, every day, without a central authority enforcing the rules. The resulting[London Clearing House (LCH)](https://en.wikipedia.org/wiki/LCH_(clearing_house)) worked because every participant had a registered identity, a reputation that traveled with them, and a shared understanding of what behavior would get them expelled from the network.


**We are at that same moment with AI agents** .


The technical protocols for agent-to-agent communication are being built. The trust architecture has not been, and recent research from our team at[Salesforce AI Research](https://www.salesforceairesearch.com/) makes the high stakes clear. In a[study of 3.5 million comments across 78,000 AI agents interacting on an agent-only social platform](https://arxiv.org/abs/2602.20059) , our researchers found that without coordination mechanisms, even capable agents talk past each other. Sixty-five percent of agent comments added nothing to the posts they replied to; spam and off-topic noise dominated. The study calls this “interaction theater,” activity that looks like discussion but carries almost no exchange.


That finding has direct implications for enterprise leaders. As autonomous agents begin to negotiate on behalf of organizations across industries and borders, governance, legal, and ethical guardrails must be designed before the transactions become consequential.


Below are the five things every enterprise leader should be thinking about right now.


## 1. Standards augment rules


**Rules are deterministic.** “Do not disclose customer data” is a rule, and an AI agent can follow it. **Business negotiation runs on** ***standards:*** contextual, interpretive, value-laden judgments about when to hold firm, when to concede, and when strategic positioning crosses into manipulation.


Decades of AI development focused on rule-following. We are now asking these systems to navigate standards, and that requires a fundamentally different architecture, including governance frameworks that evaluate how an agent decided, not just what it decided.


## 2. Identity and reputation must travel with the agent


Anonymous participation in the Clearing House was impossible, and the same principle applies here. Cross-organizational negotiation requires verifiable identity and reputation that accumulate across thousands of interactions — a baseline requirement for any entity acting autonomously on behalf of a principal. Game theory and commercial law converge on why: good-faith behavior between self-interested parties depends on the expectation that today’s conduct shapes tomorrow’s opportunities, and on the evidentiary record that makes enforcement possible when it doesn’t. Strip away that persistence and every interaction starts from zero, with no way to reward consistency or hold bad actors accountable.


Because reputation is cumulative and non-transferable, an agent’s credibility ultimately derives from its principal — which gives organizations with established trust, regulatory track records, and institutional accountability a real head start over technically capable newcomers. In 2025, Salesforce AI Research pioneered the[Agent Cards concept](https://www.salesforce.com/blog/how-ai-protocols-will-expand-enterprise-boundaries/) : standard metadata communicating an agent’s capabilities, limitations, compliance posture, and authority to commit on behalf of its principal. Think of it as a résumé for agents. Google adopted the concept in its A2A specification. The next frontier is reputation infrastructure — a way for agents to build, demonstrate, and lose standing over time. As with people, the agents that earn the right to keep negotiating will be the ones that prove consistent at scale.


## 3. Boundaries scale better than scripts


The instinct when governing AI is to specify every scenario in advance. That approach worked for rule-based automation and breaks down for probabilistic agents operating in complex business environments. A better model comes from how we govern human professionals. Surgeons operate within standards of care, professional ethics, and oversight mechanisms rather than scripts for every possible patient presentation. Agent governance must work the same way: wide latitude within defined boundaries, with evaluation frameworks that assess judgment quality. The challenge multiplies when agents from different organizations, trained on different data, must navigate shared standards together.


## 4. Accountability must be structured, traceable, and human


When an agent commits to a price, accepts a contract term, or escalates a dispute, there can be no ambiguity about who is accountable. New organizational roles will emerge to meet this requirement: AI operations officers and agent managers with authority over deployments and responsibility when something goes wrong. Audit trails must be sophisticated enough to survive legal scrutiny, capturing how decisions were made, what information was considered, and what alternatives were evaluated. Emerging standards like Agent Cards are early examples of what auditability infrastructure looks like in practice. Designing for auditability from the start is far easier than retrofitting it after deployment.


## 5. Calibrated escalation is the line between automation and liability


Knowing when to stop may be the most critical capability of all. Agents that escalate too often defeat the purpose of automation. Agents that never escalate become a liability. The trust architecture must precisely calibrate the threshold to consequence. Routine decisions should flow autonomously. High-stakes decisions involving regulatory compliance, major contractual commitments, or significant financial exposure should automatically surface to human judgment. These are strategic design choices: should the agent escalate mid-negotiation, only at final approval, before walking away/going “pens down” or via periodic audits after the fact? The answer depends on the domain, the risk, and the relationship at stake.


## The takeaway for leaders


- Standards augment rules


- Identity that accumulates reputation


- Boundaries above scripts


- Structured accountability that traces back to a human


- Calibrated escalation that knows when to stop


These five principles form the foundation of the trust architecture that agent-to-agent commerce will require.


The London Bankers’ Clearing House emerged because thirty-one competing institutions recognized that the collective benefit of a trusted system outweighed the competitive cost of building one. **We are at that same moment with agent-to-agent communication between companies today.** The frameworks do not yet exist, the governance standards have not been written, and the legal architecture for machines that negotiate in the shadow of humanity remains largely uncharted territory. The organizations that engage with these questions early will shape the standards everyone else complies with later.


The time to engage is now.


**This thinking emerges from an unlikely partnership: a chief scientist and a chief legal officer who discovered that the hardest problems in agentic AI live at the intersection of their disciplines.** We’re grateful to Adam Earle, Patrick Stokes, Portia Bamiduro, Jacob Lehrbaum, Rob Wolf, Itai Asseo, and Karen Semone for their insights and contributions to this work.


*This piece adapts thinking from our Fortune op-ed,*[“The 19th century banking problem that AI hasn’t solved yet”](https://fortune.com/2026/03/20/salesforce-ai-infrastructure-london-clearing-house-19th-century-banking/) *(March 2026), distilled here into five practical takeaways for enterprise leaders governing AI agents at scale.*
