---
schema_version: "1.0.0"
document_id: "fb3a27bf81735cedf1771cc434ce2c8b97d2846e57135344765d6c6f3a8fb282"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-to-do-cohort-analysis-retention-revenue-and-behavioral-cohorts/"
published_at: "2026-07-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T20:47:42.945239+00:00"
content_hash: "sha256:bf17e4da7af172fb2100d80be886a3d5eb4dbead0ecc0e00296f5665ea55175f"
---

# How to do cohort analysis: retention, revenue, and behavioral cohorts

Cohort analysis groups users by a shared starting point, usually the month they signed up or first paid, then tracks how each group behaves over time. Instead of one blended retention number, you get a row per cohort and a column per period, which makes it obvious whether the product is getting stickier or whether a good month was hiding a slow decline.


This guide is a hands-on workflow. It covers the three cohort types you will actually use, the SQL patterns for building a retention table and a revenue retention table on any warehouse or production database, how to read the resulting triangle, and the mistakes that quietly make cohort charts lie. It is written for founders, product managers, analysts, and operators who want to run the analysis themselves rather than shop for a tool.


## What cohort analysis actually answers


A single retention percentage blends everyone together. If half your users churn fast and the other half stay forever, the average looks mediocre and tells you nothing about which half is growing.


Cohort analysis fixes that by holding the starting point constant. You compare the January signups to the February signups at the same age, not on the same calendar date. That comparison answers questions a blended metric cannot:


- Is retention improving for newer cohorts, which usually means onboarding or product changes are working?
- Where in the lifecycle do users drop off, and is that point moving earlier or later?
- Do users from a specific acquisition channel, plan, or onboarding path retain differently?
- Are existing customers expanding revenue faster than churned customers lose it?


If those are your questions, cohort analysis is the right tool. If you just want a current snapshot of active users, a simple dashboard metric is enough.


## The three cohort types you will actually use


Most teams over-index on signup retention and ignore the other two. Each type answers a different question, and the SQL is nearly identical once you have the pattern.


Cohort type Grouping key What you measure per period Best for


Acquisition (time-based) Signup or first-active month Percentage of the cohort still active Product stickiness, onboarding changes, activation


Revenue (dollar) First-payment month Revenue retained versus the cohort’s starting revenue Net dollar retention, expansion, pricing changes


Behavioral Whether a user did action X in a window Retention of doers versus non-doers Finding the activation event that predicts retention


Acquisition cohorts tell you whether people keep showing up. Revenue cohorts tell you whether the accounts you keep are worth more or less over time, which is where expansion and contraction hide. Behavioral cohorts tell you which early action separates users who stay from users who leave, which is the most actionable of the three because it points at something you can push people toward.


## Step by step: build a retention cohort table in SQL


The mechanics are the same on Postgres, BigQuery, Snowflake, Redshift, or a read replica of your production database. The examples below use Postgres syntax; adjust the date functions for your engine.


### Step 1: define the cohort key and the activity event


You need two decisions before writing anything:


1. **The cohort key.** Usually the truncated signup date.` date_trunc('month', created_at)` puts everyone who signed up in the same month into one cohort. Use weeks for high-frequency consumer products and months for B2B.
2. **The activity definition.** “Active” has to mean something specific: a login, a session, a meaningful action, or a payment. A vague definition is the single biggest source of misleading cohort charts. Pick the event that reflects real value and write it down.


### Step 2: assign each user to a cohort


Compute one cohort month per user from their first event.


```text
with   cohorts   as   (
select
user_id,
date_trunc(  'month'  ,   min  (created_at))   as   cohort_month
from   users
group by   user_id
),
```


### Step 3: find each user’s active periods


Reduce the activity log to one row per user per month they were active.


```text
activity   as   (
select
user_id,
date_trunc(  'month'  , event_time)   as   active_month
from   events
where   event_name   =   'active_session'
group by   user_id, date_trunc(  'month'  , event_time)
),
```


### Step 4: count active users per cohort per period offset


The offset is the number of months between the cohort month and the active month. Offset 0 is the cohort’s starting size.


```text
counts   as   (
select
c  .  cohort_month  ,
(extract(  year    from   a  .  active_month  )   -   extract(  year    from   c  .  cohort_month  ))   *   12
+   (extract(  month   from   a  .  active_month  )   -   extract(  month   from   c  .  cohort_month  ))   as   month_offset,
count  (  distinct   a  .  user_id  )   as   active_users
from   cohorts c
join   activity a   on   a  .  user_id   =   c  .  user_id
group by   c  .  cohort_month  ,   2
)
```


