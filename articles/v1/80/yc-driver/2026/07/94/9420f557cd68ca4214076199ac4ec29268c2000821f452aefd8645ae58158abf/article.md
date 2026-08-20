---
schema_version: "1.0.0"
document_id: "9420f557cd68ca4214076199ac4ec29268c2000821f452aefd8645ae58158abf"
company_key: "yc-driver"
company: "Driver"
source_id: "yc-driver-news-import-217796f58c6f"
canonical_url: "https://www.driver.ai/blog/the-archaeology-problem-why-ai-coding-agents-fail-on-legacy-codebases/"
published_at: null
first_seen_at: "2026-07-21T17:03:14.637812+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:8847aeed655c9c922ca7de8202a922050ca020e83857101c02eaf03a53451913"
---

# The Archaeology Problem: Why AI Coding Agents Fail on Legacy Codebases

# The Archaeology Problem: Why AI Coding Agents Fail on Legacy Codebases


At enterprise scale, AI coding tools don't just stop helping. They start creating new problems.


Apr 20, 2026 — 6 min read


Adam


Co-founder


Share on XShare on LinkedInShare on Facebook


Before starting Driver, I ran technical technology projects for innovation at Nike. About 1,000 people in the innovation org, roughly 7,000 engineers across global technology, all inside a matrix-organized division spanning everything from enterprise data to consumer apps to the platform teams that ran the infrastructure underneath.


Before we could start any new project, whether building a new mobile app or figuring out how to 3D-print shoe soles, we had to “connect with the enterprise.” That meant answering a long series of questions. What are the existing technologies? How does authentication work? Where’s the database of product attributes? What are the customer profile systems? What analytics tools exist? How do we send in-app messages?


Then the harder questions: What implements those technologies? Is it one codebase or many? A vendor solution? What are the actual capabilities today, and how do they work inside the code? And finally: What are the gaps between what we have and what we need for this new use case?


We called these archaeology projects. Even narrowly scoped, they could take months of engineering time. You had to rip through the code and reverse-engineer how things worked, because you couldn’t trust any other source. The Confluence pages for some of these systems were years old, sometimes almost a decade. You couldn’t rely on anything you read there to be true. You had to go back to the actual source code.


Every project started this way. It was accepted as the cost of doing business in a large, mature engineering organization. I’ve since learned it’s the same everywhere.


## The Pattern We Keep Seeing


We’ve now onboarded customers ranging from 3 million to 40 million+ lines of code, across every major language and framework. The pattern is remarkably consistent.


A director of software at a defense contractor summarized it: “We’ve seen tools work great on small codebases and tail off at scale. A million lines of code is where things break.”


A CIO at a fintech with 100 engineers and an 8 million line Ruby monolith described the daily reality: “There is quite a bit of bespoke context management that our more senior engineers have to do when they’re using AI coding tools. You have to know where those skeletons are buried in order to effectively say, here’s what I’m working on, here’s the relevant things that the agent might need to know.”


A VP of Engineering at a financial platform with 40 million+ lines across 400 repos: “Our codebase is constantly evolving, and it’s painful to keep these MD files up to date. The better the documentation, the better the agent.”


The problem isn’t that AI coding agents are unintelligent. They’re blind. They can only reason about the code they can see, and at enterprise scale, they can’t see enough.


## You Don’t Know What You Have


Here’s something we didn’t expect: no customer has ever been able to accurately state the number of lines of code or the number of repos in their organization.


One customer thought they had about 5 million lines of code based on their SonarQube data. When we processed their repositories, it was just shy of 14 million. Nearly 3x. Their CTO’s reaction: “I honestly thought we would be like five million. This was a bit of a shock.” Another customer with 400 repos described “millions of lines of just junk” they didn’t know existed.


The surprise isn’t just the volume. Customers are consistently shocked by how much cruft lives in their codebases. Code that’s totally unrelated to what the system actually does. Dead code, committed test artifacts, legacy services that nobody decommissioned. The discovery phase alone reveals how little organizations understand about their own codebases.


