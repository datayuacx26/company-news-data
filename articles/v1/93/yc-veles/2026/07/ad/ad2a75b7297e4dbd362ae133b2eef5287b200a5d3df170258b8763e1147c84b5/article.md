---
schema_version: "1.0.0"
document_id: "ad2a75b7297e4dbd362ae133b2eef5287b200a5d3df170258b8763e1147c84b5"
company_key: "yc-veles"
company: "Veles"
source_id: "yc-veles-news-import-8507ff553e08"
canonical_url: "http://www.getveles.com/blog/agentic-cpq-definition-examples"
published_at: null
first_seen_at: "2026-07-22T18:32:03.809503+00:00"
fetched_at: "2026-07-28T21:20:10.944044+00:00"
content_hash: "sha256:540a5e565cbebc4f2a9dd6983d8a56f48793db04b738f09d17bcda9c3a7370d9"
---

# Agentic CPQ Defined: Why It Matters in 2026

CPQ


# Agentic CPQ Defined: Why It Matters in 2026


By Simon Ooley, CEO and Co-Founder of Veles (YC W24) · Last updated: April 2026


**Agentic CPQ is a configure-price-quote system where AI agents are the primary operators of the quoting workflow.** Instead of a human navigating screens, selecting products, and building quotes manually, an AI agent reads the deal context, configures the quote, applies pricing rules, and produces a finished deliverable. The human reviews, approves, and sends.


The term “agentic” distinguishes this approach from traditional CPQ (rule-based automation), AI-assisted CPQ (suggestions and autocomplete), and copilot-style CPQ (AI watches, human works). In an agentic system, the AI does the work and the human approves it. The direction of effort is reversed.


---


##### Why Agentic CPQ Is Emerging Now


Three structural changes made agentic CPQ possible in 2025-2026 that were not possible before.


**Tool-use protocols matured.** Model Context Protocol (MCP), introduced by Anthropic in late 2024, gives AI models a standardized way to call external tools. A CPQ platform that exposes MCP-compatible endpoints allows any AI agent to configure products, apply discount rules, and generate quotes programmatically. Before MCP, every integration was custom.


**Every revenue tool became AI-readable.** Gong, HubSpot, Salesforce, Slack, and Fireflies all now expose interfaces that AI agents can interact with. The context required for quoting—call transcripts, CRM records, contract history, and pricing rules—is accessible to agents in real time.


**The last mile is still manual.** According to Salesforce's State of Sales report, sales reps spend only 28% of their time actually selling. Despite billions invested in conversation intelligence, CRM, and sequencing tools, the actual quote is still built by hand in a spreadsheet or legacy CPQ. That manual step is the bottleneck agentic CPQ eliminates.


---


##### What Agentic CPQ Is Not


Understanding what doesn't qualify helps clarify the category.


**It's not CPQ with a chatbot.** Adding a conversational UI on top of an existing quoting tool doesn't make it agentic. If a rep still has to open the app, browse a catalog, select line items, and generate a PDF, the chatbot just added a step.


**It's not AI-assisted CPQ.** AI-assisted means the human does the work and the software offers suggestions. This is autocomplete for quotes. Useful, but it's a feature, not a new category.


**It's not a copilot.** Copilots observe you working and offer help. Agentic systems do the work and ask you to approve it. The difference is directional: copilots assist, agents execute.


---


##### How Agentic CPQ Works


An agentic CPQ system has four characteristics that separate it from prior generations of quoting software.


###### 1. The AI Has Context


The agent reads the sales call transcript, the CRM opportunity record, the pricing rules, and the contract history. It does not ask the rep to re-enter information that already exists in the revenue stack. Context aggregation across systems is what makes the output accurate without human input.


###### 2. The AI Has Tools


Not suggestions. Tools. The agent can read from and write to your CRM, pull product and pricing data from the catalog, apply discount guardrails, route approvals, and assemble a structured quote. This is where MCP changes the equation. When your CPQ exposes callable tools, the quote becomes a natural output of the sales conversation rather than a separate workflow.


###### 3. The AI Produces a Real Deliverable


The output is not a summary or a recommendation. It is a quote with line items, pricing, terms, and a structure the buyer can review. The quote is proof of work. If the system cannot produce one from a real conversation, it is not agentic.


###### 4. The Human Reviews, Not Rebuilds


The rep opens a quote that is 90% complete. They adjust details, approve, and send. The time from verbal agreement to quote-in-inbox drops from days to minutes. In benchmarks from early adopters, this reduces average quote cycle time by 60-80%.


---


##### Traditional CPQ vs. AI-Assisted CPQ vs. Agentic CPQ


Dimension Traditional CPQ AI-Assisted CPQ Agentic CPQ


Primary operator Human Human (with suggestions) AI agent


Quote creation time 30-60 minutes 15-30 minutes Under 2 minutes


Context source Manual entry Partial CRM pull Full revenue stack (CRM, calls, contracts)


Approval workflow Manual routing Suggested routing Automated within guardrails


Integration model Point-to-point APIs Embedded AI features Tool-use protocols (MCP)


Human role Builder Builder with assistant Reviewer and approver


Error rate High (manual entry) Medium (partial automation) Low (rules-enforced, no manual entry)


Setup complexity Months Weeks-months Days-weeks


---


##### Agentic CPQ Use Cases


###### Post-Call Quote Generation


A sales rep finishes a discovery call. The AI agent reads the call transcript (from Gong or Fireflies), identifies the products discussed, pricing parameters, and deal structure. It creates a quote in the CPQ, applies pricing rules and discount guardrails, and posts the result to Slack with a PDF and a live link. The rep reviews and sends. Total time: under 2 minutes.


