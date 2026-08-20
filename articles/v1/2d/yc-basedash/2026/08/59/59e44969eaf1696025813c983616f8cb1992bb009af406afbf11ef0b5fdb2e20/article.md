---
schema_version: "1.0.0"
document_id: "59e44969eaf1696025813c983616f8cb1992bb009af406afbf11ef0b5fdb2e20"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/data-reconciliation-how-to-find-why-two-numbers-dont-match/"
published_at: "2026-08-02T00:00:00+00:00"
first_seen_at: "2026-08-02T13:45:32.830364+00:00"
fetched_at: "2026-08-05T03:48:28.288743+00:00"
content_hash: "sha256:2ea39cd22ff21db2fe044e56d55d69e0b37f3393a66b93ece299a8a75add2351"
---

# Data reconciliation: how to find why two numbers don't match

Data reconciliation is the process of comparing two sets of numbers that should agree, finding out why they differ, and either fixing the gap or explaining it. When your revenue dashboard says $412,000 and Stripe says $408,500, reconciliation is how you figure out whether one of them is wrong or whether they are measuring two different things.


The fastest way to reconcile two numbers is not to stare at both totals. It is to narrow the difference: match the exact definition and time window first, then compare row counts before you compare sums, then bisect the data until you find the specific rows that explain the gap. Most mismatches come from a small set of predictable causes, and once you know them you can usually find the culprit in a few minutes.


This guide is for operators, analysts, and founders who keep hitting the same problem: two reports that should say the same thing say something different, and nobody trusts either one. It covers what reconciliation is, the usual causes of a mismatch, a repeatable workflow to find it, a short checklist, and the mistakes that keep teams chasing phantom bugs.


## What is data reconciliation?


Data reconciliation is comparing two representations of the same thing to confirm they match, and diagnosing the difference when they do not. The two sides might be a BI dashboard and a source system (your database versus Stripe), two dashboards built by different people, a live query versus a warehouse table, or this month’s report versus last month’s version of the same report.


