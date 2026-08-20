---
schema_version: "1.0.0"
document_id: "2fe4ef8e6f22244c9d147c950cebe43947f3215e78e91d057430618cc63a24e3"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-to-build-a-budget-vs-actual-report-variance-analysis/"
published_at: "2026-07-08T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T21:08:44.176891+00:00"
content_hash: "sha256:1e9504e0468be96ee0372667c31f16f0a21b07771af7ad89cb121b877e3b9eaa"
---

# How to build a budget vs actual report (variance analysis)

A budget vs actual report compares what you planned to spend or earn against what actually happened, and calculates the gap (the variance) for each line. The whole point is to answer one question fast: where did reality diverge from the plan, and does that gap matter enough to act on?


This guide walks through how to build that report from the underlying data instead of rebuilding it by hand every month. It covers where budget and actuals live, how to join them at the same grain, the SQL to compute variance, how to flag the variances worth a conversation, and how to automate the whole thing. It is written for finance leads, FP&A analysts, and operators who own a monthly numbers review and are tired of copy-pasting a plan into a spreadsheet next to the accounting export.


## TL;DR


- Variance = actual minus budget. Variance percent = (actual minus budget) divided by budget. Everything else is presentation.
- The hard part is not the math. It is getting the plan and the actuals into the same shape (same accounts, same periods, same grain) so they can be joined.
- Flag variances with two gates, not one: a percentage threshold and an absolute dollar threshold. A line has to clear both before it shows up as an exception, so tiny accounts with wild percentages and huge accounts with small percentages both behave sensibly.
- Favorable and unfavorable depend on the account. Under budget is good for expenses and bad for revenue.
- A variance report is worth automating because you rebuild the exact same thing every close. Point a query at your actuals, keep the plan in one place, and let the report refresh itself.


## What a budget vs actual report actually does


A budget vs actual report (sometimes called a plan vs actual or a variance report) sits between accounting and decision-making. Accounting tells you what happened. The variance report tells you whether what happened matches what you committed to, and by how much.


For each line, it shows four numbers: the budgeted amount, the actual amount, the variance (actual minus budget), and the variance as a percentage of budget. Finance teams run it monthly or quarterly against a plan set at the start of the year or re-forecast during it.


The report has a narrow job. It is not a forecasting model, a cash flow statement, or a full FP&A planning tool. It answers: are we on plan, and where are we off? The follow-up work (re-forecasting, reallocating budget, asking a department head what happened) starts from its output.


## The real problem: budget and actuals live in different places


The variance formula is trivial. The reason variance reporting stays a manual, error-prone chore is that the two sides of the comparison almost never live together.


- **The plan** usually lives in a spreadsheet or a planning tool. It was built top-down at the start of the year, broken out by month and by account or department, and it changes rarely.
- **The actuals** live in your accounting system (QuickBooks, Xero, NetSuite) and increasingly in a data warehouse that syncs from it, plus operational systems like Stripe for revenue. They change continuously as the month closes.


So a real variance report is a join problem. You need both sides described with the same account names, the same time periods, and the same level of detail, then joined on those keys. Most of the manual pain (mismatched account labels, a plan that is monthly while actuals are daily, a department that got renamed) is data-shape pain, not math.


Get the shapes to line up once and the report becomes a query you can rerun forever.


## Step 1: Get the plan into a queryable place


Your actuals are already query-able if they land in a database or warehouse. The plan usually is not, because it lives in a sheet.


You have three options, in rough order of effort:


1. **Load the plan into a table.** Import the budget spreadsheet into a` budget` table in your warehouse or production database with columns like` period` ,` account` ,` department` , and` budget_amount` . This is the cleanest long-term setup and lets you join everything in SQL.
2. **Keep the plan in a sheet and connect to it.** Many BI tools can read a Google Sheet as a source. This keeps finance editing the plan where they are comfortable while the report reads it live. Good enough for smaller teams.
3. **Enter the plan directly in your BI or FP&A tool.** Some tools let you type or paste budget figures into a managed table. Convenient, but the plan then lives inside a reporting tool rather than in a system finance controls.


Whatever you choose, the plan needs the same account and period vocabulary as your actuals. This is the single highest-leverage thing you can do. If your general ledger calls it` 6100 Software Subscriptions` and your budget sheet calls it` SaaS tools` , build a small mapping table once so the join is exact and stays exact.


## Step 2: Pull actuals at the same grain


“Grain” means the level of detail a row represents. Your plan is probably one row per account per month. So your actuals query needs to roll up to exactly that: one row per account per month.


If actuals live in a warehouse table of transactions, that is a` group by` :


```text
select
date_trunc(  'month'  , posted_at)::  date   as   period  ,
account,
sum  (amount)   as   actual_amount
from   gl_transactions
where   posted_at   >=   '2026-01-01'
group by   1  ,   2  ;
```


