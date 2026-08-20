---
schema_version: "1.0.0"
document_id: "1cb1421a95b508a5bdd2d82c9f7d09e65163c1a3fb8db0b11e01d5a99bb8d737"
company_key: "yc-driver"
company: "Driver"
source_id: "yc-driver-news-import-217796f58c6f"
canonical_url: "https://www.driver.ai/blog/ai-coding-agents-should-make-migrations-easier-without-context-they-make-them-worse/"
published_at: null
first_seen_at: "2026-07-21T17:03:14.637812+00:00"
fetched_at: "2026-07-28T21:21:00.620727+00:00"
content_hash: "sha256:57708c274f5090eae7ed0cabd4164e0a0623ba64bee1d260d1d5b220082e7558"
---

# AI Coding Agents Should Make Migrations Easier. Without Context, They Make Them Worse.

# AI Coding Agents Should Make Migrations Easier. Without Context, They Make Them Worse.


You have to understand the legacy system before you can safely change it. That understanding is exactly what agents lack.


Apr 20, 2026 — 5 min read


Adam


Co-founder


Share on XShare on LinkedInShare on Facebook


A consultancy we work with was evaluating a 3 to 4 million line legacy .NET codebase for a potential modernization engagement. ASP.NET Web Forms, Entity Framework, built over five to seven years by a team they’d never met.


Their lead’s assessment: “Who knows what’s in there? We don’t have a lot of folks today that are familiar with ASP.NET Web Forms. This is legacy. We just don’t operate in this environment anymore.”


Before committing 12 people to a multi-month engagement, they wanted to understand what they were getting into. “I’d rather spend a couple thousand dollars figuring that out over three, four weeks before we invest in it and put our reputation on the line.”


This is the migration problem in its purest form. Before you can change a system, you have to understand it. And understanding a large, old codebase is exactly the kind of work that AI coding tools should excel at. But in practice, it’s where they fail most spectacularly.


## The Migration Paradox


Migrations are the highest-leverage use case for AI coding agents. Rewriting legacy code, updating frameworks, decomposing monoliths, moving between platforms. These are the projects where engineering teams spend months on work that feels mechanical, where AI should provide the most acceleration.


The paradox: migrations are also where agents need the most context and have the least. The legacy system is the thing nobody fully understands. That’s why you’re migrating in the first place. Documentation is years out of date. The engineers who built it have moved on. The code has accumulated multiple implementations of the same patterns, workarounds layered on workarounds, and implicit contracts that nobody documented.


A CIO at a fintech with an 8 million line Ruby monolith described it: “There’s metaprogramming, monkey patching, all these things. You have to know where those skeletons are buried. We haven’t done a great job pruning or consolidating. Over the years we’ve had three implementations trying to address the same problem, and we haven’t been consistent in saying from this point forward, only use implementation three.”


That’s the system the agent has to understand before it can safely change anything.


## What Happens When You Skip the Understanding


One of our customers learned this the hard way. They were running a 12-week modernization project on a 10 million line legacy Java ERP. Early in the project, they adopted Claude Code without any context infrastructure in place. The agents were productive. They wrote code. They made changes.


The problem: the changes created additional technical debt. The agents went down rabbit holes, made modifications based on incomplete understanding, and introduced inconsistencies. The team had to clean up after the agents. The project was stuck for over 9 months.


A consultant working on the project described how they tried to build understanding without pre-computed context: “Four of us, we all did the same thing on the same code with Claude and generated markdown files. Started to share them about and read through. When we all felt confident they were right, that’s when we gained confidence.” Four developers independently generating docs from an AI, cross-checking them against each other, then validating with the client team. And still finding errors.


After adding Driver’s pre-computed codebase context, the trajectory changed. Sessions ran longer without going off track. Their testing workflow produced reliable results. In their words, they went from “utterly broken, hit a wall” to “light at the end of the tunnel.”


The consultant leading the engagement at a private equity firm that funded the project put it bluntly: Claude “has no chance” on 10 million lines of code without pre-computed context.


## Five Kinds of Migration, Same Problem


We see migrations across our customers in several forms, and the underlying challenge is always the same: you need to understand the existing system deeply before you can change it.


**Language and platform migrations.** Moving from one language or framework to another. The 10 million line Java ERP being modernized. A fintech pivoting from a Ruby monolith to Python and TypeScript microservices. In both cases, the agent needs to understand the old system’s patterns, contracts, and dependencies before it can produce equivalent implementations in the new stack.


**Infrastructure and tooling migrations.** Changing source control, CI/CD, or deployment platforms. One customer migrated from Azure DevOps to GitHub. Commits transferred, but PRs didn’t. Their pipelines were still in Azure DevOps. The migration split their development history across two systems, and the context about why changes were made was partially lost.


**Architecture migrations.** Decomposing a monolith into services, or consolidating fragmented services. The fintech CIO described engineers who “didn’t grow up with this codebase” feeling trepidation about working in the monolith. New engineers build new services in Python. Legacy engineers maintain the Ruby monolith. Nobody wants to be the person who breaks the old system.


**Compliance modernization.** A defense contractor with a 20+ year C++ codebase needs to retroactively extract software requirements from existing code to align with V-model standards. Not a code migration, but the same deep understanding requirement. Their director of software said: “We’ve seen tools work great on small codebases and tail off at scale. A million lines of code is where things break.”


**Due diligence.** Consultancies and companies evaluating acquisitions or vendor transitions need to understand a codebase before committing resources. The consultancy we opened with treats this as an “insurance policy.” A few thousand dollars and a few weeks of investigation before signing a deal that puts their reputation on the line.


## What Migration Requires From Context


Every migration scenario requires the same four things from codebase context:


1.


**Exhaustive understanding of the existing system.** Not samples. Not search results. Complete, structured knowledge of every symbol, every dependency, every pattern in the legacy code.


2.


**Automatic currency.** The legacy system is still under active development during the migration. Context that’s a week old is already wrong.


3.


**Structural awareness.** Not just what code exists, but how it’s connected. Call graphs, dependency trees, implicit contracts between modules. RAG-based approaches destroy this structural information by chunking code into text fragments.


4.


**Cross-codebase visibility.** Migrations almost always span multiple repos. The monolith, the new services, the shared libraries, the infrastructure configuration. An agent working on the migration needs to see all of it.


We’ve written about[why current approaches to context fail](https://www.driver.ai/blog/why-current-approaches-fail) at delivering these properties and[why we built a compiler instead of a search engine](https://www.driver.ai/blog/intelligence-efficacy-why-we-built-compiler-not-search-engine) to solve them. For migrations specifically, the compiler approach means the agent starts every session with a complete, current, structured map of the legacy system. It doesn’t have to rediscover the architecture. It can focus on the actual migration work.


## Archaeology Into Engineering


Migrations have always been expensive because understanding the legacy system was manual work. You assigned senior engineers to read code, trace dependencies, and document what they found. That understanding was perishable: stale the moment someone pushed a commit. And it was trapped in the heads of the people who did the investigation.


Pre-computed codebase context changes the economics. The understanding is exhaustive, automatically current, and available to every engineer and agent working on the migration. The archaeology becomes engineering.


If you’re planning a migration and wondering whether AI coding tools will help or create more problems, the answer depends entirely on context.
