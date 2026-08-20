---
schema_version: "1.0.0"
document_id: "007cb0b989069f2e6165fd70a7c3d0d3af604498ee4c198f6ba3a4e193db7ec8"
company_key: "forrester-research-inc-common-stock"
company: "Forrester Research Inc."
source_id: "forrester-research-inc-common-stock-rss-7ea008fcdcc6"
canonical_url: "https://www.forrester.com/blogs/the-no-regrets-ai-investment-agenda/"
published_at: "2026-08-12T18:47:05+00:00"
first_seen_at: "2026-08-12T20:54:57.982037+00:00"
fetched_at: "2026-08-12T20:55:00.670917+00:00"
content_hash: "sha256:2cd1a3e766ab09e064a6e8cbdde587e64138c76214a4fc402d4fa941180eeb91"
---

# The No-Regrets AI Investment Agenda

The AI industry continues to make sweeping claims about autonomous agents, self-managing workflows, and enterprises run at machine speed. The reality is more complicated. AI is generating real value, but most of that value remains tightly scoped. Coding productivity is improving. Customer support workflows are becoming more efficient. Information work is accelerating. Yet the enterprise-level gains remain difficult to identify.


Many commentators respond by arguing that organizations must adopt radically new operating models. Perhaps. But before redesigning decision rights and reporting structures, it is worth asking a more fundamental question: what exactly are we trying to enable?


The current conversation often assumes that autonomy is inherently desirable. I am skeptical. We do not maximize autonomy in human organizations. We do not encourage employees to operate without controls, accountability, or supervision. Why would we expect a different answer for software?


Autonomy is a design choice. The responsibility of the technology leader is not to maximize it, but to bound and control it.


This observation leads to five questions that every organization deploying AI should be asking. Taken together, they define the foundations of bounded autonomy.


## **Identity: Who Or What Is It?**


Most large enterprises already carry substantial technical debt in digital identity. Over-provisioned service accounts, shared credentials, weak ownership, and unclear accountability are familiar problems. Deterministic software tolerated many of these weaknesses. Goal-seeking systems turn them into active hazards.


Recent security research is increasingly focused on agent identity, privilege abuse, and tool misuse. This should surprise no one. An agent can only act through the authority it has been granted. If that authority is poorly governed, the risk follows directly.


There is also an economic dimension. Agentic systems create ongoing operational costs, making inventory and accountability prerequisites for effective TokenOps and governance.


The initial investment implication is straightforward: know the actors.


Organizations will need the equivalent of an application portfolio for agents. Agent identities should be distinct. Their sponsors should be known. Their permissions should be bounded. Every agent should trace back to an accountable human authority. They should also have an explicit lifecycle, including retirement and decommissioning. While not glamorous, inventory is still foundational.


