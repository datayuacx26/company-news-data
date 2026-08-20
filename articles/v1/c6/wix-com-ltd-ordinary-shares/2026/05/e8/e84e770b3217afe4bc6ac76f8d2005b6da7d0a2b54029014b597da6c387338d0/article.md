---
schema_version: "1.0.0"
document_id: "e84e770b3217afe4bc6ac76f8d2005b6da7d0a2b54029014b597da6c387338d0"
company_key: "wix-com-ltd-ordinary-shares"
company: "Wix.com Ltd."
source_id: "wix-com-ltd-ordinary-shares-rss-e1d3c855eeb0"
canonical_url: "https://www.wix.engineering/post/from-co-pilot-to-full-automation-how-wix-is-embedding-ai-agents-across-an-engineering-org-at-scale"
published_at: "2026-05-20T07:28:09+00:00"
first_seen_at: "2026-07-20T23:18:01.039713+00:00"
fetched_at: "2026-07-28T22:13:02.720629+00:00"
content_hash: "sha256:1e7a70aa1988b454c045b24781a248647847f9e4602820e13af0f757aa09e424"
---

# From Co-Pilot to Full Automation: How Wix Is Embedding AI Agents Across an Engineering Org at Scale

When you have over 2,500 microservices and thousands of engineers working across a massive multi-product platform, "just use AI coding tools" is not a strategy. It's a starting point - and not a particularly useful one.


The question we kept coming back to at Wix wasn't whether to adopt AI in our engineering workflow. That ship had sailed. The question was: what does agentic coding actually look like when you have strong domain boundaries between services, a monorepo with years of organizational context baked in, and platform teams responsible for setting guardrails that thousands of engineers rely on? What does it look like at this scale, with this level of complexity?


This post is an attempt to answer that - not as a vision document, but as a report from inside the process, which some of it is still a work in progress.


##


**The Spectrum Problem**


Before we could build anything, we needed a shared mental model.


The AI tooling landscape in 2025-2026 feels chaotic if you look at it as a list of products. Claude Code, Codex, Cursor, GitHub Copilot, Gemini (and that’s just the “major” coding agents) - each is doing something slightly different, and the marketing language doesn't help. "Agents" gets applied to everything from a single API call to a system that autonomously opens PRs and monitors production.


The framework we found most useful is a spectrum with three tiers:


**Cloud Agents** - LLMs with tool access, but no persistent file system. Stateless per task. Useful for one-shot queries, but they have no codebase awareness, no iteration loop, no project context. Think of them as very smart autocomplete.


**Coding Agents (AKA Harness)** - The sweet spot for coding work. A specialized client that orchestrates an agent inside your actual codebase. The harness manages the outer loop - when to run, what tools to expose, what guardrails to enforce - while the agent handles the inner work. Claude Code, Cursor in agent mode, GitHub Copilot agent: these are harnesses. They support skills,


