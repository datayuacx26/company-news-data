---
schema_version: "1.0.0"
document_id: "edbcf323154486e3b7804351e8bbe1d7ddebae5c484a58fa77bf40db76c88b72"
company_key: "yc-blaxel"
company: "Blaxel"
source_id: "yc-blaxel-rss-eda12eea7869"
canonical_url: "https://blaxel.ai/blog/building-agentic-ai-applications-with-a-problem-first-approach"
published_at: "2026-02-11T22:32:32+00:00"
first_seen_at: "2026-07-20T23:20:26.598006+00:00"
fetched_at: "2026-07-28T20:54:21.606053+00:00"
content_hash: "sha256:4165bfe82e3b0f6fbb9ec8461d0024858f80890c8efc2350810b3022b7ad374e"
---

# An engineering team's guide to building agentic AI applications with a problem-first approach

Production agent projects fail when teams start with model capabilities instead of validated business problems. Your team could spend three months building multi-agent orchestration with vector databases and semantic caching. Then they discover your users needed simple document extraction that existing tools already solve. Meanwhile, token costs and orchestration overhead consumed the budget.


This pattern repeats across the AI industry. A[RAND Corporation study](https://www.rand.org/pubs/research_reports/RRA2680-1.html) found that AI projects fail at more than twice the rate of traditional IT projects, with the primary cause being teams that misunderstand or miscommunicate the problem they're solving.


Many generative AI pilots don't reach production because teams optimize for impressive demos rather than measurable outcomes. S&P Global reports that 42% of companies[abandoned the majority of their AI initiatives](https://www.spglobal.com/market-intelligence/en/news-insights/research/2025/10/generative-ai-shows-rapid-growth-but-yields-mixed-results) before reaching production in 2024, up from 17% the prior year.


Problem-first methodology exists to prevent exactly this outcome. This guide covers how it works for developing AI agents, why early validation reduces failure rates, and best practices for building production-ready systems.


## **What is a problem-first approach?**


The[problem-first approach](https://agilebrandguide.com/wiki/uncategorized/problem-first-approach-pfa/) focuses on defining specific use cases, success metrics, and problem complexity before selecting technologies or architectures. It targets workflows where traditional automation falls short and nuanced decision-making requires the flexibility of an AI agent.


## **How does problem-first differ from technology-first?**


The difference between these approaches shows up in where teams spend their first two weeks. One starts with tools, the other starts with problems.


A technology-first approach:


- Starts with "we have GPT-4, what can we build?"
- Places AI capabilities at the center of development from the outset
- Selects frameworks and architectures before validating problem fit


In contrast, a problem-first approach:


- Defines the problem space first, then selects appropriate AI technologies
- Validates whether agent autonomy is appropriate for specific use cases
- Establishes success criteria before implementing complex agentic systems


Engineering teams using problem-first methodology report measurable outcomes. For example,[Carta's internal teams](https://www.firstround.com/ai/carta) achieved 3,500+ hours in monthly time savings through validated AI agent implementations.


## **Why problem-first works for agentic AI applications**


Problem-first methodology helps teams commit to the discipline of early validation rather than jumping straight to implementation. This approach is one of the strongest predictors of whether agentic AI projects reach production or stall after the demo stage.


BCG research found that only 26% of companies[generate tangible value from AI](https://www.bcg.com/press/24october2024-ai-adoption-in-2024-74-of-companies-struggle-to-achieve-and-scale-value) , and 70% of the challenges are people and process problems, not technology limitations.[McKinsey's 2025 analysis](https://www.mckinsey.com/capabilities/quantumblack/our-insights/the-state-of-ai) further confirms this pattern: AI high performers are twice as likely to redesign workflows before selecting models compared to companies that underperform on AI initiatives.


### **Early validation prevents costly pivots**


Agentic AI projects carry significant infrastructure costs from token consumption, orchestration infrastructure, and memory systems. When teams validate problem fit before building, they avoid investing months in agents that address the wrong problems.


Consider a healthcare technology company that spends four months building a multi-agent system for prior authorization with NLP, insurance API integrations, and complex decision trees. But after deployment, the agent hallucinates authorization codes for edge-case procedures, and non-deterministic outputs mean the same claim gets different decisions every time it’s submitted. Token costs hit $8,000 per month to process documents the agent can’t reliably interpret.


A problem-first approach would have surfaced these agent-specific risks within two weeks through small-scale prompt testing and output validation. The team could have scoped a narrower agent for document completeness checking at a fraction of the cost and risk.


### **Clear success criteria allow meaningful evaluation**


Agent behavior drifts with model updates. According to[Microsoft's failure taxonomy](https://cdn-dynmedia-1.microsoft.com/is/content/microsoftcorp/microsoft/final/en-us/microsoft-brand/documents/Taxonomy-of-Failure-Mode-in-Agentic-AI-Systems-Whitepaper.pdf) , this non-deterministic behavior requires measurable success criteria defined upfront.


[LangChain's 2026 State of Agent Engineering survey](https://www.langchain.com/state-of-agent-engineering) of 1,300+ practitioners found that while 57% have agents in production, only 52% have implemented evaluations. Quality remains the number one barrier to production at 32%, ahead of technology capability. Among teams that do evaluate, human review (59.8%) and LLM-as-judge (53.3%) are the most common methods.


Agents must be continuously evaluated using code-based grading, model-based grading, and human grading for critical cases. Without these metrics established before development, your team can't distinguish between acceptable variation and genuine regression when model providers ship updates.


For example, an engineer building a code review agent might define three metrics before writing any agent logic:


1. Accuracy of flagging actual bugs (target 90%+)
2. False positive rate on clean code (target below 15%)
3. p95 latency per review (target under 30 seconds)


When the model provider ships an update and false positives spike to 25%, the team will catch the regression immediately because they established the baseline before development started.


### **Modular design follows from problem decomposition**


Problem-first methodology creates modular agent architectures through systematic problem decomposition. Several production frameworks (LangGraph, CrewAI, and the OpenAI Agents SDK among others) now recommend architectures with a deterministic orchestration layer managing state and control flow, invoking agents only where intelligence is needed. Deterministic flows are easier to debug and modify, and critical business logic executes reliably without LLM variability.


## **Best practices for building agentic AI applications**


These practices turn problem-first methodology into concrete implementation patterns. They help teams avoid scope creep, untestable agents, and systems that break on real-world edge cases.


### **1. Define success metrics before selecting frameworks**


Conduct stakeholder interviews to identify their actual pain points. Map your existing processes and identify where autonomy adds value versus introducing risk. Then define specific, measurable success metrics like "reduce data extraction time from 4 hours to 30 minutes with 95% accuracy." Without these clear metrics, your team could end up optimizing for impressive demos rather than production reliability.


[Harvey AI](https://www.harvey.ai/) demonstrates this pattern at scale. The company was founded by a former litigator who experienced the contract review problem firsthand. Rather than building a general "AI lawyer," the team started with a specific workflow: helping attorneys at firms like Allen & Overy (3,500 lawyers) process legal research queries.


Harvey defined success around accuracy on legal reasoning tasks and processing throughput, not broad AI capability. That narrow problem focus helped the company reach[over $100 million in annual recurring revenue](https://www.artificiallawyer.com/2025/08/04/harvey-reaches-100m-arr-42-of-amlaw-100/) by August 2025. The lesson applies to any domain: interview practitioners, identify the specific bottleneck, and define measurable success criteria before selecting any framework.


### **2. Design agents with narrow responsibilities**


Production-ready agents require narrow responsibilities with explicit inputs, outputs, and success criteria. Limit your tools to no more than three to five per agent. Instead of building a single "financial operations agent," create separate agents for invoice extraction, payment validation, and ledger updates. That way, each agent has clear boundaries. Failures are isolated, and teams can test each component independently.


Consider an insurance claims processing workflow. Instead of a monolithic claims agent, the team builds three focused agents: a document classification agent that routes incoming claims, an extraction agent that pulls structured data from claim forms, and a validation agent that checks extracted data against policy terms. When the extraction agent fails on a new form type, the team fixes that component without risking the validation logic.


### **3. Treat tools as contracts with strict schemas**


Well-designed tools are the difference between agents that work reliably and ones that break unexpectedly. Define explicit input and output schemas using structured types. Include validation that fails fast on invalid inputs, and document every tool with purpose, parameters, outputs, and error codes. Make sure to use version numbers for your tools (like v1.0, v2.0) and treat them as stable APIs that other code depends on.


Let's say your team has built a customer service agent that retrieves order status. The team defines a strict schema: the get_order_status tool accepts only a validated order_id string matching pattern 'ORD-\[0-9\]{8}', returns a structured object with status enum (processing, shipped, delivered, cancelled), and throws OrderNotFound if the ID doesn't exist. When the LLM passes malformed input, the tool rejects it immediately rather than propagating errors downstream.


### **4. Balance structure and autonomy with hybrid patterns**


Use structured workflows when processes are well-understood and errors are costly, since deterministic logic provides predictable, testable behavior. In contrast, you should choose agent autonomy when input is highly unstructured or the problem requires creative problem-solving, as these situations benefit from the LLM's ability to interpret and adapt. You can combine both in hybrid architectures where deterministic steps handle validation and storage while agents handle interpretation and decision-making.


[Intercom's Fin agent](https://www.intercom.com/fin) operates on this hybrid principle at scale. Fin handles customer support across 6,000+ companies with a 66% average resolution rate. The architecture routes deterministic queries (such as account status and order tracking) through structured workflows while the agent handles interpretation-heavy tasks like troubleshooting and product guidance.


[Synthesia](https://fin.ai/customers/synthesia) used Fin to handle a 690% spike in support volume while maintaining 98.3% self-serve resolution. The hybrid pattern works because it keeps agents focused on tasks that genuinely require language understanding.


### **5. Build observability from day one**


Your team needs visibility into not just what happened but why the agent made specific decisions. Instrument every agent decision with structured logging that captures tool selection reasoning. Track token usage per request for cost monitoring, and measure latency by component to identify constraints. Then use distributed tracing (OpenTelemetry) to capture decision traces. Agents require causal reasoning visibility through semantic logging.


Imagine a situation when your team is debugging a support agent that suddenly starts giving incorrect shipping estimates. Without observability, your team spends hours guessing. With structured logging, they see the agent correctly called the shipping_rates tool but misinterpreted the response because a recent API change added a new field. The decision trace shows exactly where reasoning diverged, which reduces debug time from hours to minutes.


### **6. Apply principle of least privilege with human oversight**


Production agents must operate under strict permission constraints. Scope database access to specific tables with row-level security., and limit API permissions to required actions only. Implement rate limiting on all external calls. Apply[Meta's Rule of Two](https://ai.facebook.com/blog/practical-ai-agent-security) : agents must not simultaneously process untrustworthy inputs, access sensitive data, AND change system state. Embed human approval at checkpoints involving high-value transactions, legal implications, or low agent confidence.


For example, a procurement agent could be authorized to generate purchase orders. The team applies Rule of Two: the agent can process untrusted vendor quotes (untrustworthy input) and create PO drafts (change state), but can't access employee salary data (sensitive data). High-value purchases above $10,000 require human approval. When an attacker attempts prompt injection through a malicious quote PDF, the agent's limited permissions prevent data exfiltration.


### **7. Start with single agents before multi-agent systems**


Multi-agent systems may add significant complexity without always providing proportional value. Build the complete workflow with a single agent first. Profile its performance and identify specific limitations. Move to multi-agent architectures only when single-agent limitations are documented and measured.


Klarna's trajectory illustrates both sides of this principle. The company launched an[AI customer service agent](https://www.klarna.com/international/press/klarna-ai-assistant-handles-two-thirds-of-customer-service-chats-in-its-first-month/) in February 2024 that handled 2.3 million conversations in its first month (equivalent to 700 full-time agents). Resolution time dropped from 11 minutes to under 2 minutes, and repeat inquiries fell 25%. The initial approach worked because it targeted a specific, well-scoped problem: routine customer service queries.


But Klarna then expanded aggressively,[cutting staff and pushing AI](https://www.bloomberg.com/news/articles/2025-05-15/klarna-ceo-reverses-ai-only-strategy-starts-hiring-humans-again) into broader, more complex workflows. By May 2025, the CEO acknowledged that cost was "too predominant" in the strategy and began rehiring humans for roles where the AI struggled with nuance and judgment.


The takeaway of Klarna’s story is to start narrow, validate, then expand only where documented performance data supports it. Scaling agent scope beyond validated boundaries creates the same problems as building multi-agent systems prematurely..


### **8. Plan for graceful degradation**


Production systems encounter edge cases where agents can't complete tasks due to rate limits, timeouts, or unexpected inputs. Design fallback mechanisms that maintain user experience when primary agent paths fail. Cascading failures represent a primary production failure mode that graceful degradation prevents. One component’s breakdown triggers others, but graceful degradation prevents this chain reaction.


[Air India's AI agent](https://www.airindiagroup.com/) demonstrates graceful degradation at scale. The system handles roughly 10,000 queries daily across 4 million total queries processed, with[97% resolved through full automation](https://www.tigerai.tech/p/from-turbulence-to-tailwinds-air-india-s-ai-assistant-soars) . The remaining 3% escalate to human agents with complete conversation context preserved.


Despite doubling passenger volume, call center staffing[stayed flat at around 9,000](https://azurecloud.pro/air-india-leverages-ai/) because the escalation path works both directions: agents handle routine queries, humans handle edge cases, and the boundary between them adjusts based on confidence scoring. The system has maintained zero inappropriate responses over more than a year of production operation. The pattern is clear: design for the 3% that fails, not just the 97% that works.


## **Deploy your first agent with problem-first methodology**


Building agentic AI applications with a problem-first approach prevents the high failure rate observed in technology-first implementations. The practices covered here prioritize understanding before building, separate deterministic logic from AI reasoning, and build observability and safety constraints into the foundation.


Infrastructure choices matter for production agent systems.[Blaxel](https://blaxel.ai/) 's perpetual sandbox platform provides the execution environment these systems require. Its Mark 3.0 infrastructure achieves 25ms cold start times and sub-50ms network latency, with sandboxes resuming from standby almost instantly even after weeks of inactivity. Sandboxes remain in standby indefinitely with zero compute cost.


Beyond sandboxes, Blaxel’s[Agents Hosting](https://docs.blaxel.ai/Agents/Overview) co-locates your agent logic on the same infrastructure to eliminate network roundtrip latency between agent and tools. Batch Jobs handle parallel processing for fan-out workloads, while MCP server hosting lets agents discover and call tools dynamically.


MicroVM isolation also provides stronger security boundaries than container-based approaches. Meanwhile, built-in OpenTelemetry observability captures the decision traces your team needs for debugging production issues.


[Sign up for free](https://app.blaxel.ai/) to deploy your first agent, or[book a demo](https://blaxel.ai/contact) to discuss your specific architecture requirements with Blaxel's founding team.


## **FAQs about building agentic AI applications with a problem-first approach**


### **How do you know if a problem is suitable for an agentic AI system?**


Evaluate problems against three criteria:


1. The workflow should require nuanced decision-making that changes based on context
2. Traditional automation attempts should have failed or produced unsatisfactory results
3. The volume should justify the investment, since agents consume model tokens on every invocation


Additionally, you should assess whether acceptable error rates exist for the use case, as agents produce probabilistic outputs.


### **When should you use multi-agent architectures versus single agents?**


Start with single-agent implementations and add complexity only when specific limitations emerge. Use the subagents pattern when distinct domains require specialized knowledge. Use the skills pattern when one agent needs many capabilities that can execute in parallel. Document why single-agent architecture isn't sufficient before making the switch. "Single agent can't meet latency requirements" is valid reasoning. But "multi-agent seems more sophisticated" isn't.


### **What security considerations apply specifically to agentic AI systems?**


Agentic systems that execute code require defense-in-depth security spanning multiple architectural layers. Production deployments must combine hardware virtualization through microVMs, network segmentation with strict egress filtering, and filesystem isolation with read-only base layers.


MicroVM isolation provides stronger security boundaries than container-based approaches for systems that execute generated code. Additionally, apply[Meta's Rule of Two](https://ai.facebook.com/blog/practical-ai-agent-security) to prevent prompt injection attacks from accessing sensitive data while changing system state.
