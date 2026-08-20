---
schema_version: "1.0.0"
document_id: "55ceef0f130b8732e131c4acfabc5fb04847faf3acc2da164051f038d97d0d5e"
company_key: "yc-wild-moose"
company: "Wild Moose"
source_id: "yc-wild-moose-news-import-a47316f7ff4d"
canonical_url: "https://www.wildmoose.ai/post/build-vs-buy-ai-sre"
published_at: "2026-03-04T00:00:00+00:00"
first_seen_at: "2026-07-26T05:31:33.003390+00:00"
fetched_at: "2026-07-28T22:02:33.296770+00:00"
content_hash: "sha256:dbf589b6f80fc7c6726486b401255c04ebc83cfda33461e822a9ac6e44a2e076"
---

# "We Should Just Build It Ourselves": On In-House AI SREs

Your team produced a demo. Someone wired up Claude to your Datadog account, gave it a few tools, and asked it to investigate a past incident. It pulled the right logs, correlated them with a recent deploy, and produced a root-cause summary that matched what the on-call engineer found manually. Now there's a proposal to turn this into a production system.


Before that project gets staffed, it's worth knowing what happens next. We talk to engineering teams every week who are at various stages of this journey, and the pattern is consistent: the demo takes a week, the prototype takes a month, and then the team spends the next six months discovering that the hard problems aren't the LLM, the prompting, or the tool wiring; that the AI costs end up no better than buying a solution; and that they can't even evaluate whether the thing works. The hard problems are teaching the agent what your best engineers know but have never written down; building evaluation infrastructure so you know whether the system is reliable; controlling costs that scale with every investigation; and keeping the system running as models get deprecated and APIs change underneath you.