[AGENTS.md](http://agents.md/) , MCPs, sub-agents, tool permissions. This is where most serious engineering automation lives today.


**Agent with Computer** - A generalization of the harness where the environment is a full computer. Browser, terminal, desktop - the agent can see the screen and interact with it. OpenClaw-style systems fall here. The action space is unbounded. More capability, more autonomy, more risk.


The industry is converging on the harness tier as the practical sweet spot for software development (as of the writing of this article, there’s glimpses of a “clawfication” process of this field as well). At Wix, this is also where we've focused - but the interesting part isn't the tools. It's what we've built on top of them.


##


**What We Built: AI Across the Full SDLC**


The insight that shaped our approach: AI shouldn't be a productivity plugin bolted onto the existing workflow. It should be embedded at every phase of the software development lifecycle - from the moment a task is triggered to the moment code ships to production.


We organized our efforts around five stages:


**Triggers** - The entry points for agentic work and the SLDC. This includes our Creator Kit framework, "Fix My Bug" (which routes bug reports directly into an automated coding flow), N8N-based automation pipelines for custom automations, and integrations with our internal Wix Standards tooling. The goal here is simple: reduce the distance between "something needs to happen" and "an agent starts working on it."


**Code** - The heart of the system. We built out Agent Ready Repositories: a standard for structuring a codebase so that an AI agent can contribute to it reliably. This includes


[AGENTS.md](http://agents.md/) for always-loaded minimal context, progressive disclosure through linked domain-specific markdown files, and Markdown Design Logs that keep agents aligned with architectural decisions made by the team.


To consume common patterns and Wix workflows we also create a deliver plugins for various coding agents, maintained by our


[xEngineering Guild](https://www.wix.engineering/post/the-xengineer-a-new-blueprint-for-software-engineering-in-the-ai-era) . We also run AI Migrations - automated workflows that translate Wix Standards rules into actionable PRs across the codebase and BCA (Background coding agent), a REST API that runs single-shot coding tasks at scale.


**Review** - This is where we've seen some of the most tangible impact. We’re now in a period moving away from one initial automated review solution into a coding agent based solution, which feeds into GitHub Actions with Codex to deliver automated line-by-line review on every PR. The reviewer applies Google's code review standards, generates architectural summaries, and flags bugs, performance issues, and style violations.


Crucially, it also handles what we call "deslopping" - identifying and removing low-value additions like unused functions, unsolicited features, or over-engineered solutions that AI coding tools occasionally produce.


**Build / CI** - When a build breaks, we don't want an engineer spending 45 minutes triaging CI logs. We built "Fix My Build," which automatically analyzes failure output and generates a root cause hypothesis. It's not always right, but it's right enough often enough to meaningfully reduce the mean time to resolution on CI failures, with a handy - fix button to trigger a background coding agent to automatically fix the issue.


Additionally we offer a variety of MCP tools that help developers analyze and understand CI/CD issues. Maintained by the relevant infrastructure teams.


**Monitor -** We exposed MCPs connected to our monitoring tools to allow both background agents and developers to use agents to inspect and analyze issues in production, while also integrating third party tools such as


[WildMoose](https://www.wildmoose.ai/) , improving our product's performance and stability by achieving over 85% correctness on root cause identification and a 17% improvement in the P90 of Mean Time To Resolution (MTTR) of alerts.


##


**The xEngineer Shift**


Underneath all of this tooling is a more fundamental change in how we think about engineering roles.


We've been moving from a world of specialists - backend engineers who own the backend, frontend engineers who own the frontend - toward what we're calling the xEngineer: an AI native engineer that has at least one domain expertise who owns an entire feature end-to-end, from initial architecture to production deployment.


This isn't a new idea. What's new is that AI makes it tractable. An engineer with strong design instincts and domain knowledge can now cross stack boundaries that would previously have required a handoff. The AI fills the fluency gap. The engineer provides the judgment.


This changes what platform teams at Wix need to provide. It's not enough to offer tools - we need to offer


*infrastructure* . A self-service layer that gives any team automation capabilities across the entire SDLC without requiring them to build it from scratch. The guardrails, the standards, the context engineering - these become platform responsibilities, not team responsibilities.


The three-stage adoption journey we've seen among our engineers maps onto this shift:


1.


**Co-Pilot Proficiency** - The engineer is still driving. They use AI to accelerate Architecture Decision Records, implementation, and review cycles, but they're in the seat for every decision.


2.


**Semi-Autonomous** - The engineer steps back from the IDE. They use worktrees to run background agents on isolated tasks, treating code as a stream of abstract work items rather than lines to write.


3.


**Full Automation** - The engineer defines the intent, sets the guardrails, and lets the system run. Agentic orchestration handles the loop from trigger to PR to review. The engineer reviews outcomes, not processes.


Most of our engineers are somewhere in the first two stages. A smaller, growing group is pushing into full automation. The infrastructure we're building is designed to make that progression feel inevitable rather than scary.


##


**What This Looks Like in Practice**


The most honest thing we can say is that this is still in motion. We don't have a tidy case study with a before/after graph. What we have is a system that's getting more complete - and an engineering culture that's adapting to it in real time.


A few things we've learned that might be useful if you're facing a similar problem:


**Make repositories agent-ready before you deploy agents.** An agent dropped into an unprepared codebase will produce unreliable results. The investment in


[AGENTS.md](http://agents.md/) , progressive context disclosure, and design logs pays off immediately - not just for AI, but for any new engineer joining the team, or an engineer from a different team, or even creators working directly on codebases.


**Treat the harness as a platform responsibility, not a team decision.** At Wix's scale, having every team independently configure their agent setup creates inconsistency and drift. Standardize the harness layer centrally by using curated Wix plugins; let teams customize on top of it.


**Deslopping is a first-class concern.** AI-generated code tends to over-engineer. Build review mechanisms that specifically target this - not just correctness, but necessity. An unused abstraction that passes all tests is still a liability.


**The review stage is where trust is built or lost.** Engineers will embrace AI-generated code faster if they trust that the review catches what matters. Invest in the review layer before pushing for higher automation rates upstream.


**The goal isn't to remove engineers from the loop. It's to elevate what they're in the loop for.** The engineers who are thriving in this shift are the ones who've moved their attention upstream - to architecture, system design, and the quality of the intent they're giving the agents. That's a skill worth developing deliberately.


##


**Final Thought**


The AI tooling landscape will keep changing. The agents will get better, the harnesses will get smarter, and the line between local and cloud execution will continue to blur. What won't change is the underlying challenge: how do you embed this into an engineering organization at real scale, with real complexity, without losing the standards that make the code worth shipping?


That's the problem we're working on.


This post was written by


**Nitay Rabinovich**


**More of Wix Engineering's updates and insights:**


-


Follow us on:


[Twitter](https://twitter.com/WixEng) |


[Facebook](https://www.facebook.com/WixEngineering/) |


[LinkedIn](https://www.linkedin.com/showcase/wix-engineering/) |


[Instagram](https://www.instagram.com/wix_eng/)


-


Join our


[Telegram channel](https://t.me/wixeng)


-


Visit us on


[GitHub](https://github.com/wix)


-


[Subscribe to our monthly newsletter](https://www.wix.engineering/subscribe)


-


Subscribe to our


[YouTube channel](https://www.youtube.com/WixTechTalks)


-


[Follow our Medium publication](https://medium.com/wix-engineering)


-


Listen to our podcast on


[Apple](https://podcasts.apple.com/il/podcast/wix-engineering-podcast/id1503976848) ,


[Spotify](https://open.spotify.com/show/5CmjtjpdcKkHDnr0601uYS?si=PcOf7Rx_RUmGojFj5n7CEA)