See the[AEGIS framework](https://www.forrester.com/blogs/introducing-aegis-the-guardrails-cisos-need-for-the-agentic-enterprise/) from our colleagues in Forrester’s Security & Risk service.


## **Capability: What Can It Do?**


AI capability remains remarkably jagged. A system may perform brilliantly on one task and fail unexpectedly on an adjacent one. We continue to see examples of models achieving extraordinary results on sophisticated benchmarks while struggling with activities that humans find routine. Benchmark performance is useful evidence. It is not operational assurance. The “jagged technological frontier” remains very real.


The corresponding investment is to equip the actors.


This sounds revolutionary until you look closely. MCP may be new, but APIs are not. Platform engineering and reusable business services are not new. The organizations best positioned for agentic AI are frequently the same organizations that have spent the last decade building internal platforms and treating technology capabilities as products.


Lendi provides a useful example. Its AI strategy is built on substantial prior investment in platform services, shared data resources, orchestration capabilities, and reusable business functionality. Their mortgage agents succeed because they stand on top of a platform foundation.


One of the genuinely new ideas emerging in AI is the concept of reusable skills. We now have emerging standards for packaging instructions, resources, and code into portable capabilities that can be reused across agents and environments.


However, possessing a skill is not the same thing as demonstrating competence. Proficiency is established in the real world, under real constraints, serving real customers, and there is limited real world demand.


## **Meaning: How Does It Understand?**


Semantic fragmentation is a growing AI hazard. What happens when one agent has the enterprise definition of customer, another inherits a vendor definition, and a third relies on a departmental interpretation? Human organizations have wrestled with these problems for decades. AI amplifies them.


A healthcare executive recently described this problem to me as the equivalent of drug interactions. Any individual definition may be fine. The unexpected effects emerge when the definitions interact.


The investment implication is to ground the actors with context.


Metadata, ontologies, semantic models, knowledge graphs, capability maps, and context graphs all become increasingly important. To be clear, we are looking for semantic **alignment** , not semantic **unification** .


A common objection is that increasingly capable models will simply infer meaning from messy enterprise environments. Perhaps they will infer meaning more effectively than they do today. The harder problem is authority. Which definition of “customer” is the sanctioned one for a regulated process? Which definition governs a financial report? Those are governance questions, not inference questions, and must remain deterministic.


What is needed is a dynamic, learning, navigational infrastructure: a way for humans and machines to understand how concepts (which themselves evolve and drift) relate across organizational boundaries.


## **Confidence: How Do We Know It’s Right?**


Software engineering has long distinguished verification from validation. Building the thing right is different from building the right thing. The distinction matters even more with AI.


The investment is assurance through guardrails and evaluation, and again this is not new — precursors are clear to see in DevOps practices of continuous integration and delivery, policy as code, and the like.


One intriguing development is the growing use of AI itself as part of the evaluation process. We are also seeing organizations encode architectural standards, security policies, and development conventions directly into AI working environments. The objective is simple: influence outputs as they are generated rather than auditing them after the fact. Evaluation is the emerging control of AI output using techniques like “LLM as judge.”


As accountability requirements increase, so will the investments required to sustain confidence.


## **Control: How Do We Keep It Aligned?**


Governance ultimately is a problem of feedback.


This has been the trajectory of enterprise IT. From agile to continuous integration to continuous delivery to DevOps and product management, all reflect the same underlying idea: faster learning through tighter feedback loops.


AI accelerates.


The most credible visions of AI autonomy center on feedback loops. The idea is that an AI system can take action, observe the consequences, evaluate the results, and incorporate what it learns into future behavior. Product leaders should recognize this immediately. It is simply the product feedback loop operating at machine speed.


Organizations need continuous visibility into agent behavior, outcomes, costs, and risks. They need the ability to intervene, redirect, and recover when systems behave unexpectedly. They need situational awareness rather than periodic inspection. And they need feedback loops capable of operating at the speed of the systems being governed. And systems to manage systems of such loops.


Analysts and advisors have been recommending these investment categories well before ChatGPT, and many organizations have been maturing all these dimensions, to their benefit. This is why we consider them “no regrets” investments, with value across a wide range of possible AI futures.


Start with inventory and accountable identity. Build governed capabilities on top of that foundation. Ground them in enterprise context. Add assurance proportionate to risk. Finally, create the visibility and feedback loops needed to steer the system in operation.


Every one creates value today.


The future of AI remains uncertain. The infrastructure required to manage it is considerably less so. Organizations that invest in these foundations will benefit regardless of whether the future arrives as a swarm of autonomous agents or simply a steadily expanding collection of increasingly capable software systems. Invest in the foundations. AI is the new forcing factor, and the consequences of neglect will be harder and harder to ignore.


To explore these ideas further, join us at one of Forrester’s upcoming[Technology & Innovation](https://www.forrester.com/events/technology/) events in Austin, London, or New York City. You’ll gain practical guidance from Forrester analysts and peers on building the governance, capabilities, and organizational foundations required to scale AI and deliver lasting business value.


Share


-
-
