---
schema_version: "1.0.0"
document_id: "ebe6ed12c44899bd3e380b5d323b4192dbc346e140748014d94ab5929272120f"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-to-review-ai-generated-sql-a-checklist-for-analysts-and-operators/"
published_at: "2026-05-21T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T20:50:36.289404+00:00"
content_hash: "sha256:f4d73ce16670816d88dba0bd367c8996b1808b4aa4f9ef17a8a91ceffc696942"
---

# How to review AI-generated SQL: a checklist for analysts and operators

AI-generated SQL is correct often enough to be dangerous. A query that runs, returns plausible numbers, and produces a clean chart can still be wrong in ways that are invisible from the dashboard. The number is off by a factor of two because the join doubled rows. The trend looks healthy because the filter quietly dropped a third of customers. The week-over-week comparison is misleading because the date column is` updated_at` , not` created_at` .


This guide is for analysts, analytics engineers, founders, and operators who are letting ChatGPT, Claude, an AI BI tool, or an MCP server write SQL against their warehouse and want a repeatable way to check the output. It covers the failure modes that show up most often in AI-generated SQL, a ten-point review checklist you can run in a few minutes, a rubric for when to skip the review, and the workflow patterns that catch mistakes before they reach a dashboard.


## The short answer


Read the SQL before you read the chart. Specifically:


1. Confirm the question the SQL is actually answering matches the question you asked.
2. Verify the tables, columns, and filters are the right ones for your schema.
3. Check the join cardinality and the deduplication strategy.
4. Sanity-check the result with a known number you can compute another way.


If you do those four things every time, you will catch the majority of AI SQL mistakes. The rest of this post is the longer version, with a complete checklist, the specific patterns that trip up LLMs, and a rubric for when reviewing each query stops being worth your time.


## Why AI-generated SQL needs human review


LLMs are good at producing SQL that looks right. They are less good at producing SQL that is right on your specific schema, with your specific business rules, and your specific data quality issues. The gap is usually filled by context the model does not have.


Three structural reasons AI SQL goes wrong, even with a strong model and a semantic layer:


1. **Ambiguous business definitions.** “Active user” might mean logged in this week, completed a paid action this month, or any account where` status = 'active'` . Without a clear definition the model picks one, and the answer changes silently when it picks differently next time.
2. **Schema noise and legacy columns.** Production schemas are full of` is_active` ,` is_active_v2` ,` deleted_at` ,` archived_at` ,` status` ,` state` , and three different timestamp columns per table. The model has to guess which one is canonical.
3. **Implicit filters everyone on the team knows.** Internal accounts, test users, refunded orders, and soft-deleted rows are often filtered everywhere in production code but rarely documented. The model omits them unless told.


None of these issues prevent the query from running. They just shift the answer. That is what makes AI SQL distinct from human-written SQL: the failure mode is rarely a crash, it is a confidently wrong number.


## The failure modes that show up most often


Before the checklist, it helps to know what you are looking for. These are the patterns that account for most AI SQL mistakes in practice.


### Join fanout


The model joins` orders` to` line_items` and aggregates revenue from the orders table without realizing the join multiplied each order row by the number of line items. Revenue is now inflated by 3-10x. This is the single most common AI SQL error.


### Wrong date column


` orders` has` created_at` ,` paid_at` ,` shipped_at` , and` updated_at` . The model picks` created_at` when the business cares about` paid_at` , or it picks` updated_at` and the time series gets distorted by retroactive status changes.


### Missing soft-delete filter


The model ignores` deleted_at IS NULL` ,` archived_at IS NULL` , or` is_deleted = false` because no one told it to apply them. Counts and aggregates include records that should be invisible.


### Internal and test data not excluded


The query counts internal employee accounts, demo workspaces, test users, or the founder’s seven test accounts. Small absolute numbers, but enough to skew early-stage metrics by 20 percent or more.


### Inclusive vs exclusive date boundaries


` WHERE created_at >= '2026-01-01' AND created_at < '2026-02-01'` is right.` BETWEEN '2026-01-01' AND '2026-02-01'` includes the first second of February. Off-by-one in dashboards rarely breaks anything, but it is a steady source of disagreements between two dashboards “showing the same thing.”


### Implicit type coercion


