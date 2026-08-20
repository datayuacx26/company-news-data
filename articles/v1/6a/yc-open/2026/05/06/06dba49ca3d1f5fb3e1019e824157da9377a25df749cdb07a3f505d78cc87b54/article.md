---
schema_version: "1.0.0"
document_id: "06dba49ca3d1f5fb3e1019e824157da9377a25df749cdb07a3f505d78cc87b54"
company_key: "yc-open"
company: "Open"
source_id: "yc-open-news-import-9b3d55ca046e"
canonical_url: "https://www.open.cx/blog/automate-70-percent-support-tickets"
published_at: "2026-05-13T00:00:00+00:00"
first_seen_at: "2026-07-22T07:27:48.081524+00:00"
fetched_at: "2026-07-28T21:44:24.455372+00:00"
content_hash: "sha256:f151d6f5078157a4ef61533fb03a128b70b818992807523c28d2ae41f394a353"
---

# How to automate 70% of your support tickets: a real playbook

The 70% target gets thrown around a lot. Vendors quote it on landing pages. Consultants quote it in pitch decks. Most teams trying to hit it land somewhere between 22% and 35%, which is what happens when you automate the FAQ layer and call it done.


The teams clearing 70% aren't running better chatbots. They've automated four specific ticket categories with API-connected workflows, not FAQ retrieval. This piece is the playbook for how that actually works.


## TL;DR


- 70% automation is achievable for most B2C and B2B SaaS teams, but only with API-connected workflows. FAQ deflection alone caps around 30%.
- Four ticket categories get you most of the way there: order/account status, refunds and returns within policy, password/access, and policy lookups.
- The first 30 points come from FAQ-style work. The next 30 to 40 come from giving AI access to your billing, fulfillment, and account APIs.
- The biggest reason teams stall at 30% is treating automation as a knowledge problem when it's actually an integration problem.
- Measure resolution rate (no recontact within 7 days), not deflection rate. Some "deflected" tickets are quietly failing.


## Why 70% is the right target, and why 100% isn't


70% is the rough ceiling where the cost-benefit of additional automation flips. The first 70% of tickets are repetitive, bounded, and predictable. The remaining 30% are where the actual judgment work lives: refunds outside policy, account fraud, complex billing disputes, legal questions, escalated complaints, anything that touches a relationship a CSM owns. That work is where humans add real value, and trying to automate it usually degrades the customer experience.


