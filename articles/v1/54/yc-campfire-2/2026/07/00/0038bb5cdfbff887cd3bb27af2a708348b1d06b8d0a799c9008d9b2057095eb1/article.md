---
schema_version: "1.0.0"
document_id: "0038bb5cdfbff887cd3bb27af2a708348b1d06b8d0a799c9008d9b2057095eb1"
company_key: "yc-campfire-2"
company: "Campfire"
source_id: "yc-campfire-2-rss-3be8123e2374"
canonical_url: "https://campfire.ai/blog/standardize-close-process-ember-skills"
published_at: "2026-07-21T18:24:21+00:00"
first_seen_at: "2026-07-21T19:39:03.712728+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:be96a44f76c33494717a6864d6cd149456fa08177921e12efcacba51afdfeb8f"
---

# Standardize your close process with Ember Skills

[Blog](https://campfire.ai/blog) Article


[Back to Home](https://campfire.ai/blog)


2026-07-21T18:24:21Z


# Standardize your close process with Ember Skills


Emma Roos


Product Marketing


July 21, 2026


[Finance](https://campfire.ai/blog?category=Finance)[Accounting](https://campfire.ai/blog?category=Accounting)


# **Your best accountant's judgment shouldn't live in one person's head**


Every close has a person everyone defers to. The one who knows which vendors always run late on invoices, how to prorate a mid-month prepaid, what "material" actually means for this entity. When that person is out, the close gets slower and shakier. When they leave, it gets rebuilt from scratch.


An Ember Skill is that person's judgment, written down once, so Ember runs the task the same way every time, for anyone who asks.


# **What finance teams are actually building**


When we launched Skills in May, we were curious how teams would put them to use. Looking at what teams have published so far, a few patterns stood out:


1. **Accrual and rollforward skills** are among the most common skills teams build first: department consistency checks that flag when a vendor's coding drifts month to month before it becomes a pattern, duplicate payment checks that catch a vendor billed twice, once on a card and once on a bill, in the same month, and completeness reviews that search across AP, purchase orders, and bank activity for anything that should have been recorded but wasn't.
2. **Allocation skills** come next: Splitting a shared expense across departments by headcount. Reallocating payroll costs across business units, then capitalizing the portion tied to product development. Flagging when a vendor's department coding drifts from the prior month before it becomes a pattern nobody catches.
3. **Reconciliation skills** are where teams get more ambitious: One published skill checks whether a vendor has both a card charge and a bill for the same expense in the same month, a duplicate payment check most teams do manually, if they do it at all. Another verifies that every P&L total actually ties back to the general ledger and returns a full discrepancy report, essentially a skill whose only job is catching Campfire's own mistakes.
4. **Contract-to-entry skills** handle the handoff between sales and accounting. Feed in an order form or a marketplace export, and the skill books the contract plus the billing, cash, and rev rec entries in one pass, no manual re-entry required.
5. **Payroll skills** build the monthly payroll JE directly from a payroll export and an allocation file, splitting net pay, intercompany allocations, and commissions without someone rebuilding the same spreadsheet every period.


The most sophisticated skill we've seen coordinates six sub-skills, a chart of accounts analyzer, a template analyzer, an extractor, a P&L builder, a reconciler, and a report generator, to produce a full department-level P&L end to end.


That's the range: from a single accrual to a five-part detection engine. The common thread is that none of them just generate a report. They encode a decision someone used to make from memory.


---


# **How to build one**


Skills sit in Ember's settings, next to your other configuration.


Skills are private by default. Building one doesn't put it in front of your whole team automatically. Click Create to start a new one.


Three fields, three questions:


- **Name.** What your team already calls this task.
- **Description.** One sentence on what it produces.
- **Instructions.** The real procedure: when to use it, what to gather, the steps, the format, the guardrails.


### **A worked example: searching for unrecorded liabilities**


The search for unrecorded liabilities is one of the more judgment-heavy parts of close. You're looking for goods and services received before the cutoff that never made it into AP or accruals. This one:


- Sets the scope: cutoff date, entity, currency, materiality threshold.
- Gathers AP bills, purchase orders, receiving records, recurring vendor patterns, bank and card activity, and payroll.
- Runs five detection tests: subsequent invoices with a pre-cutoff service date, recurring vendors missing a current-period bill, received-but-uninvoiced purchase orders, payments with no matching expense, and liability categories outside standard AP.
- Validates every candidate against an evidence hierarchy, an actual invoice outranks a contract, a contract outranks a historical run rate.
- Classifies each result as propose, investigate, or exclude, with the reasoning shown.
- Drafts the journal entries and stops there.


###
**Publishing it**


One click, from the same list where you built it.


### **Sharing it with your team**


Publishing makes the skill exist. Making it available to everyone is what actually shares it with your whole team. Confirm, and every accountant on your team runs the search for unrecorded liabilities the identical way.


---


### **Where to start**


Pick one task your team runs every close. Not the whole checklist, one task. Write down how your best person actually does it, including the judgment calls, not just the steps. Publish it, test it, and share it with the team.


## Frequently asked questions


Recent Articles


Loading posts...
