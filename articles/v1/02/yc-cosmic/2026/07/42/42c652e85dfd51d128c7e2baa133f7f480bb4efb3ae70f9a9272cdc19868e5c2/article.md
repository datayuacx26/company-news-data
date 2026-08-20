---
schema_version: "1.0.0"
document_id: "42c652e85dfd51d128c7e2baa133f7f480bb4efb3ae70f9a9272cdc19868e5c2"
company_key: "yc-cosmic"
company: "Cosmic"
source_id: "yc-cosmic-atom-acd624fed976"
canonical_url: "https://www.cosmicjs.com/blog/claude-sonnet-5-vs-opus-5"
published_at: "2026-07-30T00:00:00+00:00"
first_seen_at: "2026-07-31T23:21:18.245884+00:00"
fetched_at: "2026-07-31T23:21:20.497958+00:00"
content_hash: "sha256:2b8d3e43caf4b921f391c16a0f755cb04744bfedcb94012120ce9a7058e5f957"
---

# Claude Sonnet 5 vs Opus 5: A Real-World Comparison (2026)

Anthropic shipped[Claude Sonnet 5](https://www.cosmicjs.com/blog/claude-sonnet-5-benchmarks-pricing-developers) on June 30, 2026 and[Claude Opus 5](https://www.cosmicjs.com/blog/claude-opus-5-available-in-cosmic) on July 24, 2026. Both are excellent. The expensive mistake is picking one, wiring it into every code path, and then either overpaying on trivial work or under-resourcing the work that actually matters.


This is the practical breakdown: what each model is measurably good at, what the same job costs on each once you account for tokenization, and a routing rule you can ship this week.


## The short answer


Your workload Use Why


Content generation, summarization, chat, classification, tagging **Sonnet 5** Highest quality per dollar. This is the volume tier.


Day-to-day coding, refactors, test writing, PR review **Sonnet 5** 72.7% on SWE-bench Verified, a 10.4 point jump over Sonnet 4.6.


Long autonomous agent runs with tool calls **Opus 5** Anthropic reports it ranked first on Zapier's AutomationBench for end-to-end task completion.


Architecture decisions, ambiguous multi-step problems, novel reasoning **Opus 5** Self-verification and judgment are the headline improvements.


Anything where a wrong answer is expensive to unwind **Opus 5** Lowest misaligned-behavior score of Anthropic's recent releases at 2.3.


High-volume production traffic on a budget **Sonnet 5** It is the default on Claude Free and Pro for a reason.


If you want one sentence to take away: run Sonnet 5 by default and escalate to Opus 5 on the specific paths where judgment matters more than throughput.


## What the benchmarks actually say


There is a wrinkle worth knowing before you compare scores. Anthropic evaluated these two models on **different suites** , so there is no clean head-to-head table, and anyone who publishes one has filled in gaps with guesses.


Here is what was actually published for Sonnet 5:


Benchmark Sonnet 5 Sonnet 4.6 Opus 4.8


SWE-bench Verified 72.7% 62.3% 79.4%


Terminal-bench 76.1% 55.4% not published


GPQA Diamond 78.0% not published not published


MMMU 76.3% not published not published


MathVista 76.6% not published not published


CharacterEval 90.3% not published not published


And here is what was published for Opus 5:


- State of the art on coding and knowledge work, measured on Frontier-Bench and GDPval-AA.
- First place on Zapier's AutomationBench for end-to-end task completion.
- An ARC-AGI 3 score roughly three times the next-best model.
- A misaligned-behavior score of 2.3, the lowest of Anthropic's recent releases.
- Matches Claude Fable 5 on intelligence at approximately half the cost.
- A fast mode that runs about 2.5x quicker.


Notice what is missing: **Anthropic did not publish a SWE-bench Verified number for Opus 5.** So the honest comparison on classic coding benchmarks is Sonnet 5 at 72.7% against Opus 4.8 at 79.4%, with Opus 5 positioned above Opus 4.8 on the newer agentic and knowledge-work suites. Treat any "Opus 5 scores X% on SWE-bench" claim you find elsewhere as unsourced.


The practical read: Sonnet 5 closed most of the gap to the previous Opus generation on straightforward coding. Opus 5's advantage shows up in long-horizon agent work, self-verification, and problems where the model has to decide what the task even is.


## What it actually costs


List prices per million tokens:


Model Input Output Notes


Sonnet 5 $2 $10 Introductory pricing through August 31, 2026


Sonnet 5 $3 $15 Standard pricing from September 1, 2026


Opus 5 $5 $25 Same as Opus 4.8


Headline math says Opus 5 costs 1.67x Sonnet 5 at standard pricing, and 2.5x during the introductory window. That understates Sonnet 5's real cost, because **Sonnet 5 ships a new tokenizer that counts the same text as 1.0x to 1.35x more tokens.** You pay per token, not per word, so that inflation lands on your invoice.


Work a concrete example. Take a job that a baseline tokenizer counts as 1,000,000 input tokens and 200,000 output tokens:


Scenario Input cost Output cost Total Opus 5 premium


Opus 5 $5.00 $5.00 **$10.00** baseline


Sonnet 5 standard, no inflation $3.00 $3.00 **$6.00** 1.67x


Sonnet 5 standard, 1.35x inflation $4.05 $4.05 **$8.10** 1.23x


Sonnet 5 intro, no inflation $2.00 $2.00 **$4.00** 2.50x


Sonnet 5 intro, 1.35x inflation $2.70 $2.70 **$5.40** 1.85x


That 1.23x row is the one that should change your thinking. On text-heavy workloads that tokenize badly, at standard pricing, Opus 5 costs about 23% more than Sonnet 5 rather than 67% more. If judgment quality matters on that path at all, a 23% premium is easy to justify.


Two caveats so you use this honestly. The inflation range is 1.0x to 1.35x depending on your content, and where you land is an empirical question about your own data. And the September 1 price change means any cost model you build this month needs revisiting. Run 1,000 real requests through both models, compare your actual billed token counts, and decide from your numbers instead of this table.


## A routing rule you can ship


Most teams do not need a clever classifier. A static route based on task type captures nearly all of the savings:


1. **Default to Sonnet 5.** Content, chat, summaries, tagging, ordinary code changes.
2. **Escalate to Opus 5 on three triggers:** the task requires more than roughly ten tool calls, the task is architectural or ambiguous, or a wrong answer is expensive to reverse.
3. **Escalate on retry.** If Sonnet 5's output fails validation twice, retry once on Opus 5 instead of a third time on Sonnet 5. This is the highest-return rule on the list and it takes about six lines of code.
4. **Never hardcode the model name.** Anthropic shipped two frontier models in 25 days. Whatever you pin today will be stale within a quarter.


Point four is where most of the long-term pain lives. If your model identifiers are compiled into your application, every model release becomes a pull request, a review, and a deploy.


## Keep model config out of your codebase


Store your routing configuration as content instead. Then swapping Sonnet 5 for Sonnet 5.1 is a field edit that takes effect immediately, with no rebuild.


Install the SDK:


```text


```


Define the routing table as an object in Cosmic and read it at runtime:


```text


```


Use it at the call site:


```text


```


When the next model lands, a non-engineer updates one field in the dashboard and production picks it up. No deploy, no pull request, no engineer in the loop. Cosmic exposes this over a REST API and the TypeScript SDK, so the same config is readable from any framework or runtime you use.


This is the same argument we make in[why your AI stack should be model-agnostic](https://www.cosmicjs.com/blog/why-your-ai-stack-should-be-model-agnostic) : the release cadence is now fast enough that hardcoding any vendor's model name is a maintenance liability. For the previous generation's version of this decision, see[Sonnet 4.5 vs Opus 4.5](https://www.cosmicjs.com/blog/claude-sonnet-45-vs-opus-45-a-real-world-comparison) .


## Frequently asked questions


**Is Opus 5 better than Sonnet 5?**
On ambiguous reasoning, long autonomous agent runs, and self-verification, yes. On cost per acceptable output for high-volume tasks, Sonnet 5 wins clearly. They are built for different jobs.


**What does Claude Sonnet 5 cost?**
$2 per million input tokens and $10 per million output tokens through August 31, 2026, moving to $3 and $15 on September 1, 2026.


**What does Claude Opus 5 cost?**
$5 per million input tokens and $25 per million output tokens, unchanged from Opus 4.8.


**Did Anthropic publish a SWE-bench Verified score for Opus 5?**
No. Sonnet 5 scored 72.7% and Opus 4.8 scored 79.4%. Opus 5's published results cover Frontier-Bench, GDPval-AA, Zapier AutomationBench, and ARC-AGI 3.


**Which Claude model should I use for coding?**
Sonnet 5 for the majority of day-to-day work. Escalate to Opus 5 for architecture, cross-cutting refactors, and anything where you cannot cheaply verify the result. See our deeper breakdown on[choosing between Sonnet and Opus](https://www.cosmicjs.com/blog/claude-sonnet-vs-opus) .


**Why do my Sonnet 5 token counts look higher than expected?**
Sonnet 5 uses a new tokenizer that counts the same text as 1.0x to 1.35x more tokens than the previous generation. Compare billed tokens, not word counts.


## Build on infrastructure that outlives the model


Anthropic shipped two frontier models in under a month. The teams handling that well are the ones that never put a model name in a source file.


Cosmic is an AI-powered headless CMS where your content and your configuration both live behind a REST API and a TypeScript SDK. Model choice becomes a content decision, editable by anyone on your team, live in production the moment it is saved.


[Start free](https://app.cosmicjs.com/signup?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=blog-content&utm_content=inline-signup-cta) on the Free plan with 1 Bucket, 2 team members, and 1,000 Objects. No credit card required. If you want to talk through a specific architecture,[book a time to talk](https://calendly.com/tonyspiro/cosmic-intro) .


### Build AI-powered content workflows with Cosmic


Your content layer for AI agents. Structured, versioned, queryable, and analytics-ready out of the box.


[Start for free](https://app.cosmicjs.com/signup?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=blog-content&utm_content=bottom-signup-cta)[Book a demo](https://calendly.com/tonyspiro/cosmic-intro?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=blog-content&utm_content=bottom-demo)[Log in](https://app.cosmicjs.com/login?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=blog-content&utm_content=bottom-login)


## Learn how to build this in Cosmic


The[Learn Cosmic](https://www.cosmicjs.com/learn) hub has step-by-step lessons on building agentic workflows, connecting AI tools to your content layer, and shipping sites with Next.js, Astro, and more.


- [Build an AI Agent Team That Ships Content](https://www.cosmicjs.com/learn/build-ai-agent-team-that-ships-content)
- [Connect Cosmic to Cursor and Claude Code with MCP](https://www.cosmicjs.com/learn/connect-cosmic-to-cursor-claude-code-with-mcp)
- [Publish Content from Slack with an AI Agent](https://www.cosmicjs.com/learn/publish-content-from-slack-with-an-ai-agent)


[Browse all lessons →](https://www.cosmicjs.com/learn)
