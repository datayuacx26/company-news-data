---
schema_version: "1.0.0"
document_id: "9392067608251088799561bddd80d1e8cf94d8923e782adf4ddfe96bef622d8a"
company_key: "salesforce-inc-common-stock"
company: "Salesforce Inc."
source_id: "salesforce-inc-common-stock-rss-5706a1a21901"
canonical_url: "https://engineering.salesforce.com/inside-unified-planner-the-ai-brain-behind-agentforce/"
published_at: "2026-06-29T18:00:43+00:00"
first_seen_at: "2026-07-20T03:32:03.151430+00:00"
fetched_at: "2026-07-28T20:47:51.174373+00:00"
content_hash: "sha256:446c4bd477024505383fffc098da6a3705777cf5ee405e67dd41cb77be7154aa"
---

# Inside Unified Planner: The AI Brain Behind Agentforce

In our Engineering Energizers Q&A series, we highlight the engineering minds driving innovation across Salesforce. Today, we spotlight Gaurav Aggarwal, a Software Engineering Architect. Gaurav developed the Unified Planner, the new AI execution and reasoning engine behind[Agentforce](https://engineering.salesforce.com/how-agentforce-prevents-language-drift-in-600k-daily-multilingual-ai-workflows/) . All new Agentforce agents run through Unified Planner across voice, text, and chat experiences, and the platform reduced response latency from roughly **20 seconds to 2.3 seconds** in some cases while unifying previously separate execution architectures.


Explore how Gaurav’s team unified previously separate AI agent runtimes across Agentforce and MuleSoft while significantly reducing response times, and how his team designed a single AI execution engine capable of supporting voice, chat, text, and future interaction modes without fragmenting the platform.


##### **What is your team’s mission as it relates to Unified Planner and Agentforce?**


Our mission is to provide a single AI runtime capable of powering every[Agentforce](https://engineering.salesforce.com/how-agentforce-conversation-client-accelerated-accessibility-remediation-by-5x-using-ai-driven-workflows/) interaction, regardless of whether it originates from voice, chat, text, or future modalities. Unified Planner serves as the execution and reasoning layer behind Agentforce, allowing developers to build agents once while relying on a common runtime that handles execution, observability, memory, tool orchestration, and model integration.


That mandate sounds straightforward until you consider the constraints. Voice requires extremely low latency. Complex enterprise workflows require deep reasoning and orchestration.[MuleSoft](https://engineering.salesforce.com/how-mulesoft-is-raising-the-trust-bar-for-ai-generated-code/) and other platforms need the same underlying execution capabilities without Salesforce-specific dependencies. Future modalities like video introduce entirely new interaction patterns. And all of this has to work while customers expect extensibility and platform teams require consistency.


Simple solutions fall short because they optimize for one constraint at the expense of another. Systems built for speed often sacrifice flexibility. Systems optimized for reasoning frequently introduce latency. Separate runtimes solve local problems but create long-term fragmentation.


The challenge isn’t building an AI runtime. The challenge is building a single AI runtime that powers every Agentforce agent across voice, chat, text, MuleSoft integrations, and future interaction modes without sacrificing latency, extensibility, or platform consistency.


*Building blocks of Unified Planner: The kernel that all harness components plug into.*


##### **What limitations in previous systems made it difficult for Agentforce agents to operate consistently across voice and chat experiences?**


Before Unified Planner, different execution systems had evolved independently. Agentforce’s[Agent Graph](https://engineering.salesforce.com/agentforces-agent-graph-toward-guided-determinism-with-hybrid-reasoning/) focused on reasoning and orchestration, while Voice Planner optimized for low-latency interactions. Both solved important problems, but over time they developed separate capabilities, architectural assumptions, and operational models.


As Agentforce adoption expanded, maintaining those differences became increasingly costly. Features built for one runtime often required separate implementations in the other, creating duplication and slowing down innovation across the platform. Developers also encountered different capabilities depending on which execution engine their agents used, making the overall experience less predictable.


Rather than continuing to maintain parallel systems, the team separated platform-managed concerns like prompt injection detection, interruption handling, execution infrastructure, and AI runtime services from customer-defined business workflows. **That separation made it possible to unify previously isolated runtimes while preserving the specialized capabilities that made each system successful** , resulting in a common execution foundation that delivers a more consistent development and operational experience across[Agentforce](https://engineering.salesforce.com/agentforces-agentscript-building-deterministic-control-for-enterprise-ai-workflows/) .


##### **When agents were taking roughly 20 seconds to respond, how did Unified Planner reduce latency to approximately 2.3 seconds?**


The biggest change was aggressively embracing parallel execution. Many operations that had historically run sequentially were redesigned to run concurrently whenever dependencies allowed.


Tasks like prompt injection detection, citation generation, grounding validation, knowledge retrieval, and context gathering each added latency individually. Taken together, they created noticeable delays throughout the request lifecycle. Waiting 20 seconds for a response might be tolerable in some chat scenarios, but it becomes unacceptable for voice interactions where responsiveness directly shapes the user experience.


The team redesigned the AI execution engine so platform services could execute in parallel, then extended that same capability to customer workflows through configurable parallel tool execution. **Multiple tool calls that previously ran one after another can now execute simultaneously when dependencies allow.** Model selection was also optimized, giving developers the ability to choose models aligned with their own latency and reasoning requirements while using lightweight internal classifiers for specialized tasks.


Together, those architectural changes reduced response times from roughly **20 seconds to approximately 2.3 seconds,** making real-time AI interactions significantly more practical.


##### **What made extensibility the hardest challenge when designing a single execution engine for Agentforce, MuleSoft, and future interaction modes?**


The hardest challenge was designing for requirements that didn’t yet exist. Voice introduced strict latency constraints. Agent Graph required sophisticated orchestration.[MuleSoft](https://engineering.salesforce.com/from-concept-to-reality-developing-mulesofts-new-flex-gateway-api-management-solution/) needed a runtime capable of operating independently of Salesforce-specific implementations. Future interaction modes like video introduce execution patterns that are still evolving. And on top of all that, every team wanted the ability to extend the platform without touching the core runtime itself.


A straightforward unification effort would have produced a monolithic system optimized for current requirements but difficult to evolve as the ecosystem expanded. Instead, the team focused on reusable execution primitives that support different reasoning patterns without prescribing a single implementation strategy. The core AI execution engine stays shared, while individual products extend it through integrations, policies, and platform-specific services.


This architecture allows Agentforce and MuleSoft to share the same execution foundation while preserving the flexibility required for different deployment models, customer requirements, and future innovations.


##### **What made migrating production agents onto Unified Planner difficult to execute safely?**


The challenge was replacing the reasoning layer underneath production agents without disrupting customer experiences. Different agents had evolved under different assumptions, prompting strategies, and model behaviors. Because large language models are inherently probabilistic, even small changes can produce observable differences in output. So, what looks like a minor runtime change can alter behavior in ways customers notice immediately.


Unified Planner also introduced capabilities that weren’t consistently supported across every client. Some interfaces could immediately consume new functionality, while others needed additional work before supporting the same experiences. Voice created a unique wrinkle because historical systems treated the end of a phone call as the end of a session, while Unified Planner introduced session portability across interaction modes.


Successfully executing the rollout required extensive testing, customer validation, selective feature activation, and close collaboration across teams. By treating migration as an engineering problem rather than a deployment exercise, the team was able to transition customers while minimizing disruption.


##### **As Agentforce expands into video and future interaction modes, what engineering challenges are emerging next?**


The next major challenge is extending reasoning systems beyond text and voice into fully multimodal environments, supporting goals based open-ended reasoning. Video adds a another new layer of complexity because information arrives through multiple streams simultaneously, including visual content, spoken language, contextual signals, and user interactions. Unlike traditional text-based systems, multimodal experiences require the AI runtime to determine how those signals should be represented, interpreted, and incorporated into reasoning workflows.


Two approaches are emerging. One transforms continuous streams into structured inputs before reasoning begins. The other leans more heavily on multimodal foundation models that handle those transformations internally. Both introduce tradeoffs across performance, accuracy, operational complexity, and extensibility, and determining which delivers the best balance remains an active area of exploration.


As multimodal systems continue to evolve, solving these challenges will shape how future generations of[Agentforce](https://engineering.salesforce.com/how-agentforce-data-and-apps-turned-the-salesforce-stack-into-agentforce-360/) agents reason, interact, and operate across increasingly diverse environments.


##### **Learn more**


- Stay connected — join our[Talent Community](https://flows.beamery.com/salesforce/eng-social-2023) !
- Check out our[Technology and Product](https://www.salesforce.com/company/careers/teams/tech-and-product/?d=cta-tms-tp-2) teams to learn how you can get involved.
