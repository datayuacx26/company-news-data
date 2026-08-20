---
schema_version: "1.0.0"
document_id: "b33eb7b55fac9a4e995b0971cfecf7e8de920d7e8ead3fc8c8274e915255a924"
company_key: "yc-fini"
company: "Fini"
source_id: "yc-fini-news-import-0b9bde040a70"
canonical_url: "https://www.usefini.com/blog/peaksware-case-study"
published_at: "2026-06-04T00:00:00+00:00"
first_seen_at: "2026-07-25T04:52:41.892283+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:27eec0ef0ab2833d83e57172cccb49e0aeaff19b49e9c0d80dc0331f81ef7ebe"
---

# How Peaksware Cut Support Queues by 70%+ Across Two Brands With Fini

*TrainingPeaks and Train Heroic deployed one Fini agent. 26-second response times. 70%+ of the queue resolved before a human sees it. Users keep replying as if they're talking to a person.*


**70%+** Queue Reduction


**26 seconds** Avg Response Time


12% improvent in CSAT


**2 Brands** Handled by One Fini Agent


## At a Glance


**Industry**


Endurance & strength training software


**Brands**


TrainingPeaks, Train Heroic


**User types**


Coaches, athletes


**Support channel**


Zendesk (email + ticket)


**Peak week volume**


1,700 tickets (TrainingPeaks alone)


**Fini deployment**


Single AI agent across both brands


## The Company


Peaksware builds digital infrastructure for people who train. Triathletes preparing for Ironman, powerlifters following strength programs, coaches managing dozens of athletes. Two of their brands sit at the center of that work: TrainingPeaks (endurance) and Train Heroic (strength).


The brands share a parent but not much else operationally. Each one has its own users, terminology, and support patterns. A coach on TrainingPeaks asks completely different questions than an athlete on Train Heroic. The workflows are incredibly complex and don't overlap.


Kristina Green is Senior Support Rep on TrainingPeaks. Darren Lacy is Senior Support Rep on Train Heroic, where he also runs onboarding and trains the AI agent. Both have been with the company for nearly three years. Support sits at the center of both brands.


## The Problem: A Seasonal Surge That Multi-Brand Complexity Made Worse


Peaksware's support volume follows a brutal seasonal rhythm. It starts around Black Friday. Athletes finishing their fall training start planning next year, buying new training plans, trying new platforms, onboarding coaches. By January, the queue is a flood. It doesn't let up until February.


Most tickets weren't complex, subscription cancellations, coach additions, basic questions, but the sheer volume across two different products was crushing.


**Peak season reality, before Fini:**


Brand


Pain point


Scale


TrainingPeaks


Peak-week ticket flood


1,700 tickets / week


Train Heroic


Monday backlog


200–250 tickets every Monday


Both brands


Multi-brand answer confusion


Generic AI tools mixed policies across products


> *"We have very busy seasons. It can be very tedious and challenging to get to our coaches within a twenty-four hour time frame."*
> — Kristina Green, Senior Support Rep, TrainingPeaks


Then came the second layer of difficulty. Each brand has its own terminology, policies, and user types. Generic automation that confused TrainingPeaks answers with Train Heroic answers would create more problems than it solved.


> *"It seems like we were one of your customers that was very different than the rest. There's so many nuances to our company."*
> — Kristina


## How Fini Solved It


Together, we deployed a single Fini agent across both Peaksware brands.


Fini plugs directly into Zendesk. It reads incoming tickets, identifies which brand the user is on, whether they're a coach or an athlete, and which policies apply, then it either resolves the ticket or escalates with context attached. Fini doesn't just suggest answers; it takes actions. Cancellations, coach swaps, account changes, Fini executes them when the rules allow, and routes the rest to the right human with the full picture already gathered.


**What Fini does on every ticket:**


Step


Fini's job


Why it matters


1. Read


Parses the ticket and identifies the brand (TrainingPeaks vs. Train Heroic)


No cross-brand answer leakage


2. Classify


Detects user type (coach vs. athlete) and intent


Right answer for the right persona


3. Act or escalate


Executes the cancellation / coach swap / account change, or routes to the human queue with context