This matters because every existing approach to giving AI tools context depends on knowing what you have. You can’t curate markdown files for code you don’t know exists. You can’t configure RAG pipelines for repositories you haven’t inventoried. The archaeology problem starts before you even begin.


## AI Without Context Makes Things Worse


This is the finding that changed how we think about the problem. AI coding tools without proper codebase context don’t just fail at scale. They actively make things worse.


One customer was running a 12-week modernization project on a 10 million line legacy Java ERP. They adopted Claude Code early in the project, before they had any context infrastructure in place. The result: the agent created additional technical debt. It went down rabbit holes, made changes based on incomplete understanding, and introduced inconsistencies that the team then had to clean up. The project was stuck for over 9 months.


After adding pre-computed codebase context, the trajectory changed. Their sessions ran longer without going off track. Their testing workflow started producing reliable results. In their words, they went from “utterly broken, hit a wall” to “light at the end of the tunnel.”


We’ve seen this pattern at other customers too. A CTO at a European software company told us just this week: “What we see with \[their AI coding tool\] is people come up with surprisingly similar problems all the time and not always a lot of re-use, because \[the agents\] don’t talk to each other basically.” His conclusion: “I need to have a better shared context layer.”


The failure mode at scale isn’t “the agent can’t help.” It’s “the agent confidently does the wrong thing, and you don’t find out until later.” This is worse than no agent at all, because it creates cleanup work on top of the original problem.


## What Actually Drives Complexity


There’s a common assumption that codebase complexity scales with lines of code. It doesn’t, at least not linearly, and LOC alone is a poor predictor of how hard a codebase is for an agent to work with.


What we’re finding is that complexity is driven more by structural properties: the density of symbols per file, how things are connected across modules, the ratio of meaningful code to cruft. Two codebases with the same line count can have dramatically different complexity profiles. A 5 million line codebase with high symbol density and deep dependency graphs can be harder than a 10 million line codebase with simple, repetitive structure.


This is why approaches that treat “context” as a search problem fail at scale. Embedding-based retrieval doesn’t capture structural relationships. Runtime discovery (grep, file search, agent exploration) does a random walk through the codebase each time, with no guarantee of exhaustiveness. The more complex the structure, the wider the gap between what these approaches find and what actually matters.


We’ve written at length about[why current approaches to context fail](https://www.driver.ai/blog/why-current-approaches-fail) and[why we built a compiler instead of a search engine](https://www.driver.ai/blog/intelligence-efficacy-why-we-built-compiler-not-search-engine) . The short version: codebase context needs to be pre-computed exhaustively and kept current automatically. Anything less, and you’re back to archaeology.


## What Changes With Pre-Computed Context


When customers connect their codebases to Driver, a few things happen quickly.


First, they discover what they actually have. The transpiler processes every file, builds complete symbol tables and dependency graphs, and produces structured context at every level of abstraction. This is the inventory step that most organizations have never done.


Second, their AI tools stop going down rabbit holes. One customer reported that multi-repo inspection went from 30 minutes and 80,000 tokens to 1 to 3 minutes. Their pilot users said: “Can’t imagine how Claude would work without Driver anymore.” An engineering lead who didn’t know a codebase well used Driver’s context to evaluate pull requests during a code freeze: “This helped me a lot to evaluate, what is this doing? What is this for?”


Third, the context stays current without anyone maintaining it. Every push triggers an incremental update. No markdown files to keep in sync. No Confluence pages to audit. The documentation stays accurate because it’s derived from the code, not written by humans and left to rot.


## The Archaeology Problem Has a Solution


The archaeology projects I ran at Nike were expensive because they were manual, because they started from zero every time, and because the results were stale the moment someone pushed a commit. We accepted that cost because we didn’t see an alternative.


The alternative is treating codebase context as infrastructure. Pre-compute it exhaustively. Keep it current automatically. Make it available everywhere your engineers and agents work. The archaeology problem doesn’t go away, but the archaeologist becomes a compiler, and the dig site becomes a map.


If your AI coding tools are tailing off at scale, it’s not the tools. It’s the context.
