---
schema_version: "1.0.0"
document_id: "2005777f22a34b8452ab4d3d4deeb5474ef525658ed3b9389d443e7f1a20eb89"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/data-quality-metrics-what-to-measure-and-how-to-track-it/"
published_at: "2026-07-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T21:08:37.946927+00:00"
content_hash: "sha256:9835179c54cc1e6d6c08dc715c96993fab679d168e5f2631eea732dc2d0e6983"
---

# Data quality metrics: what to measure and how to track it

Data quality metrics are the specific, repeatable measurements that tell you whether a dataset is trustworthy enough to make decisions on. The most useful ones map to six dimensions of data quality: completeness, validity, accuracy, consistency, uniqueness, and timeliness. For each dimension you pick a concrete metric, such as the percentage of non-null values in a required column or the share of rows updated in the last 24 hours, run it as a query on a schedule, and compare the result to a threshold. When a metric drops below its threshold, someone gets a signal before a broken number reaches a dashboard.


This guide is for analysts, data engineers, and operators who have been asked to “make sure the data is right” and want something more concrete than a vibe. It covers which metrics to track, how to measure each one, where to run the checks, and how to turn the results into a scorecard your team will actually look at.


## TL;DR


- A data quality metric is a number, run on a schedule against a threshold, that measures one property of a dataset. Without a threshold and a schedule, it is just a chart.
- Group metrics by the six dimensions: completeness, validity, accuracy, consistency, uniqueness, and timeliness. Most teams over-index on completeness and ignore validity and timeliness.
- Every metric should be a query you can run on your warehouse or production database. If you cannot express it as a check, you cannot monitor it.
- Measure at the layer where the data is consumed, not only in the pipeline. A row can pass every ingestion test and still be wrong in the report.
- Start with the three to five columns that drive real decisions. Measuring quality on tables nobody reads is wasted effort.


## What are data quality metrics and dimensions?


A data quality dimension is a category of “correctness.” A data quality metric is a specific measurement inside that category. The dimension tells you what kind of problem you are guarding against; the metric tells you how bad it is right now.


The six-dimension model is the most widely used framework and comes from DAMA UK’s white paper “The Six Primary Dimensions for Data Quality Assessment” (DAMA UK Working Group, 2013). The dimensions are completeness, uniqueness, timeliness, validity, accuracy, and consistency. You do not need all six on every table. You need the two or three that would actually cause a wrong decision if they broke.


Here is what each dimension means in practice:


Dimension The question it answers Example metric


Completeness Is the data all there? Percent of non-null values in a required column


Validity Does the data fit the rules? Percent of rows matching an allowed format or range


Accuracy Does the data match reality? Percent of records reconciled against a source of truth


Consistency Does the data agree with itself? Percent of rows where related fields do not contradict


Uniqueness Is anything duplicated? Count of duplicate business keys


Timeliness Is the data current enough? Hours since the last successful update


The rest of this guide walks through how to measure each one.


## How do you measure completeness?


Completeness is the share of expected values that are actually present. It is the easiest dimension to measure and the one teams start with, which is why it often gets over-weighted.


Measure it per column, not per table. A table can be 98% populated overall while the one column your revenue report depends on is 40% null. The metric that matters is the null rate on the specific fields a decision relies on.


```text
select
count  (  *  )   as   total_rows,
count  (email)   as   rows_with_email,
round  (  100  .  0   *   count  (email)   /   count  (  *  ),   2  )   as   completeness_pct
from   users;
```


Set the threshold based on how the column is used. A required field like` customer_id` should be 100% complete, and anything below that is an incident. An optional field like` phone_number` might be fine at 60%. The mistake is applying one blanket target to every column.


## How do you measure validity?


Validity is whether values conform to a defined format, type, or range. A row can be present (complete) but still invalid: an email with no` @` , a country code that is not in your list, a negative order amount, a signup date in the future.


Validity checks are rules expressed as queries. Each rule counts the rows that violate it.


```text
select
count  (  *  )   as   total_rows,
sum  (  case   when   email   not   like   '%_@_%.__%'   then   1   else   0   end  )   as   invalid_emails,
sum  (  case   when   amount   <   0   then   1   else   0   end  )   as   negative_amounts,
sum  (  case   when   created_at   >   now  ()   then   1   else   0   end  )   as   future_dates
from   orders;
```


Validity is where the most business logic lives, and where the biggest quality gains usually come from, because invalid values often slip past ingestion tests that only check for nulls and types.


## How do you measure accuracy?


