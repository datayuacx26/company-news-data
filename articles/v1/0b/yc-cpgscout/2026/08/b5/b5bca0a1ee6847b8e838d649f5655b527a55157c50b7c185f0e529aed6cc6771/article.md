---
schema_version: "1.0.0"
document_id: "b5bca0a1ee6847b8e838d649f5655b527a55157c50b7c185f0e529aed6cc6771"
company_key: "yc-cpgscout"
company: "Scout"
source_id: "yc-cpgscout-news-import-62dfcaa4b82f"
canonical_url: "https://www.cpgscout.ai/blog/retail-data-analyst-job-description"
published_at: "2026-08-03T00:00:00+00:00"
first_seen_at: "2026-08-03T19:57:47.477802+00:00"
fetched_at: "2026-08-03T20:38:58.255627+00:00"
content_hash: "sha256:4b47fb821b423be1eb90e0c1857fd755458ff7fec949b16f7ea33390208adfc2"
---

# Retail Data Analyst Job Description

Most retail data analyst job description templates describe a business intelligence role with the word retail in front of it. The result is a hire who can build a dashboard and cannot tell you why the store number and the chain number disagree. This is a job description for the role as it actually exists, plus the interview questions that separate the two.


The distinguishing feature of retail data work is that the inputs disagree with each other by design. POS says one thing, receiving says another, the supplier's own service report says a third, and the syndicated panel uses a different week definition than any of them. Reconciliation is the job. Visualization is what happens after the job is done.


## What the role actually does


At a 62-store grocery operator, a retail data analyst's week is dominated by four recurring pieces of work.


- Reconciling feeds that disagree. Item files change case packs, stores close for remodels, suppliers substitute products, and each of those quietly corrupts a metric. Finding the 25 percent unit-conversion error before it reaches a buyer is the highest-value thing this role does.
- Building and maintaining metric definitions. On-shelf availability measured as a share of items and weighted by sales are different numbers, both defensible. Someone has to own which one the company uses and keep it stable across quarters.
- Producing the exception lists that operations actually run on: lines outside their weeks-of-supply band, suppliers crossing a service threshold, stores whose inventory records show drift.
- Answering the decomposition question. A category is up 6 percent. The analyst's job is to say whether that is velocity in the same stores or distribution gained in fourteen new ones, because those demand opposite responses.


## A retail data analyst job description you can copy


Adapt the specifics to your estate size and systems. The responsibilities below are ordered by how much of the week they consume in practice, not by how impressive they sound.


### Responsibilities


- Own the definitions of core retail metrics (velocity, weeks of supply, on-shelf availability, on-time in-full, promotional lift) and keep them stable and documented.
- Reconcile POS, receiving, on-order, and syndicated data into a single item-and-store view that operations and buying both trust.
- Produce weekly exception reporting for replenishment, supplier service, and inventory record accuracy.
- Support buyers with pre-negotiation analysis: supplier scorecards, realized lead-time distributions, and the case-level translation of service gaps.
- Investigate and resolve data-quality defects at source, including case-pack changes, store trading-status errors, and substitution handling.
- Build and maintain the store, category, and executive reporting tiers so the same measure aggregates consistently across all three.


### Requirements


- Strong SQL. This is non-negotiable and it is the single best predictor of success in the role.
- Comfort with messy joins across systems that were never designed to agree, and the instinct to check a suspicious number before publishing it.
- Working knowledge of retail metrics, or demonstrable ability to learn them quickly. Candidates from adjacent analytics roles often pick this up faster than candidates from retail who cannot write SQL.
- The ability to explain a number to a buyer in one sentence, including its caveats.
- A spreadsheet habit that includes checking totals against a second source.


### Nice to have


- Experience with syndicated data (SPINS, Circana, NielsenIQ) and their week and hierarchy conventions.
- Python or R for the analysis that outgrows SQL.
- Exposure to a merchandising or ERP system's data model, which is where most of the traps live.


## Interview questions that predict performance


Skip the puzzle questions. The following four have high signal because they cannot be answered from a template and they map directly onto the daily work.


- "Category sales are up 6 percent and units are up 1 percent. What do you conclude, and what would you check next?" Strong answers immediately separate price and mix from real demand and ask about distribution changes. Weak answers say growth is good.
- "An item shows 11 units on hand and has not scanned in nine days at a store where it normally sells daily. What are the possible explanations?" You are looking for at least three: the stock is not physically there, it is in the backroom or lost a facing, or the record is wrong. This is the highest-signal question on the list.
- "A supplier says their fill rate is 96 percent and our report says 82 percent. Both are computed correctly. How is that possible?" Tests whether the candidate understands that a metric name is not a definition.
- "Walk me through a time your analysis was wrong." What you want is a specific mechanism (a stale join, a denominator that included closed stores) and evidence they now check for it. A candidate who has never been wrong has never shipped.


## The first 90 days


A good first quarter is unglamorous and looks like this: the analyst has documented the definitions of the six metrics the business argues about most, found and fixed at least two real data defects, and produced one exception report that a buyer or store team uses without being reminded to.


What it should not look like is a redesigned dashboard. Rebuilding the reporting surface before understanding why the numbers disagree is the most common way this hire spends a quarter and produces nothing durable.


## What this role is not


It is not a BI developer. If your actual need is someone to build and maintain dashboards on a dataset that is already clean, hire for visualization skill and say so, because a reconciliation-minded analyst will be bored and leave.


It is also not a data engineer. If the pipelines do not exist or break weekly, that is a different hire, and asking an analyst to do both usually produces neither. The clearest test: if most of the pain is getting data to arrive, hire engineering; if most of the pain is that the arrived data disagrees, hire this role.


## Frequently asked questions


What should a retail data analyst be paid?It varies widely by market and estate size, so anchor on your own analytics band rather than a national figure. The practical guidance is that this role should sit at or above your general BI analyst band, because the reconciliation skill is scarcer than dashboard skill and the cost of getting it wrong is a buyer making a decision on a broken number.


Should they sit in finance, merchandising, or IT?Merchandising or a central analytics function, reporting close to the buyers who consume the output. Placing the role in IT tends to turn it into a ticket queue, and the value comes from the analyst understanding the commercial question well enough to challenge it.


Do they need retail experience?Helpful, not required. Retail metric conventions are learnable in a few months; SQL and the instinct to distrust a clean-looking number are not. Weight the technical screen heavily and treat category knowledge as trainable.
