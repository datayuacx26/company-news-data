---
schema_version: "1.0.0"
document_id: "9f83b24001dd6c480396a83c1dae82d8d8c6f56ecf4bbfcc62f73a6e6c88e7b4"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-e107b7ff8c21"
canonical_url: "https://www.elastic.co/blog/soc-platform-architecture-agentic-security"
published_at: "2026-07-27T00:00:00+00:00"
first_seen_at: "2026-07-27T21:07:22.969409+00:00"
fetched_at: "2026-07-28T20:32:33.872616+00:00"
content_hash: "sha256:f467d0122ba1a15fc46a6feb0f16ece16f374deab5ffebdb22f1394e32bb8556"
---

# From tool procurement to platform architecture: Rethinking the SOC for machine-speed threats

# From tool procurement to platform architecture: Rethinking the SOC for machine-speed threats


By


[Joe DeFever](https://www.elastic.co/blog/author/joe-defever)


July 27, 2026


- Share on Twitter


Share on Twitter


- Share on LinkedIn


Share on LinkedIn


- Share on Facebook


Share on Facebook


- Share by Email


Share by Email


- Print this page


Print


The gap between attacker speed and defender readiness is widening. Attackers can now move from initial access to full domain control in less than a minute using AI.


1


Large language model-generated phishing campaigns are achieving click-through rates 4.5 times higher than traditional methods.


2


Most enterprise SOCs weren't built for this tempo and fidelity. They were assembled tool by tool with over a decade of point-solution buying: a SIEM here, an XDR there, a SOAR bolted on top, and a growing collection of dashboards no analyst has time to reconcile. The result is a security architecture that taxes the defender before the adversary even arrives.


For security teams, the question worth asking in 2026 isn't which product to buy next. It's whether the underlying architecture can support the speed, transparency, and economics that AI-era defense now requires.


## The fragmentation tax on security operations


Fragmentation of data isn't an aesthetic problem. It's an operational and financial one that shows up in every incident timeline.


Analysts navigate an average of 11 security consoles during an active investigation, and 91% of security teams trace a serious incident directly back to friction between disconnected tools.


3


SOC team members are spending far too much time each week manually aggregating data across those tools. While this is certainly a productivity problem, it’s also, more importantly, a risk exposure directly attributable to architectural choices.


The financial layer compounds it. Per-endpoint pricing forces coverage decisions to become budget decisions. Rehydration fees on historical data create blind spots during active incidents when full context matters most. Separate SOAR licensing turns automation into a line item instead of a native capability. Each of these is a vendor-imposed tax that quietly shifts risk from the vendor's balance sheet to yours.


## Why AI can't repair a fragmented foundation


The industry's answer to fragmentation has been to layer AI on top of it. But this approach is already showing its limits.


AI models — whether they’re classification systems, machine learning detectors, or agentic reasoning frameworks — depend on unified access to historical context. When telemetry is scattered across dozens of systems in inconsistent formats, models inherit the same blind spots as the humans they're meant to augment. This isn’t a solid foundation for autonomous reasoning so much as it is a foundation for expensive hallucination.


For security teams, AI investments made on top of fragmented data infrastructure will underperform their business case.


[Retrieval augmented generation](https://www.elastic.co/lp/retrieval-augmented-generation) (RAG), agentic workflows, and behavioral analytics all assume the model can reason across the full attack surface in real time. If the underlying data can't be queried in place, the AI layer becomes yet another silo, not a solution.


## The architectural inflection point


Two signals suggest architectural questions are moving from theory to procurement.


1.


**Adoption pressure:**


Nearly two-thirds of organizations are experimenting with AI agents in security operations, but


[fewer than one in four have deployed them into production](https://www.elastic.co/security-labs/why-2026-is-the-year-to-upgrade-to-an-agentic-ai-soc) . That gap reflects the difficulty of running agentic systems on infrastructure not designed for them. Governance models, transparency requirements, and cost controls are maturing rapidly, but they require an architecture that supports them natively.


2.


**Executive expectation:**


80% of CISOs are prioritizing AI-driven security in their 2026 budgets.


4


Boards aren't approving those budgets for incremental improvements. They expect measurable changes in mean time to detect (MTTD), mean time to respond (MTTR), and analyst retention. Delivering on that expectation with a stack designed for a pre-AI threat model is unlikely.


## What an open, unified security architecture actually requires


A platform built for the AI era looks different from a SIEM with an AI feature bolted on. Security teams evaluating architectural direction should hold vendors to a specific set of criteria:


-


**Unified telemetry across domains:**


Endpoint, identity, network, and cloud signals flow into a single detection layer, not separate ones stitched together at query time.


-


**Open standards as first-class citizens:**


Open schemas and frameworks like OpenTelemetry (OTel) and


[Elastic Common Schema](https://www.elastic.co/guide/en/ecs/current/index.html) are supported natively, so detections written once work across environments.


-


**Query in place, not constant ETL:**


Analysts run a single query across hot, warm, and archival tiers without rehydration penalties or migration projects.


-


**Model-agnostic AI with transparent reasoning:**


Every autonomous decision produces an auditable trace of tools invoked, evidence retrieved, and reasoning applied.


-


**Native automation, not bolted-on SOAR:**


Response actions live inside the platform, so containment doesn't depend on brittle integrations that fail during active incidents.


-


**Deployment flexibility across regulatory boundaries:**


On-premises, cloud, hybrid, and air-gapped options are available for organizations governed by GDPR, HIPAA, DORA, or PCI DSS.


Each item corrects a specific structural weakness of the tool-procurement approach. Together, they define what "unified" actually means beyond the marketing category.


## Agentic operations: From alert management to attack neutralization


When the architecture is right, the operational model can finally change. This is where agentic AI moves from concept to production.


An


[agentic security operations platform](https://www.elastic.co/security/ai) is not an autonomous SOC. The distinction matters. Autonomous systems remove humans from the loop and, in a security context, that's neither responsible nor auditable. Agentic systems move the human to the top of the loop: The platform investigates, correlates, and stages a response plan, while the analyst reviews the evidence, applies judgment, and approves the action.


The practical difference shows up in the incident timeline. In a traditional SOC, three related alerts (e.g., a suspicious login, an unusual process execution, and an anomalous outbound connection) might arrive in three different tools within seconds of each other and take hours to correlate. In an agentic model, they surface as a single high-priority attack chain with the reasoning trace already built. Total time from first alert to approved response can be compressed substantially.


For security teams, this shifts the operational metric from alerts triaged to attacks neutralized.


## Building the SOC for what's next


The security teams who move ahead successfully over the next 24 months won't be the ones with the biggest, most expensive tool inventories. They'll be the ones whose architecture supports speed, transparency, and agentic operations as native properties, not features added later.


That shift starts with a candid assessment of where the current stack imposes taxes on the defender: coverage gaps caused by pricing, response delays caused by integration friction, AI initiatives constrained by fragmented data. Each of those is a solvable architectural problem. Solving them is what separates a SOC that keeps pace with AI-powered threats from one that continues to fall behind.


For a deeper look at how open, unified architecture reshapes detection and response, check out


[our article on how to architect a unified, open security platform](https://www.elastic.co/resources/article/siem-vs-xdr) .


**Sources:** ****


1


TechRadar, “


[Security at machine speed: why the SOC must be rebuilt for the AI era](https://www.techradar.com/pro/security-at-machine-speed-why-the-soc-must-be-rebuilt-for-the-ai-era) ,” June 2026.


2


The Register, “


[AI makes phishing 4.5x more effective, Microsoft says](https://www.theregister.com/special-features/2025/10/16/ai-makes-phishing-45x-more-effective-microsoft-says/1050375) ,” October 2025.


3


Microsoft, “


[State of the SOC](https://info.microsoft.com/ww-landing-state-of-the-soc.html?lcid=en-us) ,” 2025.


4


Ctech, “


[CISOs to pour 2026 budgets into AI as cybersecurity priorities shift](https://www.calcalistech.com/ctechnews/article/te4mdxjye) ,” February 2026.


*The release and timing of any features or functionality described in this post remain at Elastic's sole discretion. Any features or functionality not currently available may not be delivered on time or at all.*


*In this blog post, we may have used or referred to third party generative AI tools, which are owned and operated by their respective owners. Elastic does not have any control over the third party tools and we have no responsibility or liability for their content, operation or use, nor for any loss or damage that may arise from your use of such tools. Please exercise caution when using AI tools with personal, sensitive or confidential information. Any data you submit may be used for AI training or other purposes. There is no guarantee that information you provide will be kept secure or confidential. You should familiarize yourself with the privacy practices and terms of use of any generative AI tools prior to use.*


*Elastic, Elasticsearch, and associated marks are trademarks, logos or registered trademarks of elasticsearch B.V. in the United States and other countries. All other company and product names are trademarks, logos or registered trademarks of their respective owners.*


## Share


- Share on Twitter


Share on Twitter


- Share on LinkedIn


Share on LinkedIn


- Share on Facebook


Share on Facebook


- Share by Email


Share by Email


- Print this page


Print
