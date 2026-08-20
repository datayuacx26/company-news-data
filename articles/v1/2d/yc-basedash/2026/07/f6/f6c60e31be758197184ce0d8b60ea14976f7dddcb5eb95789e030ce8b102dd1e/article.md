---
schema_version: "1.0.0"
document_id: "f6c60e31be758197184ce0d8b60ea14976f7dddcb5eb95789e030ce8b102dd1e"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/kpi-vs-metric-whats-the-difference-and-how-to-decide-what-belongs-on-a-dashboard/"
published_at: "2026-07-09T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T21:08:44.176891+00:00"
content_hash: "sha256:c51c6950353bcd35fe25002a98fe945ef1d5ddb5a2c6cbe76b97336a1a84d30b"
---

# KPI vs metric: what's the difference and how to decide what belongs on a dashboard

A metric is any quantitative measure you can track, like signups, page views, or average order value. A key performance indicator (KPI) is a metric you have deliberately chosen because it tracks progress toward a specific goal. Every KPI is a metric, but most metrics are not KPIs. The difference is not the number itself; it is whether the number is tied to an objective, has a target, and drives a decision.


That distinction sounds academic until you build a dashboard. Then it becomes the difference between a screen people check every Monday and a wall of charts nobody reads. This guide explains what separates a KPI from a plain metric, gives concrete examples for a SaaS company, and offers a simple test for deciding what earns a spot on your dashboard. It is written for founders, operators, product managers, and analysts who are trying to figure out which numbers actually matter.


## TL;DR


- A metric is any number you measure. A KPI is a metric attached to a goal, a target, and an owner.
- Every KPI is a metric. The reverse is not true, and treating every metric like a KPI is the fastest way to make a dashboard useless.
- To decide whether a metric should be a KPI, run it through five gates: is it tied to a goal, does it have a target, does it have an owner, is it actionable, and is it reviewed on a cadence.
- Keep KPIs few (roughly 5 to 9 per team or dashboard). Everything else is a supporting metric you look at when a KPI moves.
- Prefer a mix of leading and lagging indicators so you can act before the result is already set.


## What is a metric?


A metric is a measurement. It answers “how much” or “how many” over some period, usually at a defined grain (per day, per user, per account).


Examples of metrics:


- Number of signups this week
- Average session length
- Monthly recurring revenue (MRR)
- Support tickets opened
- Page load time
- Feature adoption rate


Metrics are neutral. They describe what happened without saying whether it is good, bad, or worth acting on. A metric like “support tickets opened” is useful context, but on its own it does not tell you whether the team is doing well. Rising tickets could mean a broken release or simply more customers. Metrics are the raw vocabulary of analytics. You need a lot of them, and most of them stay in the background.


## What is a KPI?


A KPI is a metric you have promoted. You picked it because it directly reflects progress toward a goal that matters right now, you set a target for it, and someone is accountable for moving it.


The word “key” is doing real work. A KPI is not just an important-sounding metric; it is one of the few numbers that, if it moves in the wrong direction, changes what the team does next. If a number can go up or down and nobody would change any behavior, it is a metric, not a KPI.


Examples of KPIs (assuming each is tied to a current goal):


- Net revenue retention, with a target of 110 percent
- Activation rate for new signups, with a target of 40 percent within seven days
- First-response time on support, with a target under two hours
- Gross margin, with a target of 75 percent


Notice that each of these is a metric plus context: a target, a time frame, and an implied owner. That added context is what turns a measurement into a performance indicator.


## The difference in one line


A metric tells you what is happening. A KPI tells you whether you are winning.


Here is the same idea across the attributes that actually separate the two:


Attribute Metric KPI


Purpose Describes what happened Measures progress toward a goal


Target Usually none Always has a target or threshold


Owner Often none One accountable owner


Count Many (dozens to hundreds) Few (roughly 5 to 9 per team)


Review cadence Ad hoc, when needed Recurring (weekly, monthly, quarterly)


Reaction when it moves May prompt investigation Should prompt a decision


Example Number of logins Weekly active accounts vs target


The practical takeaway: KPIs are a curated subset of your metrics. You track hundreds of metrics because you might need them. You elevate a handful to KPIs because you are managing to them.


## The promotion test: deciding if a metric should be a KPI


The hard part is not defining the terms. It is looking at a specific number and deciding whether it belongs on the dashboard or in the background. Use this five-gate test. A metric becomes a KPI only if it passes all five.


1. **Tied to a goal.** It measures progress toward a specific objective you are pursuing this quarter or year. If you cannot name the goal it serves, it is not a KPI.
2. **Has a target.** There is a number you are trying to hit or stay under, not just “let’s watch it.” A KPI without a target is a chart, not a goal.
3. **Has an owner.** One person is accountable for the number. Shared ownership usually means no ownership.
4. **Is actionable.** Someone can influence it through decisions or work. If the number moves only because of factors outside your control, it belongs in context, not on the KPI dashboard.
5. **Reviewed on a cadence.** It shows up in a recurring review (a weekly team meeting, a monthly business review) rather than only when someone goes looking.


If a metric fails any gate, keep it as a supporting metric. Supporting metrics are not less important in an absolute sense; they are just diagnostic. You look at them when a KPI moves and you need to understand why.


A quick way to apply the test: for each candidate, finish the sentence “If this number gets worse, we will ___.” If you can name a concrete action, it is probably a KPI. If the honest answer is “nothing” or “look into it,” it is a metric.


## KPI vs metric examples for a SaaS company


The same underlying number can be a metric for one team and a KPI for another, depending on whose goal it serves. That context is the whole point.


The number As a plain metric When it becomes a KPI


Signups Volume of new accounts created Growth team’s weekly signup target during an acquisition push


