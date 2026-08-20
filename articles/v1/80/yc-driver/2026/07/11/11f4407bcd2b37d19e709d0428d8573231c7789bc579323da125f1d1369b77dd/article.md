---
schema_version: "1.0.0"
document_id: "11f4407bcd2b37d19e709d0428d8573231c7789bc579323da125f1d1369b77dd"
company_key: "yc-driver"
company: "Driver"
source_id: "yc-driver-news-import-217796f58c6f"
canonical_url: "https://www.driver.ai/blog/from-one-branch-to-every-branch/"
published_at: null
first_seen_at: "2026-07-21T17:03:14.637812+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:92f2325f3ae9c540d3b5a00c53dc6533132a5833775084df3c4951695b68e27b"
---

# From One Branch to Every Branch

# From One Branch to Every Branch


Driver adds multi-branch support for enterprise scale codebases.


May 4, 2026 — 8 min read


Daniel


Co-founder


Shane


Jacob


Share on XShare on LinkedInShare on Facebook


## How Driver Became Infrastructure for Code Evolution


Building the infrastructure to compile context over arbitrarily sized and shaped codebases and deliver it at enterprise scale has involved a lot of foundation laying. Up to this point, Driver has had a one-branch view of the world: one blessed (default) branch per codebase, and that’s it.


We’ve now expanded to the full, multi-dimensional truth of a living codebase by putting in place a final keystone to our original vision: multi-branch support. In this post, I’ll discuss our motivations, how we built it, and interesting challenges along the way.


## Why This Matters Now


### The Branch Gap: My Experience


I’ll motivate by sharing my experience as a daily user of Driver. I haven’t written a line of code by hand since December 2025, but I’ve shipped more substantial code in Q1 2026 than ever before. This story is being told a lot today and my version is for another day (although we’re *very* excited to share more about our agentic SDLC at Driver).


For me, and many others, what made this possible isn’t just how flagship models and agent rigs came together at the end of 2025, but also using disciplined, structured AI SDLC processes on top. Driver the product has been a crucial part for all of us developing this way at Driver. To paint with broad strokes, our SDLC process includes major pillars with concrete transition points between them and effective orchestration.


The first two pillars are research and planning, and Driver is particularly critical in these stages. They’re dominated by fact-finding, bringing threads together, concerted design, and making informed decisions. Missing critical context early poisons the whole process. You want to capture all of the critical context so you can make the right design decisions and guarantee smooth downstream implementation.


I’ve gotten very comfortable and confident with this Driver-infused process. But as I move into later stages, I’ve had to stop using Driver because its state is only up-to-date with the default` develop` branches in our codebases.


Fast-forward past planning and validation: we’re deep into implementation with Claude. Something breaks, an unexpected integration problem surfaces, or you realize you need to revise even the best initial plan. What I really need is to evaluate in the context of both the codebase’s pre-feature state *and* the partial implementation I’m deep into. Because Driver lacks the latter, I must forge ahead without it — losing that warm trust that comes from Driver’s exhaustive guidance for Claude.


I ran into more and more of these scenarios where I wanted to reach for Driver but couldn’t because the delta between the relevant state and Driver’s single-branch knowledge would be a problem:


- **Handoff to reviewers:** We’re not just using AI to write code, but to help us review and assess. Ideally a reviewer uses Driver to ask questions about the feature, validate architectural concerns, and be a critical advisor in review. But the feature branch isn’t tracked.
- **Pre-merge debugging:** Driver is an excellent tool for investigating bugs and issues. But when testing surfaces bugs *prior* to merge, Driver is at maximum delta with respect to its knowledge of the code under test.
- **Cross-branch awareness:** We’re building fast and in parallel at Driver. Sometimes I want to pull in information from a colleague’s parallel feature branch that I know will need to work together with mine.


### Multi-Branch is a Must for Enterprise Scale


For larger enterprises, there are further high-value scenarios that depend on multi-branch support: long-term debugging and support for release branches, very long-lived branches beyond individual feature scope, and broad ticket analysis and project management efforts. Multi-branch is table stakes for large-scale enterprise adoption.


