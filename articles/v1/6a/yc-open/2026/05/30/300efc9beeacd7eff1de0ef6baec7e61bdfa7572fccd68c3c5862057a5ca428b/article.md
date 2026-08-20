---
schema_version: "1.0.0"
document_id: "300efc9beeacd7eff1de0ef6baec7e61bdfa7572fccd68c3c5862057a5ca428b"
company_key: "yc-open"
company: "Open"
source_id: "yc-open-news-import-9b3d55ca046e"
canonical_url: "https://www.open.cx/blog/what-is-csat"
published_at: "2026-05-13T00:00:00+00:00"
first_seen_at: "2026-07-22T07:27:48.081524+00:00"
fetched_at: "2026-07-28T21:44:24.455372+00:00"
content_hash: "sha256:59ac9d3283aeb4e5abdb3f6eb64dc128ee3e1cc206d1cc0eed5abf60ff358446"
---

# What is CSAT? A complete guide, with a score calculator

CSAT is the simplest customer satisfaction metric and the one most teams measure first. It's also one of the most misused. Teams compare their score against the wrong benchmarks, change how they measure it without telling anyone, or chase the number without understanding what's actually moving.


This guide covers what CSAT actually is, how to calculate it correctly, what a good score looks like in your industry, and the practical things that move it. There's also a CSAT calculator at the bottom for direct use.


## TL;DR


- CSAT (Customer Satisfaction Score) measures the percentage of customers who rated their experience as satisfied or very satisfied (usually 4 or 5 on a 5-point scale).
- Formula: (Number of satisfied responses ÷ Total responses) × 100.
- The cross-industry average sits around 76% to 78%. Anything above 80% is strong; anything below 65% is a warning sign.
- Benchmarks vary by industry. Consulting averages 83%, banking around 79%, e-commerce around 80%, ISPs around 68%. Compare within your industry, not across.
- The metric is useful but limited. CSAT tells you how customers felt about a specific interaction; it doesn't tell you why or what to fix. Pair it with qualitative feedback and recontact rate.


## What CSAT is


CSAT (Customer Satisfaction Score) is a survey-based metric that captures how satisfied customers are with a specific interaction, product, or service. It's typically measured by asking customers a single question after a touchpoint:


> "How satisfied were you with \[your experience / our service / your support agent\]?"


The customer rates their satisfaction on a scale, usually 1 to 5 or 1 to 7. The CSAT score is the percentage who rated at the top of the scale (the "satisfied" or "very satisfied" responses).


The metric is interaction-specific. It captures sentiment about a particular moment (a support ticket, a purchase, an onboarding session), not the overall customer relationship. For overall relationship measurement, NPS (Net Promoter Score) or CES (Customer Effort Score) are the alternatives.


## How to calculate CSAT


The most common method, sometimes called Top-Two-Box or T2B, is:


**CSAT = (Number of "satisfied" or "very satisfied" responses ÷ Total responses) × 100**


A 5-point scale example:


- 150 responses total
- 60 rated 5 ("very satisfied")
- 50 rated 4 ("satisfied")
- 25 rated 3 ("neutral")
- 10 rated 2 ("dissatisfied")
- 5 rated 1 ("very dissatisfied")
- CSAT = (60 + 50) ÷ 150 × 100 = **73.3%**


