---
schema_version: "1.0.0"
document_id: "4275908cf6de1bcb8b13106aeb01b27488c9a3068641f9fafd3d5d360e70b96d"
company_key: "yc-brex"
company: "Brex"
source_id: "yc-brex-news-import-5eb786253ae8"
canonical_url: "https://www.brex.com/journal/magpie-ai-cost-visibility-dashboard"
published_at: "2026-06-01T00:00:00+00:00"
first_seen_at: "2026-07-21T11:29:48.523536+00:00"
fetched_at: "2026-07-28T21:24:31.593744+00:00"
content_hash: "sha256:21bd92802130551eb3563170a0713f1b74b848241cf3f32274e3d57865e59d0f"
---

# AI cost visibility is a problem for every company. Here's how we're solving it.

## You can’t manage what you can’t see


There’s a particular kind of frustration that comes from being held accountable for numbers nobody trusts. That was my Q1.


Brex was spending real money on AI across AWS Bedrock, OpenAI, Codex, Cursor, Anthropic, and vendors that didn’t expose usage APIs at all. We had dashboards to track it, but pricing assumptions had drifted, vendor configs had gone stale, and some data sources were missing. We couldn’t even tell which parts were wrong, or by how much.


We needed a trustworthy way to answer: Where is our AI spend going? So we built an internal tool called Magpie to provide it.


While this started as a way to solve our own problem, it quickly became part of our vision for what spend management will look like in the AI era.


## How Magpie works


Magpie ingests data from several sources, including Bedrock, LLM Gateway, OpenAI, Codex, Cursor, Anthropic, and CSV invoices, all in different formats.


For example, Bedrock data has to be reconciled with internal invocation logs, LLM Gateway data captures product AI calls and customer context, developer tools provide employee-level usage, and invoice-only vendors require manual uploads and mapping.


It then normalize that data into a common cost-event model that includes service, vendor, model, customer, employee, token usage, and dollar cost. So the dashboard can become an investigation layer.


Employees can drill from overall AI spend into a specific vendor, model, service, or customer. They can get instant answers to questions like: What are we spending on AI this month and where? Which customers are the most expensive to serve? Which workloads are worth optimizing?


## Trustworthy AI cost data on demand


With Magpie, we have a real-time view of AI spending by vendor, model, customer, employee, and internal AI pillar. On the home page, we can see trends and the biggest AI cost drivers, with customizable time frames.


Magpie: Dashboard home page


From there, we can drill into spend details by model, API, vendor, customer, and more, including tracking cost per call for each AI vendor.


Dashboard: By Model Vendor > API > Model


We can also analyze spend by our AI pillars, such as corporate AI usage by employee and product AI costs by customer. This lets us understand which parts of the business are generating AI costs and where those costs are worth it.


Corporate AI: By Employee > Caller > ModelProduct AI: By Customer > Model Vendor > Model


We also built a debug view where we can diagnose data quality and investigate usage. For example, we can search for a specific feature to see the total cost, calls, average cost per call, hit rate, completion, and more, including how each caller’s metrics compare to org averages.


Debug: Investigate Caller


Finally, we built employee-level leaderboards to understand where AI adoption is accelerating, while recognizing that more usage is not always better. The goal is to identify where AI is creating true leverage.


Leaderboard: Employee AI spend


## Why AI spend is uniquely difficult to manage


We needed to build Magpie because AI spend behaves differently than traditional SaaS spend. Usage is dynamic, distributed, and directly tied to behavior at the model, prompt, and workflow level.