### Step 5: convert counts to retention percentages


Divide each cell by the cohort’s offset-0 count. A window function does this without a self-join.


```text
select
cohort_month,
month_offset,
active_users,
round  (
100  .  0   *   active_users
/   max  (active_users)   filter   (  where   month_offset   =   0  )
over   (  partition   by   cohort_month),
1
)   as   retention_pct
from   counts
order by   cohort_month, month_offset;
```


Pivot` month_offset` into columns in your BI tool and you have the classic retention triangle. The` date_trunc` and` filter` clauses are standard SQL features documented in the[PostgreSQL date functions reference](https://www.postgresql.org/docs/current/functions-datetime.html) ; most warehouses support equivalents.


## Revenue cohorts and net dollar retention


Swap the count for a sum of revenue and you measure dollar retention instead of user retention. This is where expansion shows up: a cohort that keeps 70% of its users can still retain more than 100% of its revenue if the remaining accounts upgrade.


```text
with   first_payment   as   (
select   customer_id, date_trunc(  'month'  ,   min  (charged_at))   as   cohort_month
from   charges
group by   customer_id
),
monthly_revenue   as   (
select
customer_id,
date_trunc(  'month'  , charged_at)   as   revenue_month,
sum  (amount)   as   revenue
from   charges
group by   customer_id, date_trunc(  'month'  , charged_at)
),
counts   as   (
select
fp  .  cohort_month  ,
(extract(  year    from   mr  .  revenue_month  )   -   extract(  year    from   fp  .  cohort_month  ))   *   12
+   (extract(  month   from   mr  .  revenue_month  )   -   extract(  month   from   fp  .  cohort_month  ))   as   month_offset,
sum  (  mr  .  revenue  )   as   cohort_revenue
from   first_payment fp
join   monthly_revenue mr   on   mr  .  customer_id   =   fp  .  customer_id
group by   fp  .  cohort_month  ,   2
)
select
cohort_month,
month_offset,
round  (
100  .  0   *   cohort_revenue
/   max  (cohort_revenue)   filter   (  where   month_offset   =   0  )
over   (  partition   by   cohort_month),
1
)   as   revenue_retention_pct
from   counts
order by   cohort_month, month_offset;
```


Read the offset-12 column as net dollar retention for that cohort. Values above 100% mean expansion outran churn and contraction. Because this pattern uses the same starting point as your user cohorts, you can put both triangles on the same page. If you are already building a revenue view, this slots naturally next to the metrics in a[SaaS revenue dashboard](https://www.basedash.com/blog/how-to-build-a-saas-revenue-dashboard-metrics-data-sources-and-structure) .


## Behavioral cohorts: finding the activation event


Behavioral cohorts split users by an action rather than a date. The goal is to find the early behavior that best predicts long-term retention, so you can build onboarding around it.


The pattern: flag whether each user completed the candidate action inside a fixed early window, then compare retention curves between the two groups.


```text
with   signups   as   (
select   user_id, date_trunc(  'week'  , created_at)   as   cohort_week, created_at
from   users
),
did_action   as   (
select distinct   s  .  user_id
from   signups s
join   events e
on   e  .  user_id   =   s  .  user_id
and   e  .  event_name   =   'created_first_report'
and   e  .  event_time   <=   s  .  created_at   +   interval   '7 days'
)
select
case   when   da  .  user_id   is not null   then   'did action'   else   'did not'   end   as   segment,
count  (  *  )   as   users
from   signups s
left join   did_action da   on   da  .  user_id   =   s  .  user_id
group by   1  ;
```


Extend this by joining to the same activity table used above and computing retention per segment. If the “did action” group retains 40 points higher at week 8, you have found a candidate activation metric. This is closely related to funnel work; the[funnel analysis guide](https://www.basedash.com/blog/how-to-build-a-funnel-analysis-dashboard-sql-patterns-and-common-mistakes) covers the SQL for ordered event sequences that often feed these definitions.


## How to read a cohort table


A cohort table (or triangle) has cohorts as rows, period offset as columns, and a retention value in each cell. Read it in three passes:


1. **Read down a column, not across a row.** Fixing the offset (say month 3) and scanning down compares every cohort at the same age. A rising month-3 column means newer cohorts retain better, which is the clearest sign your changes are working.
2. **Read across a row to see one cohort’s decay.** The shape matters more than any single number. A curve that drops steeply then flattens is healthy: you have a stable core. A curve that keeps sliding toward zero has no retained base.
3. **Watch where the flattening happens.** The offset where the curve stops falling is your retention floor. If it moves earlier over time, users are finding value faster.


The bottom-right of the triangle is always sparse because recent cohorts have not aged yet. Do not read those partial cells as a trend.


## Common mistakes that make cohort charts lie


- **A vague activity definition.** “Active” defined as any page load inflates every number and hides real churn. Tie the definition to a valuable action and keep it constant across every cohort chart.
- **Reading immature cohorts.** The most recent cohorts have only a few periods of data. Treating their offset-1 number as a trend is the most common misread. Grey out or exclude cohorts that have not reached the offset you are comparing.
- **Tiny cohorts.** A cohort of 12 users produces retention percentages that swing wildly on one person leaving. Roll up to a period (weekly to monthly) or a wider segment until each cohort is large enough to be stable.
- **Mixing calendar time and relative time.** Cohort analysis is about age, not date. Comparing “January’s cohort in March” to “March’s cohort in March” is a category error that seasonality will punish.
- **Blending acquisition channels.** Paid, organic, and referral users often retain very differently. A flat overall curve can hide one channel improving while another decays. Segment before you conclude anything.
- **Counting events instead of distinct users.** In the retention SQL,` count(distinct user_id)` is not optional. Counting rows lets a chatty user or a retry loop distort the whole column.
- **Confusing user retention with revenue retention.** They move independently. Report both, and never present one as if it were the other.


## When to use SQL, and when to reach for a tool


You do not always need to write the SQL above. Use this to decide.


**Write the SQL yourself when:**


- Your data lives in a warehouse or production database and you already work in SQL.
- You need a custom cohort key, a nonstandard activity definition, or revenue logic your analytics tool does not model.
- You want the cohort table to live alongside other metrics in a shared dashboard rather than a separate product analytics app.


**Use a product analytics tool when:**


- You are instrumenting client-side events and do not have a warehouse yet. Amplitude, Mixpanel, and PostHog build cohort and retention views from event streams with no SQL.
- You need self-serve behavioral cohorts for non-technical teammates and can accept the tool’s definitions.


**Use a SQL-native BI tool when:**


- You want the flexibility of SQL plus a shared, refreshable dashboard.[Basedash](https://www.basedash.com/) , Metabase, Hex, Mode, and Looker all run the queries above against your database and pivot the result into a cohort grid, so an analyst writes the query once and the whole team reads the triangle. Basedash’s AI can also draft the cohort query from a plain-English description, which shortens the first version.


For a full breakdown of dedicated options, see the[cohort analysis software guide](https://www.basedash.com/blog/the-complete-guide-to-cohort-analysis-software-tools-strategies-and-best-practices-for-2026) and the roundup of[customer analytics tools for retention and churn](https://www.basedash.com/blog/best-customer-analytics-tools-for-retention-and-churn-2026) .


## FAQ


### What is the difference between retention analysis and cohort analysis?


Retention analysis measures whether users come back. Cohort analysis is the method: it groups users by a starting point so you can compare retention fairly across time. Most retention analysis is done with cohorts, but you can also run cohort analysis on revenue or behavior, not just return visits.


### Weekly or monthly cohorts?


Match the cohort period to your usage frequency. Consumer products with daily or weekly use are best read as weekly cohorts. B2B and subscription products with monthly cycles are clearer as monthly cohorts. If cohorts are too small to be stable, widen the period.


### How is net dollar retention related to cohort analysis?


Net dollar retention is a revenue cohort read at a fixed age, usually 12 months. Build a revenue cohort table, then read the offset-12 cell: it is the percentage of the cohort’s starting revenue retained after expansion, contraction, and churn. Above 100% means expansion outran losses.


### How many periods should a cohort table show?


Enough to reach the point where the retention curve flattens, plus a little margin. For most SaaS products that is 6 to 12 months. Showing 36 columns when everything is decided by month 4 just adds noise and sparse cells.


### Can I do cohort analysis in a spreadsheet?


For a one-off with a few hundred rows, yes. Beyond that it becomes error-prone and hard to refresh. Once the analysis matters enough to check weekly, move it into SQL or a BI tool so the definition is version-controlled and the numbers update on their own.
