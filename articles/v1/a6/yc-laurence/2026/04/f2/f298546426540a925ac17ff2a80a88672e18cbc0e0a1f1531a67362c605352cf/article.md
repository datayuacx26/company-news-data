---
schema_version: "1.0.0"
document_id: "f298546426540a925ac17ff2a80a88672e18cbc0e0a1f1531a67362c605352cf"
company_key: "yc-laurence"
company: "Laurence"
source_id: "yc-laurence-news-import-aad1c15d62c5"
canonical_url: "https://www.laurence.com/blog/laurence-knowledge-flywheel"
published_at: "2026-04-17T00:00:00+00:00"
first_seen_at: "2026-07-24T02:00:49.130192+00:00"
fetched_at: "2026-07-28T21:56:44.009033+00:00"
content_hash: "sha256:287fbe05334afd77b9d4bf041a0b9319feb2370b2de15aab20a1bc996bdfd7ff"
---

# Our knowledge flywheel: how Laurence remembers and learns from every campaign decision

## Why we’re building this: scaling requires becoming AI-native


Every new customer adds another store’s worth of ASINs, campaigns, keywords, and hour-by-hour bidding decisions. Human analysts don’t scale linearly with that surface area. The only path that compounds is giving AI agents the same context a senior ops lead carries in their head — why a bid was changed, what an experiment proved, which playbook applies to a CVR drop.


Being "AI-native" means that context has to live somewhere agents can read **and** write — not locked in Slack threads, call recordings, or a single analyst’s memory. We need a **robust framework for agent learning** : every campaign change, Slack discussion, and meeting note becomes structured memory that the next morning’s diagnosis agent, next quarter’s algorithm tuning, and next year’s new optimization agents all read from.


It’s also how we iterate fast on the algorithm itself. Changes to our bid model, clustering logic, or exploration policy — and the results they produce — are captured as linked wiki pages. Learnings from store A inform store B automatically instead of dying in a one-off investigation.


What follows is the system we built: one Obsidian vault that humans and agents both write and read, fed by every surface the team already uses, distributed to every agent that needs it.


## The wiki framework


The knowledge base is an **Obsidian vault** checked into the monorepo. Every page is a markdown file with YAML frontmatter and structured metadata. Internal references use` \[\[wikilinks\]\]` exclusively, so the vault forms a navigable graph — both in Obsidian’s graph view and for agents that follow links programmatically.


The Obsidian graph view of our wiki. Each node is a page, each edge is a wikilink. Color indicates entity type — stores, ASINs, campaigns, algorithms, playbooks, and more.


A schema file called` AGENTS.md` is the contract between humans and agents. It defines every entity type, its naming convention, required metadata, linking rules, and the ingest and query workflows agents must follow. Any agent that writes to the wiki reads this file first.


Pages are organized by entity type:


- **Store / ASIN / Campaign** — per-account operational context.
- **Strategy / Playbook** — reusable approaches and step-by-step solutions.
- **Experiment / Reflection** — what we tried and what we learned.
- **Algorithm / Concept** — authoritative reference for the math and domain knowledge behind the bidding system.


The vault is organized into **three layers that separate noise from signal** :


- **` raw/` — immutable source material.** Slack exports, call transcripts, Granola notes, operational dumps, screenshots. Verbatim and noisy by design; never edited.
- **` sources/` — LLM-generated summaries.** A structured digest of each raw artifact: decisions made, entities mentioned, causal claims, with explicit links back to the raw file.
- **Synthesized entity graph.** Store, ASIN, campaign, playbook, and reflection pages that downstream agents actually read. A central index and an append-only log make the vault self-describing — an agent with no other context can read the index and know exactly what exists.


The three layers of the vault. Raw sources stay noisy and immutable. Summaries distill them once. The synthesized entity graph — the only layer downstream agents read on every run — links back through the summaries to raw when a claim needs verification.


## Data flow


Before diving into each piece, here is the full pipeline from human action to morning diagnosis:


Notes from any surface flow through the ingest queue, get processed by Claude Code, and inform both the daily diagnosis and on-demand Ask Laurence.


## Multi-surface ingest: how notes enter the wiki


All ingest surfaces converge on one database table. A scheduled Claude Code ingest agent polls this table. Regardless of where a note originates — the frontend, Slack, or a config change — it enters the same queue and gets the same treatment.


