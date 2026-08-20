---
schema_version: "1.0.0"
document_id: "76291e4b4418f293d9cdf6dcf7539eead988e4150bb5a7c29c13250ee48bfaf4"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/dau-wau-mau-how-to-measure-active-users-without-fooling-yourself/"
published_at: "2026-08-13T00:00:00+00:00"
first_seen_at: "2026-08-13T14:45:57.801423+00:00"
fetched_at: "2026-08-13T14:46:02.064938+00:00"
content_hash: "sha256:812e355c95aa0782efb2d2e3332ad28366eacec74903e5503ac54ed0c9c91740"
---

# DAU, WAU, and MAU: how to measure active users without fooling yourself

DAU, WAU, and MAU count the number of unique users who did something in your product over a rolling one-day, seven-day, or thirty-day window. The metrics themselves are trivial to compute. The number that actually decides whether they mean anything is your definition of “active”: the specific action a user has to take to be counted. Pick a shallow action like “opened the app” and your active-user counts inflate every time someone lands on a login screen. Pick a real value action like “ran a query” or “sent a message” and the numbers track something you can make decisions on.


This guide is for founders, product managers, and analysts who want to track active users off their own database or warehouse instead of guessing at a chart in a product analytics tool. It covers what each metric measures, the definition decision that makes or breaks all three, rolling versus calendar windows, how to calculate them in SQL, how to read the DAU/MAU stickiness ratio, and the mistakes that quietly distort the numbers.


## What DAU, WAU, and MAU actually measure


All three are counts of distinct users who performed a qualifying action within a time window. The only thing that changes is the length of the window.


Metric Window Counts Best for


DAU 1 day Unique active users in a single day Daily-use products, launch monitoring, real-time health


WAU 7 days Unique active users across a rolling week Products used a few times a week, smoothing out weekday and weekend swings


MAU 30 days Unique active users across a rolling month Overall reach, the denominator for stickiness, board and investor reporting


Two properties trip people up. First, these are unique counts, not visit counts. A user who logs in ten times in one day is one DAU, not ten. Second, the windows overlap in membership: a single daily active user is also, by definition, part of that week’s WAU and that month’s MAU. You cannot add DAU across 30 days and expect to get MAU, because the same person shows up on many days. MAU is almost always smaller than the sum of daily counts and larger than any single day’s DAU.


## The decision that makes or breaks the metric: what counts as “active”


Before you write a single query, decide what a user has to do to be counted. This is the highest-leverage choice in the entire exercise, and most teams make it by accident.


There are roughly three levels of “active,” and they produce wildly different numbers off the same data:


- **Opened or loaded.** The user reached the product. This is the loosest definition and the easiest to inflate. Background refreshes, push-notification opens, and bounced sessions all count.
- **Performed a core action.** The user did the thing your product exists to do: sent a message, ran a report, created a record, completed a workout. This is the definition most worth tracking because it maps to value delivered.
- **Reached a value milestone.** The user hit a threshold that signals real engagement, like completing three actions or spending two minutes in a workflow. Stricter, more meaningful, harder to compare against other companies.


A simple rubric for choosing: pick the earliest action in a session that reliably indicates the user got value, and exclude anything a user can trigger without intent. For a messaging product that is “sent a message,” not “app foregrounded.” For a BI tool it is “opened a dashboard” or “ran a query,” not “loaded the login page.” Write the definition down, list the exact events or tables that qualify, and put it next to the chart. A DAU number without a stated definition is not a metric, it is a rumor.


One more rule: keep the definition identical across DAU, WAU, and MAU. If DAU counts message senders but MAU counts anyone who logged in, your stickiness ratio is meaningless because the numerator and denominator measure different populations.


## Rolling windows vs calendar periods


There are two honest ways to draw the window, and they answer different questions.


**Rolling windows** look back a fixed number of days from each point in time. Rolling MAU on August 13 counts distinct users active from July 15 through August 13. This is the version most product analytics tools show by default, and it is what you want for a smooth trend line, because it does not lurch at the start of each month.


**Calendar periods** count distinct users within a named month, like “August MAU.” This is easier to explain to a board and lines up with billing and cohort reporting, but it creates a sawtooth: the count climbs through the month as more people show up, then resets to near zero on the first. Comparing “MAU so far this month” against “all of last month” is a classic apples-to-oranges error.


Neither is more correct. Pick one, label it explicitly on the chart, and never compare a rolling number to a calendar number. If you report to investors monthly, calendar MAU is usually cleaner; if you are watching a launch, rolling windows read better day to day.


## How to calculate DAU, WAU, and MAU in SQL


These examples assume a Postgres-style` events` table with one row per action: a` user_id` , an` event_name` , and a` created_at` timestamp. Swap in your own qualifying action wherever the query filters on` event_name` . Every query here is read-only.