[Intercom's Fin reports an average resolution rate that increases roughly 1% per month](https://www.intercom.com/help/en/articles/8205718-fin-ai-agent-outcomes) , with top-performing customers in retail and e-commerce sustaining rates of 70% to 84%.[Klarna's deployment hit two-thirds automation in its first month in 2024](https://www.usefini.com/blog/klarna-automates-two-thirds-of-customer-service-with-ai-assistant) , handling 2.3 million conversations and cutting average resolution time from 11 minutes to under 2. Both numbers are real. They are also from teams that did months of integration work before the AI shipped.


100% is a different conversation. The companies chasing 100% have either redefined what counts (any conversation without escalation) or are willing to accept lower CSAT on the long tail.[Klarna walked back parts of its AI-only strategy in 2025](https://www.entrepreneur.com/business-news/klarna-ceo-reverses-course-by-hiring-more-humans-not-ai/491396) for that reason. The CEO publicly said the company had focused too much on efficiency and cost, and customer experience suffered.


## The four ticket categories that get you to 70%


The categories are predictable across most B2C and B2B SaaS support volumes. The mix changes; the categories don't.


### Typical B2C SaaS ticket mix


Four automatable categories ≈ 70%


70%


Automatable


-


Order/account status


28%


85–95% resolution once integrated


-


Refunds & returns (in policy)


14%


70–85% resolution with policy as code


-


Policy & procedure lookups


18%


70–85% on a clean help center


-


Password & access


10%


75–90% via identity provider


-


Complex / judgment / escalations


30%


Humans add real value here


Illustrative mix · varies by industry and product surface


### 1. Order/account status and lookups


The single highest-volume category in e-commerce, fintech, marketplaces, and most subscription businesses. "Where is my order." "What's my current balance." "When does my plan renew." "What's my next billing date."


These are pure API calls. A model looks up the customer, calls one or two endpoints, returns the answer in natural language. Resolution rates of 85% to 95% are standard once the integration exists.


Why teams miss this: they automate the FAQ ("how can I check my order status?") and link to a portal, instead of just answering the question directly. The bot says "click here to check your order"; the customer says "just tell me where it is."


### 2. Refunds and returns within policy


The second-highest leverage category, especially in retail and subscription. A clear policy that's codifiable. The AI checks order eligibility, applies the policy, issues the refund or initiates the return, sends confirmation.


The integration scope is small: refund authorization API, order lookup, refund policy as structured rules. The judgment scope is also small if the policy is clear (e.g., "refunds within 30 days for unused subscriptions, full price"). Resolution rates of 70% to 85% are typical.


The gotcha: out-of-policy requests. A clear escalation path matters here. The customer who's 32 days in shouldn't get a flat "no" from a bot. They should get a human, with context.


### 3. Password resets and account access


A bounded action with clear success criteria. Resolution rates of 75% to 90% are standard, and the integration is usually trivial because most identity providers expose a password reset endpoint already.


This category clears 5% to 12% of total volume in most SaaS teams. It's small individually, but free to automate once everything else is in place.


### 4. Policy and procedure lookups


"Can I share my account?" "What's your refund policy?" "How do international orders work?" "What's your cancellation window?"


This is pure Layer 2 work: retrieval from your help center, answered conversationally. Resolution rates of 70% to 85% depending on how clean the help center is. This category alone can clear 15% to 25% of volume for SaaS, more for marketplaces.


The catch is what makes this category fail. Old articles that contradict newer ones. Same information in three places, slightly different each time. Articles written for SEO instead of for answering questions. If your help center is messy, this category underperforms. Cleaning the top 50 articles by traffic usually does more for resolution rate than swapping AI vendors.


## What "automate" actually means for each category


The word "automate" hides a big difference between the categories. The teams stuck at 30% generally only automate one type of work.


Category What automation looks like Why it works


Order/account status API call:` getOrder(customerId, orderId)` , return data in natural language Customer-specific, deterministic answer, no judgment


Refunds within policy API call:` checkEligibility(orderId)` , then` issueRefund(orderId, amount, reason)` Policy as code, audit trail, bounded action


Password reset API call:` triggerPasswordReset(email)` with identity verification Standardized flow, identity provider handles security


Policy lookup Retrieval from indexed help center, conversational reply No customer data needed, pure information


The first three are API-driven. The fourth is retrieval-driven. Teams that only do retrieval cap their resolution rate around 25% to 30% because they can never actually do anything for the customer.


## The setup work: knowledge, API access, guardrails


This is where most automation programs underinvest. The AI vendor's onboarding usually covers configuration; the integration work is your team's problem.


### Knowledge


The minimum: audit your top 50 to 100 help center articles by traffic. Find and resolve contradictions. Tag articles by topic so retrieval can scope correctly. Mark articles the AI shouldn't use (deprecated content, internal-only docs, articles flagged for legal review).


This usually takes one person two to four weeks. It's not glamorous, but the resolution rate impact is significant.[Knowledge base quality is the single most-cited variable in AI deflection benchmarks](https://www.usepylon.com/blog/ai-ticket-deflection-reduce-support-volume-2025) . See:[Structuring your knowledge base for AI](https://www.open.cx/blog/intercom-knowledge-base-for-ai) .


### API access


For most B2C and SaaS teams, the integration scope is six to ten endpoints:


- Customer/account lookup
- Order or subscription lookup
- Refund authorization (with policy checks)
- Address or profile update
- Password reset trigger
- Order cancellation
- Plan change or upgrade
- Credit application
- Status/health check for the customer's own service or order


The work isn't trivial. Auth, rate limits, error handling, idempotency, audit logging. But it's a one-time build that compounds. Once your AI agent can call` issueRefund(orderId, amount, reason)` , it can resolve thousands of cases a month from that one endpoint.


### Guardrails


What the AI cannot do without human approval. Common ones:


- Refunds outside policy
- Account closure or major plan downgrades
- Anything touching fraud signals
- Anything involving legal or compliance language
- Multi-thousand-dollar transactions
- VIP customer escalations (if you flag them)


The escalation path matters as much as the rule. A bad escalation message ("I can't help with that, please wait for an agent") undoes most of the trust the AI builds with a good first response. A good one summarizes the issue, what the AI tried, and what it couldn't do, then[hands off with context](https://www.open.cx/blog/ai-human-support-handoff) .


## The metrics that matter


The single biggest mistake in AI customer support automation reporting is conflating deflection rate with resolution rate.


- **Deflection rate** : percentage of conversations the AI handled without escalating. Includes customers who gave up and customers who got the wrong answer.
- **Resolution rate** : percentage where the issue was actually solved. Measured by no recontact within 7 days on the same topic.


The gap between these is usually 10 to 25 points. A team reporting 60% deflection with a recontact rate of 30% has an actual resolution rate around 42%. The "saved" tickets came back.


Other metrics worth tracking:


- CSAT on AI-handled tickets vs. human-handled. Should be within 5 points.
- Cost per resolved conversation, including escalations and recontacts.
- Hours returned to the human team (deflected volume × average handle time of those categories).
- Time to first useful response.


## Which category to attack first


A short decision tree based on your ticket mix.


**If you're an e-commerce or subscription business with order status as your top category** : start there. It's the fastest API integration and the highest resolution rate. You can clear 15% to 30% of volume in 4 to 6 weeks.


**If you're SaaS with billing and account access dominant** : password reset and account status first, then refunds within policy. You'll likely hit 30% to 40% within two months.


**If your top category is "general questions" or "product help"** : fix your help center before deploying AI. Clean retrieval beats messy retrieval by a wide margin, and the AI exposure of bad articles can hurt CSAT.


**If you're enterprise B2B with low volume but high complexity** : 70% isn't realistic. Aim for 30% to 50% on the routine categories and keep humans on the rest. The unit economics are different at low volume.


## Common reasons teams stall at 30%


The pattern is consistent across teams that get stuck.


**They only automated Layer 2.** FAQ retrieval works for information requests but caps at the percentage of tickets that don't require customer-specific action. For most B2C and SaaS, that's 25% to 35%.


**The help center is contradictory or outdated.** Retrieval pulls from whatever exists. If three articles say different things, the AI either picks the wrong one or hedges and hands off.


**APIs weren't exposed.** Without API access, the AI can answer questions but can't do anything. The customer asks "cancel my subscription" and gets "here's how to cancel your subscription," which is the worst of both worlds.


**Escalations are unmanaged.** A 60% escalation rate with no context handoff is worse than a 20% escalation rate with clean context. The first feels like a wall; the second feels like a smooth transfer.


**No observability.** The team doesn't know what the AI is saying or where it's failing. Failures are discovered through customer complaints rather than through internal sampling.


## Related guides


To put the 70% playbook in context, these companion pieces go deeper.


- [How to automate customer support with AI](https://www.open.cx/blog/how-to-automate-customer-support-with-ai) : the full operations playbook behind the four-category approach.
- [How much support can AI automate](https://www.open.cx/blog/how-much-support-can-ai-automate) : why automation ceilings vary by industry and how to estimate yours.
- [Fin vs dedicated AI agents](https://www.open.cx/blog/fin-vs-dedicated-ai-agents) : which type of platform actually gets you the API-connected workflows that push past 30%.


## A final note


The 70% target is real, but the path to it isn't a chatbot. It's a knowledge base audit, six to ten API integrations, a clear policy on what the AI can and can't do without human approval, and the discipline to measure resolution rate rather than deflection rate. The teams that hit 70% built all of that. The teams stuck at 30% built one of those things and called it done.
