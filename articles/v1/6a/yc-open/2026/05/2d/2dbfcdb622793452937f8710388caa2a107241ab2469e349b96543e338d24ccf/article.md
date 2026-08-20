---
schema_version: "1.0.0"
document_id: "2dbfcdb622793452937f8710388caa2a107241ab2469e349b96543e338d24ccf"
company_key: "yc-open"
company: "Open"
source_id: "yc-open-news-import-9b3d55ca046e"
canonical_url: "https://www.open.cx/blog/how-much-support-can-ai-automate"
published_at: "2026-05-13T00:00:00+00:00"
first_seen_at: "2026-07-22T07:27:48.081524+00:00"
fetched_at: "2026-07-28T21:44:24.455372+00:00"
content_hash: "sha256:5622e432588dd86a5b0258f03324cbd8e85ea70b4c6529e8b3b4e6072714d91b"
---

# How much customer support can AI actually automate?

Every AI customer service vendor publishes a different number. Some say 30%. Some say 50%. A few say 80%. Klarna said 75% in early 2024 and walked it back by 2025. The numbers aren't lies. They're measuring different things, and the gap between them is where the real conversation lives.


The honest answer for most B2C and B2B SaaS teams in 2026 is 60% to 80% of ticket volume is automatable with current technology. The remaining 20% to 40% is where the actual customer experience value lives, and that's the part most automation pitches quietly ignore.


## TL;DR


- Vendor automation rates range from 30% to 80% because they measure different things. Some count any conversation without escalation; some require verified resolution.
- For B2C and B2B SaaS, 60% to 80% of ticket volume is realistically automatable. Industries with heavy compliance, fraud, or high-touch sales have lower ceilings.
- The variables that move the number 20 points in either direction: knowledge base quality, API access depth, ticket category mix, and observability discipline.
- What stays human is more interesting than what gets automated. The complex 20% to 40% is where CSAT, retention, and customer loyalty get made.
- You can estimate your own ceiling in about 30 minutes using a basic ticket category audit.


## Why every vendor's automation rate is technically true and practically useless


The most useful exercise when reading a vendor's automation rate is to ask three questions: what does "resolved" mean, what's the denominator, and over what time window.


