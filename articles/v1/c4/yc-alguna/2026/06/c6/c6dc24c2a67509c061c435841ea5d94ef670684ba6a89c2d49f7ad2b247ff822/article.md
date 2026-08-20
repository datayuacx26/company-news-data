---
schema_version: "1.0.0"
document_id: "c6dc24c2a67509c061c435841ea5d94ef670684ba6a89c2d49f7ad2b247ff822"
company_key: "yc-alguna"
company: "Alguna"
source_id: "yc-alguna-rss-1656ebd3f341"
canonical_url: "https://blog.alguna.com/ai-dunning/"
published_at: "2026-06-29T20:06:54+00:00"
first_seen_at: "2026-07-24T15:12:55.315719+00:00"
fetched_at: "2026-07-28T20:47:51.174373+00:00"
content_hash: "sha256:1be872d009a5a30d7d61e3837010b6184be8d6f6768dd7198e3e2136b9bc39c2"
---

# AI dunning: A practical guide for finance teams

Chasing overdue invoices is one of the most predictable drains on a finance team's week. The work is repetitive, the cadence is rigid, and the hours rarely match the results.


[43% of the value of B2B credit sales](https://group.atradius.com/knowledge-and-research/reports/b2b-payment-practices-trends-in-north-america-2025?ref=blog.alguna.com) in the US is currently tied up in overdue invoices, which is cash that should be funding payroll, hiring, and growth instead of being ignored in someone's inbox.


AI dunning *changes* the economics of that work.


Instead of firing the same template on a fixed timer, dunning AI reads what customers actually say, adapts its outreach, and escalates only the accounts that need a person. (They key is context.)


In this guide, we'll cover what AI dunning is, how it differs from traditional dunning, the best practices that make it effective, and today's top AI tools for automated dunning workflows and invoice reminders.


## What is AI dunning?


ℹ️


****AI dunning is the use of machine learning to manage the dunning process**** , meaning the sequence of reminders and conversations a business uses to collect payment on overdue invoices.


Traditional dunning runs on a fixed schedule. Dunning AI runs on context. The most autonomous form of AI dunning is an AI dunning agent, which we cover below.


Where a basic reminder tool sends the same message on a timer, a dunning AI system can:


- Predict which accounts are likely to pay late, based on their payment history
- Prioritize outreach so your team works the highest-risk accounts first
- Draft and send follow-ups that match each customer's situation and tone
- Read replies, capture promises to pay, and pause outreach automatically
- Detect disputes and route them to a person with full context


The result is a shift from a reminder engine to something closer to a collections specialist that works your entire aging in the background.


For a wider view of how this fits the receivables function, see our guide to[AI in accounts receivable](https://blog.alguna.com/ai-accounts-receivable/) and how an[accounts receivable AI agent](https://blog.alguna.com/accounts-receivable-ai-agent/) handles collections end to end.


## AI dunning vs AI dunning agent: What's the difference?


**Cases handled by Alguna's agent.**


These two terms get used interchangeably, but they describe different things. AI dunning is the approach: using AI to run the collections process. An AI dunning agent is the thing that does it, an autonomous worker that carries out AI dunning on your behalf.


The distinction matters when you're comparing tools. Plenty of platforms offer AI dunning in the sense of risk scoring and templated personalization, yet still expect a person to operate the system and approve every message.


An AI dunning agent goes further. It opens a case for each overdue account, drafts and sends follow-ups on your cadence, reads the replies, captures promises to pay, pauses on disputes, and escalates only the exceptions, all within the guardrails and autonomy level you set.


Put simply, AI dunning describes what's being done, and an AI dunning agent describes who, or rather what, is doing it. The agent is what turns dunning from a task your team operates into a process that largely runs itself.


Dimension


AI dunning (the approach)


AI dunning agent (the actor)


What it is


An approach: using AI to run the dunning process


An actor: the autonomous software that performs that process


What it does


Scores risk, personalizes messages, and automates parts of outreach


Owns the workflow end to end, from drafting to escalation


Autonomy


Often still needs a person to run and send


Acts on its own within the limits you set


Where it lives


A capability that can sit inside a billing or AR tool


A dedicated agent with its own cases, queue, and audit trail


Your team's role


Operate the tooling and approve each step


Review exceptions while the agent handles the routine


Not every tool labeled “AI” includes a genuine agent. As you evaluate options, the question to ask is whether the platform acts on customer replies on its own, or whether it simply hands your team a smarter to-do list.


## AI dunning vs traditional dunning


The word “AI” gets attached to a lot of tools that are functionally scheduled reminders. The distinction that matters is whether the platform understands what the customer said and adapts. Here's how the two approaches compare.


Dimension


Traditional dunning


AI dunning


What triggers outreach


A fixed calendar (day 7, day 14, day 30)


Invoice events, due dates, risk scores, and the customer's last action


Personalization


Merge fields like name and amount


Tone, context, and history matched to each account


Customer replies


Ignored, or manually triaged from a shared inbox


Read and classified in real time, with promises and disputes detected


Disputes


Outreach keeps firing until someone notices


Cadence pauses, the case is flagged, and a human steps in with context


Prioritization


Every account treated the same


Highest-risk and highest-value accounts surfaced first


Team effort


Hours of copy-paste follow-ups


Humans review exceptions, not the whole aging report


This is not a small efficiency gain.[McKinsey](https://www.mckinsey.com/capabilities/strategy-and-corporate-finance/our-insights/how-finance-teams-are-putting-ai-to-work-today?ref=blog.alguna.com) reports that finance teams using AI robustly spend 20 to 30% less time crunching data, and that agentic workflows are now enabling the next level of automation across both payable and receivable processes.


Dunning is one of the clearest places to capture that time back.


## How AI personalized dunning emails work


**Alguna's agent chasing payments over email.**


Generic dunning blasts are easy to ignore. AI personalized dunning emails are harder to dismiss because they read like a person wrote them with the account in mind. Under the hood, a few things are happening at once.


- Context: the agent pulls the invoice amount, due date, aging, and prior contact history before it writes anything
- Tone: a configurable persona keeps early reminders warm and lets escalated balances get appropriately firm, all from your domain and in your brand's voice
- Reply reading: when the customer responds, the agent classifies the intent, whether that's a payment confirmation, a promise to pay, a dispute, or a document request
- Promise capture: if the customer commits to a date, the agent logs the promise, links it to the invoice, and pauses outreach until the promise date passes


The payoff is outreach that feels personal at a scale no team could match by hand. It's the same logic behind[intelligent collections](https://blog.alguna.com/intelligent-collections/) , where the goal is to work smarter on the accounts that matter rather than sending more email to everyone.


## 7 best practices for AI dunning


Getting value from AI dunning is less about the model and more about how you set it up. These practices separate the teams that move their numbers from the teams that just automate the noise.


1. **Start with clean data.** Accurate invoice records, contact details, and payment history are what let the agent predict risk and personalize outreach. Garbage in, generic out.
2. **Segment by risk and value.** A $500 SMB balance and a $50,000 enterprise balance shouldn't get the same treatment. Let the system prioritize so your team spends attention where it pays off.
3. **Keep a human in the loop early.** Start in a suggest or monitor mode where the agent drafts and a person approves, then increase autonomy as your confidence grows.
4. **Set hard guardrails.** The agent should never make legal threats, admit fault, or offer discounts it isn't authorized to give. Bake those limits in rather than relying on careful configuration.
5. **Personalize tone, not just merge fields.** Swapping in a first name isn't personalization. The message should reflect the relationship and the stage of the conversation.
6. **Anchor cadences to events, not arbitrary days.** Tie steps to invoice sent, due date, last action, or promise date so the timing always makes sense to the customer.
7. **Measure DSO and recovery, then tune.** Track the metrics that prove the system is working and adjust from there. Our guides to[DSO management](https://blog.alguna.com/dso-management/) and[how to reduce DSO](https://blog.alguna.com/how-to-reduce-dso/) are good starting points, alongside our[B2B collections best practices](https://blog.alguna.com/b2b-collections-best-practices/) .


## How to implement AI dunning in 6 steps


You don't need to rip out your stack to get started. A measured rollout protects cash flow and builds trust in the agent before you hand it more control.


1. **Map your current dunning process.** Write down every reminder, owner, and escalation you run today. You can't automate a process you haven't defined.
2. **Connect your data sources.** Link your billing system, CRM, and accounting tools so the agent works from a single, accurate view of each account and the wider[invoice-to-cash cycle](https://blog.alguna.com/invoice-to-cash-cycle/) .
3. **Define cadences and personas.** Set your multi-step sequences and the tone for each stage, from a friendly first nudge to a firmer final notice.
4. **Set autonomy and approval rules.** Decide what the agent can send on its own and what needs sign-off, often tied to invoice amount.
5. **Pilot on a segment.** Run the agent on one slice of your aging in suggest mode, review its drafts, and confirm the classifications are right before you widen the scope.
6. **Scale and monitor.** Expand coverage, raise autonomy where the agent has earned it, and keep an eye on recovery and[cash application](https://blog.alguna.com/accounts-receivable-cash-application/) accuracy. Teams running[usage-based billing](https://blog.alguna.com/usage-based-billing) should confirm the system handles their model before scaling.


## Top AI tools for automated dunning workflows and invoice reminders


Not every platform that markets AI does real reply reading, promise capture, and dispute handling.


Below are three tools worth a look, each suited to a different kind of team.


For a wider field, see our full breakdown of[accounts receivable AI software](https://blog.alguna.com/accounts-receivable-ai-software/) and our roundup of[AI-driven AR platforms](https://blog.alguna.com/ai-driven-ar-platforms/) .


Tool


Best for


Standout strength


Alguna


Scaling B2B SaaS and AI-native teams that want collections, billing, and quoting in one place


An autonomous AR Agent and Control Tower workspace, plus an autonomy ladder and hard-coded guardrails


Monk


Mid-market finance teams with a dedicated collections function


AI collections prioritization and 99.8% cash application accuracy


Paraglide


High-volume teams where the billing inbox is the bottleneck


Conversational agents that handle two-way email in 100+ languages


### Alguna


**Alguna's accounts receivable AI agent - Dashboard.**


**We built**[Alguna](https://alguna.com/?ref=blog.alguna.com) **as an end-to-end revenue automation platform, and the AR Agent is the part that handles dunning.**


It opens a case for every overdue account, drafts and sends follow-ups on your cadence, reads replies, captures promises to pay, pauses itself on disputes, and escalates edge cases to your team, all from a workspace we call Control Tower. An autonomy ladder lets you start in monitor or suggest mode and graduate to fuller autonomy as confidence builds, and hard-coded guardrails keep the agent from making legal threats or unauthorized offers.


Because the platform is modular, you can run the AR Agent on its own or connect it to quoting, billing, and revenue recognition, with support for[usage-based](https://blog.alguna.com/usage-based-pricing/) , seat-based, and[hybrid pricing](https://blog.alguna.com/hybrid-pricing/) that most dunning tools can't touch. Alguna is SOC 2 and GDPR compliant.


### Monk


Monk is an[intelligent collections platform](https://blog.alguna.com/intelligent-collections-software/) built for mid-market teams with a dedicated AR function. Its strengths are collections prioritization, where machine learning scores each account's payment likelihood, and cash application, where it reports 99.8% matching accuracy across ACH, wire, card, and check, including parent-child hierarchies.


It's primarily a collections and cash application tool rather than a full billing platform, so it tends to layer onto an existing stack.


### Paraglide


Paraglide takes a conversational approach. Rather than automating reminder sequences, it deploys agents that handle two-way billing communication, reading and responding to customer queries, document requests, and payment confirmations across more than 100 languages. That makes it a strong fit for high-volume, international teams where the billing inbox is the real bottleneck.


As an early-stage product focused on the communication layer, it's less suited to complex billing models or full[quote-to-cash workflows](https://blog.alguna.com/quote-to-cash-process/) .


## Stop chasing, start collecting


The gap between teams that get paid on time and teams that don't is increasingly about tooling, not effort. AI dunning removes the manual follow-ups, catches the replies that used to slip through the cracks, and gives finance a real-time picture of what's outstanding.


Start small, set clear guardrails, measure the impact on DSO, and expand from there.


If you'd like to see how Alguna's AR agent would handle your collections,[book a demo](https://alguna.com/book-a-demo?ref=blog.alguna.com) and we'll walk through it with your use case in mind.
