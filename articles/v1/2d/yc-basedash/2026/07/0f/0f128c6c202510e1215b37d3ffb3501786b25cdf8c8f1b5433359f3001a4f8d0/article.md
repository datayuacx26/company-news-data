---
schema_version: "1.0.0"
document_id: "0f128c6c202510e1215b37d3ffb3501786b25cdf8c8f1b5433359f3001a4f8d0"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/slowly-changing-dimensions-how-to-track-historical-data-in-your-warehouse/"
published_at: "2026-07-25T00:00:00+00:00"
first_seen_at: "2026-07-25T15:14:53.687180+00:00"
fetched_at: "2026-07-28T20:32:33.872616+00:00"
content_hash: "sha256:1cd9befc01316b1e2dbd6b2ca7c83bd5ae59255bf48639e5ad4c1c3134b7f1c9"
---

# Slowly changing dimensions: how to track historical data in your warehouse

A slowly changing dimension (SCD) is a technique for tracking how a descriptive attribute changes over time, so your reports stay correct when things like a customer’s plan tier, a sales rep’s territory, or a product’s category change. The core decision is simple: for each attribute, do you keep only the current value, or do you preserve what it was at the time each historical event happened?


Get this wrong and a chart like “revenue by plan tier” quietly rewrites history every time a customer upgrades. Get it right and the same chart shows what was actually true in each period. This guide covers the SCD types, a decision framework for choosing one, the SQL and dbt patterns to implement them, and how a BI tool queries the result.


