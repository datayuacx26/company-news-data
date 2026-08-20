---
schema_version: "1.0.0"
document_id: "59bcd87c4cc21479ee3f3d57e4f1505d5e74158f6244ae44c15d7f2bc1fb46c3"
company_key: "yc-risotto"
company: "Risotto"
source_id: "yc-risotto-news-import-1fe94fa93287"
canonical_url: "https://www.tryrisotto.com/blog/itsm-roi"
published_at: "2026-08-03T00:00:00+00:00"
first_seen_at: "2026-08-06T20:42:37.233822+00:00"
fetched_at: "2026-08-06T20:42:38.384963+00:00"
content_hash: "sha256:ccc87b138695c353d45619797e671d35a357f29335921bd8d67b76a527483c98"
---

# How to Calculate AI ITSM ROI for Your Budget

Every ticket your team resolves manually costs the business twice: once in agent time, and again in the hours the employee spends waiting instead of working.


That is the real financial case for moving from traditional ticketing to AI-driven ITSM. ROI shows up when you factor in what a manual queue costs and what automation removes, and you need to be able to demonstrate the savings in terms your CFO will accept.


And if your board is already running a cost-cutting mandate across software spend, this is the math that decides whether your service desk line survives it.


This guide gives you the framework to run that math yourself, step by step. You’ll convert ticket volume, handle time, and auto-solve rate into annual dollars, benchmark against real implementations, and build a business case that survives a finance review.


## TL;DR


- To calculate ITSM ROI, baseline your current costs first, including fully burdened cost per ticket and the time employees spend blocked in queues, because you can’t prove savings without a starting number.
- Forecast your auto-solve rate conservatively; Risotto customers typically automate 20% to 70% of Tier-1 tickets, so model a defensible midpoint and let results exceed the plan.
- Present the full cost of ownership, including implementation, knowledge upkeep, and change management, because a license-only pitch loses a CFO's trust immediately.
- Translate everything into payback period and NPV over a three-year horizon, keeping hard labor savings separate from soft productivity gains so finance can verify both.


## A 4-step framework to calculate ITSM automation ROI


Every credible ITSM business case starts with the same equation your finance team uses for any investment:


*ROI = ((net financial benefits - total cost of investment) / total cost of investment)) x 100*


The formula is simple. The real work lies in filling in benefits and investment costs accurately.


Net financial benefits looks at every dollar the investment returns to the business. For AI-driven ITSM, that breaks into three categories:


- **Direct labor savings.** The agent hours no longer spent on requests the platform resolves on its own. This is your auto-solve rate times ticket volume times the loaded cost of handling each ticket manually.
- **Reduced tool redundancy.** Licenses and point solutions the platform makes unnecessary, such as a standalone knowledge base tool or a separate access-request workflow product.
- **Employee productivity gains.** The hours employees get back when they stop waiting for access or an answer. Finance teams sometimes discount this bucket, so calculate it separately rather than blending it into labor savings.


Total cost of investment is everything on the other side of the ledger:


- **Licensing and implementation.** The annual platform cost, plus setup, integration with your ticketing system and identity provider, and any internal time spent on configuration.
- **Data and knowledge preparation.** The hours needed to get your documentation into shape so the AI has something accurate to work from.
- **Change management.** Communicating the rollout, training the team, and driving adoption.


