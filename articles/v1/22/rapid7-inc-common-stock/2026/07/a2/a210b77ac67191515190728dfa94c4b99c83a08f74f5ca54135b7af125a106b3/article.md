---
schema_version: "1.0.0"
document_id: "a210b77ac67191515190728dfa94c4b99c83a08f74f5ca54135b7af125a106b3"
company_key: "rapid7-inc-common-stock"
company: "Rapid7 Inc."
source_id: "rapid7-inc-common-stock-rss-ea5a9037191f"
canonical_url: "https://www.rapid7.com/blog/post/so-red-teaming-offensive-methodology-multi-agent-ai-architecture"
published_at: "2026-07-02T13:32:24+00:00"
first_seen_at: "2026-07-20T23:19:41.599179+00:00"
fetched_at: "2026-07-28T20:47:34.280666+00:00"
content_hash: "sha256:3fd854c082398d66aa8b579298ef85e5bf344b96ccb61ce6e0ece09581cdf4a4"
---

# Formalizing Red Teaming Offensive Methodology as a Multi-Agent AI Architecture

Threat actors are integrating AI into their exploit chains, accelerating reconnaissance, automating vulnerability discovery, and scaling social engineering in ways that compress the timeline between initial access and impact. The barrier to sophisticated offensive operations is dropping fast.


Rapid7's Red Team is doing the same. Over the past year we formalized our approach into a structured multi-agent system that follows our penetration testing methodology end-to-end from scoping an engagement to validating findings to generating reports. We built it as a production system, not a proof of concept, and the process of designing and operating it taught us as much about defending against AI-enhanced attacks as it did about conducting them.


The system also proved its value as part of Anthropic's


