---
schema_version: "1.0.0"
document_id: "4e91e6f0a38ecec7937879a09809391575b9e2e520fde8dcb841492c039eb106"
company_key: "yc-mage-legal"
company: "Mage Legal"
source_id: "yc-mage-legal-news-import-389f7de0f02c"
canonical_url: "https://magelegal.com/blog/disclosure-schedule-traps-sellers-counsel-misses"
published_at: "2026-04-26T00:00:00+00:00"
first_seen_at: "2026-07-24T03:07:51.367861+00:00"
fetched_at: "2026-07-28T22:03:45.188546+00:00"
content_hash: "sha256:4430d880dd37d8598ec606fcd43c00d1ae084ef501f8af7a76dce5482cbca631"
---

# Disclosure Schedule Traps Sellers' Counsel Misses

The disclosure schedule is where post-closing indemnification claims are won and lost. Get it right and the buyer can't sue you for things you disclosed. Get it wrong and the seller's escrow is at risk for issues the deal didn't price in.


This is the practitioner's view of the recurring traps and how to avoid them.


## Why disclosure schedules matter


Schedules are the single biggest risk-allocation lever sellers have. The mechanics:


- The SPA contains representations and warranties from the seller about the target.
- The schedules list specific exceptions to those representations.
- Items disclosed in the schedules are typically excluded from indemnification coverage. The seller said "we have these contracts, this litigation, this customer concentration"; the buyer priced it in.
- Items NOT disclosed are subject to the rep that they don't exist. The buyer can claim post-close, and the seller's escrow or rep-and-warranty insurance is on the hook.


The schedule's job is to inventory every exception so the buyer can't surprise the seller post-close. Done well, schedules are protective. Done badly, they create exposure.


## Trap 1: incomplete schedules


The most common trap. Sellers' counsel pull schedule templates from a prior deal without thoroughly cross-referencing the current target's actual contract portfolio. Items the target has but didn't disclose become indemnification claim exposure.


Concrete example: target has 47 material customer contracts; schedules list 31. The 16 missing contracts are now subject to the rep that all material contracts are disclosed. Buyer post-close discovers a missing one with a problematic provision; seller's escrow is at risk.


The fix: build the schedules from the underlying source agreements, not from a template. Every material contract in the data room gets evaluated against each schedule's threshold criteria; if it qualifies, it goes in.


This is exactly what AI-augmented diligence is good at. The system extracts the relevant fields (counterparty, term, dollar value, risk flags) from every contract and maps them to the schedule's threshold criteria. The associate reviews and verifies. The completeness of the result is structurally better than a template-driven approach.


## Trap 2: mis-categorized items


The second trap. The same item may belong on multiple schedules with different thresholds and structures. Mis-categorization creates exposure even when the item is disclosed.


Example: a customer contract with a top-25 customer that contains an exclusivity clause. It belongs on the material contracts schedule (under the dollar threshold), the exclusivity-and-non-compete schedule (substantive obligations), and possibly the IP schedule (if it includes IP licensing). A schedule that lists it on only one of these may not satisfy the rep covering the others.


The fix: maintain explicit mapping between the SPA's reps and each schedule's content. Every rep gets a corresponding schedule (or NONE noted if the rep is fully accurate without exceptions). Every contract that triggers any rep gets evaluated for inclusion on the schedule covering each rep it triggers.


## Trap 3: generic catch-all language


A persistent bad habit: schedules that include catchall language ("see also any other contracts material to the business not specifically listed"). This is intended as protective; in practice courts often disregard it.


The schedules need to specifically identify items, not catch-all reference them. A buyer challenging the disclosure post-close points to specific items not specifically named; the catch-all gets argued and frequently loses.


The fix: name items specifically. If you don't have the specific name, list the items by counterparty, contract title, and date. The named-item approach is what holds up.


## Trap 4: missing knowledge qualifiers


Many reps have knowledge qualifiers ("to seller's knowledge..."). The qualifier matters because it limits the rep's scope to actual knowledge of designated officers.


The trap: schedules sometimes describe items as "we don't know about" without preserving the formal knowledge qualifier. A buyer post-close argues that the seller did know, just not specifically by named officer; the seller's defense is weakened.


The fix: preserve knowledge-qualifier language consistently throughout the schedules. If a rep says "to seller's knowledge", the schedule's exceptions should also be framed in terms of knowledge.


## Trap 5: post-signing-pre-closing changes not updated


The other big trap. Schedules drafted at signing have to be updated for events occurring in the interim period: new contracts signed, customer terminations, pending litigation, employee departures, regulatory inquiries.


The interim covenants in the SPA usually require updates. Sometimes they require pre-closing notice; sometimes they require schedule supplements. Failing to update creates either:


- A breach of the interim covenant directly, or
- A breach of the rep covered by the (unupdated) schedule.


The fix: maintain the schedules as a live document during the signing-to-closing window. Track interim events. Push updates per the SPA's mechanics. We have a separate post on the broader interim covenants topic in[Signing-to-Closing Interim Covenants](https://magelegal.com/blog/signing-to-closing-interim-covenants) .


## How AI-augmented prep helps


Manual schedule preparation typically runs 80-120 hours on a mid-market deal. AI-augmented prep drops that to 20-30 hours with better completeness.


The mechanics:


- Every material contract in the data room gets ingested and classified.
- For each schedule, the system applies the threshold criteria (dollar value, contract type, counterparty kind, term length).
- Qualifying items get drafted into schedule entries with citations to source.
- The associate reviews, verifies, and signs off.


The completeness is structurally better because the system doesn't skip contracts. The speed is faster because the system doesn't re-read each contract from scratch. The output is more consistent because the same threshold criteria apply uniformly.


We have a deeper case study in[How a Seller's Counsel Built Disclosure Schedules 10x Faster](https://magelegal.com/blog/how-sellers-counsel-built-disclosure-schedules-10x-faster) .


## Companion reading


- [How to Prepare Disclosure Schedules That Protect Sellers](https://magelegal.com/blog/how-to-prepare-disclosure-schedules-that-protect-sellers) — broader sell-side prep guide
- [Indemnification Caps and Baskets](https://magelegal.com/blog/indemnification-caps-and-baskets-deal-economics) — the related indemnity-package topic
- [Signing-to-Closing Interim Covenants](https://magelegal.com/blog/signing-to-closing-interim-covenants) — interim period mechanics
- [AI Due Diligence: An Operational Playbook](https://magelegal.com/blog/topics/due-diligence) — workflow walkthrough


If you are sellers' counsel on a current deal and want to see how Mage handles disclosure schedule preparation:[request a demo](https://magelegal.com/?demo=1) .
