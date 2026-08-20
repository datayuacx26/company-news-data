---
schema_version: "1.0.0"
document_id: "e33af09013cb9a4de03f70744c86b138d9979fc9eb7df35928b8ff4fdbfa1013"
company_key: "yc-raindrop"
company: "Raindrop"
source_id: "yc-raindrop-news-import-6a6050810bab"
canonical_url: "https://www.raindrop.ai/blog/introducing-triage/"
published_at: "2026-05-04T00:00:00+00:00"
first_seen_at: "2026-07-22T10:48:02.200992+00:00"
fetched_at: "2026-07-28T22:15:27.754583+00:00"
content_hash: "sha256:a4ec67e3606c35343ca6f1c24dcf826bb9652e3d6268c2cd67b499e9ea2deb84"
---

# Introducing Raindrop Triage

Today we're launching Raindrop Triage, an agent that investigates your AI agents.


It lives in Slack and Web. It's also an MCP. And it's the foundation of what comes next at Raindrop.


> "The triage agent is great for the messy part of understanding agent behavior. You can ask quick ad hoc questions, spot broader patterns, and jump into the traces that show what's actually happening."
>
>
> [Kaloyan Videlov](https://www.linkedin.com/in/kaloyan-videlov/)
>
>
> AI Engineer at Framer


> "We have Triage running a Monday digest of our top issues and new patterns. It surfaces things we would've missed otherwise and points us to the relevant troublesome conversations."
>
>
> [Tuan Ahn Pham](https://www.linkedin.com/in/tuananh-pham/)
>
>
> AI Engineer at Speak


## The problem with finding agent issues


Picture a normal Tuesday.


A customer tweets that your agent is broken. Someone screenshots it into Slack. An engineer opens your dashboard, starts digging through traces, and goes hunting for the pattern. Hours later, they post in the thread: "yeah, the agent's making up order IDs when the lookup tool times out." Then someone has to actually go fix it.


Most of that time wasn't spent fixing anything. It was spent looking through dashboards, filtering, investigating, assembling datasets, finding the pattern, figuring out how many people are affected, and whether it's still happening.


With Raindrop Triage, all of that boils down to a single question (without leaving Slack).


## Triage in Slack


Just


` @raindrop` from any channel and ask it anything you'd ask a teammate who lives in your data:


-


` @raindrop` every Monday, summarize the biggest issues enterprise customers had


-


` @raindrop` how's the rollout on flag checkout-v2?


-


` @raindrop` is this still happening? (with a screenshot of a customer complaint pasted in)


Triage replies in thread, keeps context across follow-ups, and you can keep poking. "Didn't we fix it last week?" "Yes, but it regressed."


You can schedule automated briefs. You can run experiments. You can point it at customer complaints, Raindrop issues, or anything else you want investigated. And you never have to leave Slack.


## Triage Agent as an MCP


Triage isn't just a Slack bot. It's also available through Raindrop MCP. Your coding agent can use Triage as a first-class citizen.


This wasn't an afterthought for us. It's laying the foundation for what we call


**self-healing agents** .


Example:


- Set up a Devin cron


- "Every night, ask Raindrop for the most important issue and fix it"


bash


```text
> Devin "check raindrop for the top enterprise issue and fix it"


→ Calling raindrop...
→ Found: list_projects tool fails when filtered by keyword
(43 conversations affected, started April 22)
→ Reading src/tools/projects.ts
→ Editing src/tools/projects.ts, removing keyword from tool schema
→ Running tests... ✓ all passing
→ Opening PR #847
```


Claude Code, Cursor, Codex, Devin: they can all


` @raindrop` now. Your coding agent pulls a live issue from production, reads the failing examples, edits the file, runs the tests, and opens the PR.


### Why we made our agent an MCP tool


At first glance, it might seem strange to launch our agent as an MCP tool. Why not just do what everyone else does: expose the underlying tools through the MCP and let other agents call them directly?


That would've been cheaper and simpler for us (and we do


*also* expose those tools!). But making the agent itself available through the MCP has a few special advantages. It's also setting the scene for what we call the


**Multi-Agent Covenant** (coming soon).


**1. Task-specific harness**


The first, and probably most obvious: we've tuned Triage's harness to be


*very* good at debugging agent issues: understanding production data, navigating conversations, accurately calculating impact, etc. Our harness (generally) significantly outperforms Claude Code using the same tools (in speed, accuracy, etc.).


When we tried to hill-climb performance for a specific coding agent (e.g. Devin, Codex, etc.), we found those efforts to be largely futile, and also out of our control.


Exposing Triage as an MCP tool allows us to control the model-harness matching, and makes it instantly portable between any coding agent.


**2. Personalization**


As you use Raindrop, it learns what you do and don't care about:


- When you refine signals


- When you give feedback on an issue


- When an issue gets fixed


- As you use Triage agent (through Web, Slack, and MCP)


Exposing Triage as an MCP allows us to continually share these learnings with your coding agent.


**3. Context handoff**


Imagine this flow:


- Someone posts a bug in Slack


- You tag @Raindrop to look into it


- You find the issue


- You tag @Devin to fix


By exposing Triage as a tool, Devin can pick up the conversation right where you left off. No context lost.


## What's next


Triage is the foundation of the next generation of Raindrop. Most observability tools can't even tell you what broke. Raindrop closes the loop, from signal, to investigation, to fix.


***Now, your agent can fix itself.***


Get started:


[install the Slack app](https://auth.raindrop.ai/en/signup) and


[connect the MCP](https://www.raindrop.ai/docs/mcp/overview) .
