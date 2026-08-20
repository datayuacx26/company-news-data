---
schema_version: "1.0.0"
document_id: "2a892fd2ae6dac928fb8952eb1d7aba2db3cc84ffbec61094cc83fa70ab1d264"
company_key: "gitlab-inc-class-a-common-stock"
company: "GitLab Inc."
source_id: "gitlab-inc-class-a-common-stock-atom-8616b2ef668b"
canonical_url: "https://about.gitlab.com/blog/building-an-ai-dev-workflow/"
published_at: "2026-08-19T00:00:00+00:00"
first_seen_at: "2026-08-19T20:17:55.762427+00:00"
fetched_at: "2026-08-19T20:17:57.671505+00:00"
content_hash: "sha256:0e9bf88d0d398a8c6528feb1b928c3df323c7e434384acb7dff4dcb09a455396"
---

# From chaos to context: Building an AI dev workflow

There's a particular kind of frustration that happens when prompting an AI assistant with the same correction multiple times in a single session. The marvels of modern large language models (LLMs) make it so you're working with the most enthusiastic apprentice you'll ever have. However, that apprentice also happens to be an amnesiac. “Yes, I really do want my commit messages formatted that way, we've had this conversation three times already.”


Or perhaps you've experienced the trouble of trying to orchestrate several parallel AI sessions, only to watch them independently start solving the same problems and deleting each other's work.


