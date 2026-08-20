---
schema_version: "1.0.0"
document_id: "8825422a84a0b77d55bb0bbf85c4a73a893fb001098617a8983de84a9242854c"
company_key: "yc-gusto"
company: "Gusto"
source_id: "yc-gusto-engineering-rss"
canonical_url: "https://engineering.gusto.com/worth-the-squeeze-27ccf56777fa"
published_at: "2026-03-27T00:07:31+00:00"
first_seen_at: "2026-07-19T22:15:27.842622+00:00"
fetched_at: "2026-07-28T22:00:16.885857+00:00"
content_hash: "sha256:f17c81e156fc225a8490959587750a40c80fadd170e149e48e72d5d613d78eb0"
---

# Worth the Squeeze

# **Worth the Squeeze**


[Jose Miguel Colella](https://medium.com/@colellajosemiguel?source=post_page---byline--27ccf56777fa---------------------------------------)


8 min read


·


Mar 27, 2026


--


How Agentic Workflows Make Large-Scale Refactors Justifiable


Press enter or click to view image in full size


Before solving a Rubik cube, a person thinks about the different steps to solve it


Every mature codebase carries its history in the code. Patterns that were once best practice become deprecated. Constructs that were convenient when the project was small become performance bottlenecks at scale. As engineers, we see these opportunities for improvement all the time. We know exactly what should change. And yet, the question we keep coming back to is always the same: ***“Is it worth the squeeze?”***


I’ve asked myself this question more times than I can count. The math rarely worked out. Hundreds of instances scattered across dozens of teams, each requiring careful migration, test updates, and code review. Multiply that by the context-switching cost for reviewers who own those files, and suddenly the “right thing to do” becomes a quarter-long initiative competing with product roadmap priorities.


Agentic workflows change this equation. But not in the way you might think.


The revelation wasn’t that AI could generate the replacement code, that part was almost mechanical. It was that the new engineering skill is iterating on the approach itself. Spending more time planning how to decompose and ship the work across an organization mattered more than the execution. The code was the easy part.


Let me walk you through what that iteration looked like.


## The Cost of Standing Still


Here’s a pattern I suspect every engineer recognizes: a deprecated construct proliferates through the codebase because it works “well enough.” Teams naturally prioritize product work over migrations when the cost is high and the benefit feels abstract.


In our case, the culprit was[OpenStruct](https://ruby-doc.org/stdlib-2.5.1/libdoc/ostruct/rdoc/OpenStruct.html) , Ruby’s dynamic struct that defines singleton methods on every instance for every attribute.


Creating an OpenStruct is 20–50x slower than a Struct or Hash. Each instance uses ~15x more memory. The worst part is that every singleton method definition invalidates Ruby’s global method cache.


Historically, knowing all of this still wasn’t enough. The cost to fix it was simply too high for any single engineer or team to absorb. The business case existed, but the execution plan didn’t.


## The Naive Approach (and Why It Fails)


When powerful agentic AI tools first made it possible to generate replacement code at scale, the temptation was obvious: one massive pull request. Scan the codebase, replace everything, ship it.


I must say, this was tempting. The Agents could churn through files in minutes. But anyone who’s worked on a large codebase knows why this fails spectacularly:


1. Having a single pull request that creates large scale changes can increase the likelihood of regressions and is harder to revert than smaller pull requests that are localized.
2. A large pull request is more likely to have merge conflicts in a company as large as Gusto resulting in delays to merge code
3. Reviewers are also accountable for approval of changes to parts of the codebase that are not owned by their teams and get pulled out of their current work to review a refactor they didn’t ask for.


Code throughput is not review throughput. AI can generate changes at enormous speed, but humans still need to review, understand, and approve those changes. Getting it reviewed and merged was where everything slowed down.


This realization is what set me on the path of iterating on the decomposition strategy itself.


## Iteration 1: Group by Pack


My first instinct was to decompose at the most granular level. Our codebase is organized into packs, modular boundaries you can read about[here](https://engineering.gusto.com/a-how-to-guide-to-ruby-packs-gustos-gem-ecosystem-for-modularizing-ruby-applications-e236126b8c2c) , so I created one PR per pack. Each PR was small, focused, and easy to review in isolation.


Within an hour, the AI had generated dozens of PRs ready for review.


The result? Reviewer fatigue.


Press enter or click to view image in full size


*Diagram showing one PR per pack causing reviewer fatigue for the reviewing team*


Here’s the structural mismatch I missed: one team often owns many packs. Creating a PR per pack meant a single reviewer might get pinged ten separate times, once for each module they maintain. Ten “quick reviews” still add up to a significant interruption. I was optimizing for code boundaries when I should have been optimizing for review boundaries.


## Iteration 2: Group by Team


The pivot was straightforward once I saw the problem: match work units to review units. Instead of one PR per pack, I created one PR per team covering every pack they owned. If that meant a single PR touching six packs, it resulted in one review, one merge, one interruption.


For teams with unusually large change sets, I broke them into batches. But in most cases, a single pull request covered everything. Review throughput went up immediately. The same body of work that created reviewer fatigue in Iteration 1 now moved smoothly through the organization.


## Iteration 3: Creating a Repeatable Skill


After two iterations of manually refining the decomposition strategy, I realized the process itself had become a formula:


1. Scan the codebase for instances of the deprecated pattern


2. Group instances by team ownership


## Get Jose Miguel Colella’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


3. Generate replacement code for each group


4. Create one PR per team with contextual descriptions


5. Measure performance impact before and after


[Claude Code](https://code.claude.com/docs/en/overview) has a[skill system](https://code.claude.com/docs/en/skills) that lets you package a workflow as a set of reusable instructions. I turned this decomposition process into one, so any engineer could run it against a different deprecated pattern. It bakes in the lessons: group by team, not module. Split big teams into chunks. Measure performance. Write PR descriptions with performance numbers so the work justifies itself.


```text
I want to be able to create a claude skill that allows me to run the following:  - Given a team as input  - Let us go ahead and refactor all instances found for %{team}.  Create a team of agents.   - Agent #1 (refactored) will create a git worktree that will refactor instances.   - Agent #2 (performance analyst) needs to quantify performance improvements   from OpenStruct refactors, providing memory and read improvements with   benchmark-ips and memory-profile and I would like to see results   from before and after in a branch separate from agent one work.   - Agent #2 should only be able start work when Agent #1 finishes.
```


Press enter or click to view image in full size


*The five-step refactoring pipeline packaged as a reusable Claude Code skill: scan, group by team, generate replacements, create PRs, measure performance*


## The Living Dashboard


One of the most valuable practices that emerged from this effort was using a persistent layer, in this case GitHub issues as a living dashboard for the entire refactor.


The GitHub issue served as the initial audit of the number of instances by team and pack, as well as continuously used to track the performance improvements with each merged pull request.


As the refactor progressed, the issue evolved. Each batch of PRs was logged with status updates. Teams could see which packs had been migrated and which remained. The decision log lived right there in the comments: why we grouped by team instead of packs, why certain edge cases needed special handling.


The GitHub issue became the context for Claude Code itself. When I started a new session with Claude Code, I could point it at the tracking issue and it would pick up exactly where we left off. It could read its own progress, understand what had been done, and identify what remained. The living dashboard doubled as shared memory between me and the AI agent


## Scaling Up: Agent Teams and Worktrees


Once the framework was solid, the next question was: how do we parallelize?


Claude Code supports[worktrees](https://code.claude.com/docs/en/common-workflows#run-parallel-claude-code-sessions-with-git-worktrees) , which are isolated working directories that allow multiple agents to operate on the same codebase without stepping on each other.


I set up a two-agent team:


**Agent 1: The Refactorer** . This agent ran the decomposition skill. It scanned for instances, generated replacements, fixed failing tests, and created PRs grouped by team.


**Agent 2: The Performance Analyst** . This agent’s sole job was measurement. For each batch of changes, it would run[allocation profiling](https://github.com/SamSaffron/memory_profiler) and[benchmarks](https://github.com/evanphx/benchmark-ips) before and after, calculate memory savings, benchmark instantiation speed, and produce a summary of concrete impact numbers.


Press enter or click to view image in full size


*Two-agent architecture: Agent 1 (Refactorer) creates worktree and generates code changes, Agent 2 (Performance Analyst) waits for completion then benchmarks before and after*


The two agents worked in parallel but with a dependency: the performance analyst waited for the refactorer to complete a batch, then measured the delta. This data-driven approach meant every pull request could include not just the code changes but the impact of the changes, given memory profiling benchmarks.


Here’s what those numbers actually looked like for one team’s batch:


1. 25–39x faster instantiation
2. Over 90% memory reduction
3. Above 1.5x read improvements.


Automated refactoring plus automated measurement answered the ***“is it worth it?”*** question with hard numbers instead of hand-waving. It transformed the refactor from a ***“trust me, this is better”*** conversation into a data-backed improvement with receipts.


## What I Learned


The modern software engineer’s superpower is problem decomposition and quality control.


Think of a lemonade stand. Historically, software engineering meant optimizing how you cut and squeeze lemons; better frameworks, better tooling, better technique. With AI agents, the squeezing is handled for you. Your job shifts to managing the stand: designing the menu, giving clear instructions, and tasting every batch before it goes out. The AI writes the code, but you own the quality of what ships.


Press enter or click to view image in full size


A napkin sketch showing the transition from manual labor and mechanical tools (IDEs/Frameworks) to AI Agents, where AI handles execution and humans focus on quality control and management.


## Worth the Squeeze


Was it worth the squeeze? A refactor that would have taken months got compressed into weeks. We removed a deprecated pattern entirely from the codebase, got measurable performance improvements, and built a reusable framework along the way.


What AI tools, like Claude code, have actually changed here wasn’t the coding. It made iterating on the approach cheap enough that I could get it wrong twice before getting it right. That matters more than the speed. When experimenting is cheap, “is the effort worth the outcome?” stops being the blocker.


Next time you look at a codebase-wide improvement and think “not yet,” the math is different now. Figuring out how to slice the work, what order to do things in, where the boundaries are, that’s still on you.


Onto the next major refactor. Time to drink that lemonade!


Gusto is hiring engineers who care about code quality at scale. Learn more at[https://gusto.com/about/careers](https://gusto.com/about/careers)