[95% of enterprise AI pilots](https://fortune.com/2025/08/18/mit-report-95-percent-generative-ai-pilots-at-companies-failing-cfo/) deliver zero measurable return, according to an MIT study.[Over 40% of agentic AI projects](https://www.gartner.com/en/newsroom/press-releases/2025-06-25-gartner-predicts-over-40-percent-of-agentic-ai-projects-will-be-canceled-by-end-of-2027) will be canceled by 2027 per Gartner. These are the projects that looked great in the demo.


Here's what we've seen go wrong, and why.


## The "LLM + Tools" Trap


The pitch sounds straightforward: take Claude or GPT, give it MCP servers for your observability tools (Datadog, PagerDuty, your logging platform, Kubernetes), and let it investigate incidents autonomously. In a demo, this works. The model calls the right APIs, reads the logs, and produces a plausible root-cause summary.


Then you try it on a real incident. An investigation is a branching decision tree: the agent interprets results, picks the next step, interprets again, picks again. Errors compound.[Research on agent failures](https://arxiv.org/abs/2509.25370) confirms what practitioners discover quickly: a single wrong interpretation early on propagates through every subsequent decision. The agent doesn't backtrack when it should. Meanwhile, every tool call stuffs its inputs and outputs into the model's context, and[LLM performance degrades as context grows](https://arxiv.org/abs/2307.03172) , particularly for information buried in the middle. Observability data makes this worse: a single log query can expand to hundreds of thousands of tokens. Datadog[found](https://www.datadoghq.com/blog/building-bits-ai-sre/) that each additional tool call scales input tokens linearly as the context window grows, degrading model performance or hitting context limits.


The natural next step, and we are seeing this pattern a lot recently, is to decompose: instead of one agent doing everything, build an orchestrator that routes to specialist sub-agents. A Kubernetes expert, a networking expert, a metrics analyst, a runbook executor, etc.[Komodor](https://komodor.com/blog/the-war-room-of-ai-agents-why-the-future-of-ai-sre-is-multi-agent-orchestration/) built a "war room" architecture pairing a central orchestrator with domain-specific specialist agents. AWS published a[reference architecture](https://aws.amazon.com/blogs/machine-learning/build-multi-agent-site-reliability-engineering-assistants-with-amazon-bedrock-agentcore/) for the same pattern using Bedrock AgentCore. A[production deployment on Azure](https://www.linkedin.com/posts/michaelmirel_aks-network-storage-activity-7424322270401159170-kNdu/) uses a LangGraph supervisor orchestrating five specialist agents (AKS, networking, storage, VM, database), escalating to a separate AI tool for "complex diagnostics".


This is already a more serious engineering project than the original pitch suggested. But that is still the tip of the iceberg; things quickly get much more complex. Microsoft's AI SRE team[documented some of the issues with this approach](https://techcommunity.microsoft.com/blog/appsonazureblog/context-engineering-lessons-from-building-azure-sre-agent/4481200/) . Agents couldn't discover capabilities more than a few handoffs away. Conflicting system prompts between sub-agents broke reasoning chains. Agents got stuck in infinite loops bouncing requests between each other. Komodor describes the hallucination-arbitration problem: when your Kubernetes agent says with full confidence that it's a scheduling issue and your database agent says with equal confidence that it's connection pooling, the orchestrator has to adjudicate between two confident, potentially wrong answers. Microsoft's team also found that MCP-style tool calls are fundamentally insufficient for this domain: observability data is too large to pass through model context, so they had to route tool outputs to files and let a code interpreter analyze them instead. Their conclusion: "Six months ago, we thought we were building an SRE agent. In reality, we were building a context engineering system that happens to do Site Reliability Engineering."


## The Organizational Knowledge Problem


The difference between "there's a spike in 5xx errors on the checkout service" and "this is the third time this month that the payment provider's webhook has timed out during their maintenance window, which happens every other Tuesday" is organizational knowledge. It lives in Slack threads, in the heads of senior engineers, in Jira tickets that nobody reads, in that one wiki page that's two years out of date.


An LLM connected to observability and other SaaS sees fragments. A metric spike. A choppy Slack thread. A crash dump from somebody's laptop. Organizational jargon. Confused emojis. It doesn't know that the` cart-service` was rewritten last quarter and the old alerting thresholds no longer apply. It doesn't know that your team deploys to canary regions first and a 2% error rate in` us-west-2` is expected for 15 minutes after each deploy. It doesn't know that when` db-replica-lag` crosses 500ms, the right move is to page the database team, not restart the application.


Encoding this knowledge is its own project. Google's SRE book notes that most teams don't expect new hires to be[on-call ready for three to nine months](https://sre.google/sre-book/accelerating-sre-on-call/) , and that's just the ability to respond to incidents, not full expertise. Your AI agent is starting from zero, without the benefit of overhearing hallway conversations or pairing on incidents. You need to extract, structure, and maintain this knowledge as machine-readable context. That's a standalone project with its own maintenance burden.


## Procurement Won't Like This Either


One argument for building in-house is that you avoid paying a vendor margin. It doesn't work. The token cost of agentic solutions gets too high, too fast; vendors are able to offer solutions similarly priced, or even cheaper, than what you'll end up paying. Here's why.


When you're running your AI SRE on a few test incidents, the cost is negligible. A few dollars here and there barely registers. The problem is that cost scales linearly with usage, and the numbers get uncomfortable fast. A single incident investigation might involve:


- 50–100 log entries (structured JSON, verbose): ~100K tokens
- Metrics from 5–10 services over a 30-minute window: ~50K tokens
- 3–5 trace spans with metadata: ~30K tokens
- Recent deployments and config changes: ~20K tokens
- Prior incident context and runbook content: ~50K tokens


That's 250K tokens of input before the model even starts reasoning. With an agentic loop that makes 5–10 round-trips (each re-processing the growing context), you're looking at 1–2M input tokens per investigation. At Claude Sonnet pricing ($3/M input tokens, doubling beyond 200K per request), a single investigation costs $5–15. And that's just the LLM API bill. It doesn't include the engineering time to build and maintain the system, which we cover below.


This is also the optimistic estimate. In practice, agents over-fetch data, retry failed tool calls, and explore dead ends. Real-world token consumption is typically 2–3x the theoretical minimum.


So what do vendors charge?[Datadog Bits AI](https://www.datadoghq.com/pricing/) runs about $30 per conclusive investigation.[Azure SRE Agent](https://techcommunity.microsoft.com/blog/appsonazureblog/context-engineering-lessons-from-building-azure-sre-agent/4481200/) runs around $7.50. Your in-house system lands in the same range, plus the engineering overhead.


Incidentally, the $5–30-per-investigation cost implies a deeper problem that even buying doesn't necessarily solve: you can't run on every alert, and you can't evaluate your agent on enough past incidents to know whether it actually works. We've built an architecture that does root-cause analysis at a very small cost per alert, which changes what's possible, but that's a topic for another post. The evaluation problem, however, deserves a closer look.


## The Evaluation Gap


How do you know your AI SRE actually works? Not in a demo, not on the three incidents you tested during development, but across the real distribution of problems your systems produce? Most teams don't find out until it's too late.


A proper evaluation suite might look something like this: a labeled dataset of past incidents with known root causes, each with the full observability context (logs, metrics, traces) as it existed at investigation time. Most companies retain logs for 14–90 days, traces for 1–7 days, and high-granularity metrics for 7–30 days, so if you haven't captured and stored that context, it's gone. You run your agent against each incident, compare its conclusion to ground truth, and repeat across enough incidents to get statistical significance. Then you re-run the entire suite every time you change a prompt, swap a model, or modify a tool.


At the per-investigation costs described above, a single evaluation pass across 30 incidents costs $150–450. You'll want to run it dozens of times during development. Most teams skip rigorous evaluation entirely and ship undertested agents. That's the difference our micro-agent architecture makes: not just cheaper investigations, but the ability to actually know whether your system works.


## The Maintenance Treadmill


Maintenance is a cost of any software project. But an in-house AI SRE has two properties that make it worse.


The main reason is the pace of change in the underlying technology. A[Cleanlab survey](https://cleanlab.ai/ai-agents-in-production-2025/) found that 70% of regulated enterprises with production AI agents are changing their entire LLM stack every three months. Every model swap requires re-evaluating your agent, re-tuning prompts, and verifying that the new model's interpretation of your tools and instructions hasn't drifted. And these aren't graceful migrations: when GPT-5 launched, it[broke existing applications](https://www.docker.com/blog/gpt5-api-deprecation-ai-app-failure/) as teams discovered that "subtle but significant changes in output format, tone, and logical reasoning" had silently degraded their agents.


## What's Actually Hard


The LLM, the prompting, and the tool calls are the easy part. What's hard is everything that makes it work reliably in production:


- **Knowledge engineering** : capturing and maintaining the organizational context that turns data into understanding
- **Evaluation infrastructure** : building the feedback loops that tell you whether your system actually works
- **Cost optimization** : architectures that keep token spend predictable instead of open-ended
- **Operational reliability** : graceful degradation, rate limit handling, concurrent investigation management
- **Safety and compliance** : data handling, access controls, audit trails
- **Continuous maintenance** : surviving model deprecations, API changes, and infrastructure evolution


None of these show up in a proof-of-concept, but they're where most of the engineering effort goes. They're why building an AI SRE in-house is almost always the wrong call.


The build-vs-buy numbers reflect this. In 2024, the split was roughly even. By 2025,[76% of enterprises](https://beam.ai/agentic-insights/the-great-ai-flip-why-76-of-enterprises-stopped-building-ai-in-house) purchase AI capabilities rather than build in-house. The question isn't whether your team can build a demo. The question is whether the multi-year investment of making it actually work, and keep working, in production is worth it.
