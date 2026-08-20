---
schema_version: "1.0.0"
document_id: "93b4ecc16d8ae845f5f4ae2c4f8d2d86d9e35aa7bb17b14b924e53291055aab5"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-to-build-a-funnel-analysis-dashboard-sql-patterns-and-common-mistakes/"
published_at: "2026-05-28T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T21:11:58.236396+00:00"
content_hash: "sha256:f97f1e13dd773944ccfb9e413032652c4f33fa8f524eff52ccf6ea0d2c1343de"
---

# How to build a funnel analysis dashboard: SQL patterns, layout, and common mistakes

A funnel analysis dashboard answers one question: where are people dropping off, and how much does each drop cost? Built well, it tells a product team which step to fix this week and a growth team where a paid campaign is leaking spend. Built poorly, it produces conversion rates that move randomly because the underlying SQL is wrong.


This guide walks through how to build a funnel analysis dashboard end to end. It covers the metrics that actually matter, the SQL patterns for computing strict and open funnels on event data in Postgres, BigQuery, Snowflake, or any modern warehouse, the layout that makes a funnel readable in a glance, and the mistakes that make most homegrown funnel dashboards untrustworthy. It is aimed at product managers, growth leads, founders, and analysts building the first version of a signup, activation, checkout, or onboarding funnel for a SaaS or consumer product.


## TL;DR