Reconciliation is not the same as[data quality monitoring](https://www.basedash.com/blog/data-quality-metrics-what-to-measure-and-how-to-track-it) . Quality monitoring asks “is this data internally correct and complete?” Reconciliation asks “do these two numbers, which claim to measure the same thing, actually agree?” A dataset can be perfectly clean on its own and still fail to reconcile against another source because the two were built on different definitions.


The goal of a good reconciliation is not always to make the numbers identical. Sometimes the right outcome is a documented, understood reason they differ, for example “the finance report excludes trials and the product dashboard includes them.” An explained difference is a resolved reconciliation.


## Why two numbers disagree: the usual causes


Almost every mismatch traces back to one of a handful of categories. Before you assume a bug, walk this list. It is faster to rule out a definition difference than to re-audit a pipeline.


Cause What happens How to catch it


Time window and time zone One side counts a day as UTC, the other as local time, so events near midnight fall on different days Compare the exact start and end timestamps, in the same zone, on both sides


Metric definition “Revenue” means gross on one report, net of refunds on the other Write the definition in plain language and confirm both sides match it


Filters and segments One report silently excludes test accounts, internal users, or a region List every filter applied on each side and diff them


Grain and duplicate rows A join fans out one row into many, inflating a count or sum Compare row counts at the grain before comparing totals


Deleted, test, or refunded rows Soft-deleted or refunded records are included on one side, excluded on the other Check the delete and refund handling on each source


Currency and units Amounts stored in cents versus dollars, or mixed currencies summed without conversion Confirm units and currency handling explicitly


Rounding and data types Floating-point storage or per-row rounding drifts the total by small amounts Check the column type; money should be a decimal, not a float


Late-arriving data and freshness One source has synced more recently than the other Compare the last updated timestamp on each side


Null handling` SUM` and` COUNT(column)` ignore nulls, so a null-heavy column undercounts Count nulls in the join and aggregate columns


Access and row-level filters The viewer only sees rows their permissions allow, so two people see different totals Reconcile as the same role, with the same row-level access


The three that catch people most often are time zones, definitions, and grain. Stripe is a good illustration of the first: in the Stripe Dashboard, financial reports default to your account’s local time zone, but you can switch them to UTC, and that choice changes both how the date range filters the data and how timestamps are displayed ([Stripe reports options](https://docs.stripe.com/reports/options) ). Meanwhile, Stripe’s Sigma and API report runs default to UTC ([Stripe: access data in the dashboard](https://docs.stripe.com/stripe-data/access-data-in-dashboard) ). If your warehouse stores` created_at` in UTC and your Stripe report is set to Pacific time, a payment at 11pm Pacific lands on different days in the two systems, and a daily total will never match until you align the zone.


## A step-by-step reconciliation workflow


Work top-down. Agree on what you are measuring, then align the window, then check the shape of the data before the totals, then bisect.


### Step 1: Pick the source of truth and pin the exact definition


Decide which side is authoritative for this number. Usually it is the transactional system: Stripe for payments, your production database for signups, your CRM for closed deals. The dashboard is what you are checking against it.


Then write the definition in one sentence, including every qualifier. “Monthly recurring revenue is the sum of active subscription amounts, in USD, net of discounts, excluding trials and test accounts, as of the last day of the month.” Vague definitions are the single largest source of unreconcilable numbers. This is where a[single source of truth for metrics](https://www.basedash.com/blog/what-is-a-single-source-of-truth-and-how-to-build-one-for-analytics) earns its keep: if the definition lives in one place, both sides can reference it instead of reinventing it.


### Step 2: Match the time window and time zone


Set both sides to the identical window down to the timestamp, and confirm they use the same time zone. A date range that reads “June 1 to June 30” is ambiguous until you know the zone and whether the endpoints are inclusive. Stripe, for example, treats a selected range as inclusive of the full final day ([Stripe reports options](https://docs.stripe.com/reports/options) ), while a naive` WHERE created_at < '2026-07-01'` in SQL is exclusive of that boundary. Off-by-one-day gaps are almost always a window or zone problem, not a data problem.


### Step 3: Reconcile the grain and row counts before the totals


Do not compare sums yet. Compare how many rows each side thinks it has at the same grain. If one side has 10,240 orders and the other has 10,311, you have a row-level difference to explain, and no amount of staring at the dollar totals will surface it.


Watch for join fan-out here. If you join orders to a table that has more than one matching row per order, each order’s amount gets counted multiple times, and your total balloons. A quick check:` SELECT count(*)` before the join versus after. If the count grows, the join changed the grain.


### Step 4: Bisect the discrepancy


Once counts differ, split the data and see which half carries the gap. Group by day, by product, by region, by plan, whatever dimension is natural, and compare the two sides per group. The difference almost always concentrates: one day, one product, one segment. That tells you where to look instead of auditing everything.


Keep splitting the offending group until the gap is small enough to inspect directly. This is the highest-leverage step in the whole workflow. Bisecting turns “the totals are off by $3,500 somewhere in a million rows” into “these 12 refunded orders are counted on one side and not the other.”


### Step 5: Compare row by row at the breaking point


When you have narrowed to a small set, pull the actual rows from both sides and line them up. Now the cause is usually obvious: a refund with a null` refunded_at` , a duplicate from a bad join, a test account that one filter missed, a currency stored in cents. Remember that SQL aggregates skip nulls, so` sum(amount)` and` count(amount)` silently drop rows where` amount` is null ([PostgreSQL aggregate functions](https://www.postgresql.org/docs/current/functions-aggregate.html) ); a` count(*)` that exceeds` count(amount)` points straight at them.


### Step 6: Write the difference down, or codify it as a check


Once you understand the gap, record it. If it is a real bug, fix the query or the pipeline. If it is a legitimate definition difference, document it next to both reports so the next person does not re-run this whole investigation. Better still, turn the reconciliation into a standing check: a small query that compares the two totals on a schedule and flags when the gap exceeds a threshold. Reconciliation you only do in a fire drill is reconciliation you will redo every quarter.


## A 10-minute reconciliation checklist


When someone pings you with “these two numbers don’t match,” run this before diving into the data:


- Do both sides use the identical metric definition, written in one sentence?
- Are the date range and time zone exactly the same on both sides, including whether endpoints are inclusive?
- Have you compared row counts at the same grain, not just totals?
- Did any join change the grain (row count grew after the join)?
- Are refunds, cancellations, and soft-deletes handled the same way?
- Are test and internal accounts filtered identically?
- Are currency and units consistent (cents versus dollars, single currency)?
- Is the money column a decimal type, not a float?
- Has each source synced through the same point in time?
- Are you viewing both as the same role, with the same row-level access?


If all ten pass and the numbers still differ, then you have a genuine data bug worth escalating.


## Common mistakes


- **Comparing totals first.** The total is the last thing to check, not the first. Counts and grain surface the cause faster.
- **Assuming a bug before a definition check.** Most mismatches are two correct numbers measuring different things. Rule that out before touching the pipeline.
- **Ignoring time zones.** A one-day offset is almost always a zone or inclusivity issue, and it is the most common reason a daily total looks wrong.
- **Reconciling against a moving target.** If one source is still syncing, you are comparing a partial number to a complete one. Pin both to a settled point in time. This is a[data freshness](https://www.basedash.com/blog/data-freshness-how-current-your-dashboard-data-really-is) problem in disguise.
- **Fixing the number without fixing the definition.** If you patch a query to match without understanding why, the gap comes back the next time someone rebuilds the report.


## When to reconcile once versus continuously


Not every number needs a standing reconciliation. Match the effort to the stakes.


Reconcile once, informally, when the number is internal, low-stakes, and you just need to trust it for a decision this week. A quick count-and-bisect is enough.


Reconcile continuously, as an automated check, when the number feeds finance, billing, investor reporting, or anything customer-facing. Revenue, active accounts, and usage-based billing totals are worth a scheduled query that compares the dashboard to the system of record and alerts on drift. The cost of a wrong number in those places is far higher than the cost of the check.


You generally do not need heavy reconciliation for exploratory analysis, one-off questions, or early product metrics that are still being defined. Reconciling numbers that nobody is going to act on is busywork.


## How BI tools help with reconciliation


Reconciliation is much easier when both sides live where you can query them the same way. If your dashboard runs on live SQL against the same database as your source of truth, you can drop from a chart into the underlying rows in seconds rather than exporting two CSVs and diffing them by hand. Tools that let you[combine data from multiple sources](https://www.basedash.com/blog/how-bi-tools-combine-data-from-multiple-sources-federation-blending-and-warehouses) also let you build the comparison query itself, joining the two sides in one place.


[Basedash](https://www.basedash.com/) fits this pattern for teams that want to reconcile fast: it connects directly to your production database or warehouse, so a chart, the SQL behind it, and the raw rows are all one click apart. When a number looks off, you can bisect by any dimension, inspect the exact rows, and turn the reconciliation into a saved query without leaving the tool. Basedash is one option among many, and for accounting-grade financial reconciliation you may still want your finance system as the authority, but for the day-to-day “why don’t these match” questions, keeping the dashboard and the data in the same place removes most of the friction.


## FAQ


**What is the difference between data reconciliation and data validation?** Validation checks whether a single dataset is internally correct: right types, no impossible values, complete rows. Reconciliation compares two datasets that should agree and explains why they differ. A dataset can pass validation and still fail to reconcile against another source because the two use different definitions or windows.


**Why doesn’t my dashboard match Stripe?** The most common reasons are time zone and definition. Stripe Dashboard financial reports default to your account’s local time zone while API and Sigma reports default to UTC, so events near midnight shift days. Beyond that, check whether your dashboard includes refunds, trials, and test accounts the same way Stripe’s report does.


**How do I reconcile two dashboards that show different numbers?** Pin both to the same definition and time window, then compare row counts at the same grain before totals. Group by a dimension like day or product to see where the gap concentrates, then pull the actual rows at that point. The difference is usually a filter, a join that changed the grain, or a definition mismatch.


**Should reconciliation be automated?** For high-stakes numbers like revenue, active accounts, and billing totals, yes. A scheduled query that compares the dashboard to the system of record and alerts when the gap exceeds a threshold catches drift before it reaches a report. Low-stakes internal numbers usually only need a one-off check.


**Why is my total inflated after a join?** A join to a table with more than one matching row per record fans out the rows, so each amount gets counted multiple times. Compare` count(*)` before and after the join; if it grows, the join changed the grain. Aggregate at the correct grain first, or de-duplicate before summing.