` user_id` joined to` customer_id` when one is an integer and the other is a string, or a timestamp compared to a date without explicit casting. The query may return zero rows, the wrong rows, or work on Postgres and fail on Snowflake.


### Window function partitions


The model writes` ROW_NUMBER() OVER (ORDER BY created_at)` instead of` OVER (PARTITION BY user_id ORDER BY created_at)` . The deduplication works for a small sample, then quietly breaks on real data.


### Aggregation level mismatches


The question asked for revenue per customer per month, but the model groups only by month, or only by customer. The output looks plausible at a glance but answers a different question than the one you asked.


### NULL handling


` COUNT(column)` excludes NULLs,` COUNT(*)` does not.` AVG()` ignores NULLs.` SUM()` returns NULL when all rows are NULL, not zero. Subtle differences that shift averages and conversion rates.


### Currency, timezones, and units


Revenue is summed across multiple currencies without conversion. Timestamps are stored in UTC but reported in the wrong local timezone. Page load times are in seconds in one table and milliseconds in another. The query runs cleanly and produces nonsense.


If you keep this taxonomy in mind, the checklist becomes faster, because you know what to look for.


## The ten-point review checklist


Run this every time you accept AI-generated SQL into a dashboard, a Slack post, or a report. It takes two to five minutes per query once you are practiced.


### 1. Restate the question the SQL is answering


Read the query and write down, in one sentence, what it actually returns. Compare that to the question you asked. If they do not match, stop and ask the AI to rewrite. Most other checks are wasted effort until this one passes.


### 2. Confirm the tables are the canonical ones


Some schemas have` orders` and` orders_v2` , or` users` and` customers` , or a raw table and a transformed dbt model. The AI might pick a staging table that is not refreshed, or a deprecated table that still exists. Verify each table in the` FROM` and` JOIN` clauses is the one you would use yourself.


### 3. Confirm the columns are the canonical ones


For every column referenced, ask whether it is the right one.` revenue` might be gross, net, or recognized.` signup_date` might be the email confirmation date, the first session date, or the row insert time. If your warehouse has documentation, comments, or a metrics layer, cross-check there. If not, this is the place where most subtle errors live.


### 4. Check every join for cardinality


For each` JOIN` , ask: what is the relationship between the two tables? One-to-one, one-to-many, many-to-many? If you join a parent table to a child table and aggregate columns from the parent, you will double-count unless you deduplicate. The fix is usually a` DISTINCT` , a subquery, or aggregating before joining. If you cannot tell at a glance what the cardinality should be, run a quick` COUNT(*) ... GROUP BY` to find out.


### 5. Check every filter


Look at the` WHERE` clause and ask: is anything missing? Common omissions are soft-delete flags, internal account filters, status conditions, and refund or chargeback handling. Add anything the team would normally apply.


### 6. Check date boundaries explicitly


Confirm the date filter uses half-open intervals (` >= start AND < end` ), the column is the right one for the question, and the timezone matches what the consumer expects. If the dashboard is daily, confirm the truncation function uses the right grain (` DATE_TRUNC('day', ...)` vs` DATE_TRUNC('week', ...)` ).


### 7. Check aggregation level and GROUP BY


Every non-aggregated column in the SELECT should appear in the GROUP BY. The grain of the result (one row per what?) should match the question. If the question asks for “monthly revenue by customer,” the result should be one row per customer per month.


### 8. Sanity-check with a known number


Run the query and compare one cell of the result to a number you can verify another way: a known monthly revenue figure, a row count from an admin tool, a number from a trusted dashboard, or a quick` SELECT COUNT(*)` . If your sanity-check number does not match, do not publish. The cost of finding out from a stakeholder is much higher than running an extra query.


### 9. Spot-check edge cases


Pick the highest and lowest values in the result and verify they make sense. If a single customer accounts for 80 percent of revenue, is that real or a join fanout? If a region shows zero orders this month, is that accurate or a filter issue? Edge cases are where wrong queries usually betray themselves.


### 10. Read the query out loud


This sounds silly, and it works. Reading the SQL as if you were explaining it to a coworker forces you to slow down and notice the part that does not quite make sense. Most experienced analysts catch their own mistakes this way.


You do not need to run all ten checks for every query. A simple aggregate against one well-understood table can pass with checks 1, 5, and 8. A multi-join, multi-CTE query with window functions deserves the full pass.


