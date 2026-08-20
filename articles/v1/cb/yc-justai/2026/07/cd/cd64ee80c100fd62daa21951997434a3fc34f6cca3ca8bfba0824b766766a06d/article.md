---
schema_version: "1.0.0"
document_id: "cd64ee80c100fd62daa21951997434a3fc34f6cca3ca8bfba0824b766766a06d"
company_key: "yc-justai"
company: "JustAI"
source_id: "yc-justai-news-import-e4de3da632ca"
canonical_url: "https://www.getjust.ai/blog/how-clickup-personalized-lifecycle-across-300-segments-with-justai"
published_at: null
first_seen_at: "2026-07-25T10:24:54.291702+00:00"
fetched_at: "2026-07-28T21:16:50.994015+00:00"
content_hash: "sha256:f4817329fc93cd05ef5db87b1b88456d42343ea1891d9124673172d1a1154061"
---

# How ClickUp personalized lifecycle across 300+ segments with JustAI

> *"JustAI allowed us to rapidly scale our lifecycle experimentation without having to scale our team size." - Dillon Nuanes*


###
The Challenge: Personalization Breaks When Complexity Becomes Real


Every lifecycle marketer wants to personalize onboarding. The problem isn’t intent, it’s scale.


For ClickUp, early activation hinged on getting new users to take the *right first actions* : creating views, tasks, dashboards, and sending chats. These actions are strong predictors of long-term retention, but influencing them early is hard. At that stage, users don’t yet have rich behavioral history. What *does* exist is workspace-level context.


The challenge was clear:


**How do you use that context meaningfully, without turning lifecycle into an unmanageable mess of segments, rules, and tests?**


### The Reality of “Just a Few Dimensions”


ClickUp already had several practical dimensions that any marketer would want to use.


##### Company segment (derived from team size)


-


Micro


-


SMB


-


Mid-market


-


Enterprise


##### Workspace role


-


Owner


-


Admin


-


User


-


Guest


##### Workspace department / function


-


Creative & Design


-


Marketing


-


Engineering & Product


-


Operations


-


Sales & CRM


-


PMO / Project Management


-


Professional Services


-


Finance & Accounting


-


HR & Recruiting


-


Support


-


Startup


-


Leadership


-


Education


-


Healthcare


-


Personal Use


-


Other


Individually, these dimensions are straightforward. Together, they create a combinatorial explosion.


Just combining:


-


4 company segments


-


4 workspace roles


-


15+ departments


results in **200+ meaningful audience combinations,** *before* accounting for journey type or activation goal.


This is where most lifecycle programs stop. Not because personalization isn’t valuable, but because **humans can’t manage this decision space manually** .


### How ClickUp Used JustAI to Change the System


Instead of asking marketers to guess which segments mattered most or pre-define hundreds of rules, ClickUp used **JustAI’s contextual bandit capabilities** to let performance emerge dynamically.


The shift was subtle but fundamental:


-


Marketers defined the **dimensions** that could matter


-


Marketers defined a small set of **lifecycle strategies**


-


JustAI handled the learning, prioritization, and traffic allocation


No static segments.
No hardcoded logic.
No manual rebalancing.


### How Learning Happened in Practice


ClickUp started with a limited number of high-level lifecycle strategies designed to guide early users toward activation. These strategies were sent broadly across new users, spanning all combinations of company segment, role, and department.


For every send, JustAI evaluated performance across:


-


Engagement signals


-


Downstream activation actions (creating tasks, views, dashboards, chats)


As signals came in, the system continuously adjusted:


-


Which strategy each cohort saw


-


How much traffic each strategy received


-


Which combinations deserved more exploration vs. exploitation


The result was **dynamic traffic distribution at the cohort level** .


*Simulation of how AI distributed a 100 sends across 3 emails for each combination of segment:*


For example:


-


A *Micro + Owner + Healthcare* workspace might quickly converge toward one dominant strategy


-


A *Micro + User + Creative* workspace might split traffic very differently


-


Other combinations continued exploring until clear patterns emerged


At no point did a marketer need to:


-


Create separate campaigns for each combination


-


Decide winners upfront


-


Pause, restart, or rebalance tests


The system learned in flight.


###
What This Enabled Across Onboarding Journeys


With this foundation in place, ClickUp applied the same approach across multiple onboarding journeys, all focused on driving first-value actions.


##### A snapshot of what ClickUp could achieve against baselines within weeks


-


📈 **+89%** create view rate *(Inbox)*


-


📈 **+37%** create view rate *(Dashboards)*


-


📈 **+26%** click rate *(Red Pill – Users/Guests)*


-


📈 **+20%** create task rate *(Tasks)*


-


📈 **+19%** create view rate *(Admins/Owners)*


-


📈 **+12%** view rate *(Short CTA: “Start Now”)*


These weren’t surface-level engagement wins.


They were **core product actions** that signal real value realization.


###
The Real Outcome: Reusable Lifecycle Intelligence


Because JustAI tracks performance at the cohort level, ClickUp didn’t just see lifts, they uncovered **repeatable lifecycle patterns** .


##### Role-specific strategies mattered


Owners and Admins consistently responded to strategies that emphasized structure, visibility, and setup. Execution-oriented users responded better to strategies focused on speed and getting work done.


##### Company size changed framing


Micro and SMB teams favored immediacy and simplicity. Larger teams responded to strategies emphasizing scale, standardization, and operational control.


##### Department context shaped activation paths


Healthcare, PMO, and Professional Services teams gravitated toward centralization and governance-oriented strategies. Creative and Marketing teams responded more strongly to flexibility and organization of work.


These insights didn’t live in a slide deck. They became **inputs into future lifecycle strategy** , without requiring new segmentation or test setup.


### Why This Matters for Lifecycle Marketers


This wasn’t about sending better emails. It was about changing how lifecycle decisions are made.


Instead of:


-


Manually defining segments


-


Running isolated A/B tests


-


Learning slowly and locally


ClickUp moved to a system where:


-


Hundreds of audience combinations learn simultaneously


-


Traffic shifts automatically based on real outcomes


-


Marketers focus on strategy, not orchestration


**Marketing strategists defined what could matter. AI figured out where it actually did.**


### Conclusion


ClickUp treated lifecycle as a decisioning problem, not a messaging problem, and built a system that could learn at the same scale as their user base. By defining the right dimensions, embracing real complexity, and letting performance, not guesswork, drive prioritization, ClickUp turned what would normally be an unmanageable personalization challenge into a repeatable, scalable operating model for lifecycle.


A huge kudos to[Dillon Nuanes](https://www.linkedin.com/in/dnuanes/) **, Director of Lifecycle and Retention at ClickUp** , for driving this innovation forward. Dillon and his team set a strong example of what the next generation of lifecycle marketing looks like - where complex strategies scale through systems, not headcount.