The two rules that prevent most reconciliation headaches:


- **Match the period boundaries.** If the plan is monthly, truncate actuals to month. Watch time zones and the accounting cutoff so a transaction posted late on the last day of the month lands in the right period.
- **Match the account rollup.** If the plan is set at the department level, roll actuals up to department. Do not compare a monthly plan line to a report that silently includes an extra half-month of actuals, which is the most common way a variance report ends up wrong.


## Step 3: Join the plan to actuals and compute variance


With both sides at the same grain, the variance report is one query. Use a` left join` from the budget so that a planned line with zero spend still shows up (a big favorable or unfavorable variance you would otherwise miss), and` coalesce` the missing actuals to zero.


```text
select
b  .  period  ,
b  .  account  ,
b  .  budget_amount  ,
coalesce  (  a  .  actual_amount  ,   0  )   as   actual_amount,
coalesce  (  a  .  actual_amount  ,   0  )   -   b  .  budget_amount   as   variance,
round  (
(  coalesce  (  a  .  actual_amount  ,   0  )   -   b  .  budget_amount  )
/   nullif  (  b  .  budget_amount  ,   0  )   *   100
,   1  )   as   variance_pct
from   budget b
left join   actuals a
on   a  .  period   =   b  .  period
and   a  .  account   =   b  .  account
order by   b  .  period  ,   b  .  account  ;
```


Two details that matter more than they look:


- **` nullif(b.budget_amount, 0)`** guards against dividing by zero when something was not budgeted. Spend against a zero budget is real and worth surfacing, so handle it as its own case rather than letting the percentage blow up.
- **Full outer join if actuals can exist without a plan.** A` left join` from budget will miss accounts that had spend but were never budgeted. If unplanned spend is a risk you care about, use a` full outer join` and coalesce both sides.


## Step 4: Flag favorable, unfavorable, and material variances


A raw variance column is not a report. Two lines of interpretation turn it into one.


**Direction.** Whether a variance is good or bad depends on the account. Coming in under budget is favorable for an expense and unfavorable for revenue. Encode the account type so the report labels itself:


```text
case
when   account_type   =   'revenue'   and   variance   >=   0   then   'favorable'
when   account_type   =   'expense'   and   variance   <=   0   then   'favorable'
else   'unfavorable'
end   as   direction
```


**Materiality.** In any real chart of accounts, most lines are within a rounding error of plan every month. If you highlight every non-zero variance, the report becomes noise and people stop reading it. Filter to what is material.


The mistake here is using a single threshold. A percentage-only rule flags a $40 office-snacks line that came in 300% over. A dollar-only rule flags a large line that moved 1%, which is nothing. Neither is worth a meeting.


Use a **two-gate rule** : a line is an exception only if it clears both a percentage threshold and an absolute dollar threshold.


```text
where   abs  (variance_pct)   >=   10
and   abs  (variance)       >=   5000
```


This is the piece most homegrown variance reports get wrong, and it is the single change that makes the output trustworthy. Set the two thresholds to your scale (a seed-stage startup might use 10% and $2,000; a larger company 5% and $50,000), then let the report show you the five or ten lines that actually deserve a conversation.


## Step 5: Visualize it so the gaps are obvious


A variance report is mostly a table, and that is fine. But a few chart choices make the gaps read faster.