These frustrations led me down a path of iteration. I worked on early renditions of two GitLab AI features:[Explain this vulnerability](https://docs.gitlab.com/ee/user/application_security/analyze/duo/) and[Resolve this vulnerability](https://docs.gitlab.com/ee/user/application_security/vulnerabilities/#vulnerability-resolution) features. They felt naive at the time, and I wanted more from them.


Agentic AI delivered that. Instead of one-shot suggestions I had to prompt for and paste back, an agent could read the codebase, make the change, and run the tests on its own. It was doing the work rather than just advising on it. From there, I moved through GitLab Duo Custom Agents, VS Code integrations, and eventually[OpenCode](https://opencode.ai/) , an open source agent that describes itself as helping you "write code in your terminal, IDE, or desktop."


Along the way, I've distilled what's been working for me. AI coding assistants are genuinely transformative, but they need your engineering instincts to guide them. They amplify both good decisions and bad ones, so direction matters. The tools will keep changing, but here's what's helped my day-to-day engineering so far.


## An optimized AI workflow


Before diving into the principles, it's worth showing what an optimized AI workflow actually looks like in practice.


**Priority comes to me.** When I start a session, the AI loads my active sessions, unresolved blockers, standing directives, and recent decisions. It then fetches my GitLab todos, active MRs, and tracked epics, presenting them in my defined priority order: stale items first (things falling through the cracks), review requests from others (don't block teammates), questions needing my response, my own blocked MRs, and finally everything else. I no longer spend time figuring out what I should be doing; the context comes to me.


**The focus paradigm has shifted.** Software development typically required hours of intense, uninterrupted focus to get anything meaningful done. That's changed. In a 15- or 30-minute window, I can ask what's at the top of the queue, have the AI load context for that item (prior decisions, blockers, relevant procedures), and start delegating coding and testing. The AI brings me up to speed nearly instantly, rather than me needing to rebuild mental context from scratch.


**Parallel work on multiple merge requests.** I use git worktrees with isolated test databases to have AI sessions work on multiple merge requests simultaneously. Each worktree gets its own session, its own database, and a claiming system prevents sessions from stepping on one another. The AI runs tests, ensures the code works, and I review for correctness.


**Recurring tasks are made into procedures.** For a priority epic I track, the AI handles weekly status updates by fetching current state from GitLab, comparing to the previous week's state to compute deltas, and drafting the update with progress metrics. The procedure is documented in my directives so any session can execute it consistently.


**Token efficiency matters.** I've contributed improvements to GitLab's[REST and GraphQL API](https://docs.gitlab.com/api/rest/) and the[OpenCode GitLab plugin](https://gitlab.com/vglafirov/opencode-gitlab-plugin) to reduce context overhead, and built a local workflow Model Context Protocol ([MCP](https://about.gitlab.com/topics/ai/model-context-protocol/) ) server that encodes common patterns. Instead of the AI reconstructing how to do something from scratch each time, it calls an optimized tool that handles the data gathering, leaving reasoning to the[LLM](https://about.gitlab.com/blog/what-is-a-large-language-model-llm/) . This makes responses faster and keeps token usage manageable.


**Active delegation is the underlying pattern.** I preserve top-level context and alignment while the AI executes specific tasks. Before working on any merge request, the AI claims it to prevent conflicts, loads its history from memory, and checks for relevant directives. I maintain oversight of direction; the AI maintains execution velocity.


## Write surgical directives, not vague instructions


Every session starts fresh. Without intervention, you'll re-explain the same preferences, the same conventions, the same quirks of your codebase.


The solution is explicit, persistent directives. Agentic AI tools have begun standardizing on AGENTS.md configuration files that load as boot context. But the key insight isn't that you need directives; it's that they need to be specific.


A vague instruction like "be careful with comments" doesn't work. The AI will acknowledge it and then do whatever it was going to do anyway. What works is something specific:


> "ALWAYS verify user IDs exist before posting comments under their name. STOP and ask if unsure."


The all-caps keywords aren't just for emphasis. They seem to make the AI respond more reliably to the instruction.


**Pattern to apply:** When the AI tool makes a mistake, don't just correct it. Ask it what directive would prevent this mistake next time. Have that dialogue, then ask it to note it for next time. Over time, you build a set of instructions tailored to your actual workflow, not hypothetical best practices.


My[agents-config repository](https://gitlab.com/ghavenga/agents-config) used to contain over a dozen specialized configuration files that emerged this way: code review guidelines, merge request workflows, database review procedures, and comment writing standards. Each one exists because I was doing it often enough that it made sense to proceduralize and get the AI to execute it consistently. The memory system I describe later in this article lightened this approach; those directives now live in searchable memory rather than static files. The repository now contains practical examples and a[usage guide](https://gitlab.com/ghavenga/agents-config/-/blob/master/USAGE-GUIDE.md) showing how I work day-to-day.


## Parallel sessions need coordination primitives


Running multiple AI sessions simultaneously can be efficient, if you can manage it well. Three terminals, three assistants, three parallel streams of work. But without coordination, you'll probably encounter familiar problems:


- Sessions editing the same files without knowing the other exists
- Multiple sessions working on the same item (assuming you have a way to claim work, which you probably don't yet)
- Contradictory decisions made in isolation
- Sessions deleting each other's work, or blithely committing it with their own changes to unrelated MRs
- Duplicated effort because neither session knows what the other did


If you've done any concurrent programming, you'll recognize these as classic coordination problems. The solutions are similar, too: You need primitives for claiming work, tracking state, and sharing context.


My first solution was file-based working notes. They worked, but they weren't searchable, weren't linked to the work they described, and didn't persist well across days. I needed something that could surface relevant context automatically.


This led me to build[opencode-memory](https://gitlab.com/ghavenga/opencode-memory) , a persistent semantic memory system. The architecture reflects solutions to problems I kept solving by directive: hybrid search (keywords for exact terms, semantics for fuzzy recall), session coordination through claim/release, and boot context that surfaces critical directives automatically.


The system has grown considerably since its first iteration. It's now a full knowledge graph with over 760,000 indexed code entities, 15,000+ memories including conversation summaries, and 27,000 links between them. When GitLab announced[Orbit](https://docs.gitlab.com/orbit/) , a knowledge graph that indexes your entire software development lifecycle (SDLC), it lined up almost exactly with the direction I'd been taking locally. Orbit answers cross-SDLC questions like "what breaks if I change this service?" by connecting code, merge requests, and pipelines. My memory module already indexed my codebase locally for fast recall, so rather than reinvent the wider view I wired Orbit in to enhance it. The local memory holds session-level context like decisions, blockers, and procedures, and Orbit adds GitLab's broader SDLC graph on top. One of the best outcomes is that when I mention a function in conversation, the AI recalls exactly where it lives without me having to look it up.


Prior decisions also surface automatically when starting work on a merge request. Blockers persist until explicitly resolved. Procedures defined once are available forever. Reminders automatically bring themselves to my attention where they matter. And the combined graph, my local memory plus Orbit's SDLC data, answers those questions far more efficiently and effectively than a plain text search ever did.


## Check before you build


No idea is unique. Searching for memory systems for AI coding assistants reveals dozens of approaches. Somebody probably already built what you were thinking about in a coffee-fueled AI rampage three weeks before you imagined it. There's even a PyPI package called` opencode-memory` that does something very similar to what I built, just with a different vector database backend.


The barrier to building what you need has dropped so dramatically that many people independently arrive at similar solutions. You can go from idea to working prototype in a week.


I'd recommend checking whether something already exists before spinning up a new project. If it almost solves your problem, consider whether contributing might be better than creating another variant. This advice is as old as software development itself; AI just exacerbates it a hundredfold.


My memory system barely offered much over existing solutions at first. It's only after months of iteration and deep GitLab integration that I feel it's somewhat more justified. I did at least contribute improvements back to the[OpenCode GitLab plugin](https://gitlab.com/vglafirov/opencode-gitlab-plugin) rather than forking it, because that's where my changes could help the most people.


The question worth asking yourself: Did I bother to look if something already exists and solves this problem, or has AI made it so easy to code that I've ignored all forms of due diligence?


## From active recall to passive context


The vectorized memory search was an immediate win. I transitioned to using it the same day I built it. Recalling useful details became trivial. The next challenge was getting the AI to know there was something worth remembering implicitly, without me prompting it every time.


I'd made progress: token-efficient memory of specific procedures, a growing corpus of innate recall memories. But it felt like I was just rebuilding AGENTS.md with fancy additions. I needed something smarter.


The solution was proactive context injection. Instead of the AI calling recall tools explicitly, the system now automatically searches for relevant memories before each interaction. When I mention a merge request number, relevant prior decisions appear in context without me asking. When I'm about to write a comment, the comment-writing guidelines surface automatically. Most of the time, at any rate. It's still a work in progress, but each day I hone it a little further.


This shift from active to passive recall made a real difference. Over 30 days, the system achieved around 91% effectiveness at surfacing relevant context automatically. Sessions with proactive injection needed zero explicit recall calls on average, compared to 17 without it. It's not perfect; there are still moments where I need to tell the AI to remember something or to improve. But it's an iterative process, and it's getting better.


The remaining misses were instructive. Many happened while I was iterating on how boot context loading worked. The fix was a boot gate: a minimal trigger in the startup context that tells the AI to pause and load directives before doing anything else. Even with proactive injection, sometimes the AI needs to be told to stop and think first.


## What AI still can't do


AI can execute procedures reasonably consistently, if those procedures are in context. It can improve its own instructions, if prompted to think about it. It can coordinate across sessions, if given the primitives to do so.


But AI doesn't notice things implicitly. It doesn't feel that a procedure is awkward. It doesn't recognize that you've hit this same problem three times this week. It doesn't have the pattern recognition that comes from years of debugging production systems at 2 a.m. while wondering if perhaps carpentry might have been a better career choice. At least, not yet.


The sweet spot seems to be using AI to eliminate menial work, providing the right context at the right times, and watching for what it misses. You provide strategic oversight and pattern recognition. The AI provides tireless execution and enthusiasm for tasks we once found tedious and time-consuming. That's extremely empowering.


I've watched AI enthusiastically build features while introducing concurrency bugs into its own tooling, blocking itself with synchronous operations. It had no idea. In hindsight, this was an alignment problem. I could have planned with it earlier to ensure a good async pattern. I'd hoped it would build a better pattern from the start, but I was being optimistic. One redirect from me pointing out the architectural flaw, and it was fixed in minutes.


That's the pattern: human spots the problem, AI executes the fix rapidly. I could have done it myself, just not as quickly. AI can't detect meta-inefficiencies yet, though I'm sure someone's busy writing a dedicated agent for that.


## Invest in your tools


Working this way means accepting constant change. My day-to-day work has shifted completely, and repeatedly, in the space of months. Compare that to how workflows changed slowly over years earlier in my career.


Don't try to settle into a "new normal." It will change. The tools that help today may be obsolete next week at the rate we're going.


Rather, iterate on your tooling itself. The workflow that helps you work faster becomes the subject of optimization. I've contributed new API endpoints to GitLab, including[group uploads](https://gitlab.com/gitlab-org/gitlab/-/merge_requests/230537) and[GraphQL mutations for MR workflows](https://gitlab.com/gitlab-org/gitlab/-/merge_requests/220801) , specifically because I needed them for AI-assisted development.


It's a meta-loop: better tools lead to more productivity, which creates more capacity to improve tools. There's an old adage: "Give me six hours to chop down a tree and I will spend the first four sharpening the axe." I spend a fair bit of time these days sharpening my axe. I've never had a better grindstone.


## The takeaway


Code is a commodity now. We're no longer paid primarily to type it. We're paid to know what code should exist. To recognize when an approach is fundamentally flawed. To spot inefficiencies before they become problems. To provide the direction that turns raw capability into useful outcomes.


I'd been working on guidance for how GitLab's CREDIT values applied to AI use, and then[GitLab Act 2](https://about.gitlab.com/blog/gitlab-act-2/) retired CREDIT entirely, replacing it with new operating principles built for the agentic era. A good example of how fast this space shifts: My own guidance was overtaken before it landed. But the core insight remains: Getting this balance right matters at an organizational level, not just a personal one. AI should augment human work, not substitute for it.


The tools will keep evolving. The specifics will change. But this fundamental insight won't: AI amplifies human judgment. It shouldn't replace it, though it tries if you let it, usually at the cost of code quality and stability. Vibe coding can take you far, but even with the most thorough AI reviews, there's an assurance of quality and careful consideration in design that I've not yet seen AI provide on its own.


Several colleagues at GitLab have started using variations of this workflow, and watching them experience the same "aha" moments has been validating. One recently messaged me: "Definitely noticed an improvement in my sessions since using the plugin," meaning the opencode-memory module. I won't pretend that didn't make my entire day. The questions shift from "how do I get AI to do X" to "how do I give AI the context it needs to do X well." That's the real unlock: not the tools themselves, but the realization that context is the bottleneck.


Fascinatingly, the better you get at this, the more you start to realize that you may be the bottleneck. But sometimes that's also an indication that your own processes could be improved even more. I don't feel like I've reached a maximum yet, not by far. Different tasks benefit from different approaches, even when AI-empowered, and there's still plenty of experimenting to do.


One final note on pace: The memory system has seen 143 commits in under a month. It's built on MCP, which means it works with any agent that speaks the protocol: not just OpenCode, but Claude CLI, Cursor, and others. By the time you read this, I've probably added features I haven't thought of yet. That's the nature of working in this space right now. The tools evolve faster than the documentation.


*The tools mentioned in this article are open source:[agents-config](https://gitlab.com/ghavenga/agents-config) for AI directive configurations, and[opencode-memory](https://gitlab.com/ghavenga/opencode-memory) for persistent session memory.*
