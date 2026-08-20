---
schema_version: "1.0.0"
document_id: "b155230d0ba788a4d0bcfbe9ab7424c01dac7e4bdfe7427f700878fa1a844514"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-to-choose-a-north-star-metric-a-practical-framework/"
published_at: "2026-07-14T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:67813cea9bad8c36addacde22d7671bd0fe9b9edd3a747cdd4cc2222f3437243"
---

# How to choose a north star metric: a practical framework

A north star metric is the single number that best captures the value your product delivers to customers. It sits above your dashboard as the one measure the whole company works to move, and it works best when growing it means customers are genuinely getting more value, not just when a vanity number ticks up. For a messaging app it might be messages sent between people who know each other. For a marketplace it might be transactions completed. For a subscription analytics tool it might be weekly active teams running queries.


Choosing one well is harder than it looks. The wrong north star pushes teams to optimize a number that goes up while the business quietly stalls. This guide gives a practical framework for choosing a north star metric, examples by business model, the anti-patterns that trip most teams up, and a note on when a single north star is the wrong idea. It is written for founders, operators, and product leaders who want one metric the team can actually rally around.


## TL;DR


- A north star metric is the one number that best represents the value customers get from your product. Moving it should reliably move long-term revenue and retention.
- A good north star passes five tests: it reflects customer value, it is a leading indicator of revenue, the team can influence it, it is easy to explain, and it is measured consistently.
- Pick it by mapping what “getting value” actually means for your product, then choosing the measure closest to that moment of value.
- Pair the north star with a small set of input metrics (the levers you pull to move it). The north star is the destination; inputs are the steering.
- Avoid the common traps: choosing revenue itself, choosing a vanity count, or letting every team invent its own north star.
- Not every company needs one. Early-stage products still searching for fit, or holding companies with unrelated product lines, are often better served by a short list of metrics.


## What is a north star metric?


A north star metric (sometimes called a north star KPI) is a single, high-level measure that a company chooses as its primary indicator of success. The idea is that if you pick the right one, then focusing the whole organization on moving it will pull revenue, retention, and growth along with it.


