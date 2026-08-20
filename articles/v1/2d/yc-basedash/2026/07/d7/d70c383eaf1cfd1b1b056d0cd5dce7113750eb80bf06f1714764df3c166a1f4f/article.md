---
schema_version: "1.0.0"
document_id: "d70c383eaf1cfd1b1b056d0cd5dce7113750eb80bf06f1714764df3c166a1f4f"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-to-build-a-customer-health-score-signals-weighting-and-sql/"
published_at: "2026-07-10T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T21:08:37.946927+00:00"
content_hash: "sha256:66b9fda8393c7c0a995ef792bdceb85d30229e78ca0e484abf46bf06ab6448d5"
---

# How to build a customer health score: signals, weighting, and SQL

A customer health score is a single composite number, usually on a 0 to 100 scale, that estimates how likely an account is to renew and expand rather than churn. It combines signals from product usage, feature adoption, support activity, relationship strength, and billing into one figure your customer success and RevOps teams can sort, filter, and act on. This guide covers which signals to include, how to weight them, how to calculate the score in SQL, how to set thresholds, and how to check that the score actually predicts churn.


It is written for CS ops and RevOps leads, heads of customer success, and founders at SaaS companies who want a health score they can trust without buying a heavyweight customer success platform first.


## TL;DR


- A health score is a weighted composite of leading signals, not a single metric. Login count alone is not a health score.
- Use five signal categories: product usage, breadth and depth of adoption, support health, relationship and sentiment, and commercial signals. Weight them, do not average them blindly.
- Start with a transparent weighted model you can explain to a CSM in one sentence. Reach for a machine-learning model only after you have enough churn events to train on.
- Calculate the score per account per period so you can trend it. A static snapshot hides the decline that actually predicts churn.
- Set three tiers (healthy, at risk, critical) and tie each to a playbook. A score with no action attached is a vanity metric.
- Validate the score against real churn before you trust it. If churned accounts were mostly green last quarter, the model is wrong.


## What a customer health score is and is not


A health score is a prediction, expressed as a number, of whether an account will stay and grow. Its job is to help a CSM decide where to spend the next hour, and to help leadership forecast retention. It is not a report of what already happened, and it is not a churn model in the statistical sense unless you have deliberately built one.


Three failure modes are worth naming up front:


- **The proxy trap.** Teams pick one convenient signal, usually logins or seats, and call it health. Logins measure habit, not value. An account can log in daily and still churn because it never reached the outcome it bought the product for.
- **The averaging trap.** Combining signals by simple average lets a strong usage number mask a critical red flag. An account with great usage but an open executive escalation and a renewal in 30 days is not healthy, and an average will say it is.
- **The vanity trap.** A score that nobody acts on is decoration. If the number does not change what a CSM does this week, it is not earning its place.


