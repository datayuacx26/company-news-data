---
schema_version: "1.0.0"
document_id: "1faeb9d9293051186aadde029e6c68667eb0a1ed362ee8c8c2f4a6f3cb2465d3"
company_key: "yc-driver"
company: "Driver"
source_id: "yc-driver-news-import-217796f58c6f"
canonical_url: "https://www.driver.ai/blog/your-ai-coding-agent-is-burning-tokens-on-context-it-should-already-have/"
published_at: null
first_seen_at: "2026-07-21T17:03:14.637812+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:d9a6a240fe7b093563b730cd9de2c7230958fbdf590704a903d1bbad39f00048"
---

# Your AI Coding Agent Is Burning Tokens on Context It Should Already Have

# Your AI Coding Agent Is Burning Tokens on Context It Should Already Have


Pre-computed codebase context isn't just a quality improvement. It's an efficiency gain you can measure in dollars.


Apr 20, 2026 — 4 min read


Adam


Co-founder


Share on XShare on LinkedInShare on Facebook


One of our customers measured what Driver saves them. 40% reduction in time and tokens per session, 20% improvement in response quality, and tens of thousands of dollars per year in AI coding tool costs alone. Their VP of Operations called it “a no-brainer.”


Another customer’s engineer made 700 Driver calls in a single Cursor session. Not because something was broken. Because that’s how much context demand exists when pre-computed codebase context is actually available.


These numbers surprised us. We built Driver to make AI coding agents more effective on large codebases. The quality improvement was the goal. The efficiency gain turned out to be just as significant, and a lot easier to measure.


## Where the Tokens Go


When an AI coding agent works on a codebase without pre-computed context, it has to build understanding from scratch. Every session. It greps. It reads files. It follows imports. It hits dead ends, backs up, tries a different path. It’s doing archaeology in real time, and you’re paying for every step.


This is the runtime discovery problem. The agent is spending tokens on orientation, not on the task you actually asked it to do. And the larger the codebase, the more tokens get consumed before the agent even starts doing useful work.


One customer with 40 million+ lines of code across hundreds of repos found that multi-repo inspection consumed 80,000 tokens and took 30 minutes without pre-computed context. With Driver: 1 to 3 minutes. The tokens that previously went to file exploration, dead-end searches, and repeated context-building just disappeared.


The head of global data and AI engineering at another customer put it simply: “What changed wasn’t the model. It was the context.”


## The Math Is Straightforward


AI coding tool costs scale with usage. As organizations roll out Cursor, Claude Code, or GitHub Copilot to more engineers, token consumption compounds. One customer was hitting their Claude spending limit 10 days into every month before they secured an enterprise account. Another burned $23,000 in OpenAI costs in four days during a mass codebase import.


These costs aren’t going down. Models are getting more capable, which means engineers use them more, which means token consumption grows. The question is whether those tokens are doing useful work or re-discovering context that could have been pre-computed.


There’s an information theory argument for why pre-computed context is fundamentally more efficient. (We’ve written about this in depth in our post on[why we built a compiler, not a search engine](https://www.driver.ai/blog/intelligence-efficacy-why-we-built-compiler-not-search-engine) .) The short version: the Data Processing Inequality says that the ceiling on model output quality is set by the quality of the context going in. Better context means less wasted computation. Pre-computing that context once and serving it to every session is more efficient than having every session re-derive it.


## What the Numbers Look Like


Across our customers, a few patterns are consistent:


One engineering lead at a 130-engineer company quantified the savings: “The cost piece is a no-brainer on its own. If it costs us $36,000 to $48,000 a year and we’re saving all those tokens in Cursor not having to grep through the codebase nearly as much, that’ll add up quickly because our bill’s getting up there for token usage.”


A financial management platform with 300 developers found that multi-repo inspection dropped from 30 minutes and 80,000 tokens to 1 to 3 minutes. Their pilot users: “Can’t imagine how Claude would work without Driver anymore.”


A quantitative trading firm measured a 5x increase in AI coding agent effectiveness after deploying pre-computed context, with 90% reduction in manual context management. They went from pilot to deployment in under two weeks. Their projected annual value at 25-developer scale: over $1 million.


An e-commerce company with 172 engineers saw their deep context agent usage reach an order of magnitude higher than we anticipated. Their throughput nearly doubled the industry P90 per GetDX. Usage was so intense it required us to redesign our pricing model.


## The Counterfactual


It’s worth noting what happens when context delivery fails. One trading firm reported “burning through credits without seeing proportional value” during a period of unstable MCP connectivity. Same agents, same codebases, same engineers. The only variable was whether pre-computed context was reliably available. Without it, you get all the cost and none of the value.


This is the counterfactual that makes the efficiency argument concrete. You’re paying for tokens either way. The question is whether those tokens produce useful work or burn on orientation.


## Context as an Efficiency Layer


Most conversations about AI coding tool ROI focus on productivity gains: faster development, more features shipped, fewer bugs. Those gains are real but hard to measure precisely. Token cost savings are different. They show up in your bill.


Pre-computed codebase context reduces the number of tokens spent on context discovery, which reduces cost and improves quality simultaneously. It’s one of the rare cases where the thing that makes the output better also makes it cheaper.


If your AI coding tool bill is growing and you’re not sure what you’re getting for it, the answer might not be a different model or a better prompt. It might be better context.