Some teams use the Top-Box method (only the highest rating counts) or weighted scoring (each rating gets a different value).[Retently's CSAT guide](https://www.retently.com/blog/customer-satisfaction-score-csat/) covers the variants. The Top-Two-Box is the industry default, and it's what most published benchmarks measure.


A note on consistency: if you change the calculation method, your historical comparisons break. Pick one method and stick with it.


## Industry benchmarks for 2026


CSAT varies meaningfully by industry. A 75% score is mediocre in some industries and excellent in others. The cross-industry average sits around 76% to 78% per[Salesforce data](https://www.salesforce.com/) , but the spread by industry is wide.


Industry Average CSAT (2025-2026) Source/notes


Consulting 83%[SurveySparrow benchmarks](https://surveysparrow.com/blog/csat-benchmarks/)


Digital marketing agencies 83% Up significantly from prior year


Financial services 81%[Salesforce/ACSI data](https://www.salesforce.com/)


E-commerce 80% Online retail benchmark


Banking 79%[ACSI 2024-2025](https://www.theacsi.org/)


SaaS 78-80% Mid-market and enterprise SaaS


Healthcare 80%[ACSI 2024-2025](https://www.theacsi.org/)


Retail 76% General retail


Telecom 70% Historically lower industry


ISPs 68% Lowest-scoring industry consistently


What "good" means depends on context. A 75% CSAT for a consulting firm signals a problem; for an ISP, it's excellent. Always compare within your industry.


## CSAT score calculator


A simple calculator to compute your own CSAT.


```text
CSAT Calculator


Survey responses:
- Number who rated 5 (very satisfied): [____]
- Number who rated 4 (satisfied):      [____]
- Number who rated 3 (neutral):         [____]
- Number who rated 2 (dissatisfied):    [____]
- Number who rated 1 (very dissatisfied): [____]


Total responses: [calculated]
Satisfied count (4 + 5): [calculated]


Your CSAT score = (Satisfied count ÷ Total responses) × 100 = [calculated]%


```


The math is the same on a 7-point scale (count responses at 6 and 7 as "satisfied") or a 10-point scale (typically 9 and 10 as the top box, similar to NPS).


## What moves CSAT


The metric is a lagging indicator. Several inputs drive it.


### Response time


The faster a customer gets a useful response, the higher CSAT trends.[Industry data consistently shows](https://www.fullview.io/blog/customer-support-metrics) that first-response time under 1 hour correlates with 10 to 15 point higher CSAT than 24+ hour first response.


### Resolution rate


Whether the customer's issue actually gets solved. CSAT on resolved tickets averages 20 to 30 points higher than on tickets that escalated or were closed without resolution.


### Effort required


How hard the customer had to work to get a resolution. Customers who had to explain their situation three times to three different agents score 30+ points lower than customers who got resolution in one interaction.[Customer Effort Score (CES)](https://hbr.org/2010/07/stop-trying-to-delight-your-customers) measures this directly, but it shows up in CSAT too.


### Tone and empathy


Customers who felt heard rate higher than customers who got correct answers delivered coldly. This is the variable where humans typically beat AI. The CSAT gap between AI-handled and human-handled tickets is mostly here.


### Recovery on bad experiences


Counterintuitively, customers who had a problem that was handled well sometimes rate higher than customers who had no problem. The "service recovery paradox" is real if smaller than older research suggested. The corollary: badly handled bad experiences are CSAT disasters.


## Where CSAT goes wrong as a metric


Useful, but not sufficient. Common failure modes.


### Survey response bias


The customers who respond are not the typical customers. Very satisfied and very dissatisfied customers respond more often than neutral ones. The middle of the distribution is underrepresented. This can either inflate or deflate scores depending on the population.


### Survey timing


A CSAT survey sent five minutes after a positive resolution scores higher than the same survey sent three days later. Different timing produces different numbers.


### What's being measured


A survey asking "how satisfied were you with your support agent" measures different things than "how satisfied were you with the resolution." Both are CSAT; both produce different numbers.


### Single-touchpoint focus


CSAT measures one interaction. The customer who had three bad interactions and one good one might rate the good one at 5, masking the broader problem.


### Survey fatigue


Customers receiving multiple CSAT surveys per week respond less and rate lower over time. Frequent surveying hurts the metric and the relationship.


The fix isn't to abandon CSAT but to use it with other metrics: NPS for relationship sentiment, CES for effort, recontact rate for actual resolution, and qualitative feedback for the why.


## CSAT for AI-handled vs. human-handled tickets


A question that's increasingly important. The pattern in 2026:


For routine, well-defined ticket categories (order status, password reset, refunds within policy), AI-handled CSAT lands within 3 to 5 points of human-handled. Sometimes higher, because AI responds faster and is consistent.


For complex, emotional, or judgment-driven tickets, AI-handled CSAT lands 10 to 20 points lower than human-handled. The pattern: customers want human contact when the problem is hard.


The teams that maintain overall CSAT after deploying AI route the right tickets to each channel. AI handles what it's good at; humans handle what they're good at. Trying to push AI into the wrong territory shows up as CSAT decline. See:[how much support can AI automate](https://www.open.cx/blog/how-much-support-can-ai-automate) for realistic deflection ranges, and[how to automate customer support with AI](https://www.open.cx/blog/how-to-automate-customer-support-with-ai) for the rollout playbook.


[Klarna's 2025 reversal](https://www.entrepreneur.com/business-news/klarna-ceo-reverses-course-by-hiring-more-humans-not-ai/491396) is the most public example. The CEO publicly admitted: "We focused too much on efficiency and cost. The result was lower quality." CSAT was the canary.


## How to actually improve your CSAT


A few high-leverage activities, in rough order of impact.


### 1. Audit your slowest tickets


Pull the 5% of tickets with the longest first response time. Those are your CSAT killers. Faster routing, better AI deflection, or larger human capacity at peak times all help.


### 2. Audit your recontact rate


Tickets the customer had to follow up on score lower than tickets resolved in one interaction. A 20% recontact rate is dragging CSAT down by 5 to 10 points. Reducing it is harder than measuring it, but the payback is large.


### 3. Audit your escalation handoffs


When tickets move between agents (or from AI to human), how well does context transfer? Customers who have to re-explain their problem hate it. Better handoffs are mostly a process and tooling problem. See:[the AI-to-human support handoff](https://www.open.cx/blog/ai-human-support-handoff) for how to preserve context across the transfer.


### 4. Train on empathy, not just procedure


Agent training tends to focus on knowing the right answers. The CSAT differentiator is often how the answer is delivered. "Yes, here's how to do that" lands differently than "I'm sorry, I see this is frustrating. Here's how to do that."


### 5. Close the loop on bad experiences


Customers who left negative CSAT and got a follow-up reach-out often rate their next interaction higher than customers who never had a bad one. The pattern is well-documented in service recovery research.


### 6. Look at CSAT trends, not absolute numbers


A 78% CSAT trending up to 82% is a healthy team. A 90% CSAT trending down to 85% is a problem. The trend is more informative than the snapshot.


## CSAT vs. NPS vs. CES: which one to use


A short note on the alternatives.


**CSAT** measures specific interaction satisfaction. Use for: ticket-level, channel-level, agent-level evaluation.


**NPS (Net Promoter Score)** measures overall relationship loyalty ("How likely are you to recommend us?"). Use for: brand health, longer-term sentiment.


**CES (Customer Effort Score)** measures how much work the customer had to do. Use for: process simplification, friction identification.


Most mature CX teams track all three. They show different things, and the picture comes from combining them.


## Related guides


One more resource for teams measuring CSAT alongside their support tooling.


- [AI ticketing system guide](https://www.open.cx/blog/ai-ticketing-system-guide) : how modern helpdesks classify, route, and resolve tickets, with CSAT among the 90-day targets to watch.


## A final note


CSAT is useful, simple, and easy to measure. It's also incomplete. The teams that treat it as the single number to optimize end up with surveys that fish for high scores instead of feedback that reveals problems. The teams that treat it as one signal among several catch the issues that single metrics miss.


The right way to use CSAT: track it consistently, segment it by category and channel, compare against industry benchmarks, and pair it with the metrics that tell you why. Above all, watch the trend. The score's direction matters more than its absolute value.