- **The exception table is the core.** Sort by absolute variance, descending, so the biggest gaps sit at the top. Show budget, actual, variance, and variance percent, and color the variance column by direction (favorable and unfavorable), not by sign. Add a running total at the bottom.
- **A bullet chart per key line** shows actual against budget as a target marker. It is more compact and less misleading than a two-bar “budget vs actual” cluster, which invites eyeballing the wrong difference.
- **A waterfall chart** is the right tool for explaining how you got from budgeted total to actual total, one contributing line at a time. Use it for the summary view, not for every account.
- **Avoid dual-axis charts and stacked bars** for variance. They make small, important gaps hard to see. If you want more on matching the chart to the question, see[how to choose the right chart for a dashboard](https://www.basedash.com/blog/how-to-choose-the-right-chart-for-a-dashboard) .


## Step 6: Automate the refresh and set a cadence


The reason to build this as a query instead of a spreadsheet is that you run the exact same comparison every close. Automating it removes both the busywork and the transcription errors.


- **Actuals refresh on their own** if they come from a live database or a warehouse that syncs from accounting. Point the report at that source and it re-computes when the data lands.
- **The plan only changes when you re-forecast.** Keep it in one table or one sheet so there is a single version. If you re-forecast quarterly, keep prior plans as separate scenarios rather than overwriting, so you can compare against the original budget and the latest forecast.
- **Deliver it on a schedule.** Push the variance summary to the team the day after close, so the review starts from the same numbers instead of a screen-share of someone’s spreadsheet. Most BI tools support[scheduled report delivery](https://www.basedash.com/blog/scheduled-reports-in-bi-tools-how-automated-report-delivery-works) to email or Slack.


## A materiality rubric for what to escalate


Materiality flagging tells you which variances are large. It does not tell you which ones need action. Pair the two-gate filter with a simple escalation rubric so the monthly review moves quickly.


Variance size (clears both gates?) Direction Recurring or one-off? Action


No Either Either Note it, no discussion


Yes Favorable One-off Confirm the cause, no action


Yes Favorable Recurring Consider raising the forecast


Yes Unfavorable One-off Explain it in the review, monitor


Yes Unfavorable Recurring Owner presents a plan; re-forecast the line


The “recurring or one-off” column is what most reviews skip. A one-time unfavorable variance (a delayed vendor invoice that hit this month) needs an explanation. A recurring one (cloud spend that has been over plan for three months) needs a decision. Same size, different response.


## Common mistakes


- **Comparing mismatched periods.** A monthly plan against actuals that include a partial extra month is the most frequent cause of a variance report that “looks wrong.” Align the period boundaries first.
- **One threshold instead of two.** Percentage-only floods the report with tiny accounts; dollar-only hides meaningful percentage swings on smaller lines. Use both gates.
- **Ignoring unplanned spend.** A` left join` from budget hides accounts with actuals but no plan. If unplanned spend matters, use a full outer join.
- **Treating under-budget as always good.** Under-spending on a growth line can be a missed investment, not a win. Direction is a starting point, not a verdict.
- **Rebuilding it by hand every month.** If you are copy-pasting an export next to a plan in Excel, you are re-introducing transcription errors every close. This is the whole reason to make it a query.


## Tools for building budget vs actual reports


The right tool depends on how your plan and actuals are stored and who needs to touch the report.


Tool Best for How the plan gets in Tradeoff


Spreadsheet (Excel, Google Sheets) Very small teams, one-off analysis Typed in the sheet Manual actuals, easy to break, no history


BI tool on your warehouse (Basedash, Metabase, Looker) Teams whose actuals already live in a database Loaded table or connected sheet Requires actuals in a queryable source


Power BI, Tableau Established finance teams with an analyst Modeled in the tool or from a sheet Heavier setup, less friendly for non-analysts


Dedicated FP&A tools (Cube, Pigment, Mosaic) Complex, multi-scenario planning Native planning workflows More tool than a pure variance report needs


If your actuals already sync to a database or warehouse, a modern BI tool is usually the fastest path to an automated variance report.[Basedash](https://www.basedash.com/) , for example, lets you write the join-and-variance query once, connect a budget table or sheet, and share a live report that non-technical teammates can filter and ask follow-up questions of without waiting on the analyst who built it. For a broader look at the category, see[the best BI tools for finance teams](https://www.basedash.com/blog/best-bi-tools-for-finance-teams-compared-2026) and[the best financial reporting tools](https://www.basedash.com/blog/best-financial-reporting-tools-compared-2026) .


## When not to build this in a BI tool


A variance report in a BI tool works well when the plan is relatively stable and you mainly need to compare it to actuals. It stops being the right tool when planning itself gets complex: multiple simultaneous scenarios, driver-based models where changing one assumption ripples across the plan, rolling re-forecasts every week, or headcount planning tied to the budget. At that point a dedicated FP&A platform earns its cost, and the BI tool becomes the place you report the results, not the place you build the plan.


## FAQ


### What is the difference between variance analysis and a budget vs actual report?


They are closely related. A budget vs actual report is the artifact: the table comparing planned and actual figures with the variance calculated. Variance analysis is the practice of interpreting those variances, figuring out why each material gap happened, and deciding what to do. The report is the input; the analysis is the work you do with it.


### How do I calculate variance percentage?


Variance percentage is (actual minus budget) divided by budget, times 100. A positive result means actual came in above budget; negative means below. Guard against a zero budget by handling unbudgeted lines separately, since dividing by zero is undefined and an infinite percentage is not useful.


### What is a favorable vs unfavorable variance?


A favorable variance improves your result relative to plan; an unfavorable one hurts it. The direction flips by account type. For revenue, coming in above budget is favorable. For an expense, coming in below budget is favorable. This is why a variance report should label direction based on account type rather than the sign of the number.


### How often should we run budget vs actual reporting?


Most teams run it monthly, timed to the accounting close, with a quarterly rollup for board and leadership review. If your actuals refresh from a live data source, keeping the report always-on is cheap, so the monthly review becomes reading an existing report rather than rebuilding one.


### Can I do variance analysis without a data warehouse?


Yes. If your actuals live in a database (including a production database or your accounting tool’s export) and your plan lives in a sheet, a BI tool that can read both is enough. A warehouse helps when you have many data sources to combine, but it is not a requirement for a straightforward budget vs actual report.