It is written for analytics engineers, analysts, and data-savvy founders who model data that dashboards sit on top of. If you are earlier in the modeling process, start with[how to model data for BI](https://www.basedash.com/blog/how-to-model-data-for-bi-star-schemas-wide-tables-and-how-much-you-need) and come back here when you hit an attribute whose history matters.


## What is a slowly changing dimension?


In a dimensional model, facts are the events you measure (orders, payments, signups) and dimensions are the descriptive attributes you filter and group by (customer, product, sales rep). A dimension is “slowly changing” when its attributes change occasionally rather than constantly: a customer changes plans a handful of times over their lifetime, not every second.


The name is Ralph Kimball’s, from the dimensional modeling methodology that still underlies most warehouse designs. The techniques all answer one question: when a dimension attribute changes, what do you do with the old value?


There are only three real choices at the row level:


- Overwrite it and forget the past.
- Keep the old value alongside the new one.
- Add a new version of the row and mark the date range each version was valid.


Everything else is a variation on those three.


## Why changing dimensions break reports


Here is the failure mode. Suppose your` customers` table stores` plan_tier` , and a customer upgrades from “Starter” to “Pro” in March. If you overwrite the value, the customer is now “Pro” forever, including for every order they placed back when they were on “Starter.”


Now someone builds “monthly revenue by plan tier.” February’s Starter revenue silently moves into the Pro bucket, because the join looks up the customer’s *current* tier, not the tier they had in February. The chart is wrong, nobody notices for a quarter, and when they do, trust in the whole dashboard evaporates.


This is not a tooling problem you can fix in the BI layer. It is a modeling decision that has to happen where the data is stored. The SCD types are the menu of ways to store history so that “as of the time it happened” reporting is possible.


## The SCD types, explained


The types are numbered, and the numbering is mostly historical rather than logical. Here is what each one actually does.


Type What it does Storage and query cost Use when


Type 0 Keep the original value, never update it Minimal The attribute is fixed by definition (original signup date, first campaign source)


Type 1 Overwrite with the latest value, no history Minimal Nobody reports on the attribute’s past value (a corrected typo, a fixed formatting)


Type 2 Add a new row per change, with valid-from and valid-to dates High (row count grows, joins get date logic) A reported number depends on the value at the time (plan tier, territory, segment)


Type 3 Add a column for the previous value Low (one extra column) You only need “current vs previous,” not full history (a single reorg)


Type 4 Keep current values in the main dimension, push history to a separate table Medium An attribute changes so often it would bloat a Type 2 dimension


Type 6 Combine Types 1, 2, and 3 in one dimension High You need full history *and* frequently slice by the current value


A few notes that the table cannot capture:


**Type 2 is the workhorse.** When people say “we need slowly changing dimensions,” they almost always mean Type 2. It is the only type that preserves a complete, queryable history of every state a row passed through.


**Type 2 needs a surrogate key.** Because one business entity (customer id 42) now has multiple rows, you need a separate surrogate key (a unique key per *version* ) as the primary key, and your fact tables must reference that surrogate key, not the business key. This is the detail teams most often miss, and it breaks point-in-time reporting when they do.


**Type 6 gets its name from arithmetic.** 1 + 2 + 3 = 6. It keeps Type 2 history rows but also carries a “current value” column that you overwrite (Type 1 style) on every row, so you can group by either the historical value or the current value from the same table.


**Type 5 exists but is niche.** It combines a Type 4 mini-dimension with a Type 1 pointer to the current version. Most teams never need it. If you do, you already know.


## Which SCD type should you use?


Most guides list the types and stop. The harder part is choosing, and over-engineering here is the most common mistake. Adding Type 2 history to attributes nobody analyzes historically buys you slower joins, larger tables, and more complex SQL for zero reporting value.


Use this two-question test, attribute by attribute, not table by table:


**Question 1: Does any report need this attribute’s value as it was at a past point in time?**


- If no, use **Type 1** (overwrite). This is the correct default for most attributes. If the value must never change once set, use **Type 0** .
- If yes, you need history. Go to question 2.


**Question 2: How much history do you actually need?**


- Full history across every change, so any past event joins to the value that was true then: **Type 2** .
- Only the immediately previous value (“before and after” a single change like a rebrand or reorg): **Type 3** .
- Full history, but you also constantly slice by the *current* value, so you want both in one place: **Type 6** .
- The attribute changes so frequently that Type 2 would explode the row count (a churn risk score updated daily, for example): split it out with **Type 4** , or move it to the fact table where it belongs.


The practical posture: start with Type 1 everywhere. Promote a specific attribute to Type 2 only when a real report depends on its past value. Slowly changing dimensions are a targeted technique, not a setting you turn on for a whole table. This mirrors the broader principle in[data modeling for BI](https://www.basedash.com/blog/how-to-model-data-for-bi-star-schemas-wide-tables-and-how-much-you-need) : do only as much modeling as the question requires.


## How to implement Type 2 in SQL


The mechanics of Type 2 are: when an attribute changes, close out the current row by setting its` valid_to` date, then insert a new row for the new value with an open-ended` valid_to` and a` is_current` flag.


A hand-rolled version looks like this:


```text
-- 1. Close the current version when an attribute changes
update   dim_customer
set      valid_to     =   current_date,
is_current   =   false
where    customer_id   =   :id
and      is_current    =   true
and      plan_tier    <>   :new_plan_tier;


-- 2. Insert the new version with an open-ended validity window
insert into   dim_customer
(customer_key, customer_id, plan_tier, valid_from, valid_to, is_current)
values
(:new_surrogate_key, :id, :new_plan_tier, current_date,   null  , true);
```


Every row now carries three bookkeeping columns:` valid_from` ,` valid_to` (null or a far-future sentinel like` 9999-12-31` for the current version), and` is_current` . The` customer_key` is the surrogate key;` customer_id` is the business key that repeats across versions.


Doing this by hand is error-prone at scale, which is why most teams use a transformation tool. In dbt,[snapshots](https://docs.getdbt.com/docs/build/snapshots) implement Type 2 for you:


```text
{%   snapshot   customers_snapshot %}
{{ config(
target_schema  =  'snapshots'  ,
unique_key  =  'customer_id'  ,
strategy  =  'check'  ,
check_cols  =  ['plan_tier', 'account_owner']
) }}
select   *   from   {{ source(  'app'  ,   'customers'  ) }}
{% endsnapshot %}
```


Each run compares the current source rows against the snapshot. When a value in` check_cols` changes, dbt closes the old row (` dbt_valid_to` ) and inserts a new one (` dbt_valid_from` ), maintaining the history without you writing the merge logic. The` timestamp` strategy is faster if your source has a reliable “updated at” column; the` check` strategy compares column values directly when it does not. This is one of the reasons dbt sits at the center of modern[analytics engineering](https://www.basedash.com/blog/what-is-analytics-engineering-and-when-your-team-needs-one) .


## How to query point-in-time data


Storing history is only half the job. The payoff is being able to join a fact to the dimension version that was valid when the fact occurred. That is a range join on the validity window:


```text
select   o  .  order_id  ,
o  .  order_total  ,
c  .  plan_tier            -- the tier at the time of the order
from     fct_orders o
join     dim_customer c
on     c  .  customer_id   =   o  .  customer_id
and    o  .  ordered_at   >=   c  .  valid_from
and    o  .  ordered_at   <    coalesce  (  c  .  valid_to  ,   '9999-12-31'  );
```


That` coalesce` handles the current row, whose` valid_to` is null. With this join, “revenue by plan tier by month” reports the tier each customer actually had in each month, which is the whole point.


When you want current state instead of history, filter to the current version:


```text
select   *   from   dim_customer   where   is_current   =   true;
```


A well-modeled dimension supports both questions from the same table: historical truth through the range join, current truth through the flag.


## Common mistakes and tradeoffs


**Applying Type 2 to everything.** The biggest source of pain. Every Type 2 attribute multiplies rows and forces date logic into joins. Reserve it for attributes that genuinely drive point-in-time reporting.


**Facts referencing the business key instead of the surrogate key.** If` fct_orders` joins on` customer_id` alone without the validity window, or references the natural key where it should reference the version’s surrogate key, point-in-time reporting silently falls back to current-value behavior. This is the most common Type 2 bug.


**Gaps and overlaps in validity windows.** If a new row’s` valid_from` does not line up exactly with the prior row’s` valid_to` , some events fall through the cracks (a gap) or match two versions (an overlap, which fans out and double counts). Use consistent boundaries and half-open ranges (` >=` on from,` <` on to).


**Late-arriving data.** A fact that arrives after a dimension has already changed versions needs to join to the version that was valid at the event date, not the load date. Range joins handle this correctly; “join to current” logic does not.


**Doing SCD in the BI tool.** Some teams try to reconstruct history with window functions inside a dashboard query. It is slow, fragile, and duplicated across every report. History belongs in the modeled table in the warehouse, defined once. This is the same reasoning behind putting metric logic in a shared layer rather than scattering it across dashboards, covered in[where to define business metrics](https://www.basedash.com/blog/where-to-define-business-metrics-sql-views-dbt-semantic-layers-or-bi-calculations) .


## How BI tools consume slowly changing dimensions


Once the history lives in the warehouse, a BI tool does not need any special SCD feature. It just queries the modeled tables. If you built the range join into a view or a dbt model, “revenue by plan tier over time” becomes an ordinary group-by, and the tool renders it as a chart.


That division of labor matters when you pick tools. The warehouse and a transformation layer like dbt own correctness and history; the BI tool owns exploration and presentation.[Basedash](https://www.basedash.com/) is an example of the consumption layer: it connects to sources like PostgreSQL, Snowflake, and BigQuery, queries your modeled tables, and lets technical and non-technical users explore the result through natural language and charts. It does not replace the SCD modeling, and it should not have to reconstruct history itself. If your dimensions are modeled well, point-in-time questions are answerable without anyone re-deriving the logic in a dashboard.


The takeaway: slowly changing dimensions are a modeling discipline, not a BI feature. Decide per attribute whether past values matter, default to Type 1, promote to Type 2 only where a report depends on history, and put that logic in the warehouse so every tool downstream reads one consistent version of the past.


## FAQ


### What is the difference between Type 1 and Type 2 slowly changing dimensions?


Type 1 overwrites an attribute with its latest value and keeps no history, so every report reflects the current state. Type 2 preserves history by adding a new row each time the attribute changes, tagged with valid-from and valid-to dates, so events join to the value that was true when they happened. Use Type 1 by default and Type 2 only for attributes where a reported number depends on past values, such as plan tier in historical revenue.


### Do I always need slowly changing dimensions?


No. Most attributes can use Type 1 (overwrite) because no report cares about their past values. You only need Type 2 history for the specific attributes where reporting “as of the time it happened” changes a number, like segment, territory, or pricing tier. Adding history everywhere just makes tables larger and joins slower for no reporting benefit.


### What is a Type 2 surrogate key and why does it matter?


Because Type 2 stores multiple rows for one business entity, the business key (like customer id) is no longer unique. A surrogate key is a separate unique identifier for each version of the row. Your fact tables must reference this surrogate key so that each event links to the correct historical version. Skipping this is the most common reason point-in-time reports quietly return current values instead of historical ones.


### How does dbt handle slowly changing dimensions?


dbt implements Type 2 through snapshots. You define a snapshot with a unique key and either a` timestamp` strategy (using a reliable updated-at column) or a` check` strategy (comparing specified columns). On each run, dbt detects changes, closes the previous row with a` dbt_valid_to` timestamp, and inserts a new row, maintaining full history without you writing merge logic by hand.


### Where should SCD logic live: the database, dbt, or the BI tool?


In the warehouse, defined once. Snapshots or SQL models in the transformation layer are the right place because they run reliably on a schedule and produce a consistent history every downstream tool can read. Reconstructing history with window functions inside individual BI dashboards is slow, fragile, and duplicated across reports, and it leads to different dashboards disagreeing about the past.
