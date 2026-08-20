---
schema_version: "1.0.0"
document_id: "4c9d9e4df148f9ddbd0b87038330978c0c00ef1c32d1a85363fdfb6b6134a557"
company_key: "yc-orthogonal"
company: "Orthogonal"
source_id: "yc-orthogonal-news-import-993ffc785022"
canonical_url: "https://www.orthogonal.com/blog/runtype-ship-ai-products-across-every-surface"
published_at: null
first_seen_at: "2026-07-24T07:52:16.200334+00:00"
fetched_at: "2026-07-28T22:07:07.393518+00:00"
content_hash: "sha256:fe8221b2027b2af7058506f22c2c508fd1f4bfc6198204fa1dc93168f67434f2"
---

# Runtype: Ship AI Products Across Every Surface

Most AI agents live in one place. The reasoning works, the tools connect, and then the whole thing is trapped in a single interface because deploying the same intelligence across web chat, Slack, and email is three separate engineering problems. Adding evaluation, cost tracking, and versioning is three more. The gap between a working agent and a shipped product is where most teams stall.


Today we're excited to announce our partnership with[Runtype](https://runtype.com/) , a platform for turning AI capabilities into deployed products. Runtype lets teams create products that bundle flows and agents together, deploy them to over 10 surfaces, then evaluate, version, and cost-track everything in production. Orthogonal APIs are now available as native tools inside Runtype, so any flow or agent can access 100+ APIs without managing keys, vendor accounts, or billing for each one.


## What is Runtype?


Runtype is a product platform for AI. Instead of stopping at the agent layer, Runtype handles the full lifecycle: build flows and agents that chain tools together, bundle them into products, test with evals, deploy to multiple surfaces, and monitor cost and performance in production. Update the configuration with a version change, without the need to code gen or re-deploy.


### Core Capabilities


-


**Products** : The top-level unit in Runtype. A product bundles one or more capabilities (flows and agents) and deploys them across surfaces. Products give teams a single place to organize, configure, and ship AI-powered experiences.


-


**Flows** : Chain tools from multiple providers into a single workflow. A flow can call Apollo for company data, Hunter for email verification, Exa for press mentions, and PredictLeads for growth signals, all in one execution. Flows support 20+ step types including prompts, API calls, web crawling, data transformation with sandboxed code execution, conditional branching, vector search, and record management.


-


**Agents** : Autonomous AI entities that go beyond step-by-step flows. Runtype agents support multi-turn reasoning with extended thinking, tool use across built-in and MCP-connected services, voice output via text-to-speech, and configurable error handling with fallback models. External agents using the A2A (Agent-to-Agent) protocol can also be registered and orchestrated alongside Runtype-native agents.


-


**Surfaces** : Deploy any product to web chat, Slack, email, SMS, WhatsApp, Discord, Telegram, webhooks, scheduled cron jobs, REST APIs, and MCP servers for AI assistant integration. One product, multiple interfaces, each shaped for the user on the other end. Surfaces also include MCP server deployment, so AI assistants like Cursor and Claude Code can use your products directly as tools.


-


**Evals** : Test flows against edge cases before they go live. Run multi-model comparisons with per-step overrides to find the best model and parameter combination. Catch failures in staging, not production.


-


**Cost Tracking** : See per-call costs by provider, per-session costs by user, and trends across surfaces, all in one dashboard. Configurable margins and spend limits keep costs predictable.


-


**Versioning** : Full version management with draft, published, and test versions. Swap providers, adjust prompts, or add new tools, then evaluate the new version before publishing it to production.


-


**Records and Batch Processing** : A persistent data layer for storing structured records with metadata and vector embeddings. Process hundreds of records through a flow in parallel with batch execution, real-time progress via WebSocket, and concurrency control.


-


**MCP Support** : Runtype acts as both an MCP server (exposing your products to AI clients like Cursor and Claude Code) and an MCP client (connecting to external MCP servers for Slack, Google Workspace, Linear, GitHub, and custom servers).


-


**SDK, CLI, and API** : Build programmatically with the Runtype SDK, manage everything from the terminal with the CLI, or integrate directly via the REST API.


## Using Orthogonal with Runtype


Orthogonal APIs appear as native tools in the Runtype dashboard. Enable the ones your flow needs. No API keys, no vendor accounts, no configuration. Everything is set up through the UI, and also available through the Runtype SDK, CLI, and API for teams that prefer to work programmatically.


### How It Works


Open the Runtype dashboard, browse the tool selector, and toggle on any Orthogonal-powered API. Your flows and agents can use them immediately. Describe what you want the flow to do, and Runtype handles the orchestration: which tools to call, in what order, and how to synthesize the results.


