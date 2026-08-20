---
schema_version: "1.0.0"
document_id: "e43ff772bc25e3ef17e371c02e48f4845f08bcf11f9becf60fa7411630c4a328"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-to-build-an-okr-dashboard-that-tracks-against-live-data/"
published_at: "2026-08-19T00:00:00+00:00"
first_seen_at: "2026-08-19T13:49:58.433965+00:00"
fetched_at: "2026-08-19T13:50:04.829782+00:00"
content_hash: "sha256:53920f472c9137b731963d338696388f38a7d6ad40bd3ebca52dcadd2ea91112"
---

# How to build an OKR dashboard that tracks against live data

An OKR dashboard is a single view that shows every key result next to four numbers: where it started, where it needs to end, where it is now, and where it should be by this point in the quarter. The point is not to look busy. It is to answer one question at a glance: are we on pace to hit this, and if not, which key result is slipping? The best versions pull their current values straight from the systems that already hold the data, so the numbers move on their own instead of being retyped before every check-in.


This guide is for founders, operators, and analysts who run OKRs and are tired of the ritual of copying metrics into a slide the night before a review. It covers what separates a key result you can actually track from one you cannot, an original four-number model for scoring pace, how to wire each key result to a real data source, how to lay the dashboard out, the mistakes that quietly make it useless, and when a dashboard is the wrong tool for the job.


## OKRs vs KPIs: what belongs on the dashboard


OKRs and KPIs are related but not the same, and mixing them up is the fastest way to build a confusing dashboard. A KPI is a metric you watch continuously because it reflects the ongoing health of the business: monthly recurring revenue, gross margin, weekly active users. An objective is a time-boxed ambition (“make onboarding feel effortless”), and its key results are the specific, measurable outcomes that would prove you got there (“raise activation from 32% to 45% this quarter”).


