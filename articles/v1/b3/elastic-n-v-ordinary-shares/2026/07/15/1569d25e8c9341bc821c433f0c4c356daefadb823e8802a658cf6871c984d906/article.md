---
schema_version: "1.0.0"
document_id: "1569d25e8c9341bc821c433f0c4c356daefadb823e8802a658cf6871c984d906"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-rss-e107b7ff8c21"
canonical_url: "https://www.elastic.co/blog/the-future-of-governing-ai-agents"
published_at: "2026-07-08T00:00:00+00:00"
first_seen_at: "2026-07-25T01:11:00.469605+00:00"
fetched_at: "2026-07-28T21:08:44.176891+00:00"
content_hash: "sha256:9f89e347566982c5dcac8c53426d50d9473315ee6000961b03251662456da618"
---

# The future of governing AI agents

# The future of governing AI agents


How to build governance into autonomous security agents from the architecture up


By


[Marcus Jeffes](https://www.elastic.co/blog/author/marcus-jeffes)


July 8, 2026


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


The industry has moved fast on capabilities. Agents now triage alerts, investigate endpoints, create detection rules, and enrich indicators, and they are even capable of performing most actions we as security operators can perform. The architecture patterns are maturing, as are the models, but governance is not keeping pace. The discipline of proving your agents are doing the right thing continuously, under pressure, and at scale remains largely unsolved.


Most teams deploying agents in production today default to a tiered autonomy model where low-risk actions run autonomously, medium-risk get oversight, and high-risk require human approval. This tells you what an agent can do, but it tells you nothing about whether it's doing it well. A triage agent that classifies every alert as a false positive will never trigger a governance gate because it's operating within its approved tier.


Meanwhile, the regulations are not waiting.


[ISO 42001](https://www.iso.org/standard/42001) ,


[DORA](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32022R2554) ,


[NIS2](https://eur-lex.europa.eu/eli/dir/2022/2555) , and the


[EU AI Act](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32024R1689) are all converging on the expectation that you must prove your AI systems work as intended, that humans maintain meaningful oversight, and that you can explain how decisions are made. The assessment criteria specific to autonomous security agents don't exist yet, but the direction is clear. And the organizations building governance infrastructure now will be the ones ready when it arrives.


## Reasoning as infrastructure


Right now, how an agent reasons about a security investigation is baked into whichever large language model (LLM) powers it. Beyond that, probabilistic models have a tendency to not come with consistent outputs each time. Swap the model, or even have the same incident occur and the reasoning patterns change. The same alert with the same context can and produce different escalation decisions. A governance framework built around a given model’s tendencies can become less reliable when the model changes, or the model deviates due to weights outside of our control.


Reasoning is the core determinant of agent behavior. It is what decides whether an alert gets escalated or closed, whether lateral movement gets investigated, or whether a host gets isolated. When that reasoning is baked into the model, you rely on the model's general intelligence to compensate for the absence of defined methodology. We need to extract the reasoning into an explicit layer for investigation workflows. This in itself has layers of applicability. It might look like an instruction set or a knowledge base. We might invoke a workflow at runtime with baked-in escalation logic. We might change our decision based on evidence evaluation criteria and/or hypothesis generation patterns. When we define reasoning at multiple layers, the model's role then reduces to execution of tasks. A well-defined methodology executed by a smaller, cheaper model will outperform a frontier model guessing at methodology for your specific environment because the reasoning is grounded in operational context rather than the model's training data, allowing incidents to be handled faster and cheaper.


This matters in governance for three reasons.


**1. Governance requires predictability.**


Progressive trust assumes that the system being measured today is the same system that will operate tomorrow. If swapping a model changes reasoning patterns, the trust evidence accumulated under the old model doesn’t transfer, and you end up back in a maximum oversight state for every model change. The provider's concentration risk compounds this. If you need to switch providers due to pricing, deprecation, or regulatory designation, you lose both the model and the trust evidence.


**2. Evaluation requires consistency.**


If methodology is only consistent because of whichever model is running, you are unable to evaluate your own operations, as you will only ever be evaluating the model in question. Your ablation studies, benchmark suites, and quality metrics all become model-specific rather than system-specific, and every model change requires re-evaluation from scratch.


**3. Compliance requires explainability.**


When a regulator asks


*how does your agent decide to escalate an investigation?*


, the answer needs to be grounded in documented processes. If the answer comes down to


*ask the model provider*


, you can't demonstrate control. The regulator cares whether you can show them how decisions are made.


## 4-layer separation


The architecture that solves this separates into four layers.


### 1. Skills


Skills define what the agent can do across triage, enrichment, forensics, and detection engineering; each one is a composable unit of instructions, tools, and domain knowledge that defines the capabilities of the agent.


### 2. Reasoning


This defines how the agent thinks, which includes investigation methodology, escalation logic, evidence evaluation criteria, and hypothesis generation patterns. This layer is explicit, testable, and independent of which model executes it. When a triage decision follows a defined methodology, that methodology can be versioned, tested against benchmark suites, and updated through the same governance framework that gates every other consequential change. Because the methodology carries the complexity, the model doesn't have to. This is what makes it possible to run capable, governed agents on cost-effective models rather than defaulting to the most expensive frontier option for every task.


### 3. Models


While the LLM executes its own baked-in reasoning, the reasoning patterns are not the same for each model. Claude, Gemini, ChatGPT, or open source models all work either slightly or entirely differently, skewing results depending on how the model was trained and the guardrails put in place by the vendor.


### 4. Context


The depth of any investigation is bound by the platform's ability to pull environmental context into the agentic solution. A clear record of every agent decision, what evidence grounded it, and whether it was correct is what makes continuous measurement and benchmarking possible.


This separation directly addresses these three problems:


-


The trust evidence attaches to the reasoning methodology and away from the model. When you swap models, trust also transfers because the methodology hasn’t changed. You verify the new model executes the same reasoning patterns correctly and can validate performance.


-


Benchmark suites test the reasoning methodology, but model changes require a targeted check, and evaluation must be a constant baked into our operation (e.g., does the new model follow the methodology to the same standard?).


-


When a regulator asks how the agent decides to escalate, you can point to the reasoning layer with versioned, documented, and testable outcomes. When they ask about model risk, you point to the separation layer where models are swappable infrastructure. When they ask about concentration risk, you demonstrate that reasoning patterns are portable across providers.


## Progressive trust as an operating model


The idea of progressive trust is to start with maximum oversight, where every consequential action goes through a human. As evidence accumulates that the agent's recommendations are sound, oversight progressively relaxes. An agent that has been approved hundreds of times (with every recommendation justified in post-incident review) has earned a different quality of oversight than one deployed last week. This is only possible because the four-layer separation decouples trust from the model. The evidence attaches to the reasoning methodology, so it persists across model changes and accumulates over time rather than resetting with every update.


The inverse is equally important. If agent performance degrades due to model drift, data distribution changes, or novel attack patterns, we need metrics to surface it. When deterioration is observed, the response should be to tighten oversight by moving actions back to higher approval requirements, reduce auto-approve thresholds, and add evidence requirements. Trust earned over months can be revoked in seconds if the data warrants it.


But measuring trust requires the ability to observe not just what an agent did but also how it reasoned doing it.


## Evaluation as metrics and evidence


Matt Kiely, principal security researcher at Huntress, argues the need for measuring progressive trust and how progressive trust requires evidence. That evidence comes from continuous evaluation across four dimensions.


### 1. Classification accuracy


Did the agent get the right answer? This is measured through precision (how many of its positive classifications were correct?) and recall (how many actual positives did it catch?). This measurement is necessary; however, this alone tells us nothing about reliability. A correct conclusion reached through hallucinated reasoning is not a reliable agent.


### 2. Planning quality


Did the agent consider multiple hypotheses? Given a suspicious login from two distant locations in four minutes, did it consider impossible travel, token theft, and VPN usage? Or, did it tunnel-vision on the first hypothesis?


### 3. Retrieval quality


Did the agent find the right evidence? If the knowledge base contains relevant prior incidents and the agent never retrieves them, is it operating on incomplete information?


### 4. Grounding quality


Is the agent’s reasoning supported by the evidence it found? Did it claim “this IP is associated with APT28 infrastructure” because a knowledge base entry said so or because it hallucinated that connection? An agent with a high unsupported claim rate is dangerous even when its conclusions happen to be correct.


These four dimensions compose into the evidence that progressive trust requires. Classification accuracy tells you whether to trust the outputs. Planning, retrieval, and grounding quality tell you whether to trust the process.


Ablation testing makes this concrete. Build a benchmark suite with known ground truth, run the agent, and capture the full trajectory (i.e., every tool call, every knowledge base query, and every reasoning step) and then systematically vary the configuration:


-


Run with baseline only looking at the raw telemetry with no knowledge base, tools, or multistep reasoning. Can the model classify from signal alone?


-


Run without the knowledge base to isolate whether retrieval-augmented context actually improves outcomes


-


Run without investigation tools to determine whether tool use adds real value beyond what the model infers from telemetry and retrieved knowledge


-


Run the full agent with everything enabled as the comparison point


The difference between configurations reveals which components actually contribute to quality. If removing the knowledge base doesn’t change outcomes, either the knowledge base isn’t useful or the agent isn’t using it properly. These are the findings that feed back into system improvement.


## Monitoring the monitors


When AI agents become operational in security, you will need them not only to be monitoring and responding to security incidents, but you also need to be able to monitor and respond to agent based decisions. This is where we need to shift and tie observability of our agents directly with our security capability. Traditionally, we as security professionals have been focused on actions occurred; now, we must switch to why they were performed:


-


What evidence did the agent consider?


-


What hypotheses did it explore and discard?


-


What reasoning led to action?


-


Did it hallucinate a connection between two unrelated events?


-


Did it skip a critical investigation step?


-


Did its confidence score reflect genuine signals or did it create one?


This semantic gap is telemetry and assessment of that telemetry. One customer that has built a system to assess agent behavior across its estate is Uber with its


[Agentic Detection and Response](https://openreview.net/forum?id=7B91Naeszw) (ADR) system. The team so far has deployed this across 7,000+ hosts, where they are looking at the typical signals we look for in security like file writes and API calls, but they have extended this to include agent reasoning, prompts, and chains of intent leading to execution.


For security agents, specifically, the telemetry requirements are different from software engineering based agents, but the principle is identical since we want to capture:


-


The full reasoning chain: What alert data it ingested, what knowledge base entries it retrieved, what hypotheses it generated, how it weighted evidence, and what led to the classification decision.


-


The evidence: Every claim the agent makes should be traceable to a source.


-


The decision boundaries: Where did the agent’s reasoning change a decision? What alternative paths did it consider? When it chose to escalate rather than close, what was the evidence that tipped the decision?


-


The true negatives: What the agent didn’t do is just as important as what it did.


Uber's


[ADR system](https://github.com/uber/ADR) demonstrates a way of doing this at enterprise scale, pairing high-fidelity agentic telemetry capture with two-tier detection. The Sensor forwards reconstructed agent sessions to a backend for analysis and points to Elasticsearch as the telemetry store underpinning it.


[The paper](https://openreview.net/forum?id=7B91Naeszw) validates that closing the telemetry gap at enterprise scale is both achievable and necessary.


## Measuring real oversight


Human-in-the-loop has become the default answer to the question


*how do we keep AI accountable?*


But it's rarely tested as a control the way any other safeguard would be. If 99 times out of 100 the AI is right, why not approve again? The human ratifies the agent's output rather than independently evaluates it, and the human in the loop fails. Anthropic recently published engineering telemetry from Claude Code showing that users approved 93% of permission prompts with experienced users auto-approving at roughly twice the rate of new ones.


The more approvals a user sees, the less attention they pay to each one. Anthropic's own conclusion was that the oversight mechanism was degrading rather than protecting, and they had to redesign the approval model entirely. If the team building the agent cannot sustain meaningful human oversight over their own product, the assumption that an SOC analyst will do better under higher volume and higher consequence is not credible. Humans cannot keep up with the volume the next generation of attacks are bringing.


We need a monitoring layer that measures if approval reflects genuine judgment. Approval latency is the simplest signal; if every approval takes three seconds regardless of complexity, the human is not in the loop.


Beyond latency:


- How often does the human disagree with the agent?


- Did the reviewer expand the evidence section or just read the summary?


- Do quickly approved actions have worse outcomes than carefully reviewed ones?


Designing against approval fatigue requires friction proportional to consequence.


- Ratification metrics to capture a decision being reverted


- Mandatory review checklists


- Randomized retrospective audits against the reviewer’s decision quality and the agent’s recommendation quality


- Secondary approval processes for destructive actions


## The human role


The shift in the human role from operating to supervising to governing is already here and in production operations for some use cases. The most effective practitioners won’t be the deepest specialists in one domain. They’ll be the ones who understand how to write agents as well as the process, operations, and boundaries between domains well enough to set methodology and govern agents that operate across all of them.


This demands a new discipline: governance engineering. It’s the intersection between practitioners who understand what agents can do, what can go wrong, how to measure it, and how to build the infrastructure that catches failures before they become incidents.


The fully autonomous SOC is a useful North Star, but the honest current state is a spectrum. Most organizations are somewhere between chatbots with access to logs and workflows where agents perform light autonomous actions. Every additional capability introduces new governance requirements, evaluation criteria, and new areas of risk.


The organizations that will lead are the ones that can prove their agents are trustworthy continuously under adversarial pressure with evidence that satisfies both their security teams and their regulators. Trust requires evidence. Evidence requires measurement. And measurement requires the means to observe AI infrastructure.


## The regulations


ISO 42001, DORA, NIS2, and the EU AI Act are converging on the same expectation, but none yet define how to meet it for autonomous agents.


-


ISO 42001 requires performance monitoring but doesn't define minimum precision thresholds.


-


DORA requires operational resilience testing but doesn't specify evaluation methodologies for probabilistic systems.


-


NIS2 places accountability on management bodies but doesn't address how that accountability functions when decisions emerge from reasoning chains with no human authorization.


-


The EU AI Act requires conformity assessments for high-risk systems, but the assessment criteria for autonomous security agents don't yet exist.


The gap is AI infrastructure measurement and monitoring. Teams that build progressive trust frameworks, continuous monitoring, and evaluation pipelines now will satisfy whatever specific requirements materialize when the regulators catch up.


One regulatory pressure that deserves specific attention is provider concentration risk. Under DORA Articles 28-30, financial entities must assess concentration risk across their ICT providers, maintain audit rights, and document credible exit strategies for any provider supporting critical or important functions. If that provider is subsequently designated a critical third-party provider under Article 31, ESA-level oversight adds a further layer of scrutiny. The four-layer separation of skills, reasoning, models, and context directly addresses this by making reasoning methodologies portable across providers, allowing trust evidence to transfer with the methodology, and ensuring model changes do not disrupt continuity of trust.


## Elastic Security is built for this


The governance framework described here requires a platform that can do several things simultaneously:


-


Ingest and normalize agent telemetry at scale


-


Store it in a cost-effective manner


-


Query it at the speed agents operate


-


Run automated quality checks against every decision in real time


Elastic Security is built for this. Elasticsearch logsdb index mode reduces storage costs significantly for the high-volume security telemetry that sits alongside agent traces (e.g., the endpoint, network, and cloud data your agents investigate against), which matters when governance means retaining every agent action across your estate for audit. Elastic Common Schema (ECS) provides the schema foundation for this telemetry and can be extended to structure agent-specific data, reasoning chains, evidence references, decision records, so it becomes queryable alongside the security data agents act on. Machine learning capabilities and detection rules can then be pointed not just at threats on your network, but at the agents themselves, detecting drift in classification accuracy, anomalous reasoning patterns, or degradation in grounding quality before it becomes an incident.


Elastic is also uniquely positioned because these aren't separate products. The search capability needed to understand what agents have done, the observability capability to monitor how they reason, and the security capability to detect when something goes wrong all sit on the same platform. That convergence is what makes governed agentic operations possible.


We are actively developing capabilities specifically for agent observability and governance, and we welcome the conversation with teams working through the same problems. If you're building, deploying, or governing autonomous security agents, reach out to us at Elastic Security.


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
