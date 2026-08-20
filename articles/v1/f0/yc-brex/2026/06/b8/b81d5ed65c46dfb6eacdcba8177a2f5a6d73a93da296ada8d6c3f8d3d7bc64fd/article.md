---
schema_version: "1.0.0"
document_id: "b81d5ed65c46dfb6eacdcba8177a2f5a6d73a93da296ada8d6c3f8d3d7bc64fd"
company_key: "yc-brex"
company: "Brex"
source_id: "yc-brex-news-import-5eb786253ae8"
canonical_url: "https://www.brex.com/journal/agent-that-turns-customer-feedback-into-shipped-fixes"
published_at: "2026-06-22T00:00:00+00:00"
first_seen_at: "2026-07-21T11:29:48.523536+00:00"
fetched_at: "2026-07-28T21:43:28.836467+00:00"
content_hash: "sha256:b2c532dd4cfd1ef6de2a0391ada944871b03c3eb47c04551b6b4e094f9cdaa02"
---

# How we built an agent that turns customer feedback into shipped fixes

Most product designers know this reality. Product work that seems "small,” like a confusing empty state, a stale tooltip, or a keyboard shortcut that doesn't work in a modal, often gets deprioritized in favor of shinier initiatives. Over time, those little things add up, and your product's experience starts to degrade.


Product feedback lives everywhere: NPS surveys, support conversations, customer calls, Slack threads, research interviews. These signals arrive in radically different formats, scattered across tools that don’t talk to each other. As a designer, I’ve spent a lot of time noticing patterns in customer feedback, filing tickets, and nudging fixes through. The more feedback you receive, the more slips through the cracks—no single person (or team) can be everywhere at once.


So we built a system that does it automatically, taking feedback from a casual comment to a shipped fix in days, hands-free. Here’s how we did it.


## **What we built**


We built a workflow that reduces the human cost of identifying, cataloguing, and fixing small product frictions, without removing the human judgment that keeps quality high.


