---
schema_version: "1.0.0"
document_id: "224a743f59745e652b955d99028741128db75b54a80b0d8fe8a75057eba2a04b"
company_key: "yc-driver"
company: "Driver"
source_id: "yc-driver-news-import-217796f58c6f"
canonical_url: "https://www.driver.ai/blog/context-infrastructure-sdlc/"
published_at: null
first_seen_at: "2026-07-21T17:03:14.637812+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:d3f7847ce69c894dbee8fa41b90a7689ff4fb0cf1ba55909d2d85a16d9c7871c"
---

# Context Infrastructure for the AI-Augmented SDLC

# Context Infrastructure for the AI-Augmented SDLC


Context collapse breaks AI tools at scale. Learn why context is the missing infrastructure layer for the software development lifecycle.


Jan 15, 2026 — 8 min read


Adam


Co-founder


Share on XShare on LinkedInShare on Facebook


Agents are writing code. Not someday; now. Give an agent a benchmark, and it excels. Give it a real codebase, and it struggles. Not because the agent lacks capability, but because it lacks context.


Watch what happens: the agent spends tokens walking the codebase, tracing imports, trying to build a mental model on the fly. It misses dependencies. It overlooks conventions. It produces code that looks right but breaks in production because it didn’t know about a constraint three layers deep.


This isn’t a failure of intelligence. It’s a failure of information. The agent is trying to do at query time what a senior engineer spent months doing: compiling a structural understanding of how the system actually works.


## What is Context?


Context is what agents need to work. But what is context, precisely?


Context is not documentation. Documentation is written by humans, for humans, and it goes stale the moment the code changes.


Context is not code search. Search returns fragments; it can’t guarantee completeness, and it destroys the graph structure that makes code meaningful in the first place.


Context is the current state of what a system does: the architecture, the patterns, the dependencies, the conventions, the relationships between components. It’s the shape of the system, not just its parts.


A codebase with n symbols doesn’t have n pieces of information; it has n² potential relationships. The complexity scales quadratically. Context is what lets you navigate that complexity without getting lost.


We call the breakdown of context at scale “context collapse”: the moment when the complexity of a software system exceeds what any retrieval method can reliably surface. It’s why AI tools work on demos and fail on production codebases. The demo is small and self-contained. The production system is millions of lines, hundreds of services, a decade of accumulated decisions. The approaches that work at 100,000 lines fail at a million.


## Beyond Coding


Solving context collapse for coding agents matters. We’ve written extensively about why retrieval-based approaches fail and what compilation-based approaches unlock. But there’s a bigger insight here, one that goes beyond implementation. Context isn’t just needed for coding. It’s needed across the entire software development lifecycle.


Consider planning. Before you can design what’s new, you need to understand what exists. What are the dependencies? What patterns does the codebase follow? What will this change affect? Product managers write requirements without knowing what’s technically feasible. Engineers estimate work without knowing what’s already built. The planning phase is riddled with context gaps that surface later as surprises.


Consider code review. Every reviewer asks the same questions: What’s the risk of this change? What does it touch? What could break? Today, reviewers trace call graphs manually, read through files, try to build a mental model of impact. The context they need exists, but it’s scattered across the codebase, undocumented, and expensive to assemble.


Consider testing. What’s already covered? What needs manual attention? How is testing configured in this part of the system? QA teams operate with partial information, testing things that are already tested, missing things that aren’t.


Consider support. When something breaks in production, what changed? Why? How does this part of the system work? Support engineers escalate to developers because they lack the context to answer these questions themselves. Developers get pulled off delivery work to answer questions that, with the right context, wouldn’t need to be asked.


The same context, integrated across these tools and phases, is the unlock. Planning decisions flow to implementation. Implementation decisions flow to testing. Testing coverage flows to QA. Support has visibility into what changed and why. Context becomes the connective tissue through the whole process.


## The Infrastructure Gap


Now look at the infrastructure layer of modern software development:


Version control manages code. CI/CD manages deployment. Observability manages production.


Nothing manages context.


Context is broad: different for a maintainer than for QA than for support, but all derivatives of the same underlying code. Context is curated: a knowledgeable developer creates it as a leave-behind, but it needs to stay current as code evolves. Context needs governance: not everyone should see everything; permissioning matters.


Today, context lives in tribal knowledge, stale documentation, and repeated discovery. Every new engineer re-learns what others already know. Every agent re-gathers the same information. When a senior engineer leaves, the system architecture leaves with them.


This is the gap.


Git didn’t just store code. It changed how teams collaborate on code. Before Git: email patches, FTP uploads, chaos. After Git: branches, merges, pull requests, distributed collaboration. Git solved a coordination problem: how do multiple people work on the same code without stepping on each other?


Driver doesn’t just store context. It changes how teams work with shared context. Before Driver: tribal knowledge, stale docs, repeated discovery, agents re-gathering the same information. After Driver: shared context, coordinated agents, institutional knowledge that persists and stays current.


The multiplayer problem for context is this: how do multiple people and agents work with the same understanding of a system? Shared context is the coordination mechanism.


This requires pre-computation. Runtime discovery doesn’t scale. Search-based approaches break down at enterprise scale; too much code, too much noise, too little signal. Context must be derived from code, the only reliable source of truth. It must be structured, not fragmented. It must be exhaustive, not probabilistic. It must be trusted enough to act on. It must update automatically as code evolves.


Driver is context infrastructure for the AI-augmented SDLC. The code already exists. You’re not starting from scratch. Context is the current state: what the system does today. You can’t articulate a meaningful change without knowing where you are. Driver is to context what Git is to code.
