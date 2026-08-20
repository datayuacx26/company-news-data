---
schema_version: "1.0.0"
document_id: "de48df18fb241b9634798a628657620af08c792c860c1d27ff25f256f352c913"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-to-run-a-data-audit-a-practical-checklist-for-lean-teams/"
published_at: "2026-08-06T00:00:00+00:00"
first_seen_at: "2026-08-06T14:47:49.358186+00:00"
fetched_at: "2026-08-06T14:47:54.098272+00:00"
content_hash: "sha256:24df182254253e21ebf11d3ae53e93dec58910d35f63dc3801916ce146fd48f9"
---

# How to run a data audit: a practical checklist for lean teams

A data audit is a periodic, deliberate review of the data assets your team relies on: which tables, dashboards, reports, and metrics exist, who owns them, whether the numbers are correct, whether anyone actually uses them, and whether access is appropriate. Unlike a one-off bug fix, an audit is a stock-take. You walk the whole estate on a schedule and decide what to keep, fix, merge, or retire.


This guide is for founders, operators, analysts, and small data teams who have accumulated more dashboards and tables than anyone can name from memory. If you have ever asked “is this number right?” and gotten three different answers, or found two dashboards with the same title showing different revenue, a data audit is the process that cleans that up and keeps it clean.


## What a data audit is, and what it is not


A data audit answers a different question than the other data-quality practices it gets confused with. It is worth being precise, because the scope determines what you actually do.


Practice Core question When it runs


Data audit What do we have, who owns it, and is it correct and used? Periodically (quarterly or before a big change)