Companies are spending millions on[AI vendors](https://www.brex.com/journal/brex-benchmark-may-2026) that all expose usage data at different levels of granularity. Some provide model, customer, and token-level detail, while others only provide daily aggregates. And some provide nothing but invoices.


Even when we had detailed usage data, attribution was inconsistent. Product AI calls flowing through our LLM Gateway weren’t always attributed to a customer. Some operational workloads weren’t tagged cleanly, and tool-use costs were not always captured alongside token costs.


At one point, a meaningful portion of our AI spending was unattributable because our gateway was tracking token costs but not associated tool-use costs. Closing that gap required tracing response data back to the underlying calls.


## Understanding that AI spend is about more than cost


The uniqueness of AI spend also requires a unique definition of “visibility." Because AI spending is as much a product, engineering, and operational conversation as a financial one, we landed on three lenses for Magpie:


- **Customer attribution:** For every AI call we make to serve a customer across underwriting, onboarding, and in-product features, we need to track exactly what it costs and who it is for.
- **Spend by AI pillar:** At Brex, we think about our AI spend in terms of three pillars that cut across vendors, invoices, and infrastructure layers:


- Product AI: customer-facing AI features
- Operational AI: internal workflows for GTM, support, and finance
- Corporate AI: productivity tools like ChatGPT and Claude, plus coding agents including Cursor, Claude Code, and Codex


- **Org and function visibility:** For corporate AI tooling, we needed to understand which teams and employees were driving usage, where power-user behavior was emerging, and where AI spend was translating into business leverage.


## What we learned from Magpie almost immediately


#### Operational AI was cheaper than expected


We went in assuming that running AI across workflows like onboarding, support, and account management would be expensive enough to warrant scrutiny, but the data proved us wrong.


Even the most AI-intensive workflows were significantly cheaper than expected, which changed how we think about ROI for operational AI. Product quality, workflow design, latency, and operational integration are actually bigger factors than token spend in determining whether an AI use case is worth it.


#### Customer attribution improves decision making


For Product AI, we could finally see which customers were driving the most AI usage. Knowing those numbers help us calculate margin, pricing, packaging, and product strategy. The visibility also helped separate customer cost-to-serve from funnel-stage or acquisition-related operational costs, which often require different ROI calculations.


#### Prompt caching created a visible cost reduction


The visibility also created accountability for optimization. In one case, we reduced the cost of an audit agent by roughly 85% per call after introducing prompt caching and workflow changes. Without detailed visibility, that kind of optimization work is mostly guesswork.


#### Cost data leads to usage questions


Internal users immediately asked Magpie questions beyond cost. Which models are people using? Which services are using multiple models? How does usage correlate with productivity, incidents, or output? It made us realize that there is a broader opportunity around AI usage intelligence.


#### Model routing issues became easy to spot


Magpie also helped us answer model governance questions. For example, when we needed to verify that certain customer traffic was only routing to approved models, we could drill into Product AI spend by customer, vendor, and model. That made it possible to spot unintended routing quickly and confirm when it disappeared. Here, AI spend management starts to become governance.


#### Some services were too broad to be useful


We also learned the importance of granularity. For example, a broad product AI key that once covered multiple features had to be split into more specific services, like audit agent and accounting agent, before the spend data became useful.


#### Corporate AI needed better attribution


Engineering and productivity tooling is one of the largest areas of AI spend. The leaderboard helped us compare different models’ cost and token usage. It also exposed where attribution was still unclear, especially for workloads routed through shared infrastructure.


For example, some Bedrock usage had to be stitched together across Bedrock costs, internal events, and CloudWatch logs to attribute usage correctly. This showed us how important clean ingestion and metadata are.


## What comes next


We’re focused on closing attribution gaps and making the system even more actionable. We’re integrating org metadata so spend can roll up cleanly by team and function. We’re tightening LLM Gateway enforcement so customer attribution becomes mandatory. And we’re exploring anomaly alerts so teams know right away when something changes materially.


But more important, we’ll be focused on bringing AI cost visibility into Brex itself, so[customers](https://www.brex.com/customer-hub) can understand, control, and optimize AI spend the same way they manage the rest of their business spend on Brex.


Because the problem of AI spend is not unique to us. Every company adopting AI at scale will hit the same wall. They need attribution, governance, forecasting, optimization, and operational visibility into how AI is being used and what it’s costing them.


The future of spend management must understand AI natively, and this is the beginning of that shift.


*Learn more about AI-native finance from Brex at[brex.com/intelligence](https://www.brex.com/platform/intelligent-finance) .*
