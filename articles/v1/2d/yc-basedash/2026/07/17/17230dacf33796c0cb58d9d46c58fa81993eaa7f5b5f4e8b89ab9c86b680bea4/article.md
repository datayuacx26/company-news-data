---
schema_version: "1.0.0"
document_id: "17230dacf33796c0cb58d9d46c58fa81993eaa7f5b5f4e8b89ab9c86b680bea4"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/decision-intelligence-what-it-is-and-how-lean-teams-use-it/"
published_at: "2026-07-27T00:00:00+00:00"
first_seen_at: "2026-07-27T14:03:23.342009+00:00"
fetched_at: "2026-07-28T20:32:33.872616+00:00"
content_hash: "sha256:cca2c614f02a2975292508615970c9f78d638929b9ca82f8c6bb3b2194c284eb"
---

# Decision intelligence: what it is and how lean teams actually use it

Decision intelligence is the practice of connecting data directly to the decisions it is meant to inform and the actions that follow, then measuring whether those decisions worked. It treats the decision, not the dashboard, as the thing to design: what information feeds it, how options get weighed, what action results, and how the outcome loops back to improve the next call. In plain terms, it is analytics with a feedback loop attached.


This guide is for founders, operators, and analytics leads who keep hearing the term and want a straight answer: what decision intelligence actually means, how it differs from business intelligence and data science, whether you need a dedicated platform, and how a small team can practice it without buying one.


## What is decision intelligence?