[Project Glasswing initiative](https://www.rapid7.com/blog/post/ai-rapid7-accesses-anthropics-project-glasswing-exploring-frontier-artificial-cybersecurity-intelligence/) . Glasswing is a program that gives leading security companies early access to frontier cyber models before they reach wider availability, enabling security research that stays ahead of malicious adoption. We infused our red team architecture with Claude Mythos, applying it across penetration testing, vulnerability research, and red team operations. The combination of our formalized multi-agent architecture with a frontier-class model produced exceptional results in vulnerability analysis and exploit chain development. This validated both the architecture's design and the importance of getting these capabilities into defenders' hands first.


This post covers the architecture, the key design decisions, and what we learned along the way.


## Why Rapid7's Red Team built a multi-agent system


Penetration testing is labor-intensive by nature as a significant portion of any engagement is spent on structured, repeatable work like enumerating attack surfaces, tracing data flows through source code, checking security headers, documenting findings in a consistent format. The actual judgement — deciding what to test next, assessing exploitability, understanding business impact — remains deeply human.


The opportunity was straightforward: offload the mechanical work to AI agents while maintaining human insight at decision points where it matters most. Those decision points are where engagements succeed or fail: scoping what's in and out of bounds, choosing which attack paths to pursue based on business context, assessing whether a vulnerability is genuinely exploitable in a given environment, deciding when a finding is significant enough to escalate, and interpreting results in ways that translate to actionable risks. None of that is mechanical, it requires experience, judgement, and context that models routinely get wrong. And as an internal security team, we don't just report vulnerabilities, we're accountable for coverage. If something ships with an exploitable flaw we missed, that's on us. The bar for confidence is high, and that's why humans stay in the loop at every point that matters.


We also had a secondary motivation. Building a system that follows a structured offensive methodology gives us direct architectural insight into how AI agents behave in adversarial contexts including the capabilities, the limitations, and the failure modes. That understanding now informs how we assess and secure Rapid7's own AI-powered products.


## The architecture: Orchestration, not autonomy


The system isn't a single monolithic agent but a team of specialist agents coordinated by an orchestrator that mirrors how human red teams operate. The orchestrator doesn't test anything. It assesses the current state of the engagement, determines what needs to happen next, routes work to the appropriate specialist, and processes the results. Specialist agents handle enumeration, code review, dynamic testing, and reporting.Each with defined inputs, outputs, and constraints.


The architectural choice to use supervisor-style orchestration rather than a monolithic agent separates routing decisions from execution. This makes the system more predictable, auditable, and controllable,properties that matter when the agent is operating in sensitive environments.


The key design decision that made this work was methodological, not technical. We reverse-engineered the agent's architecture directly from our team's daily task lists. The to-do items our testers tracked during real engagements became the specification: which tasks repeat, in what sequence, where decisions branch, and what triggers a return to an earlier phase. The methodology we'd built over years of engagements became the orchestration logic.


## Scope decomposition: Giving every target full attention


One of the earliest lessons we learned was that throwing an entire engagement scope at an AI agent produces shallow, scattered results. LLMs have finite context windows and finite attention. A complex application with dozens of endpoints, multiple authentication flows, and layered business logic overwhelms a single-pass analysis and important details get lost in the noise.


The solution was deliberate scope decomposition. Before the agent begins any technical work, the engagement scope is broken into discrete, manageable chunks. The scope includes individual components, feature areas, or functional boundaries. Each chunk flows through the full architecture independently: enumeration, code review, dynamic testing, and reporting. The orchestrator tracks which chunks are complete, which are in progress, and which are queued.


This achieves two things. First, it ensures depth over breadth as each component receives the agent's full analytical attention rather than competing for context space with everything else. Second, it creates natural parallelization opportunities and clear progress tracking. A tester can see exactly which areas have been thoroughly assessed and which remain.


The principal maps directly to how experienced pentesters already work by breaking the target into logical units, going deep on each one, then synthesizing across them. Making the principal explicit and enforceable in the orchestration logic was the design contribution.


## Feedback loops: Why linear pipelines fail


Real penetration tests don't follow a straight line. Code review reveals new endpoints that need enumeration. Dynamic testing uncovers an attack surface that wasn't visible from source alone. Validated findings sometimes expose entirely new subsystems.


The agent handles this natively. The orchestrator maintains a routing table with progression gates — criteria that must be met before advancing — and feedback triggers that route the engagement backward when new actionable data emerges. This creates a directed graph with re-entry points, not a waterfall.


## Guardrails: Maintaining safety in a malicious context


Building an AI agent that can hack is relatively straightforward but building one that operates safely within defined boundaries is a challenge. So it was an area where we invested significant design effort.


The system uses a tiered safety model:


-


Scope enforcement — every action is validated against the engagement's authorized scope before execution. Out-of-scope discoveries are reported but never probed.


-


Action classification — before execution, every proposed dynamic test is categorized as non-destructive, destructive, or ambiguous. Destructive and ambiguous actions require human approval.


-


Human-in-the-loop by default — in our current deployment, a tester reviews and approves every dynamic test. The agent proposes; the human decides.


The system is designed with a path toward semi-automated operation where low-risk, read-only actions execute autonomously while state-modifying operations still require human approval. The decision about where to sit on that spectrum is context-dependent. Internal labs can tolerate more autonomy while client engagements demand more oversight.


## Token efficiency: Making AI practical


AI agents are expensive to run at scale. Every enumeration step, every code block analyzed, every HTTP request reasoned about will consume tokens. It is a practical concern that shaped several design decisions.


The approach was to identify mechanical tasks that don't require LLM reasoning and replace them with deterministic scripts and MCP servers. DNS lookups, header checks, input field probing, and certificate enumeration produce structured data that the agent consumes, but the data collection itself doesn't need intelligence. This reduced token consumption dramatically for enumeration-heavy phases while letting the AI focus its reasoning budget on analysis, correlation, and judgement.


Not every step in an AI workflow needs AI. Knowing where to draw that line was the difference between a demo and a production system for us.


## Securing AI from the inside out


There's a dimension to this work that goes beyond offensive operations. Rapid7 builds AI-powered products. As the internal security team, we're responsible for securing those systems and building a complex multi-agent architecture gave us direct insight into where the weak points live.


Designing the orchestrated system taught us exactly how prompt injection can propagate between agents, where trust boundaries blur when one agent's output becomes another's input, how guardrails can be bypassed through indirect manipulation, and what happens when scope enforcement relies on instruction-following rather than programmatic controls.


We now test Rapid7's AI features with the same architectural intuition we developed building this system. We know where to look because we've built the same patterns and felt where they flex. When we assess an AI system's safety, we're thinking like the orchestrator — looking for the routing decision that can be subverted, the progression gate that can be skipped, the feedback loop that can be poisoned.


Building offensive AI made us materially better at defending the AI we ship to customers.


## What we learned operating the multi-agent system


A few observations from our team:


### Methodology is the differentiator


The LLMs are commodities. The orchestration patterns are emerging in open literature. What makes an AI agent effective at penetration testing is the methodology it follows and that's built from years of institutional knowledge. Formalizing our methodology into explicit, machine-executable logic was the most valuable part of the project.


### Building AI builds intuition for securing AI


The architectural understanding we developed — trust boundaries, prompt propagation, scope enforcement failures — translates directly into more effective security assessments of production AI systems. This was an unexpected but significant return on the investment.


### The automation spectrum is context dependent


Full autonomy isn't a goal; it's one end of a spectrum. The right level of automation depends on the context.Internal labs, client engagements, and product integrations each have different risk profiles. Designing for the spectrum rather than a fixed endpoint kept the system flexible.


## What's next for Rapid7 Red Teaming in the age of AI


We're continuing to develop the system, refining the methodology mapping, expanding specialist capabilities, and exploring where purpose-built models could replace general-purpose LLM calls for specific tasks (such as severity classification, report writing, payload selection). We're also using what we learn from operating this system to inform how Rapid7 detects and responds to AI-enhanced offensive activity in the wild.


You can learn more about Vector Command, Rapid7's continuous red-teaming solution,


[here](https://www.rapid7.com/services/continuous-red-team-service) .
