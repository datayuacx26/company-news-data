---
schema_version: "1.0.0"
document_id: "1f7dea3d6ffbacb01246654fce118b7ae55e9c6ddea3030082c7b6766a4da089"
company_key: "yc-cosmic"
company: "Cosmic"
source_id: "yc-cosmic-atom-acd624fed976"
canonical_url: "https://www.cosmicjs.com/blog/claude-sonnet-5-benchmarks-pricing-developers"
published_at: "2026-06-30T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:40.519323+00:00"
fetched_at: "2026-07-28T21:08:50.237722+00:00"
content_hash: "sha256:b7ee573a3d605fe35fce0ccfcc517063dffbda6738bbe26145764c6f2738179a"
---

# Claude Sonnet 5: Benchmarks, Pricing, and What Developers Need to Know (2026)

Anthropic released Claude Sonnet 5 on June 30, 2026. This is the most capable and agentic Sonnet model yet, with performance approaching Opus 4.8 at significantly lower cost. Here's everything developers need to know.


> *Source:[Anthropic — Introducing Claude Sonnet 5](https://www.anthropic.com/news/claude-sonnet-5) , published June 30, 2026.*


## What Is Claude Sonnet 5?


Claude Sonnet 5 is Anthropic's latest mid-tier model, positioned between Haiku (fast, cheap) and Opus (most capable). Anthropic describes it as delivering near-Opus-4.8 performance at Sonnet pricing, with particular strength in agentic, multi-step tasks.


Sonnet 5 is the new default model for Free and Pro plans, and is also available on Max, Team, Enterprise, Claude Code, and the Claude API.


## Benchmark Results


All figures are sourced directly from the[official Anthropic announcement](https://www.anthropic.com/news/claude-sonnet-5) .


- *SWE-bench Verified:* 72.7% (Sonnet 4.6: 62.3% / Opus 4.8: 79.4%)
- *Terminal-bench:* 76.1% (Sonnet 4.6: 55.4%) — the biggest jump at +20.7 points
- *GPQA Diamond:* 78.0% (Sonnet 4.6: 68.0%)
- *MMMU:* 76.3% (Sonnet 4.6: 70.4%)
- *MathVista:* 76.6% (Sonnet 4.6: 67.2%)
- *CharacterEval:* 90.3% (Sonnet 4.6: 81.0%)


The Terminal-bench jump (+20.7 points) is the headline number for agent builders. It measures performance in real terminal environments on multi-step agentic coding tasks, exactly the workload Sonnet 5 is designed for.


## Agentic Cost vs Performance


The cost/performance charts show Sonnet 5 sitting in a compelling position: better than every previous Sonnet and Haiku on agentic tasks, at a fraction of Opus pricing. For teams running high-volume agent workflows, this is a meaningful efficiency improvement.


From the announcement:


> *"Claude Sonnet 5 can update Salesforce account tiers, send a launch announcement to enterprise contacts, and finish end to end."*


Anthropic is explicitly positioning this as a model for real, multi-step autonomous work.


## Pricing


All pricing from the[official announcement](https://www.anthropic.com/news/claude-sonnet-5) .


*Introductory pricing (through August 31, 2026):*


- Input: $2 per million tokens
- Output: $10 per million tokens


*Standard pricing (from September 1, 2026):*


- Input: $3 per million tokens
- Output: $15 per million tokens


*For reference:*


- Sonnet 4.6: $3 / $15 per million tokens
- Opus 4.8: $5 / $25 per million tokens


*Important tokenizer note:* Sonnet 5 uses a new tokenizer. The same input can map to 1.0–1.35x more tokens than on previous models. Anthropic set introductory pricing to be roughly cost-neutral vs Sonnet 4.6 to account for this. Audit your token budgets before switching in production.


## Safety


Anthropic reports lower hallucination and sycophancy rates vs Sonnet 4.6. Cyber safeguards are on by default. The misaligned behavior rate chart above shows Sonnet 5 performing meaningfully better than predecessors across multiple safety dimensions.


## What This Means for Developers Building Agents


A more capable agent model raises the bar on everything downstream. When a model can reliably complete a 10-step task end to end, the quality, structure, and governance of the content it reads and writes become the constraint.


That's where a structured content layer matters. With Cosmic, agents get:


- Typed, queryable Objects instead of raw blobs
- Draft-by-default writes with human approval before publish
- Scoped API keys that limit what each agent can read or write
- Full attribution tracking which agent wrote what, and when


Here's a minimal example of a Sonnet 5 agent reading and writing structured content:


```text


```


The agent does the work. The content layer enforces the governance.


## Should You Upgrade?


*Yes, if:*


- You run multi-step agentic workflows where reliability matters
- You want near-Opus quality at Sonnet pricing during the introductory window
- You need stronger coding/terminal performance than Sonnet 4.6


*Watch first:*


- Audit token budgets before switching. Up to 1.35x more tokens per prompt.
- Test your agent loops. More capable models can interpret prompts differently. Validate before hard-switching in production.


## Where It Fits in the Claude Lineup


- *Claude Haiku:* fastest, cheapest, high-volume simple tasks
- *Claude Sonnet 5:* best cost/performance for agentic and complex tasks (new default)
- *Claude Opus 4.8:* maximum capability for the hardest reasoning tasks


For most developers building content-aware agents, Sonnet 5 at introductory pricing is now the obvious starting point.


---


*Source:[Anthropic — Introducing Claude Sonnet 5](https://www.anthropic.com/news/claude-sonnet-5) , June 30, 2026.*


**Keep reading:**


- [MCP Server Complete Guide for Developers (2026)](https://www.cosmicjs.com/blog/mcp-server-complete-guide)
- [Claude Code vs GitHub Copilot vs Cursor: Which AI Coding Tool Wins in 2026?](https://www.cosmicjs.com/blog/claude-code-vs-github-copilot-vs-cursor-which-ai-coding-agent-should-you-use-2026)
- [Your AI Stack Shouldn't Break Every Time a New Model Drops](https://www.cosmicjs.com/blog/model-agnostic-cms-ai-provider-lock-in)


### Build AI-powered content workflows with Cosmic


Your content layer for AI agents. Structured, versioned, queryable, and analytics-ready out of the box.


[Start for free](https://app.cosmicjs.com/signup?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=blog-content&utm_content=bottom-signup-cta)[Book a demo](https://calendly.com/tonyspiro/cosmic-intro?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=blog-content&utm_content=bottom-demo)[Log in](https://app.cosmicjs.com/login?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=blog-content&utm_content=bottom-login)