Below are the detailed steps for calculating the ROI of[ITSM software](https://www.tryrisotto.com/blog/itsm-software) so you can prove value to leadership and defend the investment.


### Step 1: Baseline your current operational costs (the cost of inaction)


You can’t prove savings without a starting number. Establish what manual support costs you today; this baseline feeds the ROI formula and becomes your 'cost of doing nothing' argument.


#### Calculating cost per ticket


You get your support cost per ticket by dividing total annual support labor cost by annual ticket volume. Use total compensation (base pay plus benefits, taxes, and overhead), and ask finance for your loaded-cost multiplier (the factor that adds extra costs on top of salary, often 1.25x to 1.4x).


Then translate the total into the unit CFOs actually decide in: headcount. Divide projected annual savings by the loaded cost of one support hire to show how many roles the automation absorbs.


Split the math by tier: a Tier-1 password reset or software access request and a Tier-3 escalation don't cost the same. And Tier-1, where access requests alone make up the largest share of volume, is where AI-driven ITSM concentrates its savings.


#### Quantifying idle employee time


Every ticket has a second price tag: the requester sitting blocked while it waits. Multiply annual ticket volume, average hours blocked, and the loaded hourly cost of an employee. Be conservative, and present it as a separate line item; CFOs tend to overlook soft savings blended with hard ones.


The wait cost compounds for global and distributed teams: a ticket filed at 9pm sits until IT's morning. Immediate answers around the clock, even when IT is offline, is a core piece of the value of an ITSM automation tool if your company spans time zones.


#### Auditing software redundancy


Calculate how much you pay for the tools the AI platform would absorb. This can look like standalone knowledge bases, access-request workflows, chatbot add-ons, and reducible per-agent licenses. Anything retired moves into the benefits column at full annual value.


#### Finding your current tool's auto-solve rate


If you already run a chatbot, your baseline is the plateau you're stuck at. Measure what your current setup auto-solves and build the case on the difference. Legacy tools commonly stall in the single digits for years, and automating even just a few percentage points higher on average can deliver serious ROI.


### Step 2: Forecast your AI performance multipliers


Your future state is a forecast, so build it from industry benchmarks. Model conservative, expected, and best-case scenarios; your CFO trusts a range more than a single optimistic number.


#### The auto-solve rate variable


Auto-solve rate is the percentage of tickets resolved end to end without a human. Risotto customers, for example, typically auto-solve 20% to 70% of Tier-1 tickets. Model your savings at a defensible midpoint, then note the upside once you have real data to work with.


#### TTR reduction math


Even escalated tickets get cheaper when AI handles triage, routing, and context summaries for agents. Estimate the agent time saved per escalated ticket, then multiply by escalation volume and the loaded hourly cost of your agents.


For example, Risotto customer[Gusto's](https://www.tryrisotto.com/customers/how-gusto-auto-resolves-55-of-tickets-with-risotto) AI-handled tickets resolve in 5 hours versus 35 for human-handled ones, and[ThoughtSpot](https://www.tryrisotto.com/customers/thoughtspot-automates-multi-department-support-with-ai) cut average resolution from 31 hours to 6.5 after implementing Risotto.


#### Proactive resolution


Mature deployments save money before tickets are even created: proactive campaigns handle access reviews and compliance pushes, and incident workflows alert on-call responders the moment something breaks to minimize duplicate tickets. Model this last and conservatively, as it’s the hardest to forecast.


### Step 3: Map out the Total Cost of Ownership (TCO)


Pitching only the license fee is the fastest way to lose a CFO's trust. Finance teams know software carries hidden costs, and if you don't surface them, they will. A complete TCO report makes your ROI number credible.


#### Implementation costs and API integration


Count the time and resources it will take to connect your existing ticketing system, identity provider, and knowledge sources, plus configuration and testing.


Ask vendors what deployment actually took for reference customers. Risotto customer[Vidyard](https://www.tryrisotto.com/customers/vidyard-automates-80-percent-tier-1-it-requests-with-risotto) , for example, reached 80% Tier-1 automation within 30 days of deployment, but this figure will vary significantly by tool.


#### The training tax


AI performance depends on what it learns from, so budget ongoing hours for maintaining knowledge bases and tuning workflows.


You get the best ROI here from a tool like Risotto, which auto-updates its knowledge index from sources like Notion and Confluence and turns resolved Slack threads into knowledge articles.


#### Licensing structures


Pricing models change your TCO curve. Consumption-based models (per conversation or per resolved ticket) look affordable at first glance, then creep as adoption grows.


Headcount-based annual pricing, Risotto's model, keeps costs predictable as volume rises. Model different pricing structures against your three-year ticket forecast when weighing tools.


### Step 4: Model the payback period and NPV


Move past basic ROI to the advanced metrics financial teams demand. The table below lays out a sample three-year financial horizon.


Financial metric


Year 1


Year 2


Year 3


Upfront and annual costs


$50,000


$45,000


$45,000


Direct labor savings


$55,000


$85,000


$95,000


Software consolidations


$5,000*


$12,000


$12,000


Net cash flow


$10,000


$52,000


$62,000


Cumulative savings


$10,000


$62,000


$124,000


There are two additional figures that will help you demonstrate ROI to your CFO:


- **Payback period:** How long until cumulative savings cover the initial investment. In the model above, payback lands early in year two.
- **Net present value (NPV:** Adds up your three years of projected savings, discounts them because future money is worth less than money today, and subtracts your costs. Positive NPV means the investment pays.


*Consolidation savings are only partial in year one because legacy contracts run until renewal.


## CFO-proofing your pitch: pitfalls to avoid in your budget presentation


A budget usually gets rejected because finance doesn’t trust the model, and there are five mistakes that most often undermine the credibility of ITSM ROI projections.


### Forgetting that finance expects AI to cut costs


CFOs increasingly assume AI line items make things cheaper, so a platform that costs more than the tools it replaces invites the question 'why does it cost more if the technology improved?'


Answer it before it's asked and frame the case as net savings: new platform cost, minus retired licenses, minus labor recovered. Finance wants total spend to go down, and you just have to prove it will from the start.


### Relying on soft productivity savings


If you lean on a claim like ‘this saves every employee 10 minutes a day,' your CFO will discount it immediately. Recovered minutes don't reduce spend unless something changes, whether that’s lowering headcount or cutting overtime.


Lead with hard savings (labor hours redeployed, tools retired), then present productivity recapture as a separate, conservatively counted line. Keeping the two separate signals that you understand the difference.


### Ignoring the build-vs-buy math


Your finance team may very well ask you to ‘prove the existing stack can't do this,' and some IT leaders get pushed toward building their own agent with workflow tools or an LLM. If that question is coming, put the DIY option through the same framework: upfront build cost, plus an ongoing maintenance tax and the opportunity cost of engineers fixing bot bugs instead of doing their day jobs.


Teams that build in-house IT support bots tend to reach the same conclusion: a dedicated vendor arrives with most of the problem already solved, which ends up saving you time and money in the long run.


### Overestimating day one automation


Modeling a 90% auto-solve rate from month one tells finance you take a vendor's estimate at face value. Top performers do reach striking numbers (Risotto customer[Ironclad](https://www.tryrisotto.com/customers/ironclad-transforms-90-percent-it-support-with-risotto) , for example, automates 90% of IT support).


But a business case built on the best possible outcome has nowhere to go but down. Model a defensible midpoint and let results exceed expectations rather than underperform.


### Overlooking change management


Agents need training to work alongside AI, and you need to get employees to buy into your new[IT support automation](https://www.tryrisotto.com/blog/it-support-automation) tool. Budget real hours for both.


It's a small line that builds credibility, and it's also where deployment choices matter. Support that lives in Slack or Teams, where employees already work, carries a far smaller adoption lift than a new portal they have to learn.


## What high ITSM ROI looks like in practice


The framework above is a start, but getting approval requires more than assumptions. The hardest part is defending the forecast when finance asks where your estimates came from.


Risotto makes that defense straightforward, because the ROI is already visible at well-known companies:


- [Hazel Health](https://www.tryrisotto.com/customers/hazel-health-increases-it-automation-by-4x-with-risotto) increased IT automation by 4x with Risotto
- [Gusto](https://www.tryrisotto.com/customers/how-gusto-auto-resolves-55-of-tickets-with-risotto) saw a 53% resolution rate on day one of implementation
- [Vidyard](https://www.tryrisotto.com/customers/vidyard-automates-80-percent-tier-1-it-requests-with-risotto) was able to automate 80% of Tier-1 requests within 30 days
- [Zipline](https://www.tryrisotto.com/customers/zipline-automates-64-percent-it-tickets-with-risotto) achieved a 100% access grant automation rate


With these numbers available to you, you can anchor every assumption in your business case to a real customer result, model conservatively, and let the tool exceed expectations. That’s a case a CFO can verify and approve.


‍


## FAQs about ITSM ROI


### What factors go into calculating ITSM ROI?


ITSM ROI compares net financial benefits against total cost of investment, expressed as a percentage. Financial benefits include direct labor savings from automated tickets, retired redundant tools, and recovered employee wait time. Costs include licensing, implementation, knowledge base preparation, and change management.


### Does an AI ITSM business case require CFO approval?


Usually, yes. Most AI ITSM contracts are annual commitments large enough to cross finance approval thresholds. Even where IT controls the budget, a case built to CFO standards gets approved faster.


### What key metrics should an ITSM ROI framework include?


Six metrics carry the framework:


1. **Auto-solve rate** drives the labor savings math
2. **Reopen rate** on auto-solved tickets verifies those resolutions are real, not redirects
3. **Cost per ticket** sets your baseline
4. **IT staffing ratio** (buyers commonly target around 1 IT hire per 800 to 1,000 employees) frames the headcount case
5. **Time to resolution (TTR)** captures speed gains
6. **Payback period** puts everything on the timeline finance compares investments against


### How to build an ITSM business case before the budget cycle?


To build an ITSM business case, pull 12 months of ticket volume, calculate your fully burdened cost per ticket, and quantify employee wait time as a separate soft-savings line. Forecast conservatively from published customer benchmarks, map the full cost of ownership, and model a three-year horizon with payback period and NPV.