###### Usage-Based Upsell


The agent monitors usage data against contracted thresholds. When a customer approaches their limit, it automatically generates an upsell quote with the next tier, applies loyalty pricing rules, and alerts the account manager with a ready-to-send proposal.


###### Proactive Renewal


90 days before contract expiration, the agent pulls the current contract terms, applies any price escalation rules, generates a renewal quote, and routes it for internal approval. The rep receives a fully built renewal ready to present.


###### Auto-Updated Forecasting


As deals progress, the agent keeps quotes synchronized with the CRM forecast. If pricing changes or new products are added during negotiation, the quote updates automatically and the forecast reflects the current state without manual entry.


---


##### What to Look for in an Agentic CPQ Platform


When evaluating agentic CPQ solutions, these capabilities separate genuine platforms from marketing-driven relabeling.


**MCP or equivalent tool-use support.** The platform should expose its full functionality as callable tools that any AI agent can invoke. If the AI can only access the CPQ through a proprietary chatbot, it is not truly agentic. It is vendor-locked.


**Bi-directional CRM integration.** Quotes should sync to Salesforce, HubSpot, or your CRM of choice in real time. Changes in either system should reflect in the other.


**Pricing rules engine.** Discount guardrails, floor prices, approval thresholds, and tiered pricing should be enforceable programmatically. The AI must operate within the same constraints a human would.


**Audit trail and version control.** Every quote version, every approval decision, and every change should be logged. Compliance teams need visibility into what the agent did and why.


**Multi-scenario quoting.** The agent should be able to generate 3-5 pricing scenarios from a single conversation, allowing the buyer to compare options without the rep rebuilding each one manually.


---


##### The Role of MCP in Agentic CPQ


Model Context Protocol (MCP) is the infrastructure layer that makes agentic CPQ practical at scale. Before MCP, connecting an AI agent to a CPQ required custom API integrations for every workflow. MCP provides a standard interface: any MCP-compatible AI (Claude, GPT, or custom agents) can discover and call tools exposed by any MCP-compatible platform.


For CPQ specifically, this means:


- A Gong post-call agent can call your CPQ to generate a quote without custom code.
- A Slack bot can create, update, and share quotes using the same tools a human would use in the UI.
- A customer success platform can trigger renewal quotes automatically.
- Any new AI tool that supports MCP gets instant access to your quoting infrastructure.


The number of MCP-compatible tools a CPQ exposes is a reasonable proxy for how deeply agentic it actually is. A platform with 2-3 MCP endpoints is exposing basic read access. A platform with 20+ endpoints is exposing the full quoting lifecycle.


---


##### The Veles Approach


Veles is an agentic CPQ platform built from day one for AI agents to operate. It is not a traditional CPQ retrofitted with AI features.


The Veles MCP server exposes 22 tools covering the full quote lifecycle: creating proposals, listing products, configuring quotes with phases and line items, applying pricing rules, routing approvals, and generating documents. Any AI agent—whether it runs in Claude, Slack, Gong, or a custom workflow—can build a complete quote through Veles without a human touching the UI.


Veles integrates natively with Salesforce, HubSpot, Stripe, and conversation intelligence platforms like Gong and Fireflies. The platform serves revenue teams at companies including Procore, Built Technologies, and XOi.


The core thesis: the quote is the most important artifact in the deal. It is the proof of work of the revenue motion. It should be generated automatically from context that already exists in your revenue stack, not rebuilt manually after every call.


---


##### Frequently Asked Questions


###### How is agentic CPQ different from Salesforce CPQ with AgentForce?


Salesforce AgentForce adds agentic capabilities on top of Salesforce's existing CPQ infrastructure. It can assist with quoting tasks within the Salesforce ecosystem. Standalone agentic CPQ platforms like Veles are designed to work across multiple CRMs and AI agents through open protocols like MCP, rather than being tied to a single ecosystem.


###### Does agentic CPQ replace the sales rep?


No. Agentic CPQ replaces the manual data entry and configuration work that sales reps currently do. The rep's role shifts from quote builder to deal strategist. They review, adjust, and approve quotes rather than creating them from scratch.


###### What is MCP and why does it matter for CPQ?


Model Context Protocol (MCP) is an open standard introduced by Anthropic that allows AI models to call external tools. For CPQ, MCP means any AI agent can create quotes, check pricing, and route approvals through your CPQ without custom integrations. It is the difference between a CPQ that works with one AI assistant and a CPQ that works with all of them.


###### How long does it take to implement an agentic CPQ?


Implementation timelines vary, but agentic CPQ platforms designed for modern SaaS teams typically deploy in days to weeks, not months. Because the AI handles configuration through tools rather than complex admin setup, the implementation burden shifts from extensive rule programming to connecting data sources and setting guardrails.


###### Is agentic CPQ only for large enterprises?


No. Agentic CPQ is particularly well-suited for early-stage and mid-market SaaS teams that lack dedicated deal desk resources. The AI agent acts as a virtual deal desk, enforcing pricing consistency and generating professional quotes without requiring a full RevOps team.


###### What data does an agentic CPQ need access to?


At minimum: your product catalog, pricing rules, and CRM opportunity data. For full capability, the agent also benefits from access to call transcripts (via Gong or Fireflies), contract history, usage data, and communication channels (Slack or email) for delivering quotes and routing approvals.


---


Simon,[Veles](https://getveles.com/) ’ CEO
