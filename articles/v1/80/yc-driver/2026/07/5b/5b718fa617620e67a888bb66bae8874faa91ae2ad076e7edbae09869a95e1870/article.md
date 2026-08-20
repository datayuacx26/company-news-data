---
schema_version: "1.0.0"
document_id: "5b718fa617620e67a888bb66bae8874faa91ae2ad076e7edbae09869a95e1870"
company_key: "yc-driver"
company: "Driver"
source_id: "yc-driver-news-import-217796f58c6f"
canonical_url: "https://www.driver.ai/blog/why-context-matters/"
published_at: null
first_seen_at: "2026-07-21T17:03:14.637812+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:3191b6b9ce981c9f7886c7d7036495ed391783959c4b5600ad4c92bdf2d57e83"
---

# Why Context Matters

[Blog](https://www.driver.ai/blog) /


Business


Category


Business


Written by


Adam


Co-founder


Share on XShare on LinkedInShare on Facebook


# Why Context Matters


Why context is a constraint on AI coding agents, and how to think about what it's worth.


Jun 24, 2026 — 6 min read


Adam


Co-founder


Share on XShare on LinkedInShare on Facebook


## Solving context is not optional


We hear from customers and prospects every day about the pain they experience from lack of good context. That pain comes in the form of unnecessary friction, including:


- Engineers having to repeatedly guide and correct AI coding agents with context
- Issues and defects that, at best, are caught late in the release cycle and often not caught before deployment
- Leadership initially pushing to maximize use of flagship models to try to address these issues, only to later pull back on token budgets after seeing huge bills
- Teams assembling context in static markdown files as a bandaid


Regardless of the shape or size of the organization, the pain becomes so acute that organizations pursue that last point by cobbling together attempts at an internal context solution. This problem only becomes worse with scale.


## The value of context


Context pays off in two different ways, and it helps to keep them separate. On hard tasks, it changes the outcome. This is the difference between a correct change and a confidently wrong one. A wrong implementation with silent breaks is not a speed-up. In fact, it can have catastrophic downstream consequences.


On routine tasks, it changes the path. The agent reaches the same right answer with a lot less wandering and token spend.


From there, the outcomes worth watching follow on their own:


- **Velocity.** With friction removed, the same engineers can move faster because the context is correct and the reasoning behind it is easy to follow.
- **Work moving to the people closest to the problem.** When the right context exists, a non-resident expert can change it safely.
- **Correctness across boundaries.** The relationships that do the most damage live between repositories, which is exactly where an agent working inside one repo is blind.
- **Knowledge that stays.** Tribal knowledge stops leaving with the people who hold it, and a new engineer onboards instantaneously.


## What context actually is


Context is the truth about how your codebase works: the relationships, the constraints, the conventions, and the reasons things ended up the way they are. That truth lives in three places, and where it lives is what makes it hard to reach:


- **In the code.** Imports, call graphs, type hierarchies. A compiler or a grep can reach it, and the only real trick is getting to the right place without missing anything. Importantly, this includes the history of the code, too.
- **Across the code, in no single file.** Values that have to stay in sync, an entity built three different ways in three services, a convention every file follows but none of them states. You can’t read it off any one file. You have to derive it.
- **Outside the code entirely.** The intent behind a design, the contract between two services, the reason someone added a safeguard that looks redundant until you delete it and the bug comes back. This lives in a pull request, a design doc, or somebody’s head.


The first kind is the most recoverable while the last is the most valuable. Scale compounds this in two directions.


- **Depth.** The code base grows too large for anyone to hold in their head.
- **Breadth.** Across hundreds of repositories, critical but subtle relationships live between services that must be respected.


## What great context looks like


Not all “codebase context” means the same thing, and the differences are what separate context that helps from context that quietly hurts. Three principles matter most.


**Your codebase is constantly evolving, so great context must be dynamic and up to the minute.** Think about your experiences with documentation for code. It’s incredibly difficult for a team to produce comprehensive, high quality documentation at one point in time, let alone keep it properly up to date. Great context has to deliver on both quality and freshness.


**Great context must be intentionally narrow to the task at hand with high signal-to-noise ratio.** Even with large context windows, you can’t provide comprehensive context for even moderately complex codebases. Context from static markdown is by nature not tailored to a particular task at hand.


**The hardest, most valuable context is the negative space.** Restating the source is easy. The real work is surfacing what *isn’t* there: a missing constraint, a bounding qualifier, or a coupling that crosses boundaries. A table is append-only, with an index on a key but no unique constraint. An agent reads the schema, sees the index, and assumes the key is unique, so it answers as if there is exactly one row per key. Nothing crashes, and you get a confident, wrong answer, because the truth was an absence you can’t grep for.


Some of this gap is recoverable. Intent from a PR or a design doc is just an ingestion problem; anyone can build that pipeline. The durable gap is the truth that lives in the code but no single file, and that’s the part homegrown efforts can’t close.


## Solving your context problem


Whatever context solution you pursue, it will need to provide context as dynamic infrastructure. We’ve covered the key ways in which it needs to be **dynamic** :


1. Since your codebases are always evolving, context has to be kept up to date including across every branch in every repository.
2. Context has to be narrow to the task at hand so that an agent shipping a feature has context relevant to those 3 parts of the codebase and not the 37 others.


We haven’t explicitly addressed why a context solution needs to be **infrastructure** , but it’s easy to see why:


- You must maintain an always-on set of services that keep context fresh. Every push to a repository must be quickly processed and context built out so that engineers are working against up to date context.
- Even more difficult, just the right context must be dynamically assembled for each task at hand. It’s not a matter of pointing an agent at volumes of markdown, but delivering narrow, high signal-to-noise context at runtime.


Whether you build or buy, the same short test applies:


- Does it stay fresh, and is it branch-aware? Context that drifts is a liability, not an asset.
- Does it dynamically deliver narrow, high signal-to-noise context at runtime?
- Does it span repositories? The relationships that hurt most live between codebases.
- Does it capture non-code context like intent and conventions, or does it only restate what’s already in the source?
- Does it convey significance, telling the agent what to do, or does it just list what exists?
- What does it cost in prompt overhead, and what’s the security and retention posture?


There’s a temptation to start small with adding markdown files to your repos. Every team we’ve talked to that tried it is still in pain. The context that decides whether a change is correct is narrow, it’s different for every task, and it lives across the repositories and branches any of which may have just changed.


That context can’t be written down ahead of time and has to be assembled fresh at the moment the agent acts. So it’s not a set of files or a set of processes; it’s infrastructure that is always on. And the cost of not having it isn’t a slower agent, but a confidently wrong one.