Activation rate Share of signups that reach a key action Product team’s KPI for a quarterly onboarding goal


MRR Total recurring revenue Almost always a company-level KPI with a growth target


Churn rate Share of customers lost in a period Retention team’s KPI with a ceiling (for example, under 3 percent monthly)


Support tickets opened Count of inbound requests Rarely a KPI on its own; a supporting metric behind CSAT or response time


Page load time Median load across pages Engineering KPI when performance is an active objective


The lesson from the “signups” and “support tickets” rows: whether a number is a KPI depends on your goals, not on the number’s inherent importance. When onboarding is the priority, activation rate gets promoted. When it is not, it drops back to a supporting metric you glance at occasionally.


## How many KPIs should you track?


Fewer than you think. A common and reasonable range is 5 to 9 KPIs per team or per dashboard. The reason is attention, not analytics: a dashboard with 30 tiles trains people to skim past all of them. When everything is a priority, nothing is.


This is where the metric-versus-KPI distinction pays off in dashboard design. The top of a dashboard should show the handful of KPIs the team is managing to. Supporting metrics live one layer down, available when a KPI moves and someone needs to diagnose the cause. A useful pattern is to organize supporting metrics as the inputs to each KPI, which is the idea behind a[metric tree](https://www.basedash.com/blog/how-to-design-a-metric-tree-a-practical-framework-for-saas-analytics) : the KPI at the top, the metrics that drive it underneath.


If you are building for leadership specifically, the same rule holds even more strictly. See our guide on[how to build an executive dashboard](https://www.basedash.com/blog/how-to-build-an-executive-dashboard-metrics-layout-and-cadence) for the layout and cadence that keep a KPI dashboard readable.


## Leading vs lagging indicators: a better way to pick KPIs


Once you have decided which metrics deserve KPI status, a second question decides how useful they will be: are they leading or lagging?


- A **lagging indicator** measures a result after it has happened. Revenue, churn, and quarterly retention are lagging. They are accurate but slow. By the time they move, the underlying cause is weeks or months old.
- A **leading indicator** measures something that predicts the result. Trial-to-paid conversion, activation within seven days, and pipeline created are leading. They move earlier, so you can still change the outcome.


Most teams over-index on lagging KPIs because they are the ones executives ask about. The problem is that you cannot manage a number you can only observe after the fact. The fix is to pair each lagging KPI with one or two leading indicators that you can actually influence week to week. If net revenue retention is your lagging KPI, feature adoption and support response time might be the leading indicators you steer with.


A good KPI set has both: lagging indicators to confirm you are winning, and leading indicators to act before the game is decided.


## Common mistakes


- **Treating every metric as a KPI.** The most frequent mistake. It produces bloated dashboards where the important numbers are lost among the merely interesting ones.
- **KPIs without targets.** A number on a chart with no target is a metric wearing a KPI costume. Without a target, there is no way to say whether the value is good.
- **Vanity metrics as KPIs.** Cumulative totals that only ever go up (total signups ever, total pageviews) feel like progress but rarely drive a decision. Prefer rates and current-period figures.
- **Only lagging indicators.** If every KPI is a result, you are always reacting. Add leading indicators you can influence.
- **No owner.** A KPI everyone watches and no one owns tends to drift. Assign one name to each.
- **Set once, never revisited.** Goals change every quarter, so KPIs should too. A metric that was a KPI last quarter may drop back to a supporting role.


## Where KPIs and metrics live in your BI setup


The metric-versus-KPI split maps directly onto how you structure analytics. Metrics are defined once against your source data (in SQL, a[semantic layer, or a BI calculation](https://www.basedash.com/blog/where-to-define-business-metrics-sql-views-dbt-semantic-layers-or-bi-calculations) ) so that everyone computes them the same way. KPIs are a curated view built on top of those definitions: a small set of the metrics, each with a target and an owner, surfaced on a dashboard people actually check.


A modern BI tool should make both easy. In[Basedash](https://www.basedash.com/) , you connect your database or warehouse, define metrics once, and build dashboards that put the few KPIs your team manages to at the top, with supporting metrics available for drill-down when a number moves. The point is not the tool; it is the discipline of promoting only the numbers that pass the test and keeping the rest one layer down.


## Frequently asked questions


### Is every KPI a metric?


Yes. Every KPI is a metric, but not every metric is a KPI. A KPI is a metric you have selected because it tracks progress toward a goal, given a target, and assigned an owner. The set of KPIs is always a small subset of the metrics you track.


### What makes a good KPI?


A good KPI is tied to a current goal, has a clear target, has one accountable owner, and is actionable, meaning someone can influence it through decisions or work. It should also be well defined, so everyone computes it the same way, and reviewed on a regular cadence rather than only when someone asks.


### How many KPIs should a dashboard have?


Aim for roughly 5 to 9 KPIs per team or dashboard. Beyond that, attention drops and people stop distinguishing what matters. Keep supporting metrics available one layer down rather than crowding them onto the main view.


### What is the difference between a KPI and a goal?


A goal is the outcome you want (for example, reach 110 percent net revenue retention). A KPI is the metric you use to measure progress toward that goal. The goal is the destination; the KPI is the gauge on the dashboard.


### Are OKRs the same as KPIs?


No, but they are related. OKRs (objectives and key results) are a goal-setting framework; the key results are often expressed as target values for specific metrics. KPIs are the ongoing metrics you monitor whether or not you use OKRs. In practice, a key result and a KPI can point at the same number.


### Can a metric stop being a KPI?


Yes, and it should when priorities change. A metric that was a KPI while onboarding was the focus can drop back to a supporting metric next quarter when the goal shifts. Reviewing your KPI list each planning cycle keeps the dashboard aligned with what the team is actually working on.
