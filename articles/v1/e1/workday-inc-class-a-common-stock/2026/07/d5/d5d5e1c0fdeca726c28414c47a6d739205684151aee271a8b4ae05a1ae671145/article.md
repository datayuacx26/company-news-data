---
schema_version: "1.0.0"
document_id: "d5d5e1c0fdeca726c28414c47a6d739205684151aee271a8b4ae05a1ae671145"
company_key: "workday-inc-class-a-common-stock"
company: "Workday Inc."
source_id: "workday-inc-class-a-common-stock-rss-1edd291cea4c"
canonical_url: "https://medium.com/workday-engineering/beyond-chatbots-the-next-wave-of-enterprise-ai-agents-is-ambient-c3f928eff985"
published_at: "2026-07-17T21:12:31+00:00"
first_seen_at: "2026-07-20T04:35:52.231186+00:00"
fetched_at: "2026-08-20T00:44:57.479297+00:00"
content_hash: "sha256:0d32f718a51cd4451ba55beae0c9316201945b7fc36a1ff101dee75d9a5fa810"
---

# Beyond Chatbots: The Next Wave of Enterprise AI Agents is Ambient

*On the architecture shift from reactive assistants to persistent background operators*


*By Priyanka Mudgal, Principal AI Engineering at Workday*


Most enterprise AI deployments today share the same fundamental flaw: they wait. A user types a query. The system responds. The user types again. The system responds again. This request-response loop is familiar, safe, and increasingly insufficient for the complexity of real enterprise operations. At Workday, where millions of workers and billions of transactions flow through our platform, this reactive pattern leaves too much value on the table; for our customers and for their employees.


Our team in Agent Factory are rethinking what it means for AI to “wait”, designing agents that move at the tempo of real people and real business.


### The Reactive Trap


Current agent frameworks including even sophisticated ones are built around a reactive mental model. They excel at answering questions and completing discrete tasks when invoked. But enterprise workflows don’t work that way. A candidate applying for a role doesn’t wait for a recruiter to log in. An SLA breach doesn’t pause while a manager checks their inbox. A compliance deadline doesn’t reschedule itself.


The gap between real-world operational tempo and the pace of human-initiated AI interaction is where value leaks. Within Workday, we see this gap across talent, finance, and operations; a top candidate accepts another offer overnight, a financial close stalls on one missing approval, a compliance task slips past a deadline because it was in someone’s inbox. Closing the gap is as much a product and design challenge as it is an AI challenge, and it’s exactly where our Agent Factory team focuses.


Crucially, solving this does not mean abandoning real-time engagement; rather, it requires a hybrid operational architecture. While the system runs ambiently in the background to drive long-running processes, it seamlessly switches into a synchronous, reactive mode when an immediate human intervention or an on-demand query occurs. This hybrid capability ensures the agent can instantly pivot from an autonomous background operator to an interactive partner, bridging the gap between steady-state automation and ad-hoc human collaboration without losing context.


### From Reactive to Ambient


An ambient AI architecture flips the operational model. Rather than sitting idle until invoked, the system continuously observes the enterprise environment, monitoring event streams, system signals, and state changes, and acts when conditions are met, with or without a user present.


This isn’t a new idea in monitoring and alerting. What’s new is coupling that ambient awareness directly to an agentic execution layer: one that can plan, act across tools, pause intelligently for human input, and resume exactly where it left off.


The practical difference is significant. In a Workday powered talent stack, that hiring requirement often starts life in our core system of record — a headcount plan, a requisition approval, or a new business line coming online. An ambient agent can track that lifecycle end‑to‑end, long before anyone opens a chat window.


Sourcing, outreach, and shortlisting can begin immediately and run in the background. Candidates experience faster, more consistent communication; hiring managers see curated shortlists that reflect their team’s actual history. When human judgment is needed, for example for a compensation band decision: the system pauses, persists in its full state, and waits. The agent sends a notification or asks for specific input from the hiring manager on a preferred communication channel or even across multiple channels. When the hiring manager responds (in the next turn, or three days later), execution resumes from the exact checkpoint rather than restarting. This pause-and-resume capability is what separates ambient agents from ambient alerts. Alerts notify. Agents act, wait, and continue.


### The Memory Problem Nobody Talks About Enough


Most agentic frameworks treat memory as an afterthought, which is a flat vector store you query with semantic similarity when context is needed. This works reasonably well for retrieval, but it creates a persistent “cold-start” problem: every session begins without genuine organizational context.


An agent that sourced great candidates last quarter doesn’t automatically know which department-specific signals correlated with successful hires. A finance agent that processed last month’s reimbursements doesn’t carry forward the edge cases that required escalation. Each run is effectively stateless at the organizational level.


The more durable approach is to treat memory as a graph or tool rather than a flat index. Workday sits at the intersection of rich, structured enterprise data: people, roles, org structures, finance, and skills — and fast evolving AI capabilities. Treating memory as a graph over these entities is how our agents can learn which skills predict success in a role, which teams hire differently, and how policies actually play out in practice over time. Designing this kind of memory is a deeply collaborative exercise. Our applied scientists, data platform engineers, and product teams work together to decide what “institutional memory” should mean for our customers, and how to build it responsibly.


Facts and outcomes are attached to persistent entity nodes: roles, departments, vendors, employees, policies and enriched with metadata over time. Retrieval becomes traversal: when a new requisition opens for a senior engineer in a specific department, the system can follow entity relationships to prior hiring patterns for that team, not just perform a generic keyword search against all historical data.


The practical effect is that the system’s planning quality improves with every completed workflow. The longer it runs, the more institutional context it accumulates, and the more precisely it can warm-start new tasks. That compounding quality improvement is difficult to replicate with flat RAG alone.


