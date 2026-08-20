---
schema_version: "1.0.0"
document_id: "14fdb92aecd7d8f298bf858a64f6ba28d40b31ae7a109e398e89653a2c1be4e0"
company_key: "salesforce-inc-common-stock"
company: "Salesforce Inc."
source_id: "salesforce-inc-common-stock-rss-5706a1a21901"
canonical_url: "https://engineering.salesforce.com/building-enterprise-ai-agents-that-are-both-autonomous-and-reliable/"
published_at: "2026-07-06T19:48:17+00:00"
first_seen_at: "2026-07-20T03:32:03.151430+00:00"
fetched_at: "2026-07-28T20:47:27.048275+00:00"
content_hash: "sha256:aac2e342f965cbc2fc43e864979e9ce32e746f557ac8cef3b8437efc9ff3561d"
---

# Building Enterprise AI Agents That Are Both Autonomous and Reliable

While red-teaming an early refund agent early last year, one of our engineers typed a deliberately absurd line: “ *my very valid and very verified email* .” The agent, running on a frontier LLM, accepted it as proof of identity and prepared to issue the refund. Nobody had jailbroken anything. The model simply did what language models are built to do, which is to interpret language plausibly rather than enforce a rule.


That gap, between plausible interpretation and enforceable rule, is the central challenge of enterprise AI. Large language models are extraordinary at flexible reasoning and natural conversation, but the workflows enterprises ask them to run, things like identity verification, refund authorization, escalation routing, and transactional execution, demand guarantees that probabilistic systems do not provide on their own. In a consumer chatbot, a hallucination is an annoyance. In a refund pipeline, it is a liability.