- **Store note dialog.** A dialog in the frontend collects a title, body, ASIN tags, and optional images. The note is saved for the dashboard and simultaneously queued for wiki ingestion. Reusable across the product — any surface that produces a note can open it.
- **Campaign manager integration.** When an operator stages campaign changes — pausing keywords, toggling fixed bids, running a negative fill — the **Apply** button does not immediately fire. It opens the note dialog, prefilled with exactly what will change (which keywords are being paused, which bids are being set, how many negatives were filled). Only after the note is saved do the changes execute. The effect: **every batched campaign change yields a reviewed wiki note** , not a silent edit.
- **Slack` /ingest-wiki` command.** A slash command lets anyone on the team ingest recent Slack conversation. You specify a time window, a store, and optionally specific ASINs, plus free-text instructions. The handler pulls channel history, formats it as structured markdown with usernames and timestamps, downloads inline images, and queues it for ingestion. **Granola** meeting notes work the same way — paste the Granola link into the channel, then run` /ingest-wiki` .
- **Other surfaces.** Pipeline config diffs (when algorithm parameters change) and experiment creation also write to the ingest queue. Every significant action in the platform leaves a trace the wiki can absorb.


## The Claude Code ingest agent


A **Modal cron** polls the ingest queue on a recurring schedule. For each batch of unprocessed notes, it clones the repo, and runs **Claude Code headless** with a prompt that instructs the agent to first read the wiki schema, then follow a structured ingest workflow — saving raw content, creating source summaries, updating affected entity pages, maintaining the index and log, and self-checking before committing.


**Synthesis is the point.** This is the most important thing the ingest agent does, and it’s why we spend the compute here instead of at query time. A two-hour Slack export or a call transcript is mostly noise — greetings, tangents, half-finished thoughts, repeated context. If we handed that raw dump to the diagnosis agent tomorrow morning, it would burn tokens and attention reconstructing the signal on every single run, for every single store. Instead, the ingest agent reads the raw material once, identifies the entities involved, extracts the decisions, causal claims, and learnings, and writes them into the synthesized entity graph with explicit links back to the raw source. Downstream agents read the distilled pages and only follow links to raw when they need to verify a specific claim.


Concretely, processing one raw source typically produces four things:


- The **raw file** saved verbatim in` raw/` .
- A **source summary** in` sources/` — a structured digest with YAML frontmatter (date, entities touched, source type, raw path) and a short prose summary of what happened.
- Updates to affected **entity pages** (store, ASINs, campaigns) with wikilinks back to the source.
- New **playbook** or **reflection** pages if the note surfaced a pattern worth generalizing.


The ingest agent is explicitly told to focus on causality, decisions, and learnings — not raw metrics. A single ingest typically touches 5–15 pages across these layers.


All ingests accumulate as commits on a **single rolling PR** . When the PR is merged, the next ingest creates a fresh branch. This means humans review what the agent wrote before it becomes canon — the wiki never drifts silently. And because the synthesis is front-loaded into ingest, the morning diagnosis run doesn’t pay that cost: it loads a small, relevant slice of entity pages instead of trawling through every raw transcript from the last quarter.


## Distributing the vault to downstream agents


**GitHub pushes** keep the **` wiki-content` Modal volume** aligned with the repo: when changes land on the default branch, a workflow updates the volume that downstream agents mount as a local directory — **push-driven** , not a pull-based sync on a fixed interval. The volume holds a persistent sparse-checkout of` wiki/` . If a sync ever fails (corrupted state after a crash, for example), it can fall back to a fresh checkout.


Why a volume? Fast local reads, a consistent snapshot for the entire diagnosis or Ask Laurence run, and no GitHub rate-limit risk during agent work. Push-driven updates mean agents read the wiki tree that just shipped to GitHub, not a copy refreshed on a polling schedule.


## The daily diagnosis agent


Every morning, a Modal function kicks off the daily diagnosis for every managed store, analyzing the previous business day. The pipeline has three stages:


- **Triage.** Identifies which ASINs need investigation based on performance signals (profit deltas, ROAS shifts, spend anomalies).
- **Per-ASIN investigators.** Run in parallel, one per flagged ASIN, each with a full tool belt — analytics queries, bid and keyword breakdowns, search term reports, and the wiki.
- **Synthesizer.** Combines findings into a store-level headline and summary ready for the dashboard.


Triage → parallel per-ASIN investigators → synthesizer. The tool belt below is shared — every investigator has access to all of it.


Each investigator has access to wiki tools: **search** (hybrid semantic + keyword across all pages), **read page** (full content by title), and **read image** (view embedded screenshots or charts). Before the run, the wiki’s index is injected into the system prompt so the agent knows what pages exist before it starts searching. It can follow links from a store page to an ASIN page to a campaign page, reading playbooks and past reflections along the way.