Agents skip triage and go straight to solving


4. Learn


Edge cases captured by the team, added as knowledge items via dashboard


Fini improves week over week


The key differentiator: Fini doesn't bleed knowledge across brands. TrainingPeaks questions get TrainingPeaks answers. Train Heroic users get Train Heroic logic. No mixing. This is the same architecture that lets Fini act as a reliable[AI ticket deflection](https://www.usefini.com/guides/best-ai-support-ticket-deflection-tools) layer in front of the human queue without ever sending the wrong product's answer.


## How brand separation is engineered.


Most support AI agents use retrieval-augmented generation (RAG). They search a vector database for chunks of documentation that look semantically similar to the question, then ask an LLM to answer from those chunks. RAG works well in single-product companies. For Peaksware's multi-brand support, it would have failed at the architecture level.


TrainingPeaks coaches and Train Heroic coaches speak almost the same language: "athletes," "training plans," "subscriptions," "coach access." A RAG retriever ranks chunks by semantic similarity, so a Train Heroic question pulls TrainingPeaks chunks a meaningful share of the time, and the LLM confidently answers with the wrong brand's policy. Brand-tagging the chunks helps in theory; in practice the closest-sounding chunk still wins, and edge cases between brands are exactly where semantic search breaks.


Fini works differently. Brand, user type, plan, policy, and action all live as nodes in a[structured knowledge graph](https://www.usefini.com/blog/what-is-ragless) , and Fini's Rulebook reasons over discrete facts instead of retrieved prose. Every Peaksware query anchors to the correct brand node first, then navigates policy facts from there. TrainingPeaks questions get TrainingPeaks answers. Train Heroic users get Train Heroic logic. The separation is engineered into the graph from the start.


Updates stay atomic. When a Train Heroic billing policy changes, the team edits one node, and every future query touching that policy uses the new fact immediately. The fact lives in one place, so updating it once is the entire operation.


And the team stayed in control throughout. Escalation rules, keyword triggers, tone controls, and a shadow-testing mode meant Peaksware could see exactly how Fini would respond before it ever talked to a real user.


> *"Our system is so nuanced. There are times where the way someone phrases something may seem like it's a coach when it's an athlete. But the more tickets it digests, the better it gets."*
> — Kristina


## Why Fini


Peaksware needed an AI agent that could handle multi-brand complexity without confused responses, take actions on tickets, and stay current as policies changed. Fini stood out on five dimensions:


**Multi-brand architecture.** Most AI tools are built for single-product companies. Fini's architecture worked with Peaksware's environment from day one, keeping each brand's knowledge base, terminology, policies, and tone of voice cleanly separated, at the data layer and the response layer both.


**The Fini Rulebook.** LLM intelligence combined with deterministic execution. The LLM understands what the user means in natural language; the Rulebook decides whether to act, guide, or escalate based on structured facts. For Peaksware, that meant Fini could execute cancellations, coach swaps, account changes, and prorate adjustments directly, with reliable repeatable behavior across both brands.


**Live knowledge updates.** When a Train Heroic billing policy changes, the team edits one node in Fini's dashboard and the new fact ships in minutes. The update happens at the knowledge layer alone, so the model and index stay untouched and engineering stays out of the loop. Knowledge stays current at the speed of the business, and similar tickets already in the queue retry automatically with the new logic.


**Guardrails and observability.** Brand-specific tone, escalation logic, keyword triggers, and shadow-test mode for safe deployment. Every Fini response is replayable, the team can see exactly which knowledge nodes and rules produced any given answer, and catch quality drift before it spreads.


**Partnership, not vendor relationship.** The Fini team stayed in Slack, held weekly calls, and jumped in to refine prompts directly when issues surfaced. Weekly check-ins are still on the calendar.


> *"Deep (Fini's cofounder) promised really good support, and he provided that. That was a large selling point."*
> — Kristina We walk through this step by step in[How LISA Hockey Cuts Support Workload by 50% with Fini](https://www.usefini.com/blog/lisa-case-study) .


## Implementation: Built to Get It Right


Peaksware launched Fini in deliberate phases, guardrails first, then knowledge, then real traffic in shadow mode. The two teams worked side-by-side throughout, fine-tuning prompts and escalation logic in Slack and on weekly calls as both brands came online.


Phase


What happened


Outcome


1. Prompts & guardrails


Escalation rules, tone controls, and multi-brand routing wired in before Fini touched live tickets


Brand separation enforced from day one


2. Knowledge base overhaul


Peaksware got direct dashboard access to add and refine knowledge items themselves


Edge cases fixable in minutes by the team


3. Shadow testing


Fini suggested responses as internal Zendesk comments for agent review


Real-traffic validation with zero customer risk


4. Iterate & launch


Weekly Slack and call cadence with the Fini team to sharpen prompts and escalation rules across both brands


TrainingPeaks and Train Heroic live, handling tickets at peak season


> *"We felt like the responses were really, really good. And then that's when we turned it on."*
> — Darren Lacy, Senior Support Rep, Train Heroic


## Results


Fini is live across TrainingPeaks and Train Heroic. The impact shows up in two places that matter most to a support team: queue size and[response time](https://www.usefini.com/blog/wefunder-case-study) .


**TrainingPeaks (peak season):**


Metric


Before Fini


With Fini


Peak week volume


1,700 tickets


1,700 tickets


Tickets reaching human queue


All of them


~500


Tickets deflected


0


~1,200 / week


Average response time


Up to 24 hours


**25.9 seconds**


Queue reduction


—


**Over 70%**


**Train Heroic (Mondays):**


Metric


Before Fini


With Fini


Monday queue


200–250 tickets


100–120 tickets


Time to clear backlog


End of week


**By Wednesday**


**What that adds up to:** at peak, more than 1,200 of TrainingPeaks' 1,700 weekly tickets never hit a human queue. Across the January–February peak window alone, that's roughly **10,000+ tickets deflected before they ever reached an agent** . The ones that do escalate arrive triaged, Fini gathers context, identifies the brand and user type, and hands the agent a head start instead of a cold ticket.


> *"We have on our busiest week potentially 1,700 tickets submitted into Zendesk. But what we saw in our queue at max was right around 500. Clearly, Fini is taking a huge number of those."*
> — Kristina


> *"It's still providing very helpful information to expedite the completion of the ticket."*
> — Kristina


## The Real Impact: Capacity


The dashboard numbers tell one story. The team's week tells another.


**Then vs now, on a typical Monday:**


Before Fini


With Fini


Monday queue size


200–250 tickets


100–120 tickets


Cleared by


End of week


Wednesday


Time spent triaging cold tickets


Hours daily


Near zero, context arrives with the ticket


Time available for harder work


Almost none


Two recovered days, every week


Mondays used to mean a 250-ticket wall that took the whole week to chip down. Now the queue starts at 120 and is cleared by Wednesday, two full days of capacity returned to the team, every week. They spend that time on harder tickets, on coaching new agents, and on the strategic work that always lost out to the queue.


When edge cases surface, the team improves them in real time. Direct dashboard access means Darren can add a knowledge item, test it, and see the improved response within minutes.


> *"I was able to go in and add a knowledge item very easily and improve the response. That was a huge shift in progressing Fini forward."*
> — Darren


The behavioral signal lands even harder than the dashboard numbers. Users routinely reply to Fini as if they're talking to a person. They thank it for help. They send follow-up tickets and continue the same conversation thread, never asking for a human. The support team often never touches those tickets at all.


## The Takeaway


Peaksware's challenge wasn't unique. Any company with multiple products, brands, or user segments faces the same question: how do you automate support without sending users the wrong answer for the wrong product?


The transformation is simple to state. One AI agent. Two brands. 70%+ of the queue resolved before a human sees it. And users who write back to the bot like it's a teammate.


> *"Fini's definitely worth it. The bot is learning at a quite rapid pace. I have a lot of faith that it's only gonna get better, and we're only gonna get more efficient because of it. If somebody asked me, 'Should we use Fini?' I'd say yes. They're gonna be there for you. They're gonna support you. Their communication is amazing."*
> — Darren


See how Fini handles multi-brand complexity →[Book a Demo](https://calendly.com/akash-usefini/30min?month=2026-06)