Accuracy is whether the data matches the real-world thing it describes. It is the hardest dimension to measure because it requires an external reference. You cannot confirm a revenue figure is accurate by looking only at the table it lives in; you have to compare it to something you trust more.


In practice, accuracy is measured through reconciliation: compare a value against an authoritative source and count the mismatches. Common patterns are checking that a warehouse total matches the source system, or that a rolled-up metric equals the sum of its parts.


```text
-- Does the aggregated daily revenue table match the raw orders?
select
d  .  day  ,
d  .  revenue   as   reported_revenue,
o  .  revenue   as   source_revenue,
d  .  revenue   -   o  .  revenue   as   difference
from   daily_revenue d
join   (
select   date_trunc(  'day'  , created_at)   as   day  ,   sum  (amount)   as   revenue
from   orders
group by   1
) o   on   o  .  day   =   d  .  day
where   abs  (  d  .  revenue   -   o  .  revenue  )   >   0  .  01  ;
```


Any row returned is an accuracy problem. Reconciliation against a single agreed source is why establishing a[single source of truth](https://www.basedash.com/blog/what-is-a-single-source-of-truth-and-how-to-build-one-for-analytics) is a prerequisite for measuring accuracy at all. Without an agreed reference, “accurate” has no meaning.


## How do you measure consistency?


Consistency is whether the data agrees with itself, both across fields in the same row and across tables. Examples: a` churned_at` date that is earlier than` signed_up_at` , an order marked “shipped” with no shipment record, a customer count on a dashboard that differs from the count in the CRM.


Cross-field consistency is a query over one table. Cross-table consistency is a query that joins the two things that should agree.


```text
-- Cross-field: lifecycle dates that contradict each other
select   count  (  *  )   as   inconsistent_rows
from   subscriptions
where   churned_at   is not null
and   churned_at   <   signed_up_at;


-- Cross-table: paid invoices with no matching payment
select   count  (  *  )   as   orphaned_invoices
from   invoices i
left join   payments p   on   p  .  invoice_id   =   i  .  id
where   i  .  status   =   'paid'
and   p  .  id   is   null  ;
```


Consistency problems erode trust faster than any other dimension, because when two reports disagree, people stop believing both.


## How do you measure uniqueness?


Uniqueness is the absence of unwanted duplicates. Duplicated rows inflate counts, double-count revenue, and quietly break joins. The metric is the number of business keys that appear more than once.


```text
select
count  (  *  )   as   duplicate_keys,
sum  (cnt)   -   count  (  *  )   as   extra_rows
from   (
select   customer_id,   count  (  *  )   as   cnt
from   customers
group by   customer_id
having   count  (  *  )   >   1
) d;
```


Distinguish between technical duplicates (the same record ingested twice) and true entity duplicates (the same customer entered under two email addresses). The first is a pipeline fix; the second is a resolution problem that usually needs deduplication logic or a matching rule.


## How do you measure timeliness?


Timeliness, sometimes called freshness, is whether the data is current enough to trust. A perfectly accurate table that stopped updating two days ago will produce confidently wrong dashboards. The metric is the age of the most recent record, or the lag between when an event happened and when it landed.


```text
select
max  (updated_at)   as   last_update,
round  (extract(epoch   from   (  now  ()   -   max  (updated_at)))   /   3600  ,   1  )   as   hours_since_update
from   events;
```


Set the threshold to match the promise the data makes. A dashboard labeled “live” should be minutes fresh; a daily finance report can tolerate a day of lag. Timeliness is closely related to broader monitoring, and if you need alerting across many tables, dedicated[data observability tools](https://www.basedash.com/blog/best-data-observability-tools-compared-2026) automate freshness and volume checks at scale.


## A data quality scorecard you can copy


Individual checks are useful, but leadership wants one view. A data quality scorecard collects the key metrics per dataset, shows each against its threshold, and rolls up to a single health status. The point is not a vanity “97% quality” number; it is a short list of checks that are green or red right now.


Here is a starter rubric for a single important table. Adjust thresholds to your context.


Dimension Check Threshold Action if breached


Completeness Null rate on required keys 0% null Block the report, page the owner


Validity Rows failing format or range rules Under 0.5% Investigate same day


Accuracy Warehouse total vs source total Within 0.1% Halt downstream reconciliation


Consistency Contradictory or orphaned rows 0 rows Fix logic before next load


Uniqueness Duplicate business keys 0 duplicates Dedupe, then find the cause


Timeliness Hours since last update Under the SLA for that table Check the pipeline, flag the dashboard


The value of the rubric is the “action if breached” column. A metric with no defined response is a chart nobody acts on. Tie each threshold to an owner and a next step, or the scorecard becomes decoration.


## Where should you measure data quality?


Most data quality tooling runs checks in the pipeline, right after ingestion or transformation. That is the right place to catch structural problems: schema changes, null spikes, volume drops. But pipeline checks miss a whole class of issues, because a row can pass every ingestion test and still be wrong in the report. A join that fans out, a filter that silently drops rows, a metric definition that changed: none of these show up in a raw-table freshness check.


That is why it helps to also measure quality at the analytics layer, where the numbers are actually consumed. When the same tool that builds the dashboard can also run the completeness, validity, and reconciliation checks against live data, the person reading the report is the first to know it is off, not the last.


A modern BI tool like[Basedash](https://www.basedash.com/) connects directly to a production database or warehouse, so these checks live next to the dashboards they protect. An analyst can write a validity or reconciliation query, save it as a monitored view, and surface a red flag in the same place the team already looks. This is the analytics-layer complement to pipeline monitoring, not a replacement for it. For teams standardizing definitions and access across both layers, it fits inside a broader[data governance framework](https://www.basedash.com/blog/data-governance-framework-a-practical-guide-for-lean-teams) .


## Common mistakes when tracking data quality


**Measuring everything.** Running 200 checks across every table produces noise, alert fatigue, and no action. Pick the columns that drive decisions and measure those well.


**Only checking completeness.** Null rates are easy, so teams stop there. Validity, consistency, and accuracy catch the errors that actually reach the wrong conclusion.


**No thresholds.** A dashboard of quality percentages with no line for “acceptable” is a chart, not a control. Every metric needs a pass or fail bar.


**No owner.** A failing check with no assigned person is ignored by default. The scorecard’s “action” column only works if a name is attached.


**Trusting the pipeline alone.** Ingestion tests confirm data arrived intact. They do not confirm the number in the report is right after transformations and joins.


## Which data quality metrics should you track first?


You do not start with a governance program. You start with the smallest set of checks that would have caught the last time a wrong number embarrassed the team.


1. List the three to five metrics leadership actually acts on, such as revenue, active users, or pipeline.
2. Trace each back to the columns and tables it depends on.
3. On those columns, add a completeness check on the keys, one validity rule, and a timeliness check.
4. Add one reconciliation check per headline metric against its source of truth.
5. Give every check a threshold and an owner, then put them on one page.


That is roughly a dozen checks covering the numbers that matter, which is far more valuable than a hundred checks on tables nobody reads. Expand only when a real incident shows you a gap.


## FAQ


### What are the six dimensions of data quality?


They are completeness, validity, accuracy, consistency, uniqueness, and timeliness, as defined in DAMA UK’s “The Six Primary Dimensions for Data Quality Assessment” (2013). Completeness is whether values are present, validity is whether they fit the rules, accuracy is whether they match reality, consistency is whether the data agrees with itself, uniqueness is the absence of duplicates, and timeliness is whether the data is current enough to trust.


### What is a good data quality score?


There is no universal number, because the right threshold depends on how a field is used. A required key should be 100% complete and any null is an incident, while an optional field can be fine at 60%. Instead of chasing a single blended “quality score,” track each critical check against a threshold set for that specific field and its use. A blended percentage tends to hide the one broken column that matters.


### What is the difference between data quality and data observability?


Data quality is the set of properties that make data trustworthy, measured with specific checks against thresholds. Data observability is the practice of continuously monitoring pipelines and tables for anomalies such as freshness delays, volume drops, and schema changes, usually with automated detection. Quality metrics define what “good” means; observability tooling watches for when it breaks at scale.


### Can you measure data quality with SQL?


Yes. Almost every data quality metric can be written as a query: null rates for completeness, format and range checks for validity, reconciliation joins for accuracy, contradiction checks for consistency, group-and-count for uniqueness, and a max-timestamp lag for timeliness. Running those queries on a schedule and comparing the results to thresholds is a complete, low-cost quality program for a small team.


### How often should data quality checks run?


Match the check frequency to the data’s update cadence and the cost of being wrong. Timeliness and completeness checks on a live table should run every load or every few minutes. Reconciliation and accuracy checks can run after each daily refresh. Running checks far more often than the data changes just adds noise without catching anything new.


### Do you need a dedicated data quality tool?


Not to start. A handful of SQL checks with thresholds, run on a schedule and shown on one dashboard, covers most of the value for a lean team. Dedicated tools earn their keep when you need automated anomaly detection across hundreds of tables, lineage-aware alerting, or governance features. Many teams get far with checks that live next to their BI, and add specialized tooling only when scale demands it.
