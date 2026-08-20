---
schema_version: "1.0.0"
document_id: "98a41019bb09eee80259b1bd208ca80ca91167bf7803052b176bdb399244d533"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-to-do-ad-hoc-analysis-a-repeatable-workflow-for-one-off-data-questions/"
published_at: "2026-07-20T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T21:08:37.946927+00:00"
content_hash: "sha256:ceefde03299a47d9c1d0a1c8100ea3589e2a45cc00b137e283b56f60b0a65dbc"
---

# How to do ad hoc analysis: a repeatable workflow for one-off data questions

Ad hoc analysis is the investigative work of answering a specific, unplanned question by exploring data directly, instead of reading a pre-built dashboard. It is what you do when a metric moves unexpectedly, a stakeholder asks something no chart covers, or you need to check a hunch before making a decision. The questions are one-off, but the process that leads to a trustworthy answer should not be improvised every time.


This guide gives you a repeatable six-step workflow for ad hoc analysis, with SQL examples, a fully worked investigation, and the mistakes that quietly produce wrong answers. It is written for analysts, founders, operators, and product managers who already have data in a database or warehouse and need to answer questions faster than a dashboard backlog allows.


## Ad hoc analysis vs ad hoc reporting vs dashboards


These three terms get used interchangeably, but they describe different things. Ad hoc analysis is the investigative process. Ad hoc reporting is the artifact you produce for someone else. Dashboards are the recurring, always-on views.


Ad hoc analysis Ad hoc reporting Dashboards


**What it is** The investigation itself A one-off report shared with others A persistent set of monitored metrics


**Goal** Understand why or whether something is true Deliver a specific answer to a stakeholder Track known metrics over time


**Output** A conclusion and next step A table, chart, or short writeup A saved, refreshing view


**Lifespan** Minutes to hours Single use Ongoing


**Who does it** Anyone who can query data Analyst or data-literate user Built once, viewed by many


