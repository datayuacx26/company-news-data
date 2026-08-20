---
schema_version: "1.0.0"
document_id: "436108c917d529e43ad189c77d35f04d5d75047890cad4fe6bd32e503a90d992"
company_key: "yc-thematic"
company: "Thematic"
source_id: "yc-thematic-news-import-d60ff6e474dd"
canonical_url: "https://getthematic.com/insights/real-trend-or-seasonal-customer-feedback"
published_at: null
first_seen_at: "2026-08-04T09:39:53.464397+00:00"
fetched_at: "2026-08-04T09:43:50.004226+00:00"
content_hash: "sha256:8ebd63b917df96f8d3e9dd5b07e1f1ff264716a357cbeaf09cc002c1c5ae2edc"
---

# How Do You Tell Whether a Change in Customer Feedback Is a Real Trend or Just Seasonal?

Delivery complaints climb every December. Onboarding friction spikes in the first week of the quarter. Your score dips in January and nobody is ever quite sure why. Then a theme moves 40% in a month and someone senior asks whether things are getting worse. A single month-over-month comparison can't answer that, because it has no idea what a normal month looks like.


You tell a real trend from a seasonal one by refusing to judge the movement against last period alone. Four checks do the work. Compare the same period a year apart. Judge the theme against its own recent history, not one prior month. Separate a change in how many people mention something from a change in how they feel about it. Then confirm the pattern holds in more than one segment. Thematic runs the middle two automatically and supports the other two directly. Score Change compares two exact periods, such as January 2025 against January 2024. Significant Changes measures each theme against its last six periods and flags only movement beyond two standard deviations at p<0.05.


Below: what "seasonal" actually means in feedback data, the four checks worth running, where most tools stop short, and what to ask a vendor before you trust a trend line.


## What does "seasonal" actually mean in feedback data?