DAU as a daily time series over the last month:


```text
SELECT
created_at::  date   AS   day  ,
COUNT  (  DISTINCT   user_id)   AS   dau
FROM   events
WHERE   event_name   =   'ran_query'            -- your "active" definition
AND   created_at   >=   CURRENT_DATE   -   INTERVAL   '30 days'
GROUP BY   1
ORDER BY   1  ;
```


MAU for the trailing 30 days as a single number:


```text
SELECT   COUNT  (  DISTINCT   user_id)   AS   mau
FROM   events
WHERE   event_name   =   'ran_query'
AND   created_at   >=   CURRENT_DATE   -   INTERVAL   '30 days'  ;
```


WAU works the same way with a seven-day window. The interesting one is a rolling MAU as a time series, where each day counts distinct users over the prior 30 days. A self-join keeps it readable:


```text
WITH   days   AS   (
SELECT   generate_series  (
CURRENT_DATE   -   INTERVAL   '60 days'  ,
CURRENT_DATE,
INTERVAL   '1 day'
)::  date   AS   day
)
SELECT
d  .  day  ,
COUNT  (  DISTINCT   e  .  user_id  )   AS   rolling_mau
FROM   days   d
JOIN   events e
ON   e  .  created_at  ::  date   >    d  .  day   -   INTERVAL   '30 days'
AND   e  .  created_at  ::  date   <=   d  .  day
AND   e  .  event_name   =   'ran_query'
GROUP BY   d  .  day
ORDER BY   d  .  day  ;
```