## A worked example


Suppose you ask an AI BI tool, “What was monthly active revenue per customer over the last six months for paying customers in North America?”


It writes:


```text
SELECT
DATE_TRUNC(  'month'  ,   o  .  created_at  )   AS   month  ,
c  .  id   AS   customer_id,
SUM  (  o  .  amount  )   AS   revenue
FROM   customers c
JOIN   orders o   ON   o  .  customer_id   =   c  .  id
JOIN   order_items oi   ON   oi  .  order_id   =   o  .  id
WHERE   c  .  region   =   'North America'
AND   o  .  created_at   >=   NOW  ()   -   INTERVAL   '6 months'
GROUP BY   1  ,   2
ORDER BY   1  ,   2  ;
```


A two-minute review finds three problems.


First, the join to` order_items` is unnecessary, and it fanouts each order into one row per line item.` SUM(o.amount)` now multiplies revenue by the number of line items per order. This is the most common AI SQL bug, and it shows up here in plain view.


Second, the question asked about *paying* customers, and there is no filter for refunded or failed orders. Most warehouses have an` o.status = 'paid'` or` o.refunded_at IS NULL` condition that the model omitted.


Third, “monthly active revenue” likely means revenue from customers who had a paid order in that month, but it could also mean recurring revenue or recognized revenue. If your team uses MRR, this query is not it. Ask which definition the dashboard owner wants and restate the question before fixing the SQL.


A corrected version might look like:


```text
SELECT
DATE_TRUNC(  'month'  ,   o  .  paid_at  )   AS   month  ,
o  .  customer_id  ,
SUM  (  o  .  amount  )   AS   revenue
FROM   orders o
JOIN   customers c   ON   c  .  id   =   o  .  customer_id
WHERE   c  .  region   =   'North America'
AND   o  .  status   =   'paid'
AND   o  .  paid_at   >=   NOW  ()   -   INTERVAL   '6 months'
GROUP BY   1  ,   2
ORDER BY   1  ,   2  ;
```


The fix is not exotic. The point is that without the review, the original query would have run, produced a chart, and quietly misreported revenue by 2-5x. That is the failure mode AI SQL invites.


## When you can skip the review


The checklist is overhead. There are cases where the overhead is not worth it.


You can usually skip a careful review when:


- The query is exploratory and the answer is not going into a dashboard, decision, or report.
- The table is small, well-understood, and you have run similar queries before.
- The result is a single count or aggregate against one table with a clear filter, and you can verify it visually.
- The query was generated against a governed semantic layer where joins, filters, and metric definitions are already enforced.


You should run the full review when:


- The result is going on a dashboard or into a report.
- The query joins three or more tables.
- The result will be shared in Slack, email, or a board deck.
- The metric is one stakeholders disagree about often (revenue, active users, retention, churn).
- The model has access to columns or tables it has not used before.


A reasonable rule of thumb: review effort should scale with the cost of the answer being wrong. A throwaway ad hoc check gets a glance. A dashboard the leadership team looks at every Monday gets the full checklist.


## Workflow patterns that help


A few habits make AI SQL review faster and more reliable across a team.


### Always show the SQL