The concept was popularized by growth advisor Sean Ellis and later formalized in[Amplitude’s North Star Playbook](https://amplitude.com/north-star) , which frames the north star as the metric that “best captures the core value your product delivers to customers.” The emphasis on value is the important part. A north star is not your biggest number or your most impressive chart. It is the measure that sits closest to the moment a customer actually gets what they came for.


Two things a north star metric is not:


- **It is not revenue.** Revenue is the result of delivering value, not the value itself. More on why this distinction matters below.
- **It is not a vanity metric.** Registered users, total downloads, and page views can all rise while the business gets weaker. A north star should be hard to inflate without also helping customers.


If you want the broader distinction between plain measures and the ones worth managing, see our guide on[KPI vs metric](https://www.basedash.com/blog/kpi-vs-metric-whats-the-difference-and-how-to-decide-what-belongs-on-a-dashboard) . A north star is a specific, company-level KPI with an unusually heavy job.


## What makes a good north star metric?


Before you pick one, it helps to know what you are selecting for. A strong north star metric passes five tests.


1. **It reflects customer value.** When the number goes up, customers are getting more of what your product promises. A note-taking app that measures notes created is closer to value than one that measures logins.
2. **It leads revenue.** The metric should move before revenue does, so that watching it tells you where the business is heading rather than where it has already been. Retention and expansion should follow from it over time.
3. **The team can influence it.** People need a plausible path from their daily work to the metric. If no team believes they can move it this quarter, it will not change behavior.
4. **It is simple to explain.** Everyone from sales to engineering should be able to say what it means in one sentence. Composite scores with five weighted inputs fail this test even when they are technically better.
5. **It is measured consistently.** The definition has to be stable and computed the same way every time, ideally from a single source of truth. If two teams calculate it differently, it stops being a shared goal.


Use these as a scoring rubric. Write down two or three candidate metrics and rate each one against the five tests. The candidate that scores well across all five, rather than perfectly on one, is usually the right choice.


## North star metric examples by business model


The right north star depends heavily on how your product creates value. The table below maps common business models to the kind of value moment they hinge on and a candidate metric that sits close to it. Treat these as starting points, not prescriptions.


Business model What “getting value” looks like Candidate north star metric


Product-led SaaS A team does real work in the product every week Weekly active teams performing a core action


Content or media People spend time with content they find worth returning to Weekly time spent by returning users


Marketplace A buyer and seller complete an exchange Transactions completed per period


Messaging or collaboration People communicate with others who matter to them Messages sent between connected users


E-commerce Customers buy and come back to buy again Repeat purchase rate or orders per active customer


Usage-based infrastructure Customers run more real workloads on your platform Consumption of the core resource (queries, API calls, compute)


Publicly, growth teams often cite examples like nights booked for a lodging marketplace, or time spent listening for a music service. These are useful illustrations of the pattern rather than confirmed internal metrics, so borrow the logic, not the exact number.


Notice what these have in common: each is an action a customer takes that signals value, and each can be counted consistently. None of them is “revenue” or “total signups.”


## Why the north star should not be revenue


Revenue is the outcome you want, so it is tempting to make it the north star. The problem is that revenue is a lagging indicator. By the time it moves, the behavior that caused it happened weeks or months earlier. You cannot steer a car by watching the road behind you.


Revenue also invites shortcuts that damage the business. A team told to move revenue this quarter can raise prices, push aggressive upsells, or chase low-fit customers who churn later. All three lift revenue briefly and hurt the company. A well-chosen north star sits upstream of revenue, so moving it tends to grow revenue durably rather than borrowing it from the future.


The healthiest setup is to treat the north star as the primary operating metric and revenue as the business result it should predict. If your north star climbs for two quarters and revenue does not follow, that is a signal your north star is wrong, not a reason to abandon the approach.


## How to choose your north star metric


Here is a practical sequence for landing on one.


1. **Write down the value your product delivers in one sentence.** Not the features, the outcome. “Teams get answers from their data without waiting on engineering,” for example.
2. **Find the moment that value is realized.** Identify the specific in-product action that means a customer just got that outcome. That is your value moment.
3. **List candidate metrics near that moment.** Usually you get three or four: a count of the action, the number of accounts doing it, the frequency, or a breadth measure like number of distinct features used.
4. **Score each candidate against the five tests.** Reflects value, leads revenue, influenceable, explainable, consistently measured.
5. **Pressure-test with the inversion.** Ask: could this number go up while customers are worse off? If yes, tighten the definition (for example, “active teams” becomes “teams that ran at least three queries this week”).
6. **Commit and instrument it.** Define it precisely, compute it from one source, and put it somewhere the whole team sees it. A north star nobody can see is just a slide.


You will rarely find a perfect metric. Aim for the one that is directionally right and hard to game, then refine the definition as you learn.


## Connect the north star to input metrics


A north star on its own does not tell anyone what to do on Monday. It is too high-level. The fix is to break it into input metrics: the two to four levers that, when pulled, move the north star.


If your north star is weekly active teams running a core action, the inputs might be new team activation rate, the percentage of teams that reach the core action in their first session, and week-four retention. Each input is something a specific team can own and affect directly.


This is essentially a shallow[metric tree](https://www.basedash.com/blog/how-to-design-a-metric-tree-a-practical-framework-for-saas-analytics) : the north star at the top, its direct drivers below, and the tactical metrics teams manage beneath those. The tree keeps everyone pointed at the same summit while giving each function a lever it controls. When the north star stalls, you look down the tree to find which input broke.


## Common north star metric mistakes


The failures are more predictable than the successes. Watch for these.


- **Picking a vanity metric.** Total registered users, cumulative downloads, and pageviews only go up. A metric that can never fall cannot warn you when something is wrong.
- **Picking revenue.** Covered above: it lags, and optimizing it directly encourages short-term tactics that hurt retention.
- **Choosing something no team can move.** If the metric feels like weather, teams stop trying. It has to connect to work people control.
- **Letting every team have its own north star.** The point is alignment. Five north stars is zero north stars. Teams should have their own input metrics, not their own summit.
- **Changing it every quarter.** A north star only compounds if it holds long enough to shape decisions across many cycles. Redefine the calculation if you must, but do not swap the whole metric on a whim.
- **Overcomplicating it.** A weighted composite score can be more accurate and still fail, because no one can hold it in their head or explain what moved it. Prefer a single countable action.


## When a single north star is the wrong idea


A north star is a focusing tool, and focus is only useful when you know what to focus on. Skip it, or hold it loosely, in these cases.


- **Pre product-market fit.** If you are still learning what value you deliver, committing to one metric can lock you into the wrong loop. Track a small set of signals and stay flexible.
- **Multiple unrelated product lines.** A company with genuinely separate businesses usually needs a north star per line, tied together by revenue at the top, rather than one forced metric.
- **Pure efficiency phases.** When the goal is margin or cost, a growth-oriented north star can point the wrong way. Be explicit that you are optimizing something else for now.


In these situations a short, honest list of the three or four numbers that matter beats a single metric chosen for the sake of having one. Our take on[data-driven decision making](https://www.basedash.com/blog/data-driven-decision-making-a-framework-that-goes-beyond-dashboards) goes deeper on when metrics help and when they get in the way.


## How to track a north star metric


Once you have chosen a north star and its inputs, the metric needs a home the whole team trusts. That usually means computing it from your production database or warehouse rather than a spreadsheet someone updates by hand, so the definition stays consistent and the number is current.


A lightweight BI tool like[Basedash](https://www.basedash.com/) works well here: you connect it to the database that already holds the underlying events, write the query that defines the metric once, and put the north star and its input metrics on a dashboard the team sees on a regular cadence. The value of computing it live is that the definition lives in one place, so “active teams” means the same thing to everyone looking at it. Heavier enterprise platforms can do this too; the important part is not the tool but that the metric is defined once, measured consistently, and visible.


Whatever you use, keep the north star review on a fixed rhythm, weekly or monthly, and always look at it alongside its input metrics so a stall points you to a cause.


## FAQ


### What is the difference between a north star metric and a KPI?


A north star metric is a specific, company-wide KPI. It is the single most important measure of the value your product delivers, chosen to align the whole organization. A KPI is any metric tied to a goal, and a company can have many. The north star is the one at the top that the others should ladder up to.


### Should a startup have more than one north star metric?


Usually no. The purpose of a north star is focus, and multiple north stars defeat that. The exception is a company with genuinely separate product lines, which may need one per line. Individual teams should have their own input metrics, but the company should share a single north star.


### Is revenue a good north star metric?


Generally no. Revenue is a lagging result of delivering value, and optimizing it directly tends to encourage short-term tactics like aggressive upsells or price hikes that hurt long-term retention. A better north star sits upstream of revenue so that moving it grows revenue durably. Watch revenue as the outcome your north star should predict.


### How often should you change your north star metric?


Rarely. A north star only compounds if it holds long enough to shape many decisions. You may refine how it is calculated as your understanding improves, but swapping the metric itself every quarter signals that it was not the right choice. Change it only when the business model or the value you deliver genuinely shifts.


### What is an input metric?


An input metric is one of the two to four levers that move your north star. If the north star is weekly active teams, inputs might be activation rate, first-session success rate, and week-four retention. Inputs are owned by specific teams and are directly influenceable, which makes them the day-to-day steering for the north star’s destination.


### Can a north star metric be a composite score?


It can, but it usually should not. A weighted score of several signals can be more accurate, yet it fails the simplicity test: people cannot explain it or reason about what moved it. A single countable action that closely tracks value is almost always a better north star, with the richer analysis kept in the supporting metrics beneath it.