Results are stored as structured JSON — investigations, agent transcripts, and metadata — which the frontend reads to surface the diagnosis in the dashboard.


The wiki isn’t only consumed by the scheduled daily run. **Ask Laurence** — our internal on-demand conversational agent used by the ops team — pulls from the same wiki volume. When someone on the team asks a question about a store, ASIN, or campaign, Ask Laurence can search and read wiki pages in real time, grounding its answers in the same accumulated context the daily diagnosis uses. The wiki becomes a shared memory layer that both scheduled and interactive agents draw from.


Both agents retrieve wiki content through **hybrid search** — a combination of embedding-based semantic search and keyword matching. Semantic search finds pages that are conceptually relevant even when the wording differs; keyword search catches exact terms like ASIN IDs, campaign names, or algorithm parameters that embeddings might overlook. The two signals are combined to rank results, and the agent can then read the full page by title for deeper context. This means an agent investigating a CVR drop can surface a relevant playbook written months ago, even if the original note used completely different phrasing.


## Why this shape


- **Human-editable markdown.** Ops and engineering both contribute. Anyone can open the vault in Obsidian, VS Code, or a text editor — no special UI needed to read or write.
- **Ingest is gated on a rolling PR.** Humans still approve what the agent wrote before it becomes canon. The wiki never drifts silently; git history is the audit trail.
- **Wikilinks make a traversable graph.** Schema enforcement at ingest time keeps the graph coherent. The diagnosis agent can follow links from a store to its ASINs to their campaigns without guessing at file paths.
- **Synthesis is front-loaded into ingest.** Raw sources stay immutable and noisy; summaries and entity pages are distilled. Downstream agents read the synthesized layer, so the daily diagnosis doesn’t re-parse two-hour Slack exports every morning. Compute is spent once, at write time, and amortized across every future read.
- **The knowledge compounds.** Every dialog note and` /ingest-wiki` call eventually becomes linked pages that next morning’s diagnosis can cite. A Slack thread about a CVR drop today becomes a playbook the agent references three months from now.


That’s the flywheel: more context in, better diagnoses out, faster iteration on the algorithm, and a system that scales with customers instead of headcount.


## What the wiki has already caught


A few root causes the wiki-grounded investigator pinpointed in the last three days of diagnoses — each one would have been hours of analyst work, or missed entirely, without the operator context the wiki preserves:


- **Runaway spend after an AUTO shutoff, caught day 1.** AUTO campaigns were shut off the prior day for inefficient spend; the next-morning diagnosis flagged net profit had actually *widened* to −$108 on $1.1K sales (36.3% TACOS). A prior reflection tied the root cause to the bid model ignoring margin and organic halo — so shutting off AUTO didn’t help, because the waste wasn’t there.
- **Unprofitable keywords predicted by a documented lifecycle risk.** A months-old reflection had flagged that a bid method change generates more volume but *requires a follow-up run of our keyword filtering algorithm* to prune the losers it opens up. After round 1 pruned 42 keywords, the morning diagnosis spotted the same waste re-emerging on a new surface and flagged round 2 as overdue — before the spend compounded.
- **Post-relaunch ASINs still burning spend with zero sales.** Four days post-relaunch, two ASINs were accumulating spend with zero diagnosis-day sales. The investigator separated a healthy top tier (2.43x–2.82x ROAS) from the bottom tier dragging net profit and recommended the same walk-down a prior reflection had validated — instead of waiting for the weekly review.


Common thread: the diagnosis is only as good as the operator context the agent can retrieve. Wiki → investigator → dashboard, every morning.


## What’s next


**Diagnoses that write back.** Today the daily diagnosis reads the wiki but doesn’t write to it. We’re working on closing that loop — when the agent finds something significant, it should be able to create a reflection, update a playbook, or flag an anomaly page directly in the wiki, opening its own PR for review.


**Agent-proposed algorithm changes.** The wiki already documents our bidding model, clustering logic, exploration policies, and every experiment we’ve run. As that corpus grows, we want agents that can reason over the full history of what worked and what didn’t — and propose novel algorithmic approaches. An agent that reads a reflection on an exploration policy, cross-references it with the current algorithm page and recent store performance, and drafts a concrete change proposal with expected impact. The wiki becomes not just memory but the substrate for algorithmic iteration.


**Richer ingest sources.** We’re expanding the surfaces that feed the wiki: automated ingestion of Amazon Seller Central notifications, product listing changes, competitor price shifts, and seasonal event calendars. The more context that flows in, the better the diagnosis agent’s recommendations become.