Any AI BI tool worth using exposes the generated SQL alongside the answer. If your tool hides the SQL behind a chart, you cannot review it. This is one of the biggest practical differences between AI BI products. Tools like[Basedash](https://www.basedash.com/) show the SQL for every AI-generated answer, run it against a governed connection with read-only access, and log every query so that mistakes can be traced after the fact. That grounding shows up in results: in[BI Bench](https://www.basedash.com/bi-bench) , a public benchmark of AI data analyst agents against a real database with a complex schema, Basedash ranks first on accuracy.


### Pin metric definitions


Define the metrics that matter most to your business in a place the AI can read. A dbt project, a semantic layer, a metrics catalog, or even a markdown file referenced by your AI tool gives the model a stable definition for “MRR,” “active user,” or “qualified lead.” This shifts the review effort from every query to the metric definition itself, where it belongs.


### Keep a “common mistakes” doc per team


After a query gets shipped with a bug, write down what went wrong and how it slipped through. Over a few months, this becomes a short list of “things to check on this codebase” that catches the same mistake the next time.


### Pair AI generation with quick validation queries


For high-stakes results, write a second AI prompt that asks for a validation query: “Write a query that totals the same numbers a different way.” If the two queries agree, you have a strong cross-check. If they disagree, you have a free debugging signal.


### Treat the model as a junior analyst


A senior analyst would not let a brand-new hire publish a dashboard without reviewing the SQL. The right mental model for AI BI tools is the same. They produce useful first drafts. They are not yet trustworthy enough to ship without a second pair of eyes, especially on numbers that drive decisions.


## Common mistakes when reviewing AI SQL


A few patterns to avoid in the review itself:


- **Trusting the explanation more than the SQL.** The AI’s natural-language description of the query often paraphrases what it *meant* to write, not what the SQL actually does. Read the SQL.
- **Skipping the sanity-check.** This is the single highest-value step, and it gets skipped most often because it adds another query. Do it anyway.
- **Reviewing the SQL once and trusting it forever.** Schemas change, dbt models change, business definitions change. A query that was correct three months ago might be wrong today. Re-validate when anything upstream changes.
- **Reviewing only the SQL, never the data.** Sometimes the SQL is correct and the underlying data has a quality issue (missing rows, duplicate IDs, stale syncs). Spot-check the result, not just the query.
- **Letting the AI grade its own work.** Asking the same model “Is this query correct?” rarely surfaces its own mistakes. Use a different model, a different prompt, or a deterministic check (counts, totals, schemas) for cross-validation.


## When AI SQL is good enough without review


There is one case where AI-generated SQL is genuinely safe to use without per-query review: when the tool generating it can only emit queries against a constrained, governed surface where the joins, filters, and metric definitions are already correct.


That looks like:


- A semantic layer that defines metrics, dimensions, joins, and filters explicitly.
- A read-only role that cannot see internal accounts, soft-deleted rows, or non-canonical tables.
- A metrics catalog that documents the canonical definition of each KPI.
- A BI tool that enforces these definitions at query time rather than trusting the LLM to remember them.


When all of these are in place, the AI is essentially picking from a menu rather than writing SQL from scratch. The review effort moves to the semantic layer, where it is much cheaper. Without these guardrails, every AI-generated query is effectively a junior analyst’s first attempt, and it needs the same level of review.


## FAQ


### Should I ever skip showing the SQL to my stakeholders?


For most business users, the SQL is noise. Show them the chart and the metric definition. Keep the SQL accessible for the small number of people who need to verify it, including yourself. Tools that surface SQL on demand strike the right balance.


### How do I review SQL if I do not know SQL well?


Two patterns work. First, ask the AI to explain the query in plain language step by step, then verify each step against the question. Second, run a few comparison queries: total revenue, total rows, top customer. If those numbers match a trusted source, the query is probably fine. If not, ask someone who knows SQL to look.


### Is a semantic layer enough to skip review?


It removes most of the join, filter, and metric-definition mistakes. It does not remove ambiguity about which metric a stakeholder wants, mismatched date boundaries, or NULL handling errors. A semantic layer cuts review time meaningfully but does not eliminate it.


### What about AI tools that automatically run validation tests?


A few BI products run sanity checks on AI-generated SQL before showing the result: comparing row counts to expectations, checking for unusually large changes, flagging join fanouts. These are useful, but they cannot catch business-definition mistakes. Treat them as a second pair of eyes, not a substitute for review.


### How is reviewing AI SQL different from reviewing human SQL?


Human-written SQL tends to fail loudly: syntax errors, runtime errors, obvious nonsense. AI-written SQL fails quietly: plausible numbers that are subtly wrong. The review needs to be more skeptical of correctness on the *right* question, not just runnable code. The mechanical parts (cardinality, joins, NULL handling) are the same.


### Where does this fit with traditional data quality testing?


Data quality testing (dbt tests, Great Expectations, Soda) validates that the data in your warehouse is internally consistent. AI SQL review validates that the *query* is asking the right question of that data. Both are necessary. Neither replaces the other.


The pattern that scales is: invest in a semantic layer and governed metrics so the AI is choosing from correct primitives, use an AI BI tool that exposes its SQL by default, and treat per-query review as a five-minute habit rather than a one-hour audit. Done that way, AI-generated SQL becomes a genuine productivity gain instead of a slow-motion data quality incident.
