---
schema_version: "1.0.0"
document_id: "49e765250823fa291cbc851dcd5e8413ffa552f869bdccc6947c23d9613abcc8"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/customer-segmentation-how-to-build-segments-from-your-data/"
published_at: "2026-07-03T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T20:47:34.280666+00:00"
content_hash: "sha256:44bb1df2bb772817ab5e877e142f963e0a0d858492723cbeb00cebb96d356dac"
---

# Customer segmentation: how to build segments from your data

Customer segmentation is the practice of grouping customers into sets that share meaningful traits, behavior, or value, so you can treat each group differently. Done well, it turns one undifferentiated customer list into a handful of groups you can act on: who to upsell, who is about to churn, who deserves white-glove onboarding, and who to leave alone.


This guide is for analysts, founders, and operators who want to build segments from real data in a warehouse or BI tool, not personas drawn on a whiteboard. It covers the segmentation models that matter, how to build RFM and behavioral segments in SQL, how to decide which model fits your business, and how to keep segments from decaying into dead slides.


## Data-driven segmentation vs marketing personas


Most articles about segmentation are about marketing personas: fictional characters like “Enterprise Emma” invented in a workshop, useful for positioning and ads. That kind of segmentation lives in a deck.


Data-driven customer segmentation is different. It groups your actual customers using columns in your database and events in your product. A segment is defined as a query, not a paragraph, so it can be rebuilt every night, counted, and wired into dashboards and workflows. If your goal is to decide who gets an outreach email, which accounts your customer success team calls, or which cohort to test a price change on, you need this second kind.


The distinction matters because a persona describes a type of person, while a data-driven segment is a live list of specific customers who currently match a rule. The list changes as customers behave differently, and that is the point.


## The main customer segmentation models


There is no single correct way to segment customers. The right model depends on the decision you are trying to make and the data you actually have. These are the models worth knowing.


Model Groups customers by Data you need Best for Watch out for


Demographic / firmographic Fixed attributes (age, region, company size, industry, plan) Signup and CRM fields Fast slicing, territory planning, pricing tiers Attributes rarely predict behavior on their own


Behavioral What customers do (features used, actions taken, frequency) Product event data Activation, engagement, feature adoption Needs clean event tracking


RFM Recency, frequency, and monetary value of purchases Order or transaction history Ecommerce, retail, transactional revenue Assumes repeat purchases exist


Lifecycle / stage Where a customer is in their journey (trial, new, active, at risk, churned) Activity and subscription status Onboarding, retention, win-back campaigns Stage definitions must be maintained


Value-based Contribution to revenue or margin (LTV tier, ARR band) Revenue and cost data Prioritizing support and sales effort Top revenue is not always top margin


Needs-based The job the customer hired you for Surveys, usage patterns, sales notes Product strategy and positioning Hardest to derive from data alone


Most teams end up combining two or three. A common pairing is a lifecycle stage crossed with a value tier: a “high-value account that is now at risk” is a far more actionable segment than either dimension alone.


## RFM segmentation in SQL


RFM (recency, frequency, monetary) is the most durable segmentation model for any business with repeat transactions. It comes from decades of direct and database marketing practice and holds up because it uses only three facts every transactional business already has: when someone last bought, how often they buy, and how much they spend.


The trick is to score each customer relative to everyone else rather than against fixed thresholds.` NTILE(5)` splits customers into five equal buckets per dimension, so the scoring adjusts automatically as your customer base grows. Here is a full pattern on Postgres:


```text
with   customer_metrics   as   (
select
customer_id,
max  (order_date)                as   last_order_date,
count  (  *  )                       as   frequency,
sum  (order_total)               as   monetary
from   orders
where   order_date   >=   current_date   -   interval   '365 days'
group by   customer_id
),
scored   as   (
select
customer_id,
current_date   -   last_order_date   as   recency_days,
frequency,
monetary,
ntile  (  5  )   over   (  order by   last_order_date)   as   r_score,
ntile  (  5  )   over   (  order by   frequency)         as   f_score,
ntile  (  5  )   over   (  order by   monetary)          as   m_score
from   customer_metrics
)
select
customer_id,
r_score,
f_score,
m_score,
case
when   r_score   >=   4   and   f_score   >=   4   then   'Champions'
when   r_score   >=   3   and   f_score   >=   3   then   'Loyal'
when   r_score   >=   4   and   f_score   <=   2   then   'New / promising'
when   r_score   <=   2   and   f_score   >=   3   then   'At risk'
when   r_score   <=   2   and   f_score   <=   2   then   'Hibernating'
else   'Needs attention'
end   as   segment
from   scored;
```


Two details matter. Ordering` NTILE` by` last_order_date` ascending gives the most recent buyers the highest recency score, which is what you want. And the` case` statement is where business judgment lives: the cutoffs and labels should reflect how your team plans to act, not a textbook. Start with five or six named segments, not twenty-five RFM cells.