[Data validation](https://www.basedash.com/blog/data-validation-what-to-check-and-where-to-enforce-it) Does this record meet the rules before it is written? Continuously, at write or load time


[Data reconciliation](https://www.basedash.com/blog/data-reconciliation-how-to-find-why-two-numbers-dont-match) Why do these two numbers not match? On demand, when a mismatch is found


[Data quality metrics](https://www.basedash.com/blog/data-quality-metrics-what-to-measure-and-how-to-track-it) How healthy is this dataset over time? Ongoing, as monitored scores


Validation stops bad rows at the door. Reconciliation traces a specific discrepancy. Quality metrics track health as a running signal. A data audit is the wider, less frequent review that steps back and looks at the whole surface: what exists, whether it is trustworthy, and whether it earns its keep. The other three often feed into it. An audit is where you notice that a table has no validation, a dashboard has three unreconciled versions, or a dataset’s quality score has quietly decayed.


## When to run a data audit


You do not need to audit constantly. Run one when the cost of confusion is rising:


- **On a cadence.** Quarterly for most small teams; twice a year if the estate is stable.
- **Before a migration.** Moving to a warehouse, switching BI tools, or replatforming is the wrong time to carry dead assets forward.
- **After rapid growth.** A team that tripled its dashboards in six months has almost certainly grown duplicates and orphans.
- **When trust drops.** If people have started keeping private spreadsheets because they do not believe the shared dashboards, that is a signal.
- **For compliance.** Regulations like the GDPR require organizations to keep records of what personal data they process (see[GDPR Article 30](https://gdpr-info.eu/art-30-gdpr/) ). An audit is how you produce and refresh that inventory.


## The six lenses of a data audit


Rather than a vague “clean up the data,” look at every asset through six specific lenses. Each lens is a question with a concrete answer, so the audit produces findings you can act on rather than a general sense of unease.


**1. Inventory.** What actually exists? List the source tables, warehouse models, dashboards, reports, and saved queries. You cannot audit what you have not enumerated. Most teams are surprised by the count.


**2. Ownership.** Who owns each asset? Every dashboard and important table needs a named human who is responsible for it. Assets with no owner are the ones that rot.


**3. Accuracy.** Is the number correct? Spot-check the headline metrics against a trusted source. This is where reconciliation feeds in: if two revenue dashboards disagree, one of them is wrong.


**4. Usage.** Does anyone open it? A dashboard nobody has viewed in ninety days is a maintenance liability, not an asset. Usage data separates what matters from what merely exists.


**5. Access.** Who can see it, and should they? Check for over-broad permissions, especially on anything containing PII or financial data. This is the security lens.


**6. Lifecycle.** Is it current? Confirm the data is fresh, the definitions still match reality, and the asset is not describing a product or funnel that no longer exists.


If you only remember one thing, remember these six lenses. They turn “audit the data” into six answerable questions.


## How to run the audit, step by step


### Step 1: Set the scope


Decide what is in and out before you start. A first audit should usually cover the assets people make decisions from: the shared dashboards, the metrics leadership quotes, and the tables those depend on. Do not try to boil the ocean. Auditing every temporary table in a scratch schema is wasted effort.


### Step 2: Build the inventory


Enumerate the assets in scope. Pull a list of dashboards and reports from your BI tool, a list of models from your warehouse or dbt project, and the tables they read from. Put them in one place: a spreadsheet or a table with a row per asset. Capture the name, type, owner (if known), and where it lives. This inventory is the backbone of the audit and the artifact you keep afterward.


### Step 3: Assign an owner to every row


Go down the list and write a name next to each asset. Not a team, a person. Where nobody will claim an asset, that is itself a finding: it is a candidate for retirement. A[single source of truth](https://www.basedash.com/blog/what-is-a-single-source-of-truth-and-how-to-build-one-for-analytics) only works when someone is accountable for keeping each definition true.


### Step 4: Check accuracy on the assets that matter


You cannot re-derive every number, so triage. Take the ten or twenty metrics that drive real decisions and verify each against a trusted source: the billing system for revenue, the CRM for pipeline, the product database for active users. Note any that disagree and by how much. Definitions are the usual culprit; a[data dictionary](https://www.basedash.com/blog/what-is-a-data-dictionary-and-how-to-build-one-for-analytics) is how you stop the disagreement from coming back.


### Step 5: Pull usage data


For each dashboard and report, find when it was last viewed and how often. Most BI tools expose this. Flag anything with zero views in the last quarter. High-traffic assets get the most scrutiny on accuracy and access; unused assets become retirement candidates.


### Step 6: Review access and sensitivity


For each asset, note what it contains (does it include PII, salaries, customer financials?) and who can access it. Flag any sensitive asset with broad or unclear permissions. This is where audit and[data governance](https://www.basedash.com/blog/data-governance-framework-a-practical-guide-for-lean-teams) overlap: the audit surfaces the exposure, governance sets the rule that prevents it next time.


### Step 7: Triage the findings


By now you have a list of problems. Do not fix them in the order you found them. Rank them, then work top down.


## A rubric for triaging what you find


Not every finding deserves the same urgency. Score each one on two axes and let the score decide the order of work.


**Impact:** how much damage does this cause if left alone? A wrong number on the metric the board reviews is high impact. A stale label on a dashboard two people use is low.


**Effort:** how hard is it to fix? Retiring an unused dashboard is trivial. Rebuilding a broken metric definition across five reports is not.


Impact Effort Action


High Low Do it now. These are the quick wins.


High High Schedule and staff it. Worth a project.


Low Low Batch these into a cleanup pass.


Low High Leave it, or retire the asset instead of fixing it.


The most useful move an audit produces is often deletion. An unused, low-impact, high-effort asset does not need to be fixed. It needs to be archived so it stops confusing people and consuming maintenance.


## The data audit checklist


Copy this into your inventory and fill it in per asset (or per group of assets). It maps directly to the six lenses.


- **Inventory:** Is every in-scope dashboard, report, and table listed in one place?
- **Ownership:** Does every asset have a named human owner?
- **Accuracy:** Have the top decision-driving metrics been checked against a trusted source?
- **Duplication:** Are there multiple assets computing the same metric? Which one is canonical?
- **Definitions:** Does each key metric have a written, agreed definition?
- **Usage:** Has each asset been viewed in the last 90 days?
- **Access:** Does anything sensitive have broad or unclear permissions?
- **Freshness:** Is the underlying data current and loading on schedule?
- **Relevance:** Does the asset still describe something that exists today?
- **Disposition:** For each asset, is the decision keep, fix, merge, or retire?


The final line, disposition, is the point of the whole exercise. An audit that ends without a keep/fix/merge/retire decision per asset is just a description of the mess.


## Common mistakes


**Auditing everything at once.** A first audit that tries to cover every table in every schema stalls. Scope to decision-critical assets and expand later.


**Producing a report nobody acts on.** The output of an audit is not a document. It is a set of changes: assets retired, owners assigned, definitions written, permissions tightened. If nothing changes, the audit did not happen.


**Skipping usage data.** Without knowing what people actually open, you will spend effort maintaining dashboards no one reads and miss the ones that quietly became load-bearing.


**No owner, no accountability.** Assigning ownership to “the data team” means no one. Every asset needs an individual name.


**Treating it as one and done.** Estates drift. A single audit followed by two years of neglect leaves you back where you started. The cadence is the point.


## When a full audit is overkill


A data audit is a heavy instrument. You do not always need it. If your estate is small enough that one person can name every dashboard and its owner from memory, a formal audit is ceremony. Skip it and rely on validation and ownership hygiene instead. Similarly, if you are chasing a single wrong number, that is a reconciliation, not an audit. Reach for the audit when the estate has grown past what anyone can hold in their head, or when a migration or compliance requirement forces a full inventory anyway.


## How often should you audit?


For most lean teams, a light quarterly pass on decision-critical assets plus a deeper annual review is a reasonable rhythm. Fast-moving teams that ship dashboards weekly benefit from more frequent, narrower audits. Stable teams can stretch to twice a year. The right cadence is the one that keeps the estate small enough to trust. If each audit is finding a large backlog of orphaned, duplicated, and unused assets, you are auditing too rarely.


One way to shorten every future audit is to make inventory, ownership, and usage visible continuously rather than reconstructing them each time. When your team works in a shared analytics workspace instead of scattered files, the inventory is the workspace, usage is tracked automatically, and ownership is attached to each query and dashboard.[Basedash](https://www.basedash.com/) is one tool built around that model: teams explore their database, build dashboards, and share queries in one place, which means the “what do we have and who uses it” half of an audit is answered on demand instead of rebuilt from scratch. Whatever tooling you use, the more your working environment doubles as a live inventory, the cheaper every audit becomes.


## FAQ


**What is the difference between a data audit and data validation?** Validation is a continuous check that runs at write or load time to stop bad records from entering a table. A data audit is a periodic, wider review of the whole data estate: what exists, who owns it, whether it is accurate, and whether it is used. Validation is a gate; an audit is a stock-take.


**How long does a data audit take?** A first, scoped audit of decision-critical assets is usually a matter of days of focused work for one person, most of it spent building the inventory. Later audits are much faster because the inventory already exists and you are only reviewing what changed.


**Who should run the data audit?** Whoever owns analytics: a data analyst, an analytics engineer, or an operator who lives in the dashboards. It needs one accountable driver, but ownership assignment requires input from the teams who use each asset, so plan for short conversations with stakeholders.


**What should a data audit checklist include?** At minimum: an inventory of assets, a named owner per asset, accuracy checks on key metrics, a usage check, an access and sensitivity review, a freshness check, and a keep/fix/merge/retire decision for each asset. The disposition decision is the part that makes an audit actionable.


**How often should we audit our data?** Most lean teams do a light quarterly pass on the assets people make decisions from, plus a deeper review once a year. Audit more often if you ship dashboards frequently, less often if your estate is small and stable. If every audit uncovers a large backlog, audit more frequently.
