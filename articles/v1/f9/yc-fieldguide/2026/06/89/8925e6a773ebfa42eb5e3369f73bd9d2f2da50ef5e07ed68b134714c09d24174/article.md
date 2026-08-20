---
schema_version: "1.0.0"
document_id: "8925e6a773ebfa42eb5e3369f73bd9d2f2da50ef5e07ed68b134714c09d24174"
company_key: "yc-fieldguide"
company: "Fieldguide"
source_id: "yc-fieldguide-rss-652d61b18f53"
canonical_url: "https://www.fieldguide.io/blog/multi-tier-consolidations"
published_at: "2026-06-08T08:59:59+00:00"
first_seen_at: "2026-08-18T01:47:26.942382+00:00"
fetched_at: "2026-08-18T01:47:28.276336+00:00"
content_hash: "sha256:4ab62a587057174f52b86a5ea157990acdf62549e195e448b79e3f712daf3d57"
---

# Multi-Tier Consolidations: One Engagement for the Full Hierarchy

*Multi-tier consolidation is now live in Fieldguide's Financial Audit product and coming soon for Advisory. One engagement for the full hierarchy, automatic rollups, and no more workaround.*


The workaround is familiar. A client has a complex corporate structure: a parent entity, a handful of regional holding companies, and dozens of subsidiaries underneath each one. To audit that structure, firms have been spinning up a separate engagement for every entity, completing the trial balance work and adjustments in each, and then manually linking engagements together, layer by layer, hoping the numbers roll up correctly at the end.


It works. Barely. And only if you have the time to manage five open engagements simultaneously, the patience to trace a balance from a leaf entity through two intermediate tiers to the parent, and the bandwidth to catch mapping inconsistencies before the partner review. Most teams do not have all three. The result is more setup time, more fragmentation, and a review process that is harder than the underlying accounting warrants.


That workaround is now behind you. Multi-tier consolidation is live in Fieldguide's Financial Audit product, and it changes how the work gets done from the first day of setup.


## What multi-tier consolidation actually means


A multi-tier consolidation is not simply "more than one entity." It is a structure where entities roll up across more than one level: subsidiaries consolidate into regional holding companies, and those roll up into a top-level parent. Each level may carry its own eliminations and require its own review.


The firms dealing with this most often are the ones auditing affordable housing portfolios, private equity-backed structures, or any client with a layered international footprint. For those firms, multi-tier consolidations represent a small share of total engagements, but a disproportionately large share of complexity. If the platform cannot support them, the firm cannot migrate off legacy systems. Full stop.


Fieldguide's Financial Audit product now supports multi-tier consolidations as a first-class capability, with the full hierarchy, automatic rollups, and financial statements all inside one engagement.


## The structure, inside one engagement


The setup is built around a three-step wizard: define consolidation details and method, build the entity hierarchy, upload trial balances to the leaf entities. The hierarchy supports up to five levels (one parent, four levels of children), with capacity for up to 200 entities per engagement, with plans to expand to 600.


Trial balances are uploaded only to leaf entities: child entities with no children of their own. Every entity above the leaf level displays a consolidated trial balance, calculated automatically as the sum of its children's TBs, inclusive of eliminations posted at that level. When something changes at a child, syncing the trial balance propagates the update up through every affected level, all the way to the parent.


What this eliminates is the navigation overhead that makes multi-tier work so tedious. Instead of opening five engagements to understand how a balance rolled up, the auditor traces it from the consolidated parent view down to the entity level, from one place, in one engagement.


## Consolidation by account or by group


Fieldguide supports two consolidation methods, and the choice matters depending on how the client's entities are structured.


Consolidation by account matches child TBs across entities by account number. Each unique account gets its own row in the consolidated trial balance. When the same account number exists in multiple entities with different names, the system uses the first-listed entity's name, and the consolidated TB maintains its own independent copy of that account, separate from the children. Mapping changes at the child level do not automatically propagate upward; the auditor manages the consolidated TB's account structure directly.


Consolidation by group is cleaner when entities use different numbering conventions. The consolidated TB carries one row per framework group, and the value in each row is the sum of all child accounts mapped to that group. When an entity remaps an account, syncing the trial balance recomputes the affected group rows. There is no account-number conflict to resolve.


Both methods support labeled trial balances (Interim, Budget, Preliminary) appearing as separate columns, excluded from the consolidation total but visible in the same view. Labeled TBs give auditors comparative context without distorting the rollup.


## Review that follows the hierarchy


One of the real risks in multi-tier work is that consolidated balances at the parent level become untraceble. A reviewer sees a number, but cannot easily get to what produced it without navigating back through the children manually.


Fieldguide addresses this directly. The consolidated trial balance view shows each direct child as a separate column alongside the consolidated total. From any child column, the auditor can navigate to that entity's trial balance, trace the balance to its source, and return. Every adjustment posted at a child entity rolls up when the trial balance is synced; adjustments posted at a consolidated level stay at that level and are applied separately.


The Manage Trial Balances drawer shows every TB in the engagement, organized by entity hierarchy, with two viewing modes: by entity (tree structure) or by trial balance (flat list). When a TB is replaced, or when an adjustment is edited or deleted, syncing updates the entity TB view, every affected consolidated TB at every level, and any financial statement columns tied to those views.


## Built to carry forward


The hierarchy does not have to be rebuilt every year. When an engagement rolls forward, the new engagement retains the full entity structure, prior-year TBs, account mappings for each entity, financial statement columns, and document sheets. Linked external entities carry forward as placeholders with a re-link indicator. What does not roll forward is the current year trial balance: each leaf entity requires a fresh upload, and the consolidated TBs recalculate from there.


For firms auditing complex structures that do not change dramatically year over year, this is the kind of continuity that actually affects how long setup takes at the start of the next engagement.


## The engagements that determine whether a firm migrates


Multi-tier consolidation is not a feature that every engagement team will use every week. But for the firms that need it, it is binary: either the platform supports it or it does not. The engagements that require it tend to be large, complex, and closely reviewed. They are also the ones that determine whether a firm will commit to a platform long-term.


Having the full consolidation hierarchy, every tier of trial balances, all adjustments, and the financial statements inside one engagement changes what review looks like. The partner is not reconciling across multiple engagement windows. The manager is not chasing down whether the mapping in the regional holding company matches what was done at the subsidiary level. The structure is there, the rollups are automatic, and the review follows the hierarchy the way the client's actual corporate structure does.


That is what it looks like when the platform is built for the work, not retrofitted to it.


Ready to see how Fieldguide handles your most complex consolidation engagements?


[Book a demo](https://www.fieldguide.io/request-demo) and walk through multi-tier consolidation with your specific client structures in mind.


Dhruv Dhingra


VP, Product at Fieldguide


## Related posts


[View All Posts chevron_right](https://www.fieldguide.com/blog)


- [Agent Knowledge: How Fieldguide Turns Prior-Year Workpapers Into Improved Testing Accuracy May 19, 2026 AI Product Releases](https://www.fieldguide.com/blog/agent-knowledge)
- [Fieldguide 2026 Spring Release April 29, 2026 Product Releases](https://www.fieldguide.com/blog/fieldguide-2026-spring-release)
- [Fieldguide and Mercia Partner to Bring Trusted UK Audit Methodology to Industry-Leading Agentic AI June 4, 2026 News](https://www.fieldguide.com/blog/fieldguide-mercia-uk-partnership)
- [Cherry Bekaert Selects Fieldguide as Strategic AI Partner to Modernize Audit and Advisory Delivery June 3, 2026 News](https://www.fieldguide.com/blog/cherry-bekaert-fieldguide-financial-audit)