The practical difference is that a KPI has no finish line and a key result does. A KPI dashboard tracks a number over time; an OKR dashboard tracks progress toward a target inside a window. Many key results are built on top of a KPI you already track, which is fine, but the dashboard should frame it as movement toward a goal, not just a line that drifts. If you want the deeper distinction, see our breakdown of[KPIs vs metrics](https://www.basedash.com/blog/kpi-vs-metric-whats-the-difference-and-how-to-decide-what-belongs-on-a-dashboard) .


## What makes a key result trackable


Not every key result can live on a dashboard, and forcing the ones that cannot is why so many OKR trackers rot. Before a key result earns a row, it should pass four tests:


- **It has a number, not an adjective.** “Improve reliability” is an objective. “Cut p95 API latency from 800ms to 400ms” is a key result. If you cannot state a start value and a target value, it is not measurable yet.
- **The data already exists somewhere.** The current value has to come from a query, an event stream, a billing system, or a CRM. If the only way to update it is to ask three people and take an average, it will go stale by week two.
- **It describes an outcome, not an activity.** Google’s[re:Work OKR guide](https://rework.withgoogle.com/en/guides/set-goals-with-okrs) makes this point directly: “publish customer satisfaction levels by March 7th” is a result, while “assess customer satisfaction” is just an activity dressed up as one.
- **It has a single owner.** Every row needs one name accountable for the number, even if a team does the work. Shared ownership on a dashboard means nobody notices when it slips.


A useful gut check: mix leading and lagging results within an objective so the dashboard warns you early instead of only confirming the miss at the end. If an objective is all lagging outcomes, you will not know you are behind until it is too late to act. Our guide to[leading vs lagging indicators](https://www.basedash.com/blog/leading-vs-lagging-indicators-how-to-build-dashboards-that-warn-you-early) covers how to pair them.


## The four numbers every key result needs


Most OKR trackers show two numbers, current and target, and a percentage. That percentage lies, because it ignores time. A key result at 60% attainment is great in week 11 and a fire in week 2. To make the dashboard honest, give every key result four numbers:


1. **Baseline** : the value when the quarter started. Without it, you cannot tell real progress from where you already were.
2. **Target** : the value that counts as done.
3. **Current** : the live value, pulled from the source system.
4. **Expected by now** : where a linear pace would put you today, given how much of the period has elapsed.


The expected value is the piece almost everyone skips, and it is the one that turns a status color from a guess into a calculation. Compute it as:


` expected = baseline + (target - baseline) * fraction_of_period_elapsed`


Then compare current against expected. If current is at or ahead of expected, the key result is on track. If it is meaningfully behind, it is at risk, regardless of how good the raw attainment percentage looks. Here is the model applied to a real-looking set of key results, seven weeks into a thirteen-week quarter (roughly 54% elapsed):


Key result Baseline Target Current Expected by now Status


Raise activation rate to 45% 32% 45% 41% 39% On track


Cut p95 latency to 400ms 800ms 400ms 640ms 585ms At risk


Grow paid seats to 5,000 3,800 5,000 4,150 4,450 Behind


Reach 90% CSAT 84% 90% 89% 87% On track


Notice that seats look fine at 4,150 in isolation, but against an expected 4,450 they are behind pace, and the dashboard flags it while there is still time to react. That is the entire value of the fourth number.


## How to connect each key result to a data source


The difference between a dashboard people trust and a spreadsheet people ignore is where the current value comes from. Every key result should map to exactly one query or metric definition, so there is no ambiguity about what “current” means. Work through your key results and tag each with its source:


- **Product and usage results** (activation, active users, feature adoption) come from your product database, warehouse, or event pipeline.
- **Revenue results** (paid seats, expansion, MRR) come from your billing system or a Stripe-connected table.
- **Customer results** (CSAT, NPS, ticket resolution time) come from your support or survey tool, often synced into the warehouse.
- **Engineering results** (latency, uptime, error rate) come from your observability or logs backend.


For anything that lives in a database, the current value is just a saved query. A latency key result, for example, reduces to a single number your dashboard can refresh on a schedule. You can even push the pace logic into SQL so the status is computed, not eyeballed:


```text
with   kr   as   (
select
0  .  32   as   baseline,
0  .  45   as   target  ,
(  select   activated::  float   /   nullif  (signups,   0  )
from   activation_this_quarter)   as   current_value,
extract(  day   from   now  ()   -   date   '2026-07-01'  )
/   extract(  day   from   date   '2026-09-30'   -   date   '2026-07-01'  )   as   elapsed
)
select
round  (  100   *   (current_value   -   baseline)   /   (  target   -   baseline))   as   attainment_pct,
round  (  100   *   (baseline   +   (  target   -   baseline)   *   elapsed),   1  )       as   expected_pct,
case
when   current_value   >=   baseline   +   (  target   -   baseline)   *   elapsed   then   'on track'
else   'behind'
end   as   status
from   kr;
```


Key results that cannot be reduced to a query (“close the Acme deal”) are still valid OKRs, but they belong in a checklist or your OKR process tool, not on the data dashboard. Trying to force them in is what leads to manual updates, and manual updates are what kill the whole thing.


## How to lay out an OKR dashboard


Structure the dashboard the way OKRs are structured: objectives as sections, key results as rows underneath. Google recommends[three to five objectives with about three key results each](https://rework.withgoogle.com/en/guides/set-goals-with-okrs) , which is a good ceiling for a single view. Beyond that you are looking at a list, not a dashboard.


For each objective, show a short title and a rolled-up status. For each key result underneath, show the four numbers, a status color, a small trend line since the quarter started, and the owner’s name. A few layout choices that consistently help:


- **Put status by pace, not raw percentage.** Color the row against “expected by now,” so green means on pace and red means slipping, not just “far from target.”
- **Show the trend, not only the point.** A key result at 41% is a different story if it was 35% last week versus 43% last week. A sparkline carries that context.
- **Group by objective, order by risk.** Within each objective, float the at-risk key results to the top so the review starts where attention is needed.
- **Keep one owner visible per row.** Accountability is a design element, not a footnote.


The reason to build this in a tool connected to your data, rather than a slide, is that the review meeting stops being a data-entry chore. Pointed at your production database or warehouse, a tool like[Basedash](https://www.basedash.com/) can hold the saved query behind each key result and refresh the whole board on its own, and a non-technical owner can open the dashboard, filter to their objective, and ask a follow-up question (“why did seats stall in week 5?”) without waiting on an analyst. If you have not chosen the objectives yet, our framework for[picking a north star metric](https://www.basedash.com/blog/how-to-choose-a-north-star-metric-a-practical-framework) pairs well with this.


## OKR software vs a BI tool: which should you use


There are three common places to track OKRs, and they solve different problems. Dedicated OKR software runs the ceremony; a spreadsheet is the zero-setup default; a BI or dashboard tool keeps the numbers live. Many teams end up using two: one to manage the process, one to hold the data.


Approach How key results update Best for Main limitation


OKR software (Lattice, Viva Goals, Weekdone) Manual entry, some integrations Check-ins, alignment, and ownership across many teams Values are often typed in by hand and drift from source data


Spreadsheet or slide Manual A first quarter, or a very small team Stale the moment you close it; no live source or history


BI or dashboard tool (Basedash, Tableau, Looker) Query against source systems Key results that map to metrics in a database or warehouse Does not run the OKR process itself (check-ins, grading workflow)


If your key results are mostly data-backed, the dashboard is where the truth lives, and a lightweight process doc handles the rest. If your OKRs are heavy on qualitative or project outcomes, lead with process software and accept that some numbers will be manual. The honest read: OKR software is for running the ritual, and a BI tool is for making sure the numbers in that ritual are real.


## Common mistakes that make an OKR dashboard useless


- **Attainment without pace.** Showing only current-versus-target hides whether you are on schedule. Always compute expected-by-now.
- **Manual updates.** If the dashboard depends on someone retyping figures, it will be current for one meeting and wrong for the rest of the quarter.
- **Too many objectives.** Ten objectives with thirty key results is a backlog, not a set of priorities. Cap it at three to five objectives.
- **Binary key results everywhere.** “Launched or not” results carry no signal until the last day. Prefer results with a measurable range so the trend means something.
- **No baseline.** Without the starting value, “we’re at 41%” tells you nothing about how far you have actually moved.
- **Grading as performance review.** Google is explicit that OKRs[are not employee evaluations](https://rework.withgoogle.com/en/guides/set-goals-with-okrs) , and with stretch goals the sweet spot for final grades sits around 60 to 70 percent. If your dashboard punishes yellow, people will sandbag next quarter’s targets.


## When an OKR dashboard is the wrong tool


Skip the live dashboard when your key results are inherently manual or one-off: closing a specific partnership, shipping a launch, hiring a role. Those are real key results, but they update once and do not benefit from a refreshing chart. A shared doc or your OKR tool tracks them fine.


It is also premature if you have not agreed on how each key result is measured. A dashboard built on contested definitions (“what counts as active?”) will generate arguments, not decisions. Settle the metric definitions first, ideally in a[metric tree](https://www.basedash.com/blog/how-to-design-a-metric-tree-a-practical-framework-for-saas-analytics) or semantic layer, then build the view on top of numbers everyone already trusts.


## FAQ


### What is the difference between an OKR and a KPI?


A KPI is an ongoing health metric with no finish line, like MRR or weekly active users. An OKR is a time-boxed goal: an objective plus the measurable key results that prove you reached it, such as “raise activation from 32% to 45% this quarter.” Key results are often built on a KPI, but the dashboard frames them as progress toward a target inside a window, not just a number drifting over time.


### How often should an OKR dashboard update?


If the key results are backed by queries, refresh the underlying data at least daily so the numbers are current for any check-in. The OKRs themselves are usually set quarterly and reviewed weekly or biweekly. The advantage of a data-connected dashboard is that the values update on their own between reviews, so the meeting is about deciding what to do, not about entering data.


### How many key results should be on the dashboard?


Google’s guidance is three to five objectives with about three key results each, which is a sensible ceiling for one view. That keeps the total in the range of nine to fifteen rows. More than that and the dashboard becomes a task list, which dilutes focus and makes it hard to see what is actually at risk.


### Should OKR grades be tied to performance reviews?


No. OKRs are a focus and alignment tool, not a performance evaluation. When teams set stretch goals, the expected outcome is around 60 to 70 percent attainment, so treating anything below 100 percent as failure encourages people to set unambitious targets. Grade OKRs honestly and consistently, and keep them separate from individual reviews.


### Can I build an OKR dashboard on my production database?


Yes, and for data-backed key results it is the most reliable option, because the current value is a live query rather than a hand-entered number. Point a BI tool at your production database or warehouse, save one query per key result, and let it refresh on a schedule. Keep read access scoped and, where needed, query a replica or a reporting copy so dashboards never load the primary database.
