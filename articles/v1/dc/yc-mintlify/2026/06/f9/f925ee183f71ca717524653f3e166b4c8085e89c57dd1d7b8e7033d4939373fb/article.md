---
schema_version: "1.0.0"
document_id: "f925ee183f71ca717524653f3e166b4c8085e89c57dd1d7b8e7033d4939373fb"
company_key: "yc-mintlify"
company: "Mintlify"
source_id: "yc-mintlify-news-import-4dae4ee3e362"
canonical_url: "https://www.mintlify.com/blog/structured-docs-coding-agents"
published_at: "2026-06-08T00:00:00+00:00"
first_seen_at: "2026-07-22T04:29:43.596583+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:d21def22e992c3bb6deaa3e81c77fcf747a6f953edfea85290e920cdd6e601e0"
---

# Docs as an abstraction layer for coding agents

A company we work with has a dedicated team whose sole mandate is making AI coding agents more efficient across their engineering org.


That focus on efficiency tells you something about how enterprises are thinking about token spend today.


They're running Claude Code, primarily on Sonnet, across monolith repos with millions of lines of code. The agents are expensive and unreliable. The engineers using them spend more time prompting and re-prompting than they would just writing the code themselves.


We ran a controlled experiment with them to measure how much documentation structure matters for agent performance. The results were good enough that I think this applies to almost every company doing serious AI-assisted development at scale.


## Agents need structure. Raw code is a bad interface


When you point a coding agent at an undocumented codebase it crawls files, guesses at structure from naming conventions, and pieces together intent from patterns in the code. With no memory of what it figured out last time, this gets expensive.


You'll burn an enormous amount of context budget on orientation before the agent gets to whatever you asked it to do.


The second problem is that code only tells you what got built, not why. The reasoning behind architectural decisions lives in old Slack threads and a couple people's brains. An agent working from code doesn't have the intent layer. Even a hammer is complex if you don't know you're supposed to hit nails with it.


Structured documentation is an abstraction over the codebase that improves agents by encoding intent alongside implementation. That's the hypothesis we went in to test.


## How we designed the experiment


The team ran tests across their largest, most actively used repositories. For each one, they defined around 15 deterministic tasks spanning easy, medium, and hard. Things like "how do you compile the server," "identify whether this service has a dependency on library X," and several code generation tasks. Success was measured deterministically: shell exit codes, specific string matches, verifiable outputs. Each task ran five times per condition for statistical reliability.


Three conditions:


**No docs** — a branch with the README, CLAUDE.md, agents.md, and all documentation stripped. Clean baseline.


**Improved docs** — README plus human-written docs and AI/agent markdown files. The best realistic self-hosted outcome.


**Mintlify-generated docs** — the improved docs ingested into Mintlify, with Claude Code connected via MCP.


The primary metrics were precision (did the agent get the right answer) and discoverability (did it find the right context to work from).


## The numbers


Against the no-docs baseline:


- **64% more precise** answers across all task types
- **39% better discoverability** — agents found the right information significantly more often
- **~50% fewer tokens** consumed per task
- **1.5x faster** task completion


I expected structured docs to help, but was surprised by the extent of the improvements. The token reduction in particular changes how you think about the economics of AI-assisted development.


Letting agents work from raw code is a baseline. To compete and get the best results, you need to give them the structure that docs provide.


## READMEs are not a documentation system


READMEs can be a great solution, but just adding a file or inline comments isn't enough. These are snapshots that decay into another source of stale information for agents to hallucinate from.[Automating updates](https://www.mintlify.com/blog/automations) for your documentation when your code changes is the best way to keep your intent layer up to date. You want to get documentation as close to the code as possible.


## The token math


Big engineering orgs are[spending millions on tokens](https://www.mintlify.com/blog/tokenmaxxing-one-ai-budget-four-jobs) for AI coding tasks, and that number is climbing. Every budget holder we talk to is asking "why are we paying so much for results this inconsistent?"


Cutting per-task token consumption by 50% can be huge savings. If you're spending $1M per year on AI coding tokens, that's $500K back (assuming throughput stays constant, and throughput goes up when agents are faster and more accurate).


An agent using MCP to search structured docs retrieves precise, relevant context in a single targeted query. An agent without that infrastructure crawls files until it finds something usable or runs out of context. Cheap and accurate versus expensive and flaky.


## Better models don't make this obsolete


The obvious counterargument is that if models are getting better at navigating code, won't this problem just go away?


The experiments we ran are saying no. A more capable model working from raw code is still relearning the codebase from scratch every session. It's still burning context on orientation and missing the intent layer that lives outside the code.


What we found is that this architecture scales with model capability. Well architected docs plus a capable model outperforms the same model working from raw code.


## Where to start


We are planning to release a full public benchmark suite soon, so you can see more clearly how to replicate our experiment.


In the meantime, the easiest entry point is[mintlify.wiki/explore](https://mintlify.wiki/explore) , which generates a Mintlify docs instance from your repository. Connect your coding tools via[MCP](https://www.mintlify.com/docs/ai/model-context-protocol) to give your agents structured search over that knowledge instead of raw file crawling.


The second step is to make it automatically maintained via[workflows](https://www.mintlify.com/docs/workflows) . They run on repository pushes or cron jobs, detect when code changes require documentation updates, and make the updates for you.


If you want to run a similar evaluation for your own codebase,[reach out](https://www.mintlify.com/contact/sales) .