In practice they form a loop. A dashboard surfaces an anomaly, ad hoc analysis explains it, and the explanation either turns into a shared report or gets promoted into a new dashboard metric. For more on the reporting side of this loop, see our guide to[ad hoc reporting](https://www.basedash.com/blog/what-is-ad-hoc-reporting-how-on-demand-data-analysis-works) .


## A repeatable workflow for ad hoc analysis


Good analysts do not stare at data and wait for insight. They run a consistent process that narrows an open-ended question into a defensible answer. Here is a six-step loop you can apply to almost any ad hoc question.


### 1. Frame the question precisely


A vague question produces a vague answer. “Why is revenue down?” is not answerable. “Why did net new MRR fall from roughly $48k in June to $45k in July?” is. Before you write any SQL, pin down the metric, the time window, the comparison, and what decision the answer will inform.


Write the question in one sentence with concrete numbers if you have them. If you cannot state the question in one sentence, you are not ready to query yet.


### 2. Establish a baseline


Almost every ad hoc question is really a comparison. A number only means something relative to what it usually is. Before you explain a change, confirm the change is real and size it against normal variation.


```text
select
date_trunc(  'month'  , activity_month)   as   month  ,
sum  (mrr)   as   mrr
from   subscription_mrr
where   activity_month   >=   date_trunc(  'month'  ,   now  ())   -   interval   '12 months'
group by   1
order by   1  ;
```


Twelve months of history tells you whether a 6% drop is a genuine break from trend or the kind of month-to-month wobble you see all the time. Skip this step and you risk investigating noise.


### 3. Slice by one dimension at a time


Once you know the change is real, decompose it. Break the metric down by one dimension, look at the result, then move to the next dimension. Changing two things at once makes it hard to attribute what you find.


```text
select
plan,
sum  (mrr)   as   mrr
from   subscription_mrr
where   activity_month   =   date_trunc(  'month'  ,   now  ())
group by   1
order by   2   desc  ;
```


Common first dimensions: plan or tier, customer segment, acquisition channel, region, cohort, and time granularity (daily instead of monthly). The goal is to find the slice where the change concentrates.


### 4. Isolate the driver


Aggregate metrics hide their own causes. Revenue is the sum of new, expansion, contraction, and churn. Signups are the sum of every channel. Break the headline number into its components so you can see which one moved.


```text
select
movement_type,
sum  (mrr_change)   as   mrr_change
from   mrr_movements
where   activity_month   =   date_trunc(  'month'  ,   now  ())
group by   1
order by   mrr_change;
```


If churn is the culprit, keep drilling into the churn component specifically rather than the net number. This is where[drill-down and drill-through](https://www.basedash.com/blog/drill-down-vs-drill-through-in-bi-tools-what-they-do-and-when-to-use-each) matter: you follow the number down until you reach the rows that explain it.


### 5. Validate the finding


Before you report a conclusion, try to break it. Pull the actual rows behind the aggregate and eyeball them. Check that the finding holds when you change the time window slightly. Confirm your metric definition matches how the rest of the team defines it.


```text
select
account_id,
plan,
mrr_change,
churn_reason
from   mrr_movements
where   activity_month   =   date_trunc(  'month'  ,   now  ())
and   movement_type   =   'churn'
order by   mrr_change
limit   25  ;
```


If a handful of accounts explain most of the drop, that is a very different story than broad-based churn, and it changes what you recommend.


### 6. Decide and document


An analysis that ends without a decision or a written note is wasted work. State the answer, the evidence, and the recommended action in a few sentences. Save the query. If the question is likely to recur, this is the moment to promote it into a dashboard or a saved report rather than re-running the investigation from scratch next month.


## A worked example: MRR dropped 6% this month


Here is the loop applied end to end.


**Frame.** Net new MRR fell from $48k in June to $45k in July. Is this a trend break, and what is driving it? The decision it informs: whether to raise it in the leadership review as a real problem or a blip.


**Baseline.** Twelve months of monthly MRR shows net new bouncing between $44k and $50k. A drop to $45k is inside the normal range, but it is at the low end, so it is worth a quick look rather than an alarm.


**Slice.** Broken out by plan, the Pro tier is flat and Enterprise is down. The change concentrates in Enterprise.


**Isolate.** Decomposing Enterprise MRR movement shows new and expansion are healthy, but churned MRR roughly doubled versus the prior month. The problem is churn, not slower acquisition.


**Validate.** Listing the churned Enterprise accounts shows three accounts account for most of the churned MRR, and all three list the same churn reason: they were acquired by a single parent company that consolidated onto one contract. That is a one-time event, not a demand signal.


**Decide.** Report that the dip is real but explained by a single consolidation, that underlying acquisition and expansion are healthy, and that no action is needed beyond noting the consolidated account. Save the churn-decomposition query so next month’s check takes two minutes.


Without the baseline step, this looks like a scary revenue decline. Without validation, it looks like a churn problem worth a task force. The workflow is what turns a scary headline into an accurate, boring answer.


## Common mistakes in ad hoc analysis


The failure modes in ad hoc analysis are rarely about SQL syntax. They are about jumping to a conclusion the data does not support.


- **No baseline.** Reacting to a single number without knowing its normal range. Most “problems” are within ordinary variation.
- **Stopping at the first plausible cause.** The first explanation that fits is not necessarily the right one. Keep going until the numbers actually add up to the change you are explaining.
- **Aggregation hiding the story.** A total can be flat while two segments move in opposite directions, or a trend can reverse when you split by a group (Simpson’s paradox). Always check whether the aggregate is masking segment-level movement.
- **Mixing metric definitions.** If your ad hoc “revenue” includes refunds and the finance dashboard’s does not, your numbers will never reconcile. Anchor to a shared definition, ideally a[semantic layer](https://www.basedash.com/blog/what-is-a-semantic-layer-the-complete-guide-for-modern-bi) or governed metric, before you start slicing.
- **Confusing correlation with cause.** Two lines moving together is a lead to investigate, not a conclusion. Validate with the underlying rows before you claim causation.
- **Ignoring data freshness and completeness.** If today’s data has not finished loading, the current period will look artificially low. Confirm the window you are analyzing is complete.


## When to stop, and when to promote to a dashboard


Ad hoc analysis is meant to be temporary. Two decisions keep it from turning into wasted effort or dashboard sprawl.


**Stop when** you can state the answer in one sentence, the numbers reconcile with the headline change, and you have validated the finding against the raw rows. Further slicing past that point usually produces detail nobody needs.


**Promote to a dashboard when** the same question has come up three or more times, when the metric needs continuous monitoring rather than a one-time answer, or when multiple people need a shared, consistent view. If a question is genuinely one-off, resist the urge to build a permanent dashboard for it. For turning a validated query into something durable, see[how to build a SQL dashboard people trust](https://www.basedash.com/blog/how-to-build-a-sql-dashboard-from-queries-to-a-dashboard-people-trust) .


The healthiest pattern keeps analysis and monitoring separate: dashboards watch the metrics you already know matter, and ad hoc analysis handles everything you did not plan for. Teams that try to answer every new question by adding another chart end up with dozens of dashboards nobody trusts.


## Where tooling fits


Ad hoc analysis needs a tool that connects directly to your database or warehouse and lets you move quickly from question to result. SQL editors like[Basedash](https://www.basedash.com/) , DBeaver, and DataGrip give you full control for complex investigations. Visual query builders such as Metabase and Mode suit data-literate users who want speed without writing every query. AI-native tools that translate plain-English questions into SQL now let non-technical teammates run the first few steps of this workflow themselves, then verify the generated SQL before trusting the result.


Whatever the interface, the workflow is the same. The tool determines how fast you can move through the loop, not whether the loop is worth following. For teams that want more people doing this safely without a data-request queue,[self-service BI](https://www.basedash.com/blog/self-service-bi-the-complete-guide-to-empowering-teams-with-data-access) is the broader practice ad hoc analysis lives inside, and a good[data-driven decision framework](https://www.basedash.com/blog/data-driven-decision-making-a-framework-that-goes-beyond-dashboards) is what connects the answer to an action.


## Frequently asked questions


### What is the difference between ad hoc analysis and exploratory data analysis?


They overlap heavily. Exploratory data analysis (EDA) is a broad, open-ended survey of a dataset to understand its shape, distributions, and relationships, often before modeling. Ad hoc analysis is narrower and more targeted: you start with a specific business question and investigate until you can answer it. EDA asks “what is in this data?”; ad hoc analysis asks “why did this specific number change?”


### Do you need SQL to do ad hoc analysis?


Not always, but it helps. SQL gives you the flexibility to answer questions no pre-built view anticipated, which is the whole point of ad hoc work. Visual query builders and AI-powered natural-language tools now let non-technical users run many ad hoc analyses without writing SQL, though the most complex investigations still benefit from direct query access. When using AI-generated SQL, review the query before trusting the result.


### How long should an ad hoc analysis take?


Most well-scoped ad hoc questions should take minutes to a couple of hours, not days. If it is taking longer, the question is usually too broad, the data is not modeled for it, or the answer really requires a proper project rather than an ad hoc query. Reframe the question more narrowly before investing more time.


### How do you keep ad hoc analysis reproducible?


Save the query with a comment describing the question it answered, anchor calculations to shared metric definitions so numbers reconcile, and note the date range and any filters you applied. If the same query keeps getting rebuilt by different people, promote it to a saved view or dashboard so the logic lives in one place.


### How do you avoid ad hoc queries slowing down production?


Run ad hoc queries against a read replica or a data warehouse rather than the primary production database, use a read-only role, and set query timeouts and row limits. Heavy investigative queries can compete with application traffic for resources, so isolating analytical reads protects both the app and the analysis.
