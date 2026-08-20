---
schema_version: "1.0.0"
document_id: "038c992b5457d9ce127da021ad8e523741c3806a7c5334db83a464ca97722f63"
company_key: "yc-dot"
company: "Dot"
source_id: "yc-dot-news-import-8b97b36288bf"
canonical_url: "https://www.getdot.ai/blog/openai-data-agent-lessons"
published_at: "2026-02-02T00:00:00+00:00"
first_seen_at: "2026-07-21T16:52:13.925276+00:00"
fetched_at: "2026-07-28T22:22:42.278869+00:00"
content_hash: "sha256:3086186d2ae2c109224ece92380e8fcf39d129c453f57a3e13571cbea33d06e4"
---

# What OpenAI's Data Agent Teaches Us (2026)

**OpenAI says "less is more." Then they built a 6-layer context system.**


Yesterday, OpenAI published a deep dive into their internal data agent. It's the latest in a series of "how we built our AI data tool" posts from major tech companies, following Vercel's d0 reveal last month.


The timing is interesting. Two very different approaches. And buried in OpenAI's lessons learned is a contradiction worth unpacking.


## The Setup


OpenAI's data platform serves 3,500 internal users across 600 petabytes of data and 70,000 datasets. At that scale, just finding the right table is a challenge. As one internal user put it:


"We have a lot of tables that are fairly similar, and I spend tons of time trying to figure out how they're different and which to use."


Sound familiar? This is the universal data problem. Not a lack of data, but too much of it with too little context.


Their solution: a GPT-5.2-powered agent available in Slack, web interfaces, IDEs, and directly in ChatGPT via MCP connectors.


OpenAI's data agent architecture


## Two Companies, Two Philosophies


**Vercel's approach with d0:** Delete everything. They stripped their agent down from 11 specialized tools to just two: file operations and bash commands. Let Claude navigate their semantic layer using` grep` ,` cat` , and` ls` . The result? Success rate jumped from 80% to 100%. Read the[full d0 case study](https://www.getdot.ai/blog/d0-vercel-ai-data-agent-future-analytics) for implementation details. Response times dropped 3.5x.


**OpenAI's approach:** Build more infrastructure. Six layers of context, to be precise:


The six layers of context in OpenAI's data agent


Both teams claim success. Both share the same first lesson. So what's actually going on?


## The Paradox Resolved


OpenAI's Lesson #1 is literally titled "Less is More." They write:


"Early on, we exposed our full tool set to the agent, and quickly ran into problems with overlapping functionality... To reduce ambiguity and improve reliability, we restricted and consolidated certain tool calls."


This sounds exactly like what Vercel did. But OpenAI still built six layers of infrastructure. The difference is subtle but important:


**The "less" isn't about system complexity. It's about decision surface.**


Both companies reduced the number of choices their agent has to make at runtime. They just did it differently:


Same principle, different implementations. The common thread: don't make your agent think about things it shouldn't have to think about.


## Context and Architecture Are Connected


This is the insight that emerges when you compare these approaches: the amount of context you provide determines how complex your agent architecture needs to be.


Vercel can get away with a minimal agent because their semantic layer is comprehensive. Every entity has a YAML file with descriptions, dimensions, measures, sample values, join relationships, and example questions. When your context is this rich, the agent can figure out the rest with basic tools.


OpenAI operates at a different scale. 70,000 datasets means no single semantic layer can cover everything. So they built systems to synthesize context on demand: crawling code with Codex, searching Slack and Docs, maintaining memory of past corrections.


Their Lesson #3 captures this well: "Meaning Lives in Code." Schemas describe shape; query history describes usage. But the true semantics live in the ETL pipelines that produce the data. By crawling the codebase, their agent understands freshness guarantees, business logic, and edge cases that never surface in metadata alone.


**The lesson for data teams:** Don't copy either approach blindly. Assess your context quality first. If your documentation is sparse, you'll need smarter infrastructure. If your semantic layer is comprehensive, keep your agent simple.


## What About Speed?


Here's something neither OpenAI nor Vercel emphasize: response time.


In OpenAI's screenshots, one query shows "Worked for 6m 7s." That's over six minutes for a single analysis. Vercel's d0 averages 77 seconds after optimization, down from nearly 5 minutes before.


OpenAI's agent can take minutes to complete complex analyses


These tools are optimized for depth, not speed. They're designed for complex, multi-step analyses that would take a human hours. For that use case, minutes is acceptable.


But most data questions aren't that complex. When a sales leader asks "What was revenue last quarter?" or a product manager wants "DAU trend this month," waiting six minutes is a non-starter.


This is where we see the tradeoff clearly: deep analysis versus rapid answers. Both are valid. The question is what your team actually needs most often.


At Dot, we've found that 80% of questions can be answered in under 20 seconds with the right context in place. For the remaining 20% that require deeper investigation, we support multi-minute agentic analysis. But the default should be fast.


## Evals: The Unsexy Foundation


One detail from OpenAI's post deserves more attention: their evaluation system.


They use the OpenAI Evals API to continuously test their agent against curated question-answer pairs. Each question has a "golden" SQL query. The system compares generated SQL against expected results—not through string matching, but by executing both queries and comparing outputs. The grader produces a score plus an explanation, capturing both correctness and acceptable variation.


This matters because it's how you prevent quality drift. Without systematic evaluation, you're flying blind.[DoorDash uses similar LLM-as-Judge evaluation](https://www.getdot.ai/blog/doordash-ai-platform-agents) , scoring responses across five quality dimensions. A change that improves one query type might break three others.


We've built something similar at Dot. Our Evaluation feature lets customers create test questions with expected SQL, then automatically grade the agent's responses. The comparison isn't naive equality—we use similarity-based matching with configurable tolerance (defaulting to 0.9), so numeric results that are 95% similar still pass. The system tracks pass rates, flags unclear results, and even uses an LLM to suggest documentation improvements when queries fail.


It's not glamorous work, but it's the foundation that makes everything else reliable.


## The Real Lesson


If you're building AI for your data team, here's what these posts actually teach:


**1. Simplify the agent's decision surface.** Whether through minimal tools (Vercel) or rich pre-computed context (OpenAI), reduce what the agent has to figure out at runtime.


**2. Invest in context proportional to your scale.** Small semantic layer with great docs? Keep it simple. Thousands of tables across multiple systems? Build the infrastructure.


**3. Measure relentlessly.** Both Vercel and OpenAI emphasize evaluation. Success rate, token usage, response time.[LinkedIn's experience validates this](https://www.getdot.ai/blog/linkedin-sql-bot-data-agent) : 95% user satisfaction despite 53% technical accuracy proves the right metrics matter. If you're not measuring, you're guessing.


**4. Match the tool to the task.** Six-minute deep analyses have their place. So do 20-second quick answers. Build for how your team actually works.


The future of data agents isn't about choosing between OpenAI's approach and Vercel's. It's about understanding why each works, and building something that fits your reality.


—


*If this excites you, we'd love to hear from you.*[Get in touch](https://www.getdot.ai/) *.*