Seasonality is any movement that repeats on a predictable cycle and tells you nothing new about your business. In[customer feedback](https://getthematic.com/insights/customer-feedback-analysis) it comes from more places than the word suggests.


- **Calendar seasonality.** Holiday shipping volume, tax season, back-to-school, end-of-quarter renewals.
- **Operational cycles.** A monthly billing run, a quarterly release train, a survey that goes out on the same day every month.
- **Who responded, not what happened.** A campaign, a promotion, or an outage pulls in a different mix of customers than usual. The feedback changes because the respondents changed.
- **Weather and external events.** A storm, an industry news cycle, a competitor's failure. Real causes, but not signals about your product.


The distinction that matters is between movement that repeats and movement that persists. Seasonal movement returns to baseline on its own. A real trend does not. Everything below is about telling those apart before you spend a quarter fixing the wrong thing.


## What does it take to tell a real trend from a seasonal one?


Four checks, in order. Each one rules out a different way of being wrong.


**Check 1: compare like periods.** If the cycle is annual, the comparison is this January against last January, not January against December. Month-over-month is the default in most tools, and against an annual cycle it manufactures alarms on schedule.


**Check 2: judge against a baseline, not a single prior period.** One prior month tells you nothing about normal variation. A theme that swings between 3% and 6% every month has not done anything interesting by hitting 6% again. You need enough history to know the theme's usual range before you can say a value falls outside it.


**Check 3: separate volume movement from score movement.** These are different findings with different responses.


**What moved** **What it usually means** **What to do**


Mentions up, score flat More people are hitting the same thing. Often a volume or exposure change, not a quality change. Check whether your respondent mix or traffic changed first.


Mentions flat, score down The same number of people are having a worse time. This is usually the real signal. Investigate the experience itself.


Both moving together A genuine and spreading problem. Escalate.


**Check 4: confirm it holds across segments.** A movement that shows up in one region, one product line, or one channel and nowhere else is usually an operational event in that segment. A movement present across segments is closer to a genuine trend.


## Where do most feedback tools fall short?


- The default comparison is the previous period, and changing it is buried or impossible.
- A percentage change is reported with no indication of whether it exceeds normal variation.
- Volume and[sentiment](https://getthematic.com/insights/sentiment-analysis-customer-experience) are collapsed into one "theme is trending" indicator, so you can't tell which one moved.
- Alerts fire on thresholds someone picked by hand, which means they fire constantly on noisy themes and never on stable ones.
- There is no significance test anywhere, so every movement looks equally real.


One question at demo time separates the tools: **ask to see a theme that moved and did not get flagged, and ask why not.** A tool that can't explain a non-alert has no model of normal, which means its alerts are just thresholds wearing a lab coat.


## How does Thematic tell the difference?


**Thematic compares two exact periods, including the same month a year apart.**[Score Change](https://getthematic.com/insights/quantify-themes-moved-nps-score) lets you click a period to compare it against the previous one, or pick two specific periods such as January 2025 against January 2024. Granularity is a choice: weekly, monthly, quarterly, biannual, or a 90-day rolling window. That is check 1, and it takes one selection.


**Significant Changes judges each theme against its own recent history.** Thematic looks at the last six time periods to establish the theme's average and its expected variation, then flags values that fall outside that range. It surfaces themes more than two standard deviations from their recent average, filters for statistical significance at p<0.05, and ranks what survives by magnitude. That is check 2, automated. The tool is built for longer time horizons rather than simple month-over-month moves, which is exactly the failure mode it exists to prevent.


**Volume changes and score changes are reported separately.** Thematic flags the largest increases and decreases in theme volume beyond expected fluctuation, and separately flags themes whose associated score has moved significantly even when mention volume is steady. That second case is easy to miss, because nothing about the theme's size changed. It is usually the one worth acting on. That is check 3.


**Significance is applied across the analysis tools, not just in one view.** Thematic treats a difference as significant when there is a 5% or less chance it is due to randomness, using a two-tailed test. In the Comparison tool, hovering a theme shows whether the volume difference between two filtered segments is statistically significant, which is how you run check 4 without eyeballing it.


**Small themes get their own treatment.** Thematic separately surfaces rare themes on the rise: themes under roughly 1% volume with significant increases. A theme going from 0.4% to 1.1% is invisible in a ranked list and is often the earliest sign of something real. Thematic's[Emerging Themes Detection](https://getthematic.com/insights/spot-emerging-support-issues-customer-feedback) works the same territory from the other direction, flagging issues at a 0.5% mention rate.


Two limits worth knowing. Thematic does not perform automatic seasonal adjustment, so the year-over-year comparison is one you choose rather than one the system applies for you. And Significant Changes can be unavailable on datasets with fewer than six weekly or monthly periods, because there is no way to define unusual without enough history to define usual. If you launched a feedback program two months ago, no tool can tell you whether this month is seasonal.


## What does this look like in practice?


A large metropolitan water utility went through the clearest possible version of this problem. Two major storms caused burst mains, sewage overflows, and widespread service disruption, arriving on top of a repair backlog left by pandemic staffing shortages. The support center took a massive influx of calls.


The volume spike was not the insight. Anyone could see the calls. The useful reading was in the composition: the insights team could see that detractors were increasing, that customers were unhappy about wait times, and that there was a growing trend of issues going unresolved. Those are three different movements, and only one of them was going to fade when the weather did.


What changed the response was narrower still. The feedback showed customers were asking for better updates on repair progress. The utility shifted from putting out fires to proactive communication, and returned to benchmark service levels within a few months.


Read as a volume spike, that event was weather. Read as a set of separable theme movements, it contained a genuine and fixable service problem that would have persisted after the storms passed.


## A buyer's checklist for trend detection in feedback


Ask any vendor these six questions:


1. Can I compare this month against the same month last year in one step?
2. How many prior periods does the tool use to decide what counts as normal for a theme?
3. Does it apply a significance test before flagging a movement, and what threshold?
4. Can I see whether a theme's volume moved, its score moved, or both?
5. How does it treat a theme that is small but growing fast?
6. How much history does the tool need before its trend detection is usable at all?


Question two is the one that separates real detection from a threshold alert. Question six is the one vendors avoid.


## The short answer


A change in customer feedback is a real trend when it survives four checks. It holds against the same period last year. It falls outside the theme's own recent range, not just last month's value. Its volume and score movements point the same way. And it appears in more than one segment. Thematic runs the second and third checks automatically, using a six-period baseline, a two-standard-deviation threshold, and a p<0.05 significance filter. It supports the first with a two-period comparison you choose. Run one test on whatever you use today: pick a theme that moved last month and ask what its normal range is. If the tool can't tell you, it can't tell you whether anything happened.
