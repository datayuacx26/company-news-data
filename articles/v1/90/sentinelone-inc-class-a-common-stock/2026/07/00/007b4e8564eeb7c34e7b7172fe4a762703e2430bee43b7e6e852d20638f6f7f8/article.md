---
schema_version: "1.0.0"
document_id: "007b4e8564eeb7c34e7b7172fe4a762703e2430bee43b7e6e852d20638f6f7f8"
company_key: "sentinelone-inc-class-a-common-stock"
company: "SentinelOne Inc."
source_id: "sentinelone-inc-class-a-common-stock-rss-86808feccfbf"
canonical_url: "https://www.sentinelone.com/blog/the-autonomous-soc-revisited-what-18-months-on-the-road-has-taught-us/"
published_at: "2026-07-01T13:00:45+00:00"
first_seen_at: "2026-07-20T23:17:13.754958+00:00"
fetched_at: "2026-07-28T21:52:24.088997+00:00"
content_hash: "sha256:000e177c099d1ae348ffd632345ccea03abbe4983fd410d1766610538a133c61"
---

# The Autonomous SOC, Revisited: What 18 Months on the Road Has Taught Us

This post revisits SentinelOne’s Autonomous SOC maturity model, first introduced in “[Autonomous SOC Is a Journey, Not a Destination](https://www.sentinelone.com/blog/introducing-the-autonomous-soc-maturity-model/)


*” (December 2024).*


When SentinelOne® introduced the Autonomous SOC maturity model, we made a deliberate choice: describe a journey, not promise a destination.


The industry had no shortage of vendors declaring that AI would transform security operations. We thought the more useful contribution was a framework for understanding what that transformation looked like, at what pace it was realistic, and what conditions each stage of progress required.


Security teams found the model useful. Not as a marketing claim, but as a map. CISOs and SOC leaders started placing their organizations on it, asking what it would take to move forward.


What happened next was telling. By RSAC 2026, ‘autonomous SOC’ appeared in vendor keynotes and product launches from companies that hadn’t used the term twelve months earlier. Add in pseudonyms like Agentic SOC and AI SOC, and the list explodes. Fast adoption brings loose definitions. For us, it’s worth being precise about what the concept means and what it doesn’t.


Here is what SentinelOne has learned from 18 months of real-world Autonomous SOC deployments.


## What Held Up


Today, the progression still maps accurately to where organizations are and what separates each stage from the next. That accuracy holds even for a framework built before most organizations had meaningful AI deployment experience. The inflection points reflect real operational transitions at each maturity step.


The “journey not destination” framing has proven more important than we anticipated when we wrote it. In early 2026, Gartner published guidance to help buyers evaluate AI SOC claims more critically, noting that vendor credibility in this space depends on honest representation of where the technology is:


“Some vendors exaggerate capabilities (like being able to deliver a fully autonomous SOC), risking buyer trust and harming the reputation of legitimate solutions.” 1


A maturity model is structurally honest. It reflects where you are, not where a vendor wishes you were. Gartner’s research found that **while 40% of organizations are actively evaluating AI SOC capabilities, only 18% have actually deployed2** . The gap between evaluating and deploying is rarely about technology. Most organizations cannot advance because they lack a clear view of where they stand or what the next stage requires.


When security leaders use the model as a reference point, the evaluation conversation changes. The question shifts from “does your product make my SOC autonomous?” to “what would it realistically take to advance, given where we are today?” A feature list cannot answer that question. An honest vendor can.


Watch our webinar on why most AI SOC deployments stall


[here](https://assets.sentinelone.com/pai/ai-soc-deployments-stall) .


## What We Underestimated


The levels were always sound. What we underestimated was how much organizations needed to build before they could operationalize them. Customers understood where they wanted to go. But achieving Partial Autonomy (Level 3) requires a data foundation, a workflow architecture, and AI readiness that most teams were still building when we first published this model. That’s a fact about where most security organizations were in 2024.


The transition from AI-Assisted Operations (Level 2) to Partial Autonomy (Level 3) is primarily a governance problem, not a tooling one. The tools are capable. What most organizations are missing is an understanding of the foundation of data and trust that Partial Autonomy (Level 3) requires, including the role humans play in building it.


When analysts work with AI assistance, they leave traces. Which queries they accept. Which results they act on. Which steps they modify or override. Over time, the system learns which investigation patterns the team trusts, which AI recommendations get acted on, and where analyst expertise is required – the kind of institutional knowledge that only comes from doing the work. Partial Autonomy is built on that record, not installed on top of an existing stack.


The path from AI-Assisted Operations to Partial Autonomy starts earlier than most organizations realize. It begins before they’re thinking about autonomy at all. Every assisted workflow is building toward what comes next.


## What Holds Organizations Back


The primary barrier between AI-Assisted Operations (Level 2) and Partial Autonomy (Level 3) is accountability.


Consider how the automotive industry defined its equivalent of Partial Autonomy – SAE Level 3.


Taxonomy and Definitions for Terms Related to Driving Automation Systems for On-Road Motor Vehicles3


The designation applies only within specific, defined operational conditions. Outside those conditions, the human must take control. What qualifies a system for L3 is defined before autonomous operation begins: explicit parameters, a defined scope, and clear conditions for human override. Governance precedes autonomy.


Consider Waymo. It is the most capable autonomous system deployed at scale today — L3+ — operating without a safety driver under defined conditions. The vehicle is remarkable. But Waymo’s primary innovation is the organization built around it: the cloud infrastructure that keeps cars in autonomous condition, the human operations that handle exceptions the system cannot cover. The more autonomous the system, the more organizational maturity it required to build. **High autonomy is an organizational capability** .


The same logic applies in security operations. Accurate AI is the foundation. What makes Partial Autonomy legitimate is what gets built on top of it: defined rules of engagement, pre-approved policies, audit trails, and a clear organizational answer to who is responsible when an AI verdict is acted on. That accountability sits with the security team. When automation fires, it fires because someone made a deliberate governance decision to allow it. That is what makes it auditable, defensible, and durable.


Gartner’s readiness criteria for AI SOC deployments require that operational workflows be established in playbooks before AI is introduced4. In practice, the organizations that advanced most consistently treated that requirement as a sequencing discipline, not a box to check. They defined their rules of engagement before turning on automated response.


The second learning was the attacker asymmetry. Defenders who stall between AI-Assisted Operations and Partial Autonomy have often done the validation work. The AI logic checks out. What remains is the decision to extend that trust to autonomous action — and that decision takes time. Attackers move differently. They deploy, observe what works, and iterate. Governance is an externality. Trial and error with no consequences for failure is a significant operational advantage. **The gap between a defender’s trust-building timeline and an attacker’s operational tempo is structural. It compounds.**


## Why High Autonomy Stays on The Horizon. And Why That Matters Less Than We Thought


Eighteen months of deployment have also changed how we think about the upper end of the model.


When the original post was written, High Autonomy (Level 4) was described as dependent on a level of AI reasoning we hadn’t yet seen in production security environments. That remains the right framing. What’s changed is how close that horizon has become. Two years ago, asking a model to reason through a multi-stage attack, correlate signals across data sources, and produce an auditable verdict required significant scaffolding and produced inconsistent results. That’s no longer true. The gap between where AI was and where High Autonomy requires it to be has narrowed substantially.


High Autonomy still requires more than capable models. Institutional trust takes time to build. Accountability structures have to go beyond controlled tests to survive real incidents. Human oversight has to be redefined from reviewing individual actions to governing a system’s behavior within a defined scope. Those are organizational problems, technology doesn’t solve them. That work is already underway at Partial Autonomy (Level 3). What the road to High Autonomy requires is only visible from Partial Autonomy. Organizations that haven’t operated there yet are planning for a destination they haven’t seen. The knowledge of what it takes is path-dependent, and it emerges from operation, not from design.


As organizations move deeper into Partial Autonomy, the distinction between levels matters less in practice. **What security leaders actually want is** ***relevant control:*** **governance over the decisions that matter, without being burdened by the ones that don’t.** You cannot be responsible or accountable for a system that asks you to review everything.


Control over the right decisions is what matters. An analyst reviewing every alert has maximum control and minimum leverage. A system that acts autonomously on well-understood threat patterns, surfaces only the ambiguous and novel cases for human judgment, and maintains a complete audit trail, gives the analyst control over exactly what deserves their attention. That is a better and more focused version of human oversight.


High Autonomy, seen through this lens, is AI that has earned sufficient trust within a defined scope. The remaining human decisions are the ones that require human judgment, because the governance architecture evolved to allocate human attention correctly.


In the same way, a pilot does not manually adjust every control surface for the duration of a flight. They set the destination, define the parameters, and monitor the instruments. The system handles thousands of micro-corrections that would be impossible to manage directly. The pilot’s job is to govern the conditions under which the aircraft flies itself, not to manage every control input directly. Nobody describes this as a lack of pilot control. It is a better allocation of pilot judgment. And it works because of the environment surrounding the autopilot: pilot training standards, airline operational doctrine, air traffic control, and regulatory frameworks. The technology is one layer of a much larger system.


The governance work done at Partial Autonomy is the same work that produces High Autonomy. Organizations investing in it now are not waiting for a future capability release. They are building the foundation on which High Autonomy operates.


## **What This Means for the Road Ahead**


The first step toward Partial Autonomy is a policy decision. Define the conditions under which your organization will allow a system to act: which response actions, against which threat types, within what scope, under whose authority. Write it down, however rough. That document is the actual starting point. Without it, the tooling is irrelevant.


The work at Partial Autonomy is real, meaningful, and available now. Security teams that define accountability structures before deploying autonomous systems, build a record of AI efficacy in their specific environment, and treat governance as a prerequisite rather than an afterthought, are the ones that reach and sustain Partial Autonomy. They are also the ones best positioned for what comes next. That work produces a more integrated SOC — data, AI, and response operating as a unified system.


High Autonomy remains the north star. This clearly articulated ideal state stops organizations from settling too early. It is the same function that “zero trust” serves as an architectural principle: no organization fully achieves it. Every organization is better for pursuing it.


The tools are capable. The frontier models have advanced significantly since our maturity model was first introduced. The capability gap that once made waiting feel reasonable has narrowed. What remains is the institutional work. That work is always harder than buying a product, which is why vendors who are honest about it are worth paying attention to.


SentinelOne customers operating the Autonomous SOC are seeing it in their numbers: 75% faster investigations, 4x more threats handled, 42% fewer false positives5.[Read the IDC Business Value Snapshot.](https://assets.sentinelone.com/ai-siem/idc-bv-si-siem-snapshot-en)


## References


1 Gartner, “AI SOC Agents: Harnessing Innovation, Managing Expectations,” Kevin Schmidt, Alex Tytarenko, Steve Santos, 25 February 2026. G00841784.


2 Gartner, “AI SOC Agents: Harnessing Innovation, Managing Expectations,” Kevin Schmidt, Alex Tytarenko, Steve Santos, 25 February 2026. G00841784.


3 SAE International, “Taxonomy and Definitions for Terms Related to Driving Automation Systems for On-Road Motor Vehicles,” SAE Standard J3016_202104, April 2021.[https://www.sae.org/standards/content/j3016_202104/](https://www.sae.org/standards/content/j3016_202104/)


4 Gartner, “AI SOC Agents: Harnessing Innovation, Managing Expectations,” Kevin Schmidt, Alex Tytarenko, Steve Santos, 25 February 2026. G00841784.


5 IDC Business Value Snapshot, “The Business Value of SentinelOne Singularity AI SIEM,” Michelle Abraham and Matthew Marden, May 2026, sponsored by SentinelOne. #US54435826-BVS.


***Third-Party Trademark Disclaimer***


*All third-party product names, logos, and brands mentioned in this publication are the property of their respective owners and are for identification purposes only. Use of these names, logos, and brands does not imply affiliation, endorsement, sponsorship, or association with the third party.*