### Spawning Dynamic Agents without Hand-Wired Workflows


One of the harder engineering problems in multi-agent systems is handling the long tail of real-world requests. Pre-defined workflow graphs handle common paths well. They degrade rapidly when requests are ambiguous, cross-domain, or simply haven’t been anticipated.


A more resilient approach is to generate execution plans at runtime rather than select from a pre-built library. When an incoming request doesn’t match a known structured path, the system constructs a plan on the fly, drawing on available tools, reusable skill modules, and few-shot examples rather than rejecting the request or falling through to a generic fallback.


In Agent Factory, we treat the orchestrator, planner, and execution layer as first class platform components that serve multiple Workday products, from talent to finance. That means our engineers design for multi‑tenant scale, strict correctness, and auditability from day one.


This isn’t an unconstrained generation. The planning step is gated: the agent must produce an aligned execution plan before any workflow tools are invoked. The plan is based on the defined workflow and tools in skills. This prevents partially-formed plans from triggering real side effects and forces explicit reasoning before action. The generated plan also exists at two levels simultaneously: One is detailed and technical with every tool call, data retrieval step, checkpoint, and yield point; the other is simplified and outcome-oriented with the version a business user or manager would actually want to read. Both are maintained in sync throughout execution, so the system’s internal state and the user-visible progress view never diverge.


### Plan Continuity Across Boundaries


One of the subtle failure modes in complex agent systems is plan fragmentation. An orchestrator creates a plan, hands it off to a sub-agent, and the sub-agent either reconstructs a new plan or loses track of where it is in the original one. Sessions end, processes restart, and context evaporates.


A more robust model treats the plan itself as a first-class persistent artifact. Plans are stored durably and identified by a stable mechanism that travels with the workflow across agent handoffs, session boundaries, and transitions between interactive and background execution modes. When a sub-agent picks up work, it resumes from the stored plan checkpoint rather than starting over. This plan persistence is what makes the interactive-to-ambient transition seamless. A workflow can start as a conversation, move into background execution, pause for human confirmation, and resume conversationally all under the same plan identity, with no loss of context at any handoff.


### The Quiet Failure Mode


There’s a failure mode in agentic systems that doesn’t get enough attention: the agent may confidently report that an action succeeded when the underlying tool call failed or was never made.


This isn’t always a hallucination in the traditional sense. It’s often an artifact of how LLMs are trained to be helpful — they produce the expected success narrative because that’s what usually follows a tool invocation. The fix isn’t purely a prompting concern. It requires architectural enforcement: the agent is structurally prohibited from reporting any action as complete unless the execution layer has verified the result. Reported state must be derived from actual tool outputs, not inferred from the plan.


In enterprise contexts, where a “successfully sent” offer letter or “approved” expense reimbursement has downstream consequences, this property matters considerably more than it does in conversational settings. Workday is entrusted with some of our customers’ most sensitive workflows, from offer approvals and benefits elections to payroll and financial close. In that context, “the AI said it worked” is never enough. Our architecture requires verified tool outcomes, traceable logs, and clear audit trails before any agent can claim success.


### The System That Learns to Run Itself


What’s most interesting about this architecture isn’t any single component, it’s how the pieces work in harmony. Each one closes a gap that the others expose. Ambient detection without async execution creates blocking bottlenecks: the system notices an event but stalls the user interface waiting for resolution. Async execution without plan persistence loses state at handoffs: a sub-agent picks up work with no memory of where the orchestrator left off. Plan persistence without graph memory produces warm-looking restarts with cold-start quality: the checklist survives the session boundary, but the reasoning behind each step doesn’t. And all of it falls apart without execution verification, a system that can’t confirm its own actions will eventually report success on a workflow that silently failed.


Taken together, these components describe something meaningfully different from the current generation of enterprise AI tools. For Workday customers, that means agents quietly keep hiring pipelines warm, surface internal talent, nudge managers at the right moment, and help employees navigate complex processes without friction. Not a smarter chatbot, but a persistent background operator: one that monitors its environment continuously, executes work asynchronously, accumulates organizational context over time, and surfaces itself to humans precisely when judgment is needed and nowhere else. The human-in-the-loop moments become genuinely high-value, not interruptions caused by a system that can’t proceed, but deliberate checkpoints where the system has done everything it can autonomously and needs a decision only a person can make.


For practitioners building in this space, the implication is that the interesting engineering work has shifted. Prompt design and model selection still matter, but they’re no longer the primary leverage points. The leverage is increasingly in the infrastructure layer: how state is managed across session boundaries, how plans are persisted and hydrated across agent handoffs, how memory is structured so that retrieval improves with organizational history rather than staying flat, and how execution results are verified before they’re surfaced to users. These are distributed systems problems as much as AI problems. Teams with strong infrastructure instincts are well-positioned here.


The shift from reactive to ambient or hybrid is genuinely underway and what makes this moment exciting is that the foundations are tractable. This shift shows up as concrete projects at Workday: ambient agents that monitor talent pipelines, support workforce planning, assist with financial close, and more. For builders, it’s an opportunity to shape how AI supports people at work for years to come, as a core part of the platform. This isn’t moonshot research. It’s careful systems thinking applied to a problem that’s clearly worth solving.


---


[Beyond Chatbots: The Next Wave of Enterprise AI Agents is Ambient](https://medium.com/workday-engineering/beyond-chatbots-the-next-wave-of-enterprise-ai-agents-is-ambient-c3f928eff985) was originally published in[Workday Technology Blog](https://medium.com/workday-engineering) on Medium, where people are continuing the conversation by highlighting and responding to this story.