The full window-function reference, including` NTILE` and` PERCENT_RANK` , is in the[PostgreSQL documentation](https://www.postgresql.org/docs/current/functions-window.html) . The same pattern works on Snowflake, BigQuery, and Redshift with minor syntax changes.


## Behavioral segmentation from product events


For software products, what a customer does inside the product predicts retention far better than who they are. Behavioral segmentation groups customers by the actions they take, usually over a recent window so the segment reflects current engagement rather than a one-time burst at signup.


```text
select
u  .  user_id  ,
case
when   e  .  core_actions   >=   20   and   e  .  active_weeks   >=   4   then   'Power user'
when   e  .  core_actions   >=   5                            then   'Engaged'
when   e  .  core_actions   >=   1                            then   'Trialing'
else   'Dormant'
end   as   behavior_segment
from   users u
left join   (
select
user_id,
count  (  *  )   filter   (  where   event_name   =   'core_action'  )        as   core_actions,
count  (  distinct   date_trunc(  'week'  , event_time))            as   active_weeks
from   events
where   event_time   >=   current_date   -   interval   '30 days'
group by   user_id
) e   on   e  .  user_id   =   u  .  user_id  ;
```


The important design choice is picking the right` core_action` . It should be the event that correlates with real value, such as sending an invoice, running a query, or publishing a report, not a vanity action like a page view. If you have already done[funnel analysis](https://www.basedash.com/blog/how-to-build-a-funnel-analysis-dashboard-sql-patterns-and-common-mistakes) , you probably already know which activation event matters.


## How to choose a segmentation model


Do not start from the model. Start from the decision you want the segment to drive, then pick the smallest model that supports it.


- **If you sell repeat purchases** (ecommerce, retail, consumer subscriptions), start with RFM. It is cheap to build and immediately actionable for lifecycle marketing.
- **If you run a software product** , start with behavioral segmentation tied to your activation event, then layer in a value tier once revenue data is clean.
- **If you sell to businesses** , combine firmographics (company size, industry) with account-level behavior. A 5-seat account using the product daily is a different animal from a 500-seat account that logged in once.
- **If you are setting product strategy** , needs-based segmentation is worth the extra effort, but expect to combine survey data with usage rather than deriving it from SQL alone.


A useful rule: if you cannot name the action a team will take for each segment, the segment is not ready. “Champions” should map to “ask for a referral or review.” “At risk” should map to “trigger a save play.” A segment with no owner and no action is a chart, not a tool.


## Making segments operational, not one-time


The most common failure mode is not choosing the wrong model. It is building segments once, presenting them, and letting them rot. Customers move between segments constantly, so a snapshot taken in a workshop is stale within weeks.


To keep segments useful, treat the definition as living infrastructure:


1. **Define the segment as a query or view** , not a manual export. The definition should live in version control or your BI tool’s metric layer so everyone shares one source of truth.
2. **Refresh on a schedule** so the segment reflects current behavior. Recency-based segments in particular are meaningless if they are a month old.
3. **Expose it in a dashboard** with segment as a filter, so anyone can see how each group’s revenue, retention, or usage is trending. Building this alongside your[SaaS revenue dashboard](https://www.basedash.com/blog/how-to-build-a-saas-revenue-dashboard-metrics-data-sources-and-structure) keeps segmentation next to the numbers it should influence.
4. **Wire it into workflows.** Push the “at risk” list to your customer success team, sync the “champions” to a referral campaign, or trigger in-app messaging. This is where[operational analytics](https://www.basedash.com/blog/what-is-operational-analytics-how-real-time-data-drives-better-decisions) turns a segment from a report into an action.


Tools like Basedash make this loop shorter because the segment query, the dashboard that visualizes it, and the people who need it all live in one place, and you can ask for a segment in plain English and refine the SQL from there. The principle holds in any stack, though: a segment is only as good as the workflow attached to it.


## Segmentation vs cohort analysis


These get confused because both split customers into groups, but they answer different questions.


Segmentation groups customers by traits or behavior that can change. A customer can be a “power user” this month and “dormant” next month, and moving between segments is exactly the signal you want.


[Cohort analysis](https://www.basedash.com/blog/how-to-do-cohort-analysis-retention-revenue-and-behavioral-cohorts) groups customers by a fixed starting point, usually their signup month, and never lets them switch. The point is to compare how the January group ages versus the February group.


Use segmentation to decide who to act on now. Use cohort analysis to see whether the product is getting better over time. They complement each other: you can run retention curves within each segment to see which groups are worth the most effort to keep.


## Common customer segmentation mistakes


- **Too many segments.** If a person cannot hold the list in their head, no team will act on it. Five to eight named segments is usually the ceiling.
- **Segmenting on things you cannot act on.** Grouping by browser type is interesting trivia. Grouping by “trialing but hit an error in setup” is a to-do list.
- **Freezing segments in time.** A segment built once and never refreshed describes the past, not the present.
- **Reaching for clustering too early.** Unsupervised machine learning can find segments, but if nobody can explain why a customer landed in cluster 3, nobody will trust it. Rule-based segments you can read in SQL beat a black box for most teams.
- **Confusing revenue with value.** Your biggest-spending customers may also be your most expensive to support. Pair a value tier with a cost or margin view before you decide who gets premium treatment.
- **No owner.** Every segment should have a team responsible for the action it implies. Otherwise it is a slide, not a system.


## FAQ


### What is customer segmentation?


Customer segmentation is the practice of dividing customers into groups that share meaningful traits, behavior, or value so each group can be treated differently. In an analytics context it means defining each group as a query over your customer and product data, then using those groups to guide marketing, sales, support, and product decisions.


### What is the difference between customer segmentation and market segmentation?


Market segmentation divides a whole addressable market into broad groups for positioning and go-to-market planning, often before those people are customers. Customer segmentation works on people who are already customers, using real account and behavioral data to decide how to serve or grow each group.


### What is RFM segmentation?


RFM segmentation scores each customer on how recently they bought (recency), how often they buy (frequency), and how much they spend (monetary), then groups them by those scores. It is popular for ecommerce and retail because it needs only transaction history and reliably surfaces best customers, at-risk customers, and lapsed ones.


### How many customer segments should you have?


Enough to change your behavior, but few enough to remember. Most teams are well served by five to eight named segments. Beyond that, the segments tend to overlap, the differences stop mattering, and no team can act on all of them consistently.


### Do you need machine learning for customer segmentation?


Usually not. Rule-based segmentation in SQL, such as RFM scoring or activation thresholds, is transparent, easy to maintain, and easy for teams to trust. Clustering algorithms can be useful once you have a mature data practice and a specific question, but they are rarely the right starting point.