A single flow can chain 10+ Orthogonal providers in one execution. One of our early use cases with Runtype was a prospect research flow: give it a company name, and it calls Apollo for the company profile, Hunter for verified C-suite emails, Exa for recent press, PredictLeads for growth signals, Aviato for funding data, Brand.dev for brand assets, and Scrape Creators for social activity. Nineteen tool calls across seven providers, synthesized into a single intelligence brief.


The teammate who built it didn't sign up for Apollo. Didn't configure Hunter. Didn't read Exa's docs. They enabled the tools in the Runtype dashboard, described what they wanted, and shipped it.


### Deploying to Surfaces


Once an agent or flow works, Runtype deploys it everywhere your team operates. Bundle them into a product, then add surfaces: a web chat widget on your site, a` /research` command in Slack, an email trigger that returns a brief to your inbox, a scheduled cron job for daily reports, or an MCP server so AI assistants can call it directly. One product, many surfaces, each shaped for how that user actually works.


The rep in Slack doesn't know the same product powers the web widget. They type a command like` /research Vercel` and get the answer where they already are.


### Cost Tracking Across Providers


Every Orthogonal API call flows through Runtype's spend tracking. You see per-call costs by provider, per-session costs by user, and trends across surfaces, all in one dashboard alongside your other infrastructure costs. When Hunter calls spike because someone researches a 500-person org, you see it in the same place as everything else. Configurable spend limits and margins ensure no surprises.


### Evals Before Launch


Before any surface goes live, Runtype runs product evals across edge cases: companies with sparse public data, conflicting information across providers, rate-limited APIs mid-flow. Run multi-model comparisons to test whether GPT-5 or Claude outperforms on your specific use case, with per-step overrides so you can mix and match. Evals catch failures before a single user encounters them. You know how the flow breaks because you tested it before shipping, not after.


## Use Cases


### Prospect Intelligence


Chain Apollo, Hunter, Exa, PredictLeads, and Aviato into a single research flow. Give it a company name and get back a full intelligence brief: company profile, verified C-suite emails, recent press, growth signals, and funding data. Bundle it into a product and deploy to Slack for reps, web chat for the site, email for inbound research.


### Lead Enrichment Pipeline


Build a flow that takes a list of domains and enriches each one through People Data Labs, Crustdata, and Apollo. Use batch execution to process hundreds of records in parallel with real-time progress tracking. Run evals to catch edge cases (companies with sparse data, conflicting information across providers) before the pipeline goes live.


### Competitive Monitoring


Combine PredictLeads for hiring signals, Exa for press monitoring, and Scrape Creators for social activity into a flow that runs on a scheduled surface. Deploy alerts to Slack when a competitor shows growth signals. Use transform-data steps with sandboxed code execution to score and filter signals before they reach your team.


### Account Research for Sales Calls


Before a sales call, a rep types` /research acme.com` in Slack. The flow pulls company data from Apollo, recent news from Exa, leadership contacts from Hunter, and brand context from Brand.dev. The rep walks in prepared. The same product is also deployed as an MCP server, so AI coding assistants and internal tools can call it programmatically.


## Why We Partnered with Runtype


Orthogonal gives agents access to APIs. Runtype empowers people to build products that put agents everywhere users already are. The combination solves a problem we hear constantly from developers: they can wire up API calls behind a prompt, but deploying that capability to every surface their team uses (evaluated, cost-tracked, and versioned) is where the project stalls.


Runtype treats Orthogonal APIs as native tools. Enable them in the dashboard, build the flow, bundle it into a product, and deploy it to web chat, Slack, email, SMS, webhooks, scheduled jobs, REST APIs, MCP servers, and more. Every API call routes through Runtype's spend tracking, so teams see exactly what each Orthogonal provider costs alongside everything else. When they swap Tavily for Perplexity to test search quality, it's a version change evaluated before it ships, not a code change they hope works.


For us, this partnership means Orthogonal APIs don't just get called. They get shipped into real workflows, monitored in production, and used by teams who never think about the API layer underneath. That's the kind of distribution that matters.


## Try It Today


Orthogonal APIs are available now in the Runtype dashboard. Runtype is free to start, and every new Runtype account gets $20 credit to use across more than 100 models, and now, over 100 tools.


[Get Started with Runtype](https://use.runtype.com/) |[Browse Orthogonal APIs](https://orthogonal.com/discover) |[Sign Up for Orthogonal](https://orthogonal.com/sign-up)