[Intercom's Fin defines a resolution as a conversation where the customer either confirms the answer worked or exits the conversation without asking for more help](https://www.intercom.com/help/en/articles/8205718-fin-ai-agent-outcomes) . The "exit without asking" portion is generous; some of those customers gave up and went to a competitor or called the bank. Fin reports an average resolution rate that grows roughly 1% per month, and top-performing customers in e-commerce reach 70% to 84%.


[Klarna's published numbers from 2024](https://www.usefini.com/blog/klarna-automates-two-thirds-of-customer-service-with-ai-assistant) said 2.3 million conversations in the first month, 75% of all customer service chats handled, equivalent to 700 full-time agents, average resolution time down from 11 minutes to under 2, and a 25% reduction in repeat inquiries. The numbers are real. They're also the headline of a press release.


[By May 2025, Klarna's CEO said the company had cut too far](https://www.entrepreneur.com/business-news/klarna-ceo-reverses-course-by-hiring-more-humans-not-ai/491396) and was hiring humans back. "We focused too much on efficiency and cost. The result was lower quality, and that's not sustainable." The CSAT story didn't make the press release that the deflection story did.


This isn't a Klarna problem. It's the pattern across the industry. The numbers vendors publish are real for what they measure. They are usually not the number you'd care about if you ran the support team.


## The honest breakdown by ticket type


A more useful frame than "what's the automation rate" is "what's automatable by ticket type." The ranges below come from observed deployments across B2C SaaS, e-commerce, and fintech.


### Automation rate by ticket type


Higher leverage at the top


- Order / account status


85–95%


Pure API lookup, deterministic


- Password & access


75–90%


Bounded action, clear success


- Refunds within policy


70–85%


Policy as code, audit trail


- Policy & procedure lookups


70–85%


Pure retrieval, depends on docs


- Returns & exchanges


60–80%


Multi-step but standardized


- Product troubleshooting


30–70%


Wide range based on doc quality


- Billing disputes


40–60%


Some judgment, often emotional


- Complex account configuration


20–50%


Variable, often needs human


- Compliance, fraud, legal


0–10%


Should not be automated


- New product feedback


0%


Belongs with humans


Strong


Variable


Limited


Should stay human


The overall automation rate depends on how much of your volume sits in the top rows versus the bottom. A retailer with 50% order status and 20% returns has a higher ceiling than a fintech with 40% billing disputes and 20% fraud-related work.


## The variables that move the number 20 points in either direction


Two teams with identical ticket mix can hit very different automation rates. The variables that explain most of the gap:


### Knowledge base quality


The single largest variable, and the most often skipped. If your help center has contradictions, outdated articles, or three versions of the same answer, retrieval performance suffers. The AI either picks the wrong article or hedges and escalates.


Cleaning the top 50 articles by traffic typically improves resolution rate 10 to 15 points on retrieval-driven categories. Most teams don't budget for this work and absorb the cost as lower performance.


### API access depth


The second-largest variable. A team with one API endpoint exposed (order lookup) automates maybe 25% of volume. A team with eight to ten endpoints (orders, refunds, returns, address changes, password reset, plan changes, credit applications, subscription pauses) automates 65% to 75%.


This isn't an AI capability gap. It's an integration project that has to happen before automation works.


### Ticket category mix


The variable you don't control. A SaaS company with 60% billing and account work has a higher ceiling than one with 60% complex technical questions, regardless of which AI vendor either uses. Acknowledging this is honest. Pretending the AI is the variable is sales.


### Observability and tuning discipline


Teams that sample 100% of AI conversations for the first two weeks, then sample the bottom 10% by confidence ongoing, catch failures fast. Teams that don't, accumulate quiet bad outputs that show up as CSAT drops and recontacts.


The teams that sustain high resolution rates aren't running fancier models. They're running tighter feedback loops.


## What stays human, and why this is a feature


The 20% to 40% that doesn't automate isn't a problem to solve. It's the actual customer experience job.


What lives there:


- **Refunds outside policy.** Where the right answer depends on customer lifetime value, relationship history, and judgment about whether bending the policy is worth it.
- **Account fraud and security escalations.** Where mistakes are expensive and the customer is often distressed.
- **Billing disputes that require investigation.** Where you need to look at the customer's actual usage data and make a judgment call.
- **Complex onboarding and account configuration.** Where the customer needs hand-holding and the AI doesn't have the context.
- **Complaints and escalations.** Where the customer is upset and an AI response, however polite, makes them more upset.
- **VIP and high-value customer interactions.** Where the relationship matters more than the efficiency.
- **New product feedback and edge cases.** Where the customer is telling you something you need to hear.


When you automate the routine 60% to 80%, the remaining work gets harder and more valuable, not easier. The team that used to spend the day on "where is my order" now spends the day on the work that actually drives retention. The hours saved don't vanish from CSAT. They get reinvested in the customers who need real attention. See:[AI to human support handoff](https://www.open.cx/blog/ai-human-support-handoff) .


This is the reframe most automation programs miss. The goal isn't to get to 100%. The goal is to get the[routine to 70%](https://www.open.cx/blog/automate-70-percent-support-tickets) so the humans can do the 30% well.


## Benchmarks by industry


Rough ranges for what's realistic by industry, based on deployment data across the last 24 months.


Industry Realistic automation ceiling Notes


E-commerce 70-85% High order-status share, codifiable returns


Subscription SaaS (B2C) 65-80% Account, billing, plan changes automatable


Subscription SaaS (B2B) 50-70% More custom config, lower volume


Fintech (consumer) 50-70% Compliance and fraud cap the ceiling


Marketplaces (two-sided) 60-75% Both sides have routine work; disputes are complex


Travel and hospitality 60-80% Bookings, changes, status; complex when things go wrong


Healthcare consumer 40-60% Compliance constraints, sensitive context


Enterprise B2B SaaS 30-50% Lower volume, higher complexity per ticket


Telecom 50-70% Technical troubleshooting is variable


These are not benchmarks for performance. They are ceilings. A team in any of these industries can underperform their ceiling. Few will significantly exceed it without changing what they sell.


## How to estimate your own ceiling in 30 minutes


You don't need a consulting engagement. You need 30 minutes and last quarter's tickets.


**Step 1.** Pull your top 20 ticket categories by volume from the last 90 days.


**Step 2.** Add a column for "is this a routine action with a clear policy, or does it require judgment?"


**Step 3.** For the routine column, mark which of these are true:


- The information exists in our help center
- The action requires only one or two API calls
- The success criteria are clear


**Step 4.** Sum the volume of categories where all three are true. That's roughly your retrieval-and-API-driven automation ceiling.


**Step 5.** Add categories where two of three are true at 50% credit. That's your stretch ceiling.


For most B2C SaaS and e-commerce teams, this exercise lands somewhere between 55% and 80%. That's your starting expectation. Vendor pitches that promise 90%+ are either selling on best-case scenarios or measuring differently.


[Zendesk's 2025 CX Trends Report](https://www.zendesk.com/newsroom/articles/2025-cx-trends-report/) , which surveyed nearly 5,100 consumers and 5,400 CX leaders across 22 countries, found 75% of CX leaders expect 80% of customer interactions to be resolved without human intervention in the next few years. That's a forward-looking projection, not a current state.[Salesforce's State of Service report](https://www.salesforce.com/news/stories/state-of-service-report-announcement-2025/) puts AI handling at 50% by 2027, up from around 30% today. Both numbers are aspirational vendor data. Treat them as direction.


## Related guides


Once you know your ceiling, these guides cover how to actually reach it.


- [How to automate customer support with AI](https://www.open.cx/blog/how-to-automate-customer-support-with-ai) : the operations playbook for closing the gap between your ceiling and your current rate.
- [Fin vs dedicated AI agents](https://www.open.cx/blog/fin-vs-dedicated-ai-agents) : how platform choice affects the API access depth that moves your number 20 points.
- [Reduce Intercom costs](https://www.open.cx/blog/reduce-intercom-costs) : if you're on Intercom, where automation actually translates into lower spend.


## A final note


The interesting question isn't how much support AI can automate. It's whether the team you have today can run the automation discipline required to hit the ceiling for your industry. The technology is rarely the limit in 2026. The deployment work is. Teams that hit 70%+ aren't running a better chatbot than teams stuck at 30%. They have a cleaner knowledge base, more APIs exposed, and tighter feedback loops on what the AI says to whom.


The automation rate is a lagging indicator of operational maturity. Look at your team's appetite for that maturity, and you'll know your real ceiling.