A warning on cost:` COUNT(DISTINCT ...)` recomputed across a rolling window is one of the more expensive things you can ask a warehouse to do, and it gets worse as your event table grows. If the rolling query is slow, precompute a daily table of distinct active users and aggregate from that, or use an approximate distinct count like Postgres extensions’ HyperLogLog, BigQuery’s` APPROX_COUNT_DISTINCT` , or Snowflake’s` APPROX_COUNT_DISTINCT` . For most early-stage products the exact query is fine; reach for approximation when it stops returning in a few seconds. See our[performance playbook for slow BI dashboards](https://www.basedash.com/blog/how-to-make-slow-bi-dashboards-fast-a-performance-playbook) for the general pattern.


## Stickiness: the DAU/MAU ratio and how to read it


The DAU/MAU ratio divides average daily active users by monthly active users, expressed as a percentage. It is the single most common measure of “stickiness,” or how many of your monthly users show up on a typical day. A ratio of 20% means the average monthly user is active about six days a month; 50% means roughly fifteen.


```text
WITH   daily   AS   (
SELECT
created_at::  date   AS   day  ,
COUNT  (  DISTINCT   user_id)   AS   dau
FROM   events
WHERE   event_name   =   'ran_query'
AND   created_at   >=   CURRENT_DATE   -   INTERVAL   '30 days'
GROUP BY   1
),
monthly   AS   (
SELECT   COUNT  (  DISTINCT   user_id)   AS   mau
FROM   events
WHERE   event_name   =   'ran_query'
AND   created_at   >=   CURRENT_DATE   -   INTERVAL   '30 days'
)
SELECT   ROUND  (  AVG  (  daily  .  dau  )   /   monthly  .  mau   *   100  ,   1  )   AS   stickiness_pct
FROM   daily, monthly;
```


The number only means something against the right comparison. The classic benchmark, a[10 to 20% ratio popularized by a Sequoia tweet](https://www.geckoboard.com/resources/kpi-examples/dau-mau-ratio/) , is dated, and Facebook’s famous 50% bar was set for a daily-communication product that almost nothing else resembles. More recent data is higher and varies a lot by category. Mixpanel’s[2026 benchmarks report](https://mixpanel.com/blog/mau/) , which analyzed billions of events across thousands of companies, puts B2B SaaS stickiness around 31%, with ecommerce closer to 20%.


The honest takeaway: there is no universal “good” DAU/MAU number. A weekly-review tool with a 20% ratio can be perfectly healthy, while a daily-use consumer app at 20% is in trouble. Judge the ratio against your product’s natural usage rhythm and, above all, against its own trend. A stickiness ratio that is climbing over time is a stronger signal than any single reading compared to an outside benchmark.


## Common mistakes that quietly distort active users


- **Counting sessions instead of unique users.** Forgetting` DISTINCT` turns active users into activity counts and inflates every number. Verify that a single user active twice in a window counts once.
- **A loose or undocumented “active” definition.** If “active” means “loaded a page,” bots, health checks, and accidental opens pad the count. Tie the metric to a value action and write the definition on the chart.
- **Mixing rolling and calendar windows.** Comparing month-to-date MAU against last month’s full total makes growth look worse than it is, and comparing a rolling number to a calendar number is meaningless.
- **Different definitions for numerator and denominator.** A DAU that counts one action and an MAU that counts another produces a stickiness ratio that measures nothing.
- **Not excluding internal and test accounts.** Your own team, QA automation, and demo accounts can dominate active users at small scale. Filter out internal user IDs and email domains before you count.
- **Time zone drift.** Bucketing timestamps in UTC while your users live in one region can split a single active day across two calendar days. Convert to a consistent reporting time zone before truncating to a date.
- **Reading averages without the distribution.** A rising MAU can hide a shrinking core of power users if a wave of one-time signups props up the count. Pair active users with a[cohort retention view](https://www.basedash.com/blog/how-to-do-cohort-analysis-retention-revenue-and-behavioral-cohorts) so you can see whether new users stick.


## Building an active-users dashboard people trust


A dashboard that survives scrutiny needs four things beyond the headline numbers. Put DAU, WAU, and MAU on the same time axis so the relationship between them is visible. Show the stickiness ratio as its own trend line, not a single stat, because the direction matters more than the value. Annotate launches and outages directly on the chart so a spike or dip has an explanation. And state the “active” definition in plain text on the dashboard itself, so no one has to reverse-engineer it from the SQL.


You do not necessarily need a dedicated product analytics tool to build this. If your event data already lives in Postgres, a warehouse like Snowflake or BigQuery, or a lakehouse, you can query it directly and put these metrics on a dashboard next to the revenue and retention numbers they need to be read alongside. This is the workflow[Basedash](https://www.basedash.com/) is built for: connect a database, write or generate the SQL for your active-user definition, and share a live dashboard with the team without exporting anything into a separate system. Keeping active users in the same place as your other business metrics also avoids the common trap of two tools reporting two different MAU numbers because they define “active” differently.


## When active-user metrics are the wrong tool


Active-user counts are a health signal, not a full diagnosis, and there are cases where they mislead.


Skip DAU as a primary metric for products with an inherently infrequent rhythm. A tax-filing app, a hiring tool, or an annual-review product is supposed to be used in bursts; a low DAU/MAU ratio there is expected, not a problem. For those, retention curves and task-completion rates tell you far more than daily counts.


Reach for a dedicated product analytics tool like Amplitude, Mixpanel, or PostHog when your core questions are about behavioral paths rather than headline reach: complex funnels, session replay, or per-event breakdowns across dozens of properties. Those workflows are what those tools optimize for. Our guide on whether you[need a product analytics tool or your warehouse can do it](https://www.basedash.com/blog/do-you-need-a-product-analytics-tool-or-can-your-warehouse-do-it) walks through the tradeoff. And when you want to understand not just how many users are active but which behaviors predict them staying, move from active-user counts to a[customer health score](https://www.basedash.com/blog/how-to-build-a-customer-health-score-signals-weighting-and-sql) .


## FAQ


### What is the difference between DAU, WAU, and MAU?


They are the same metric measured over different windows: distinct users who took a qualifying action in a rolling 1-day (DAU), 7-day (WAU), or 30-day (MAU) period. DAU is best for daily-use products and launch monitoring, WAU smooths out weekday and weekend swings, and MAU measures overall reach and serves as the denominator for the stickiness ratio.


### How do I define an “active” user?


Pick the earliest action in a session that reliably shows the user got value from your product, and exclude anything a user can trigger without intent. For a messaging app that is “sent a message,” for a BI tool “ran a query” or “opened a dashboard.” Avoid loose definitions like “opened the app,” which count background refreshes and bounced sessions. Use the exact same definition for DAU, WAU, and MAU.


### What is a good DAU/MAU stickiness ratio?


There is no universal number. The old 10 to 20% benchmark is dated, and Facebook’s 50% bar applies to daily-communication products. Mixpanel’s 2026 data puts B2B SaaS stickiness around 31%, with wide variation by category. Judge your ratio against your product’s natural usage rhythm and, most importantly, against its own trend over time.


### Should I use rolling or calendar windows?


Rolling windows (the last 30 days from any point) give a smooth trend and are best for watching launches or daily health. Calendar periods (August MAU) are easier to explain to a board and align with billing, but they create a sawtooth that resets each month. Pick one, label it on the chart, and never compare a rolling number to a calendar number.


### Do I need a product analytics tool to track active users?


No. If your event data lives in a database or warehouse, you can calculate DAU, WAU, MAU, and stickiness with a few SQL queries and put them on a dashboard. A dedicated product analytics tool earns its place when your questions shift to behavioral paths, complex funnels, session replay, or per-event breakdowns, which those tools are purpose-built for.


### Why is my MAU lower than the sum of my daily active users?


Because users repeat. The same person active on ten days counts as ten daily active users across those days but as one monthly active user. You cannot add DAU over a month to get MAU. MAU will always be smaller than the summed daily counts and larger than any single day’s DAU.
