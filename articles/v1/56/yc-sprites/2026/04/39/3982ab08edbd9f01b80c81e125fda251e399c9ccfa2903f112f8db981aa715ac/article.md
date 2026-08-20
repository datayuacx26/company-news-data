---
schema_version: "1.0.0"
document_id: "3982ab08edbd9f01b80c81e125fda251e399c9ccfa2903f112f8db981aa715ac"
company_key: "yc-sprites"
company: "Sprites"
source_id: "yc-sprites-news-import-43ec08a8b073"
canonical_url: "https://www.sprites.ai/blog/google-ads-mcp-claude"
published_at: "2026-04-19T00:00:00+00:00"
first_seen_at: "2026-07-22T14:36:47.522981+00:00"
fetched_at: "2026-07-28T21:46:35.260607+00:00"
content_hash: "sha256:5d9ceaa66b9542c187f70387b7246fd6177db796434faed478d32857b9b32178"
---

# How to Use Google Ads MCP with Claude (And Why Sprites Is Better) | Sprites

**TL;DR.** A Google Ads MCP server lets Claude query and (sometimes) modify Google Ads accounts over the Model Context Protocol. It's great for analysts running ad-hoc reports, painful for production ad operations. The Google Ads API has developer-token approval, per-customer quotas, and OAuth refreshes that MCP setups don't handle gracefully. Sprites uses a Google Ads partner integration — which is why production teams use it instead.


## **What is Google Ads MCP?**


Google Ads MCP is a Model Context Protocol server that wraps the Google Ads API so Claude (or any MCP-compatible client) can call it through natural language. Community servers like` cohnen/mcp-google-ads` expose a handful of endpoints: list campaigns, fetch reports, edit keyword bids.


MCP is Anthropic's open standard for connecting AI assistants to external systems. A Google Ads MCP server is the adapter layer between Claude and Google's APIs.


## **How to set up Google Ads MCP with Claude**


1.


**Apply for a Google Ads developer token.** Go to Google Ads → API Center. Basic access review takes a few business days. Production access requires demonstrating usage and can take weeks.


2.


**Create a Google Cloud project** and enable the Google Ads API. Create OAuth 2.0 credentials and download` credentials.json` .


3.


**Run the OAuth flow** to generate a refresh token. Store it alongside your developer token and customer ID.


4.


**Install an MCP server.** Most teams use a community npm or Python package. Configure it with the developer token, customer ID, and refresh token.


5.


**Register the server in Claude** 's` claude_desktop_config.json` with the path to the server binary and the required environment variables.


6.


**Restart Claude Desktop.** The Google Ads tools become available in the tool picker.


## **What works**


-


**Ad-hoc reporting.** "What was CPA by campaign last week?" Claude generates the GAQL query, runs it, and summarizes.


-


**Account audits.** Read-only sweeps across campaigns and ad groups.


-


**Keyword research sketches.** Listing the keywords in a campaign and asking for expansion ideas.


## **What breaks in production**


### **1. Developer token access**


Basic access developer tokens are rate-limited to 15,000 operations per day per token. Production-level access requires Google approval — and the review process expects an actual product, not an MCP experiment.


### **2. Per-customer API quotas**


Google Ads API enforces quota per` login-customer-id` and per method. Agents fan out requests ("look at every ad in every ad group") and quickly hit limits that trigger` RESOURCE_EXHAUSTED` errors.


### **3. OAuth refresh fragility**


Refresh tokens can be invalidated when Google detects unusual activity or when scopes change. MCP setups store tokens on disk and rarely auto-recover from invalidation.


### **4. Narrow tool surface**


Most Google Ads MCP servers cover a subset of the API. Performance Max asset-level operations, Smart Bidding strategy changes, and Recommendations ingestion are usually missing.


### **5. Performance Max is opaque**


PMax exposes limited reporting surfaces. Tools without a dedicated ingestion and enrichment layer can't surface asset-level performance. Sprites does that separately; MCP servers generally don't.


### **6. No approval gates**


When Claude calls a write endpoint, the change goes live. There's no "here's the plan, approve?" surface. For production Google Ads spend, that's unacceptable.


## **Google Ads MCP vs Sprites**


Capability Google Ads MCP + Claude Sprites


Read campaign data Yes Yes


GAQL query generation Yes Yes — plus pre-built reports


Developer token management Manual Partner-managed


API quota headroom Basic only Partner-grade


Performance Max asset-level insights Usually not Yes


Smart Bidding adjustments Often unsupported Yes


Approval gates None Required on every write


Audit log + rollback None Yes


Cross-channel (Meta, LinkedIn, SEO) No Yes


## **When Google Ads MCP makes sense**


Solo analysts, engineers prototyping tooling, and anyone who wants ad-hoc reporting from Claude with minimal setup. It's a legitimately good analyst workflow.


## **When Sprites makes sense**


Switch when any of the following apply:


-


**You're running production spend.** Approval gates, audit log, and rollback become required.


-


**You need Performance Max coverage.** Asset-level insights and creative refresh require surfaces MCP servers don't expose.


-


**Multiple teammates.** Shared workspace instead of per-laptop MCP configs.


-


**Cross-channel ops.** Meta, LinkedIn, SEO in the same agent.


-


**Reliability matters.** Partner-grade quotas and token management instead of manual refreshes.


## **Takeaway**


Google Ads MCP with Claude is the analyst's sandbox. Sprites is the operator's tool. For live spend, Performance Max coverage, and cross-channel work, the purpose-built AI marketing agent wins.


[See the full Sprites vs Google Ads MCP comparison →](https://www.sprites.ai/compare/google-ads-mcp)


## **Further reading**


-


[AI for Google Ads — automate Search, Shopping, and Performance Max](https://www.sprites.ai/google-ads-ai)


-


[Google Ads AI Max: what it is and why it's not enough](https://www.sprites.ai/blog/google-ads-ai-max-what-it-is-and-why-its-not-enough)


-


[How to use Meta Ads MCP with Claude](https://www.sprites.ai/blog/meta-ads-mcp-claude)