The distinction from a[customer success dashboard](https://www.basedash.com/blog/how-to-build-a-customer-success-dashboard-a-practical-guide) matters here. The dashboard is the operating view for the whole book of business. The health score is one column inside it, and this post is about how to construct that column well.


## The five signal categories


A good score pulls from several independent categories so no single behavior dominates. Independence is the point: usage and support tickets tell you different things, and you want both.


Category Example signals What it tells you Leading or lagging


Product usage Weekly active users, sessions, core-action frequency, usage trend vs prior period Whether the product is part of the workflow Leading


Adoption breadth and depth Number of features used, seats activated vs licensed, key-workflow completion Whether they reached the value they bought Leading


Support health Ticket volume, unresolved tickets, severity, time to resolution, sentiment Friction and unmet expectations Mixed


Relationship and sentiment Executive sponsor engaged, last meaningful touch, NPS or CSAT, champion still employed Whether you have an advocate inside the account Mixed


Commercial Payment failures, plan downgrades, contract value trend, days to renewal Financial and contractual risk Lagging


The most predictive category for most SaaS products is usage trend, not usage level. A high-usage account whose usage dropped 40 percent this month is often a bigger risk than a steady low-usage account. Build trend into the signals, not just point-in-time counts.


## How to weight the signals


Weighting is where judgment enters. There is no universal weighting because the signals that predict churn depend on your product and your customers. A defensible starting point for a B2B SaaS product looks like this:


Category Starting weight


Product usage (including trend) 35%


Adoption breadth and depth 25%


Support health 15%


Relationship and sentiment 15%


Commercial 10%


Two rules make weighted scores work in practice.


First, **normalize each signal to a 0 to 100 subscore before weighting.** Raw counts are not comparable, and one large number will otherwise swamp the rest. Percentile rank within a segment, or a capped ratio against a target, both work.


Second, **add hard overrides for critical events.** Some signals should cap the score regardless of the weighted total. An open severity-1 escalation, a churned champion, or a failed payment inside the renewal window should force the account into the critical tier even if usage looks fine. This solves the averaging trap without abandoning the weighted model.


## Calculating the score in SQL


Assemble the score in three steps: aggregate raw signals per account per period, normalize each into a subscore, then combine with weights and overrides. The example below uses a monthly grain and a Postgres-style syntax.


Step one, aggregate the raw signals from your usage, support, and billing tables:


```text
with   usage   as   (
select
account_id,
date_trunc(  'month'  , event_at)   as   month  ,
count  (  distinct   user_id)   as   active_users,
count  (  *  )   as   core_actions
from   product_events
where   event_type   in   (  'report_run'  ,   'dashboard_view'  ,   'query_executed'  )
group by   1  ,   2
),
support   as   (
select
account_id,
date_trunc(  'month'  , created_at)   as   month  ,
count  (  *  )   filter   (  where   status   <>   'closed'  )   as   open_tickets,
count  (  *  )   filter   (  where   severity   =   'high'  )   as   high_sev_tickets
from   support_tickets
group by   1  ,   2
),
billing   as   (
select
account_id,
date_trunc(  'month'  , as_of)   as   month  ,
seats_active,
seats_licensed,
has_failed_payment,
days_to_renewal
from   account_billing_snapshot
)
```


Step two, turn raw signals into 0 to 100 subscores. Here usage trend compares this month to last month, and adoption uses activated seats against licensed seats:


```text
scored   as   (
select
u  .  account_id  ,
u  .  month  ,
-- usage subscore: reward active users, cap at a target
least  (  100  , (  u  .  active_users  ::  numeric   /   20  )   *   100  )   as   usage_score,
-- adoption subscore: activated seats vs licensed
least  (  100  , (  b  .  seats_active  ::  numeric
/   nullif  (  b  .  seats_licensed  ,   0  ))   *   100  )   as   adoption_score,
-- support subscore: fewer open and high-severity tickets is better
greatest  (  0  ,   100   -   (  s  .  open_tickets   *   10  )
-   (  s  .  high_sev_tickets   *   25  ))   as   support_score,
b  .  has_failed_payment  ,
b  .  days_to_renewal  ,
s  .  high_sev_tickets
from   usage u
left join   support s   on   s  .  account_id   =   u  .  account_id   and   s  .  month   =   u  .  month
left join   billing b   on   b  .  account_id   =   u  .  account_id   and   b  .  month   =   u  .  month
)
```


Step three, combine with weights and apply the critical overrides:


```text
select
account_id,
month  ,
case
when   has_failed_payment   and   days_to_renewal   <=   30   then   20
when   high_sev_tickets   >=   1   and   days_to_renewal   <=   30   then   30
else   round  (
usage_score      *   0  .  35   +
adoption_score   *   0  .  25   +
support_score    *   0  .  15   +
coalesce  (relationship_score,   60  )   *   0  .  15   +
coalesce  (commercial_score,   70  )     *   0  .  10
)
end   as   health_score
from   scored;
```


Relationship and commercial subscores often live in your CRM rather than your database, so pull them in through a join or default them to a neutral value until you wire the source up. Calculating this per month, rather than once, is what lets you chart the score over time and catch decline early.


## Points-based, weighted, or machine learning


Three modeling approaches exist, and the right one depends on how much data and how many past churn events you have.


Approach How it works Use it when Watch out for


Points-based rules Add or subtract fixed points for defined events You are starting out and want something explainable today Gets brittle as rules pile up


Weighted composite Normalize signals, apply category weights, add overrides You want a transparent, tunable score for most SaaS teams Requires you to pick and defend weights


Machine learning Train a model on historical churn to learn weights You have hundreds of churn events and a stable product Hard to explain to CSMs, easy to overfit small data


Most teams should start with a weighted composite. It is transparent enough that a CSM trusts it, and flexible enough to tune as you learn. Move to machine learning only once you have enough labeled churn events to train on and a reason to believe learned weights beat your hand-tuned ones. A model nobody can explain will not get used, no matter how accurate it is on paper.


## Setting thresholds and tying them to action


A score is only useful once it maps to a tier and each tier maps to a play. Three tiers are enough for most teams.


Tier Score range What it means Default play


Healthy 70 to 100 On track to renew and possibly expand Watch for expansion signals, keep light touch


At risk 40 to 69 Slipping or under-adopted CSM outreach, adoption plan, root-cause the drop


Critical 0 to 39 Likely to churn without intervention Escalate, involve leadership, save plan with a deadline


The exact cutoffs matter less than consistency and the fact that each tier triggers a specific play. Decide the thresholds by looking at where your historical churn actually clusters, not by splitting the range into even thirds. If most churned accounts scored between 50 and 60 before leaving, your at-risk band needs to start higher.


## Validating the score against real churn


Before you let the score drive renewals forecasting, check that it predicts churn. Two checks are enough to start.


First, a backward look: pull the health score every churned account had 90 days before it left. If most of them were green, the model missed the signals that mattered, and you need to reweight or add signals. Second, a forward look: once the score has run for a quarter, compare churn rates across tiers. A working score shows critical accounts churning at several times the rate of healthy ones. If the tiers churn at similar rates, the score is not separating risk.


Recalibrate on a regular cadence, at least quarterly. Products change, customer expectations change, and weights that predicted churn last year drift. A health score is a living model, not a formula you set once.


## Common mistakes


- **Using one signal.** Logins or seats alone miss the account that logs in daily but never got value.
- **Averaging instead of weighting.** A simple average lets a strong number hide a red flag. Weight, and add overrides.
- **Ignoring trend.** A point-in-time count hides the decline that predicts churn. Compare each period to the last.
- **No action attached.** A score with no playbook is decoration. Tie every tier to a play.
- **Never recalibrating.** Weights that worked last year drift. Revisit quarterly against real churn.
- **Scoring everyone the same.** A self-serve account and a six-figure enterprise account need different signals and thresholds. Segment first.


## Where to build a health score


You have three broad options, and the right one depends on how much workflow you need around the number.


- **CS platforms** such as Gainsight, Vitally, ChurnZero, and Catalyst include health scoring plus playbooks, renewal management, and CSM workflows out of the box. They are the heaviest option and the fastest to a full CS motion, but they are a significant purchase and often assume your data is already flowing into them.
- **BI and internal-tool platforms** such as Basedash, Looker, and Metabase let you build the score directly on your production database or warehouse with SQL, chart it, and share it. They are lighter and more flexible, and they keep the score next to the raw data, but you build the workflow layer yourself.[Basedash](https://www.basedash.com/) fits teams that want to define the score in SQL against live data and let non-technical CSMs explore accounts and ask follow-up questions without waiting on engineering.
- **Spreadsheets** work for a first version with a handful of accounts, but they break down once the data needs to refresh automatically or more than one person maintains them.


Match the tool to the stage. Early teams often build the first version in a BI tool against their existing database, then graduate to a CS platform when the renewal and playbook workflow outgrows a dashboard. Whichever you pick, the modeling work in this guide is the same, and it is the part that determines whether the score is worth trusting.


For the surrounding context, see how a health score fits into a[customer success dashboard](https://www.basedash.com/blog/how-to-build-a-customer-success-dashboard-a-practical-guide) , how it connects to[retention and churn analytics tools](https://www.basedash.com/blog/best-customer-analytics-tools-for-retention-and-churn-2026) , and how to keep its definition consistent by treating it as one node in your[metric tree](https://www.basedash.com/blog/how-to-design-a-metric-tree-a-practical-framework-for-saas-analytics) .


## FAQ


### What is a good customer health score model for SaaS?


A weighted composite that combines product usage and trend, adoption breadth, support health, relationship strength, and commercial signals, normalized to a 0 to 100 scale with hard overrides for critical events. Start weighted and transparent rather than reaching for machine learning, because CSMs need to understand and trust the number for it to change their behavior.


### How many signals should a health score include?


Enough to cover the five categories without becoming unexplainable, usually five to ten normalized signals. Adding more rarely improves prediction and makes the score harder to reason about. Pick the two or three signals per category that most reliably separate accounts that renew from accounts that churn, and drop the rest.


### How is a health score different from churn prediction?


A health score is a transparent, explainable number that a CSM acts on today, built from signals you choose and weight. A churn prediction model is a statistical model trained on historical churn that outputs a probability. They can overlap, but a health score prioritizes clarity and action, while a churn model prioritizes predictive accuracy and often trades away explainability.


### How often should you recalculate a customer health score?


Recalculate the score itself frequently, daily or weekly, so CSMs see current risk. Recalibrate the weights and thresholds less often, at least quarterly, by checking the score against accounts that actually churned. Frequent scoring keeps the number current; periodic recalibration keeps the model accurate as your product and customers change.


### Can you build a health score without a customer success platform?


Yes. You can build one in a BI or internal-tool platform directly on your production database or warehouse using SQL, as shown above, then chart it and share it with CSMs. A dedicated CS platform adds playbooks and renewal workflows, but the scoring model is the same either way, and many teams build their first version in a BI tool before buying a heavier platform.