We have strong conviction about how emerging AI SDLCs and orchestrators will mature and become the dominant way software is developed. It’s an exciting new world with a lot of active experimentation. But however you slice it, the AI is systematically operating in a structured loop. The value of a structured SDLC is similar to our compiler architecture — structure and constraints provide guarantees, repeatability, and scaling. This is a sweet spot for Driver, but only if it closes over all branches of software in the SDLC.


## How We Built It


### Architecture


Supporting multi-branch required changes to our core data model and represents a major scale-up for our transpiler. Previously, we needed to consume and process hundreds or thousands of codebases, each with hundreds of thousands or millions of lines of code. Now we want to *multiply* that by processing many or all of the remote branches per codebase. That’s a big jump. A lot of our focus was on designing this to be simple, stable, and scalable.


#### Git-Inspired Content Deduplication


At the onset, we were drawn to[Git’s snapshot model](https://git-scm.com/book/en/v2/Getting-Started-What-is-Git%3F) as directly relevant to what we’re trying to do. We make significant use of the file structure of a codebase. We deviate into other graph structures — language syntax trees and symbol tables at lower levels, conceptual and functional ontologies at higher levels — but the file tree forms a directed acyclic graph (DAG) at an excellent medium granularity for representing and traversing a codebase. A Git-like snapshot model is attractive in this context for efficiency and scalability.


Properties of a multi-branch world for us:


- A considerable fraction of nodes (files and folders) in the file tree will be redundant across branches.
- From an infrastructure perspective, a branch is just another snapshot to process — our existing single-branch update flows extend naturally.


Properties of a Git-like snapshot model that make sense for us:


- Branches are cheap to create and destroy, effectively lightweight pointers on a core snapshot model.


Properties that are different or unique to us:


- Folders are first-class citizens in a way they are not for Git.
- The Driver model includes one additional level: a 1:many relationship between a node (file or folder) and many pieces of Driver-derived content *on top* of the raw source.
- Deduplication applies not just to source content (as in Git) *but also* to expensive LLM content generation. Git deduplicates *storage* ; Driver deduplicates *computation* . Same content hash means not just the same stored bytes, but the same generated documentation.


#### Branches as First-Class Entities


With the deduplication discussion and Git snapshot analogy established, we can talk about how branches fit into our core data model:


**PrimaryAsset → Branch → Version → VersionNode ↔ Node → DerivedContent**


PrimaryAssets (codebases) have many branches, which have many versions (effectively commits), which have many graph nodes (files and folders), each of which can have many kinds of Driver-derived content.


The key developments for multi-branch are the insertion of Branch into this chain and the VersionNode ↔ Node break from what is otherwise a cascade of 1:many relationships.


Two things to call out:


- The chain of 1:many relationships (PrimaryAsset → Branch → Version) gives us a clean tree structure that keeps things simple to reason about, query, and enforce constraints on.
- We intentionally break this at the Node level to gain the massive efficiency benefits of deduplication analogous to Git’s snapshot model. The same Node — identified by content hash — can appear in multiple Versions across branches via VersionNode. This is the one place we introduce relational complexity, and it’s precisely the mechanism that makes multi-branch economical. Below this, Node → DerivedContent is 1:many again.


#### Leaning on the Diff Update Flow


We’ve long built and developed an update flow so that new commits and PR merges are handled economically and precisely. Our systematic structure and file tree model — which closely mirrors VCS systems like Git — mean that when an update comes in, we can shrink-wrap generative updates to precisely the parts of our trees affected by the change.


If you squint, branches don’t fundamentally change this picture. There are devils in the details (and unlike Git, we have derived content, not just source content, to deal with), but this meant we could leverage and extend the existing update flow to handle multiple branches.


The most important consideration has been content at levels of abstraction above individual files. We generate *intermediate representations* (IRs) at many levels, all the way up to documents coupled to the codebase as a whole — our “deep context documents” like architecture and onboarding guides. These don’t line up neatly with file-delineated diffs. We’ve been building and strengthening an update flow for these higher-level components for single-branch commits for some time, and have now extended these algorithms for onboarding branches from a previous state (e.g., the direct parent branch). This is important because greenfield generation of deep context documents can be very expensive in tokens — and completely unnecessary given the extensive overlap with “nearby” branches.


### Interesting Challenges


I’ve alluded to challenges of scale and efficiency. Here I’ll highlight specific challenges the team overcame during implementation.


#### Branch Provenance


You might think branch provenance is trivial. From a full-integrity Git perspective, that’s generally true. But we are not 1:1 with every commit that has ever been pushed, for a host of reasons. These include *not* wastefully generating content for commits in the remote past relative to the onboarding date (though we *do* compile a changelog for this purpose) and the choice to treat PR merges as a single content-generating event rather than walking the incoming branch’s commits individually.


We must avoid greenfield onboarding for new branches at all costs — it’s prohibitively expensive in tokens and time at scale, and it’s assuredly unnecessary since some other branch state will be a good starting point.


Consider: a new branch appears with no immediately obvious provenance — say a commit to a branch that hadn’t been touched in two years. We also encounter cases specific to Driver onboarding (discussed in the next section). For these cases, the team built a “find best previous version” algorithm that walks parent commits and computes smallest diffs from other branches, including those with no direct relationship from Driver’s perspective, to settle on the best-fit starting point for onboarding based on the diff update flow described earlier.


#### Onboarding at Scale


Before multi-branch, onboarding events could already be large relative to continuous operation. Think of greenfield processing hundreds of codebases, each with hundreds of thousands or millions of lines of code. This is routine for large enterprise customers.


Now add that each codebase may have hundreds of branches. We delineate between *connecting* a codebase (pulling in the source, running Git-based analytics, and populating base metadata) and *generating* all of our transpiler content. These are separate steps. For the multi-branch world, our solution is to discover and upsert *all* branches at first connection. Then for content generation:


- Smart-filter the branches committed for generation to those last updated in the last N days (default 14).
- The default branch is always onboarded in full first.
- Remaining branches are processed sequentially, ordered by most recent activity.
- After initial onboarding, every new or previously unprocessed branch that receives an update is onboarded automatically.


This relatively simple heuristic serves us well. Because of the default-first, most-recently-updated ordering and strict sequential processing, subsequent branch onboarding events are likely to have ideal jump-off points: actual parent branches or very near-state branches. We only do a single heavy greenfield onboarding for the default branch. Existing feature branches, for example, will be significantly identical to the default branch and benefit from extensive deduplication. This algorithm can lead to scenarios where a child branch serves as the base to create state for a parent, but our diff update algorithm handles time inversion gracefully!


#### MCP Tool Surface Area and Complexity


The MCP tool set — and more broadly the API endpoints that back it — is a critical surface area and *the* primary product experience for many of our users. We care a lot about keeping this API surface stable and carefully vet any changes. We want to avoid both tool sprawl and excessive input complexity.


Multi-branch introduces a real tension here. Agentic systems must be able to target specific branches, but this adds another dimension to every tool call. Agents already need to index into specific codebases; now they must index into codebase *and* branch. The challenge is compounded by the diversity of usage contexts: in a local IDE, the codebase name and branch are trivially discoverable via Git metadata. But in chat agents, background agents, and other contexts divorced from an IDE-like experience, even codebase discovery is a much more dynamic problem. Adding branch specificity makes this more onerous.


Our approach: add an optional` branch_name` parameter to each tool. Omit it and the default branch is assumed — the right fallback for agents that don’t have branch context or don’t need it. Specify it and you get full branch-level targeting. We also added a` get_branches` discovery tool so branch-aware agents can enumerate available branches before querying.


## Toward New Code Evolution Infrastructure


Multi-branch elevates Driver to a ubiquitous context layer at the largest scales. Developers, product managers, and executives can all work with AI tools with high trust because Driver is now up-to-date with whatever part of the living, branching software system their current needs relate to.


The remaining gap is uncommitted local changes, a dramatically smaller surface area than the branch-wide blind spot we started with. We’ll monitor whether we need to close this gap further as we move forward.


This has been a long-anticipated step in our vision. Building the foundational infrastructure — the content pipeline, the deduplication model, the inspector system, deep context generation — took time and care. Multi-branch is a keystone. With it, we can aggressively pursue the full context layer vision: deeper automatic tracking and context generation for code evolution across branches, and deeper integration with emerging AI SDLCs.


Every branch is a line of development. Every line of development deserves comprehensive context. We’re excited about what comes next.