- A funnel dashboard exists to expose drop-off, not to celebrate conversion. Lead with the smallest step-to-step rates, not the headline overall conversion number.
- Pick one of three funnel definitions and stick with it: strict ordered, ordered with a time window, or open (any order within a window). Mixing definitions across charts is the most common reason funnels look unstable.
- For SQL, the cleanest pattern is a series of self-joins or window functions on an` events` table keyed by` user_id` ,` event_name` , and` event_time` . Use` MIN(event_time) FILTER (WHERE ...)` per user to compute step timestamps, then count users who reached each step.
- Always segment by cohort and source. A funnel chart without time and acquisition cuts hides every interesting signal.
- The reference layout has four sections: a top-line scorecard, the headline funnel chart, a step-by-step drop-off table, and a segmented view by cohort and source. Add a small “recent activity” panel if your team uses the dashboard for live debugging.
- Common mistakes include counting events instead of unique users, ignoring time windows, including internal traffic, and conflating top-of-funnel volume with conversion rate. Most “the funnel changed” alerts trace back to one of these.
- Build it in a BI tool that can run SQL against your warehouse or production database directly.[Basedash](https://www.basedash.com/) , Metabase, Hex, Mode, and Looker all work well; Amplitude and Mixpanel are easier if you do not have a data team yet.


## What a funnel analysis dashboard should actually do


A funnel dashboard has one job: make drop-off impossible to miss. Everything else is supporting detail.


In practice, that means the dashboard has to answer four questions on the same page:


1. **What is the conversion rate of each step right now** , and how does it compare to last week and last month?
2. **Which step has the worst drop-off** , in both absolute users lost and percentage terms?
3. **Which segments convert differently** by acquisition source, plan, geography, or signup cohort?
4. **Has anything changed recently** that explains a swing in the headline number?


A funnel that only shows the overall conversion rate is a vanity metric. A funnel that buries drop-off in a stacked bar chart hides the signal. The structure below is designed so the team can scan it in thirty seconds and know where to dig.


## The metrics that belong on a funnel dashboard


Resist the temptation to add every product metric. A focused funnel dashboard tracks five things.


### Step counts


The number of unique users who reached each step in the chosen window. Count distinct users, not events. A user who clicks “Sign up” five times still counts once at that step.


### Step-to-step conversion rate


For each pair of adjacent steps, the percentage of users at step N who also reached step N+1 within the time window. This is the single most useful number on the page. The smallest step-to-step rate is almost always where the next product fix lives.


### Overall conversion rate


The percentage of users at step 1 who reached the final step. Useful as a headline KPI, but easy to misread on its own. A 4% improvement in overall conversion could come from one step, several steps, or a change in the mix of acquisition sources.


### Time to convert


The median (or 90th percentile) time between the first step and the final step, for users who converted. A growing time-to-convert with a flat conversion rate often signals an onboarding friction that has not yet shown up in drop-off.


### Drop-off by segment


The same conversion rates, broken down by acquisition source, plan tier, country, device, or signup cohort. A flat top-line funnel can hide a meaningful drop in one segment that is being offset by growth in another.


## Three funnel definitions, and why the choice matters


Before writing any SQL, choose a funnel definition and apply it consistently across every chart on the dashboard. Mixing definitions silently makes numbers disagree.


**Strict ordered funnel.** Step N+1 only counts if it happens after step N for the same user, with no other steps in between. This is the most restrictive definition and matches a true linear flow like checkout. It is the right choice for purchase, payment, or signup flows where order matters.


**Ordered funnel with a time window.** Step N+1 counts if it happens after step N for the same user, within a fixed window (24 hours, 7 days, 30 days). Other events in between are allowed. This is the most common choice for product activation, onboarding, and trial funnels where users wander between steps.


**Open funnel.** Each step counts if it happens at all within the window, regardless of order. This is the right choice for engagement funnels where a user might do step 3 before step 2 (for example, “viewed pricing” before “viewed product page”).


Pick one, document it on the dashboard, and use the same window across charts. A common error is showing a 7-day funnel at the top of the page and a same-session funnel in a segmented chart below, then wondering why the totals do not match.


## SQL patterns for funnel queries


The cleanest pattern is to start from an` events` table with three columns:` user_id` ,` event_name` , and` event_time` . Most product analytics setups already produce this, either from Segment, Snowplow, RudderStack, a homegrown event tracker, or app database triggers. If your data lives across separate tables (` users` ,` signups` ,` subscriptions` ),` UNION` them into a single virtual events view first. The funnel SQL becomes much simpler.


### Ordered funnel with a 7-day window


This is the most common pattern. For each user, compute the timestamp of the first occurrence of each step, then check that each step happened after the previous one within the window.


```text
WITH   user_steps   AS   (
SELECT
user_id,
MIN  (event_time)   FILTER   (  WHERE   event_name   =   'signup'  )             AS   step_1,
MIN  (event_time)   FILTER   (  WHERE   event_name   =   'created_workspace'  )   AS   step_2,
MIN  (event_time)   FILTER   (  WHERE   event_name   =   'invited_teammate'  )   AS   step_3,
MIN  (event_time)   FILTER   (  WHERE   event_name   =   'completed_setup'  )    AS   step_4
FROM   events
WHERE   event_time   >=   CURRENT_DATE   -   INTERVAL   '30 days'
GROUP BY   user_id
)
SELECT
COUNT  (  *  )   FILTER   (  WHERE   step_1   IS NOT NULL  )   AS   reached_step_1,
COUNT  (  *  )   FILTER   (
WHERE   step_1   IS NOT NULL
AND   step_2   >   step_1
AND   step_2   <=   step_1   +   INTERVAL   '7 days'
)   AS   reached_step_2,
COUNT  (  *  )   FILTER   (
WHERE   step_2   >   step_1
AND   step_3   >   step_2
AND   step_3   <=   step_1   +   INTERVAL   '7 days'
)   AS   reached_step_3,
COUNT  (  *  )   FILTER   (
WHERE   step_2   >   step_1
AND   step_3   >   step_2
AND   step_4   >   step_3
AND   step_4   <=   step_1   +   INTERVAL   '7 days'
)   AS   reached_step_4
FROM   user_steps;
```


This pattern works in Postgres, BigQuery (drop` INTERVAL` syntax for` TIMESTAMP_ADD` ), Snowflake (use` DATEADD` ), and Redshift with minor syntax tweaks. The` MIN(event_time) FILTER (WHERE ...)` idiom is the workhorse. It collapses each user to one row with one timestamp per step, after which the funnel is a single` SELECT` away.


### Step-to-step conversion rates


Pivot the row of counts above into a tall format with step number, user count, and conversion rate from the prior step.


```text
WITH   counts   AS   (
-- the SELECT above
)
SELECT   step, users,   ROUND  (  100  .  0   *   users   /   LAG  (users)   OVER   (  ORDER BY   step),   2  )   AS   conversion_pct
FROM   (
SELECT   1   AS   step, reached_step_1   AS   users   FROM   counts   UNION ALL
SELECT   2  , reached_step_2   FROM   counts   UNION ALL
SELECT   3  , reached_step_3   FROM   counts   UNION ALL
SELECT   4  , reached_step_4   FROM   counts
) t
ORDER BY   step;
```


This produces the table your BI tool will turn into the headline funnel chart.


### Segmented funnel


Add segmentation by joining` user_steps` to a` users` table on the relevant attribute (acquisition source, plan, country) and grouping by it.


```text
SELECT
u  .  acquisition_source  ,
COUNT  (  *  )   FILTER   (  WHERE   us  .  step_1   IS NOT NULL  )   AS   reached_step_1,
COUNT  (  *  )   FILTER   (  WHERE   us  .  step_4   IS NOT NULL   AND   us  .  step_4   <=   us  .  step_1   +   INTERVAL   '7 days'  )   AS   reached_step_4,
ROUND  (
100  .  0   *   COUNT  (  *  )   FILTER   (  WHERE   us  .  step_4   IS NOT NULL   AND   us  .  step_4   <=   us  .  step_1   +   INTERVAL   '7 days'  )
/   NULLIF  (  COUNT  (  *  )   FILTER   (  WHERE   us  .  step_1   IS NOT NULL  ),   0  ),
2
)   AS   overall_conversion_pct
FROM   user_steps us
JOIN   users u   ON   u  .  id   =   us  .  user_id
GROUP BY   u  .  acquisition_source
ORDER BY   reached_step_1   DESC  ;
```


For dashboards, parametrize the segmentation column so a single chart can switch between source, plan, and country without rewriting the query.


### Time to convert


Use` EXTRACT(EPOCH FROM ...)` or your warehouse’s date-diff function on the difference between the first and last step.


```text
SELECT
PERCENTILE_CONT  (  0  .  5  )   WITHIN   GROUP   (  ORDER BY   EXTRACT(EPOCH   FROM   (step_4   -   step_1)))   AS   median_seconds_to_convert,
PERCENTILE_CONT  (  0  .  9  )   WITHIN   GROUP   (  ORDER BY   EXTRACT(EPOCH   FROM   (step_4   -   step_1)))   AS   p90_seconds_to_convert
FROM   user_steps
WHERE   step_4   IS NOT NULL  ;
```


Plot this over weekly cohorts. A rising p90 with a flat median is often the earliest signal that a small group of users is hitting an onboarding wall.


## Where the data actually comes from


A funnel dashboard joins three kinds of data, and the join is where most of the work lives.


**Product event data.** Page views, button clicks, server-side events, and feature usage. Most teams use Segment, Snowplow, RudderStack, PostHog, or a homegrown tracker writing to the warehouse. The minimum schema is` user_id` ,` event_name` ,` event_time` , and a properties JSON column.


**User attributes.** Acquisition source, signup date, plan, country, company size. Pulled from the application database or CRM and joined on` user_id` . This is what powers segmentation.


**Acquisition or marketing data.** UTM parameters, ad platform IDs, and campaign metadata. Captured at signup time and stored on the user row. Most teams underinvest here and then cannot answer “which channel converts best” without a painful rebuild later.


If your events live in PostgreSQL alongside your app data, you can build the funnel directly there with a BI tool that connects natively, no warehouse required. For a primer on the tradeoff, see our guide on[BI tools for PostgreSQL](https://www.basedash.com/blog/best-bi-dashboarding-tools-for-postgresql-2026) . For larger event volumes, a warehouse like BigQuery or Snowflake is usually the right call.


## A reference structure for the dashboard


The layout below works for most product, growth, and checkout funnels. It is designed to be readable in thirty seconds and useful for an hour of debugging.


### Section 1: top-line scorecard


Four to six KPI cards across the top, with week-over-week and month-over-month deltas:


- Users entering the funnel in the window
- Users completing the funnel in the window
- Overall conversion rate
- Median time to convert
- The single worst step-to-step conversion rate (with the step label)


The “worst step” card is the most underrated tile on the page. It changes which step the team is looking at without requiring a chart.


### Section 2: the headline funnel chart


A horizontal bar chart with each step as a row, the bar length proportional to user count, and the step-to-step conversion rate labeled on each bar. Avoid the classic colored “funnel” visualization unless you have exactly four steps and want to use it for an external presentation. A bar chart is easier to read and easier to update when steps change.


Pair it with a small table showing absolute counts, percentage of step 1, and step-to-step rate. Some readers want the bars, others want the numbers. Show both.


### Section 3: drop-off over time


A line chart of step-to-step conversion rates by week, with one line per step transition. This is the chart that tells you whether a recent product change moved the funnel.


Use a 6 to 12 week window. Shorter windows are too noisy for weekly cohorts; longer windows hide product changes inside historical averages.


### Section 4: segmentation


Two or three small-multiple bar charts showing overall conversion rate by:


- Acquisition source or channel
- Plan tier, country, or device
- Signup cohort (weekly cohorts going back 8 to 12 weeks)


Cohort segmentation is the single most useful cut. A funnel that looks flat in aggregate often shows clear improvement or regression when split by signup week.


### Section 5: recent activity (optional)


A small table of users who entered the funnel in the last 24 hours, with the steps they have reached. Useful for product teams that want to spot-check the data and notice broken events quickly. Skip this if the dashboard is primarily for leadership review.


## Refresh cadence and freshness


Most funnel dashboards do not need to be real-time. Daily refresh is plenty for product, growth, and onboarding funnels. The exceptions:


- **Live checkout or payment funnels** during a launch or sale, where intra-hour refresh helps catch a broken payment integration before it costs serious revenue.
- **A/B test dashboards** built on top of a funnel, where the test platform usually drives its own freshness.


For most teams, a daily refresh at 6 AM local time, paired with an automated alert on a large drop in any step-to-step rate, is sufficient. The dashboard does not need to surface every change in real time; the alert does. For more on alerting design, see our piece on[AI-powered anomaly detection in BI](https://www.basedash.com/blog/ai-powered-anomaly-detection-in-bi-how-modern-tools-catch-metric-changes-automatically) .


## Common mistakes that quietly break funnel dashboards


The SQL is usually not the hard part. Keeping the numbers trustworthy over months of product changes is.


**Counting events instead of unique users.** A funnel that uses` COUNT(*)` over events will inflate every step. Always count distinct users (` COUNT(DISTINCT user_id)` ) or, better, use the per-user pivot pattern above so each user contributes one row per step at most.


**Ignoring the time window.** A user who signed up in 2023 and completed setup in 2024 should not appear in a 7-day funnel for the 2024 cohort. The` MIN(event_time) FILTER` pattern with a window check on` step_N > step_1 AND step_N <= step_1 + INTERVAL 'N days'` is the cleanest defense.


**Including internal traffic.** Employee signups, QA accounts, and load tests will distort small funnels. Filter them out at the user level, not the event level, so the exclusion is consistent across every chart.


**Conflating volume and conversion.** A 10% rise in signups with a flat conversion rate produces more activated users, but the funnel rate did not actually improve. Separate the two on the dashboard. Show entry volume and conversion rate as distinct KPIs.


**Renaming events without versioning.** Product teams rename` clicked_signup_btn` to` signup_initiated` and the funnel breaks silently. Add a column for the previous event name in your events pipeline, or maintain a small mapping table the funnel query uses, so renames do not orphan history.


**No segmentation by source.** An aggregate funnel hides the dynamics of paid versus organic versus referral. The same funnel can look flat overall while paid is regressing and organic is improving. Always include source segmentation on the dashboard.


**No owner.** A funnel without an owner becomes inaccurate within a quarter. Product launches, event renames, and new acquisition channels all change the inputs. Assign ownership (usually product analytics or growth ops) and review the dashboard monthly.


## When to use a BI tool and when to use a product analytics tool


There are two reasonable paths.


**Path 1: a product analytics tool.** Amplitude, Mixpanel, PostHog, and Heap have pre-built funnel charts. They are the fastest route if you do not have a data team and your events are already flowing into one of them. The tradeoff is segmentation limits, sampling at scale, and pricing that scales with event volume.


**Path 2: a BI tool on top of your warehouse or production database.** Tools like Basedash, Metabase, Hex, Mode, and Looker let you build a fully custom funnel dashboard using the SQL patterns above. This is the right path when you want to combine event data with billing or CRM data, segment by your own business logic, or audit every number against the underlying rows. It takes more setup but produces a dashboard that the data team owns and trusts.


For most teams between roughly $1M and $50M in ARR, the practical pattern is to use a product analytics tool for live exploration and a BI tool for the canonical funnel dashboard that goes into weekly business reviews. The two should agree on definitions; if they disagree, the BI version usually wins because it can be audited against raw data.


## Wrapping up


A good funnel dashboard is a small dashboard. It tracks one flow, defines its steps and time window precisely, counts unique users, segments by cohort and source, and lives where the team already looks for product and growth numbers. Everything else is decoration.


If you are building one from scratch, start with the strict-ordered SQL pattern, layer in cohort and source segmentation second, and add the time-to-convert and recent activity sections only when the team is actively asking for them. Most funnel dashboards that fail do so by being too ambitious in week one and too neglected in month three. A tight, well-owned funnel chart with honest numbers beats a sprawling one with mystery conversion rates every time.