Decision intelligence (DI) is a discipline for improving how decisions get made, rather than a specific product you install. The term was popularized by Gartner, which[defines it](https://www.gartner.com/en/information-technology/glossary/decision-intelligence) as “a practical discipline used to improve decision making by explicitly understanding and engineering how decisions are made and how outcomes are evaluated, managed and improved via feedback.”


Two words in that definition do the heavy lifting: *engineering* and *feedback* . Engineering means you treat a decision like a designed process with inputs, logic, and an owner, not a one-off gut call. Feedback means you check afterward whether the decision produced the outcome you expected, and use that to make the next one better.


The idea was formalized at Google, where[Cassie Kozyrkov served as the company’s first Chief Decision Scientist](https://en.wikipedia.org/wiki/Cassie_Kozyrkov) and is credited with founding the discipline. She describes decision intelligence as turning information into better action, at any scale and in any setting. That framing is useful because it keeps the focus on the action, not the report. A beautiful dashboard that nobody acts on has produced zero decision intelligence.


## Decision intelligence vs business intelligence vs data science


These three disciplines get blurred together, but they answer different questions and produce different things. The cleanest way to see it is as a ladder.


Discipline Core question Typical output Who owns it


Business intelligence What happened, and what is happening now? Dashboards, reports, tracked metrics Analysts, operators


Data science Why did it happen, and what will happen next? Models, forecasts, experiments Data scientists


Decision intelligence What should we do, and did it work? A chosen action plus a measured outcome The decision owner


Business intelligence describes reality. Data science explains and predicts it. Decision intelligence wraps around both and asks the only question that changes anything: given all of this, what do we do, and how will we know if it was right? A helpful mental model is that[BI turns data into information and decision intelligence turns information into action](https://www.basedash.com/blog/what-is-business-intelligence-how-modern-teams-turn-data-into-decisions) . You cannot skip BI to get to DI, but you can absolutely do a lot of BI and never reach a decision.


## The decision loop: the part most teams skip


Here is a simple framework for practicing decision intelligence. Call it the decision loop. Every real decision, whether it is a pricing change or which support queue to staff, moves through six stages.


1. **Frame the decision.** Write down the actual question and what would change your answer. “Should we raise the price of the Pro plan?” is a decision. “Look at revenue” is not.
2. **Set the bar before you look.** Decide in advance what result would push you one way or the other. If you decide the threshold after seeing the data, you are rationalizing, not deciding.
3. **Gather the inputs.** Pull the relevant data, plus the context and constraints that the data does not show. Numbers rarely decide on their own.
4. **Choose and act.** Pick an option against the bar you set, and commit to a concrete action with an owner and a date.
5. **Measure the outcome.** Compare what happened to what you expected when you decided. This is the step almost everyone drops.
6. **Feed it back.** Record whether the decision was good, and why, so the next similar decision starts from evidence instead of memory.


Most teams do steps 1 and 3 well, act somewhere around step 4, and never do 5 or 6. That open loop is the single most common failure in analytics. You can tell it is open when nobody can say whether last quarter’s big pricing change actually worked, only that “we made the change.” Decision intelligence is mostly the discipline of closing that loop, and it is where a dashboard stops being a wall of numbers and starts[driving decisions you can trace](https://www.basedash.com/blog/how-to-build-dashboards-that-drive-decisions-a-practical-guide) .


## Do you need a decision intelligence platform?


Gartner now tracks a category of “decision intelligence platforms,” and there is a[Magic Quadrant for the market as of January 2026](https://www.gartner.com/reviews/market/decision-intelligence-platforms) . These are enterprise tools built to model, orchestrate, and automate decisions at scale, often combining rules engines, machine learning, and optimization. They are real and useful, but they are not what most teams mean when they say they want to be more data-driven.


Use this rubric before you shop for a platform.


**You probably need a decision intelligence platform when:**


- The same decision is made thousands of times a day, often by software (pricing, fraud scoring, credit approval, inventory allocation, ad bidding).
- You need to audit and govern the logic behind automated decisions for compliance reasons.
- Decision speed and consistency at scale are the bottleneck, not human judgment.


**You probably do not need one when:**


- Your important decisions are made by humans a handful of times per week or month.
- The bottleneck is getting to trustworthy data and having a place to reason about it together, not automating the choice.
- You have not yet closed the loop on the decisions you already make.


For most startups and lean teams, decision intelligence is a practice, not a purchase. Buying an enterprise platform to automate decisions you make twice a month is like buying a forklift to move one box. The higher-leverage move is to shorten and tighten the decision loop you already run.


## Where decision intelligence breaks down in practice


The failure modes are predictable, and they are almost never about the software.


- **The loop never closes.** A decision gets made and nobody records the expected outcome or revisits it. Without step 5 and 6, you accumulate anecdotes instead of learning.
- **The bar moves after the fact.** Setting the success threshold after seeing the results turns every decision into a story about why you were right.
- **The inputs are not trustworthy.** If people quietly distrust the numbers, they fall back on opinion, and the “intelligence” part collapses. Trustworthy, current data is the precondition for everything else.
- **You optimize a proxy.** Deciding against a metric that is easy to measure but loosely connected to what you care about (page views instead of activated users) produces confident, wrong decisions.
- **You automate a decision no one understands.** Handing a decision to a model or rule you cannot explain is not decision intelligence; it is decision abdication. Automate only after a human has run the loop enough to trust it.


## How lean teams can practice decision intelligence today


You can start closing the loop this week without new software. Four habits get you most of the way.


1. **Write a one-line decision memo before you look at the data.** State the question, the options, and what result would tip you each way. This single habit prevents most hindsight rationalization.
2. **Point your analytics at the live source of truth.** Decisions are only as good as their inputs, so the data feeding them should be current and query the real database or warehouse, not a stale export someone pasted into a slide.
3. **Make follow-up questions cheap.** The loop is only as fast as your slowest “why did that move?” If answering a follow-up takes a week and an engineer, the decision gets made on the first, shallow read. Being able to drill in during the meeting is what keeps the loop tight.
4. **Log the decision and set a review date.** Put the decision, the expected outcome, and a date to check it somewhere durable. When that date arrives, compare and record what you learned.


This is where a lightweight,[AI-native BI tool like Basedash](https://www.basedash.com/) fits a small team’s decision loop. It connects to your production database or warehouse so the numbers behind a decision are live rather than a snapshot, and a non-technical teammate can ask a follow-up question in plain language and drill into the underlying rows on the spot. That does not replace human judgment, and it should not. It just makes steps 3 and 5 of the loop fast enough that people actually complete them. If you want a fuller operating model for this, our guide to[data-driven decision making](https://www.basedash.com/blog/data-driven-decision-making-a-framework-that-goes-beyond-dashboards) goes deeper on the organizational side, and the[analytics maturity model](https://www.basedash.com/blog/analytics-maturity-model-the-5-stages-and-how-to-move-up-one) shows where closing the loop sits on the path from reactive reporting to a genuinely data-driven team.


## FAQ


### Is decision intelligence the same as business intelligence?


No. Business intelligence is about describing what happened and what is happening now through dashboards, reports, and tracked metrics. Decision intelligence sits one level up: it uses that information to reach a specific decision, drive an action, and then measure whether the decision worked. BI is a necessary input to DI, but you can do a lot of BI and never actually close a decision loop, which is the part DI is concerned with.


### Do I need a decision intelligence platform?


Most lean teams do not. Dedicated platforms are built for automating and governing high-volume, repeatable decisions at scale, such as pricing, fraud detection, or credit approval, often made by software thousands of times a day. If your important decisions are made by people a few times a week, the higher-leverage move is to tighten your existing decision process and make sure the underlying data is trustworthy and fast to query.


### Who coined the term decision intelligence?


The discipline was formalized at Google, where Cassie Kozyrkov served as the company’s first Chief Decision Scientist and is widely credited with founding it. Gartner later popularized the term more broadly and now tracks a market of decision intelligence platforms. Kozyrkov defines the field as turning information into better action, at any scale and in any setting.


### How is decision intelligence different from data-driven decision making?


They overlap heavily. Data-driven decision making is the general goal of using evidence rather than gut feel. Decision intelligence is a more explicit discipline for how to do it: framing the decision, setting a threshold in advance, gathering inputs, acting, measuring the outcome, and feeding the result back. In practice, decision intelligence is data-driven decision making with the feedback loop made deliberate.


### What tools support decision intelligence?


For most teams, the toolkit is ordinary: a place to write down decisions and expected outcomes, and a BI or analytics tool that connects to current, trustworthy data and lets people answer follow-up questions quickly. Enterprises automating decisions at scale may add a dedicated decision intelligence platform with rules engines, machine learning, and optimization. The tool matters less than whether your team actually closes the loop.