Internally, we’re calling it Little Big Details, affectionately named after[one of my favorite design Tumblrs](https://littlebigdetails.com/) .


The Little Big Details system connects to the various places where customer feedback, NPS surveys, internal conversations, and user research insights live. In our case, that’s:


- Linear (issue and project tracking)
- Slack (internal and customer feedback channels)
- Granola (call transcripts)
- Glean (internal docs and research, and a portal to a variety of other connected sources)
- Great Question (user research interviews and surveys)
- BuildBetter (user research interview and aggregated customer feedback)


It reads across all of them, finds the small polish and quality-of-life opportunities hiding in plain sight, verifies they’re still relevant, and manages them as a living backlog. When it’s time to fix one, it opens a draft pull request with a proposed solution for a human to review.


If your team uses Jira, Zendesk, Intercom, Notion, Dovetail, or anything else that has an MCP available, the same approach applies. MCPs are a standard interface, so swapping one source for another is straightforward. Whatever tools your team already generates feedback in can become inputs.


The result: **feedback that used to take weeks to surface, prioritize, and act on can now go from a casual comment to a shipped fix in days** . In our first two days running the system, it surfaced 71 real candidates, 33 (and counting) of which are already shipped, in review, or in progress.


## **How it works: three agents, one pipeline**


The workflow is split into three agents, each with a distinct job. Separating concerns this way makes each agent more effective: a narrower task means more focused instructions, better results, and easier debugging when something needs tuning.


### Agent 1: Mine (finding the signal in the noise)


The Mine agent is responsible for discovery. It systematically scans all connected sources (Linear tickets, Slack channels, customer call transcripts, internal research docs, and more) looking for mentions of small frontend frictions.


It doesn’t just keyword-search. It uses the language model’s ability to read *intent* to distinguish a genuine polish opportunity from a bug report, a feature request, or background noise. "The button is hard to find" is different from "the button is broken," and the agent knows the difference.


Here’s what it does at each scan:


- **Searches broadly across sources** , using configured keyword patterns and friction-related language (things like "annoying," "every time I have to," "why doesn’t it just...")
- **Strips personally identifiable information** before any processing to ensure privacy and compliance.
- **Normalizes what it finds** into a canonical description of the underlying friction, so the same issue described differently across sources gets recognized as one problem, not three.
- **Deduplicates** against everything already tracked, so you don’t end up with five tickets for the same thing.


A **/bootstrap** command runs an initial deep scan across six months of history, useful for setting the system up for the first time and building the initial backlog.


### Agent 2: Synthesize (keeping signal-to-noise under control)


Once Mine identifies a broad set of potential candidates, the Synthesize agent applies rigorous verification to ensure only genuinely shippable opportunities make it through. Without this step, the backlog would quickly fill with noise and the whole system loses trust.


The filter evaluates each candidate against six strict criteria:


1. **Frontend-only** : no backend changes, API modifications, or schema updates needed
2. **Low risk** : small impact radius, easy to roll back if something goes wrong
3. **High frequency or high friction** : either the interaction happens often, or the pain is bad enough that people complain about it explicitly
4. **Multi-source evidence** : confirmed by at least two independent sources (a single mention in one channel isn’t enough)
5. **Shippable in two days or less** : scoped tightly enough for one person to complete quickly
6. **Changes existing behavior only** : polishes something users already do, never sneaks in a new feature


The agent also checks Linear to see if the issue is already being tracked, and verifies in the codebase that the friction still exists (catching things that were already fixed in a recent release, or rendered irrelevant by a product change).


What survives gets written to a dedicated Linear project with rich metadata:


- The normalized (canonical) problem description
- All source evidence linked
- A priority score based on recency and frequency
- A unique fingerprint that helps future Mine runs understand what’s already been seen and what state it’s in (that fingerprint is the deduplication key; it’s how the system maintains continuity across runs without creating duplicates or losing track of evolving context)
- UX considerations for the fixer (eg. edge cases worth thinking about)


Each ticket includes a machine-readable metadata block at the bottom that looks something like this:


This is what the agents read on subsequent runs to know what they’ve already seen, which sources contributed, and when the issue was first and last observed. It’s invisible to anyone triaging the backlog, but it’s the connective tissue that keeps the system coherent over time.


**This selectivity is by design.** We’d rather miss some candidates than flood the queue with noise. A strict filter creates trust; a loose one creates busywork.


### Agent 3: Fix (from ticket to pull request)


This is the agent that proposes a solution. It reads a qualified issue from the backlog, understands the problem, finds the relevant code, and opens a draft pull request with a suggested fix.


It can run in three modes:


- **Scheduled** : picks one issue at your chosen interval automatically, a steady drip of improvements
- **On-create** : executes immediately when a new issue lands in the backlog (faster, but noisier)
- **Manual (/pick)** : you trigger it yourself, and it chooses the highest-priority untouched issue. An "I’m feeling lucky" for product polish.


The fix agent brings together two things that make this kind of work well-suited to LLMs: it can read the *intent* behind a piece of feedback (what the user actually wants to happen), and it can write code that addresses that intent. So it’s not just locating the problem. It’s proposing a fix grounded in both the user’s frustration and the actual codebase.


## Maintaining the quality bar


*“So… it’s just shipping vibe-coded slop, really fast?”*


Not quite. Here’s what we’re *not* doing: removing humans from the decisions that matter.


The suggested fix from the agent could be perfectly fine to ship as-is (sometimes it is), but we always want a human reviewer to make that call. A fix can be *technically* correct but still miss crucial business context but not meet the design quality bar.


The human reviewer sees the proposed fix and decides: ship it as-is, tweak it and ship it, or close it. The judgment about what a fix should *feel* like, whether an interaction is helpful or annoying, whether copy is clear or patronizing, stays entirely human.


In this workflow, we shift the human’s role further down the pipeline. Instead of starting from a written ticket and doing everything from scratch, the reviewer is looking at an already-built solution. In this case, the pull request *is* the ticket. The human still makes all the quality calls, but they skip the most time-consuming phase: the build and context gathering step.


## The broader takeaway


While our particular example focuses on frontend polish, the thinking can be applied to many use cases if you frame it in terms of inputs and outputs.


Think of it as an assembly line: the output of one agent becomes the input to the next. The Mine agent's output feeds into the Synthesizer, the Synthesizer's output feeds into the Fix, and so on. Each station does one focused thing, then passes its result forward. Raw material (scattered feedback) enters one end; a finished product (a shipped fix) comes out the other. And like a real assembly line, you can tune or swap any station independently without redesigning the whole system. Once you start thinking about it this way, you can compose surprisingly complex workflows out of simple, focused steps.


**Inputs:** What data sources are available to you? Documents, surveys, transcripts, Slack messages, analytics, support tickets, connected through MCPs.


**Outputs:** What can an LLM agent derive from those inputs that a traditional, rule-based automation couldn't, or isn’t as strong at?


In our case, the LLM gives us two distinct strengths:


1. **Semantic understanding across contexts.** It can recognize that two completely differently-worded complaints in two different tools are describing the same friction. Not keyword matching, but *meaning* matching.
2. **Code generation grounded in user intent.** It can take a natural-language description of a frustration and propose a concrete code fix that addresses it, informed by the existing codebase’s patterns, established best practices, and design system.


A traditional automation could search for keywords, but it can’t understand that "I always have to scroll back up" and "the page doesn’t remember where I was" are the same problem. It can file a ticket, but it can’t write the fix.


The same pattern (gather multi-source signal, normalize it, filter aggressively, act on what survives) applies anywhere that useful information is scattered across too many places for one person to connect.


The tools to build something like this are accessible to more people than ever. You don’t need to be on an engineering team. You need a problem worth solving, data sources worth connecting, and a willingness to be very selective about what makes it through.