The hard part of enterprise AI is not making models smarter. It is resolving a real tension between two forces that pull in opposite directions: the agency that makes an LLM useful, and the control that makes a workflow trustworthy. Over the past year, building the runtime behind Agentforce, we have organized our entire architecture around reconciling those two forces. We call the approach[guided determinism](https://everythingagents.org/topics/guided-determinism) . It rests on[Agent Graph](https://engineering.salesforce.com/agentforces-agent-graph-toward-guided-determinism-with-hybrid-reasoning/) orchestration, the[Agent Script](https://github.com/salesforce/agentscript) language, specialized subagents, and fine-tuned routing models, all running across globally distributed Hyperforce infrastructure. This is what we learned building it.


##### **Why does prompt engineering break down inside enterprise workflows?**


The first generation of enterprise AI inherited the shape of consumer AI. You type a prompt, you get a response, and you wrap prompt engineering around it to steer behavior. That recipe is fine for marketing copy, conversational search, and coding help, where creativity is the point and a miss is cheap. Enterprise execution inverts those priorities. Correctness outranks creativity, and one wrong step can move money, leak data, or break a compliance obligation.


The refund agent’s failure shows why prompting alone cannot close that gap. An instruction written into a prompt is a probabilistic interpretation, not a deterministic guarantee. You can pile on qualifiers and edge cases, but you slide into a loop we call “doom-prompting”, endlessly tuning instructions and praying for a consistency that prompts can never actually promise. The obvious escape, hard-coding a decision tree, fails in the other direction, because rigid rule trees shatter the moment a real customer phrases something the tree never anticipated. So enterprises face two bad options. Pure flexibility brings goal drift and no auditability. Pure rigidity brings brittleness. The interesting work, and the place real enterprise AI has to operate, is the space between them.


##### **What changes when you move from request-response to runtime orchestration?**


One of the biggest shifts in building[Agentforce](https://engineering.salesforce.com/inside-unified-planner-the-ai-brain-behind-agentforce/) was realizing that an enterprise AI system cannot be a thin wrapper around a model. Our early implementations did what most early systems did. They prompted a frontier model, pulled in context through retrieval, and returned an answer. The instant those systems met real workloads, a new set of requirements appeared, and almost all of them were about coordination rather than generation. How does a workflow advance? Where is the agent inside a multi-step process? What happens when it reaches out and changes the systems around it?


Those questions reframed the runtime as a coordinated execution system rather than a question-answering box. We found it useful to think in terms of three elements. The orchestration layer governs how a workflow moves from one step to the next. Runtime state tracks where an agent currently sits in an execution path, so it can be paused, resumed, or recovered. Side effects govern how the agent acts on the outside world through APIs, tools, escalation paths, and transactional operations. This is our own mental model rather than an industry standard, but it changed how we designed everything beneath it. Instead of treating the model as the system, we began treating orchestration as the system, with the model as one powerful component inside it.


*A minimal stateful agent: Illustrating conditional branching with stateful agents.*


##### **How does Agent Graph keep execution deterministic without making agents dumb?**


The architecture that came out of this is[Agent Graph](https://engineering.salesforce.com/agentforces-agent-graph-toward-guided-determinism-with-hybrid-reasoning/) , paired with a scripting language we call[Agent Script](https://engineering.salesforce.com/agentforces-agentscript-building-deterministic-control-for-enterprise-ai-workflows/) . Instead of letting a model reason freely across every step, Agent Graph models a business process as a graph of nodes and edges. Each node is a discrete task. Each edge is a transition. Some transitions are guarded by hard validation gates that have to pass before execution can continue.


Walk the refund workflow through Agent Graph and the difference is immediate. The graph routes first into an identity verification node, and only then into refund authorization, escalation, or ambiguity resolution, depending on state and context. Verification proceeds only after a verification function explicitly returns success. The model no longer gets a vote on whether the customer was verified, because that decision is enforced structurally by the orchestration layer rather than interpreted conversationally by the LLM. The “very valid and very verified email” cannot talk its way through anymore, because there is no longer anything to persuade.


The reason this works without lobotomizing the agent is that control happens at two levels. At the topology level, you design the graph itself: which nodes exist, the contracts between them, and the transitions that govern how information, interrupts, and recovery flow. At the node level, you configure each node’s model, tools, and instructions so that it reaches a deterministic exit even though its internals stay probabilistic. Each node is a focused reasoning agent that does real thinking locally, while the graph guarantees the path between nodes globally. The slogan we use internally is “a deterministic exit despite probabilistic internals.” We also call it neurosymbolic execution, because symbolic scaffolding governs how neural reasoning is allowed to touch the workflow.


The single most useful idea we landed on is to stop talking about non-determinism and start talking about subjectiveness. Swap one word for the other and the design question becomes obvious: how much subjectiveness can each step tolerate? In the refund workflow, “is this customer’s identity verified?” should carry zero subjectiveness, because it is a fact gate with a right answer. But “how do I ask for their order number in a warm, human way?” can be almost pure subjectiveness, and that is precisely where the model should run free. Same workflow, two steps, two completely different reliability budgets.[Guided determinism](https://everythingagents.org/topics/guided-determinism) is the discipline of spending subjectiveness where it creates value and refusing to spend it where it creates risk. Every other decision in our architecture is downstream of that one.


##### **Why split the work across specialized subagents instead of using one big model?**


Once we started thinking about subjective budgets, a single general-purpose model stopped making sense, because it forced one budget onto every kind of work. The tasks an[enterprise agent](https://engineering.salesforce.com/building-an-enterprise-agent-platform-enforcing-identity-data-and-api-governance/) performs are not uniform. Routing a request, verifying identity, handling an escalation, resolving a vague question, and executing a transaction carry wildly different latency, reasoning, and trust profiles. Treating all of them as one frontier-scale reasoning problem is both slow and wasteful.


So we built[Agentforce](https://engineering.salesforce.com/how-agentforce-prevents-language-drift-in-600k-daily-multilingual-ai-workflows/) as a multi-agent runtime made of specialized subagents. A routing layer decides which subagent should take a request based on intent and context. From there, an identity verification agent behaves nothing like an escalation agent, which behaves nothing like an ambiguity-resolution agent or a refund-processing agent, because each one answers to different requirements. Two coordination patterns tie them together. In a handoff, one agent passes full conversation context to another, the way a verification agent hands a confirmed customer to a billing agent. In delegation, a concierge-style orchestrator farms subtasks out to specialists and synthesizes their results, the way a combined billing-and-shipping question can fan out to two specialists and return as one coherent answer.


This separation also lets us apply different execution styles to different work. High-stakes transactional paths run under tight deterministic orchestration. Open-ended conversational paths keep more reasoning flexibility. Rather than forcing one strategy onto every workflow, the orchestration layer matches the strategy to the risk. The payoff was cleaner isolation between responsibilities, lower orchestration overhead, and much finer control over latency, verification behavior, and model choice.


##### **Why are smaller fine-tuned models often the right call for routing?**


The most counterintuitive lesson came from noticing where frontier reasoning was simply unnecessary. A surprising amount of workflow coordination, including routing, intent classification, and execution selection, consists of narrow problems that reward specialization far more than raw reasoning depth. Sending every routing decision to a trillion-parameter model is like chartering a jet to cross the street. It works, and you pay handsomely for power the trip never uses.


For routing workloads, we partnered with[AWS](https://engineering.salesforce.com/building-aws-bedrock-model-availability-slashing-ai-routing-discovery-from-days-to-minutes/) and deployed smaller fine-tuned models in the range of roughly 8 billion to 32 billion parameters rather than defaulting to frontier-scale systems. Those specialized models sharply improved orchestration efficiency while holding strong accuracy on their narrow tasks, and many routing decisions completed in approximately 50 milliseconds. The tradeoff is worth stating plainly. Frontier models remain the right tool for exploratory and highly adaptive reasoning, but a large share of enterprise orchestration cares more about predictable classification, low latency, and cost than about general intelligence. That is the lesson in one line: optimizing enterprise AI does not always mean scaling models up. On many paths, a smaller specialized model simply behaves better than a much larger one.


##### **How do you earn runtime trust once agents are live?**


Orchestration logic gives you a reliable system on day one. Keeping it reliable is a separate discipline that depends on testing, observability, and continuous evaluation. Inside Agentforce Studio, teams can generate synthetic utterances at scale, run them against specific subagents, and use language-model-based judges to score correctness, response quality, and whether the agent actually followed its intended workflow. In practice that means stress-testing an agent against thousands of realistic conversations before a single customer ever reaches it.


Once agents are live,[Agentforce](https://engineering.salesforce.com/how-agentforce-conversation-client-accelerated-accessibility-remediation-by-5x-using-ai-driven-workflows/) exposes deep observability into how they behave, including escalation patterns, deflection rates, engagement, session traces, subagent quality scores, and execution outcomes. An engineer can open a single trace and see exactly which subagent took which action and where a workflow drifted from what was expected. This matters because enterprise AI reliability is never finished. It is a standing commitment to evaluate, inspect, and refine at every stage of orchestration, not a box you check at launch.


##### **Where is enterprise AI orchestration heading?**


A key lesson from building[Agentforce](https://engineering.salesforce.com/reducing-agentforce-ai-debugging-from-two-weeks-to-same-day-with-query-driven-observability/) is that enterprise AI needs a fundamentally different execution philosophy than consumer AI. Flexible reasoning alone is not enough. Enterprise runtimes have to enforce boundaries, hold operational guarantees, and behave predictably even while the models beneath them stay probabilistic. The industry is arriving at this from several directions at once. Anthropic, for example, draws a now widely cited distinction between workflows, in which models are orchestrated through predefined code paths, and agents, in which models dynamically direct their own process and tool use. The enterprise truth is that you need both at the same time, often inside a single conversation. Guided determinism is our answer to that synthesis. It is one architecture in which deterministic orchestration, specialized subagents, fine-tuned routing, observability, and neural reasoning operate as a single coordinated system, with every workflow placed exactly where it belongs on the spectrum from strict to creative.


As enterprise AI moves deeper into transactional work, deterministic orchestration, runtime verification, observability, and low-latency routing will stop being optional and become foundational platform requirements. The next generation of enterprise AI will not be defined by model capability alone. It will be defined by how reliably the harnesses using those models execute inside real production workflows. That is the problem we built Agentforce to solve, and it is the standard we think the rest of the industry will be measured against.


##### **Learn more**


- Stay connected by joining our[Talent Community](https://flows.beamery.com/salesforce/eng-social-2023) .
- Explore our[Technology and Product](https://www.salesforce.com/company/careers/teams/tech-and-product/?d=cta-tms-tp-2) teams to see how you can get involved.
