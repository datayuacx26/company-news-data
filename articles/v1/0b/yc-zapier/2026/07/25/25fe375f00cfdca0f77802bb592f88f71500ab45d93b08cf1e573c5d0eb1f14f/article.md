---
schema_version: "1.0.0"
document_id: "25fe375f00cfdca0f77802bb592f88f71500ab45d93b08cf1e573c5d0eb1f14f"
company_key: "yc-zapier"
company: "Zapier"
source_id: "yc-zapier-rss-44f77ec59be6"
canonical_url: "https://zapier.com/blog/zappy-award-monthly-winners-june-2026"
published_at: "2026-07-14T04:00:00+00:00"
first_seen_at: "2026-07-20T23:24:20.076749+00:00"
fetched_at: "2026-07-28T21:08:37.946927+00:00"
content_hash: "sha256:3f61405f550846c581c1d1901f0f6e09111e800fb47b0e01f16f24fd519720af"
---

# Meet the June 2026 Zappy Award monthly winners

This month's three Zappy Award winners are turning scattered company knowledge into shared context that AI and humans can actually use.


The strongest June Zappy Award submissions had the same shape: shared context. AI can only help with work it can see. When customer history lives in someone's head, when policy hides in a stale doc, when product knowledge is scattered across videos, help articles, and slide decks, AI has to guess, and a guessing AI is an unreliable one.


[Eric McNulty](https://www.linkedin.com/in/eric-robert-mcnulty/) at[Mercari](https://www.mercari.com/) ,[Blair Mishleau](https://www.linkedin.com/in/mishleaub/) at[Patreon](https://zapier.com/apps/patreon/integrations) , and[Amber Sharp](https://www.linkedin.com/in/theamber-sharp/) at[Tivly](https://tivly.com/) each built programs that leverage AI in controlled workflows with clean lines and systems to ensure the right knowledge and context are used. Let's dive in!


---


## Eric McNulty, Systems Manager CX, at Mercari


[Mercari handles roughly 120,000 support tickets a month,](https://zapier.com/customer-stories/mercari) covering everything from return disputes to account access, payments, and customer funds.


Eric had tested dedicated AI support tools but found that while they were fine for basic questions pulled from a help center, they fell apart on Mercari's harder cases, where a system has to follow specific rules, pull the right context, and know when to hand off to a human.


This constraint is what led Eric to build Mercari's own support orchestration system on Zapier.


The center of the build is a master Zap with about 90 steps. It classifies every ticket and then pulls its prompts, category definitions, and policy logic directly from Google Sheets. That spreadsheet is the shared context layer. CX operations can update it directly when a policy changes, and the next run picks up the change.


The build means CX operations no longer wait on an engineering ticket to change a policy, and there's no stale prompt buried in a vendor tool somewhere nobody remembers to update.


He even built a Google Apps Script UI on top so non-technical teammates can run the whole thing without ever opening Zapier.


The results:


-


About 47,000 tickets a month are resolved entirely by Zapier, no human agent needed


-


First replies on roughly 97% of tickets, most within minutes


-


CSAT went up a full point on a 5-point scale


-


More than 3,000 hours of agent time freed up every month


> "We give it exactly the right context. The customer's message, scrubbed for PII, and exactly the right help center articles."


Eric doesn't let the AI roam. His shorthand for the whole pattern: "This is the data. Now do this."


Eric is injecting AI into deterministic workflows — used in specific ways, for specific purposes, not left to roam a general knowledge base. It starts from the ticket, then pulls the right policy and the right help center context. When confidence is low, the ticket goes straight back to a human. Customers never get an unsafe answer, and no ticket disappears.


Get started building your own support orchestration with this[template straight from Eric.](https://zapier.com/templates/details/resolve-support-tickets-guarded-ai-orchestration)


---


## Blair Mishleau, Senior Community Education Manager at Patreon


To help employees find trusted information more quickly, Blair used Zapier to design and build 'Treon, an internal Slack-based knowledge assistant that brings together hundreds of internal resources — including help articles, Figma files, go-to-market briefs, internal documentation, and Patreon's glossary — into a single searchable experience.


Rather than relying solely on AI-generated responses, Blair intentionally designed the system around transparency and human oversight. Every response is grounded in source material, conversations are logged to identify knowledge gaps, employee feedback helps surface inaccuracies, and resources are regularly reviewed to ensure information stays current.


Since launch, 'Treon has answered more than 1,300 employee questions, saving teammates time that would have otherwise been spent searching for information or routing questions to colleagues. More importantly, it has made trusted knowledge easier to access across the company.


---


## Amber Sharp, Director of Sales at Tivly


Amber Sharp is a recognizable face for the Zappy Awards. She won the inaugural[Top Sales Professional of the Year](https://zapier.com/blog/zappy-awards-winners-2024/) award in 2024, for using Zapier to automate everything from drip campaigns to personalized prospecting emails and referrals.


This year, Amber is back and showing off how she single-handedly onboards 400 to 500 clients annually at Tivly and hands each one off to her professional services team.


The handoff between sales and account management or professional services can cause friction and knowledge loss. At Tivly, this often meant rebuilding the client story from scratch. Amber took on that problem and built a system in Zapier that keeps context intact through the handoff, making the transition easier for the internal team and a better experience for the customer on the other end.


Her system runs in three stages. An email triage agent classifies every inbound message as a prospect, a new client, or a client mid-transition. New client questions get routed to a chatbot trained on organizational knowledge, with every question and answer logged to a Zapier Table.


When an account changes hands, a five-step handoff agent pulls the full history — plus weekly onboarding insights on campaign performance, leads purchased, and conversion — and builds a scorecard. Amber used AI to translate the scorecard from sales shorthand into plain language that a new teammate could act on immediately.


In six months, the system has generated more than 200 onboarding intelligence reviews. Onboarding volume is up more than 165%, and transition prep, which includes the manual work of rebuilding a client's story for the professional service handoff, dropped from ~25 hours a month to under one.


The same behavioral insights that made the handoff work also answered another question critical to customer success: why clients lapse.


Amber loaded a batch of whitelisted client records into Claude and asked what was driving the pattern. The answer had been sitting there the whole time, invisible until the data existed in a form structured enough to ask the question.


Now she drills into lapse risk by lead source and channel, and acts on it within the 30-to-45-day window she already spends nurturing new clients before they're fully handed off to professional services.


"We stopped rebuilding client stories manually every time ownership changed. Instead, we started tracking the behaviors behind the client journey."


AI gave the next account owner the context to make faster, better judgment calls — and gave Amber a lapse signal that didn't exist nine months ago.


---


## Why this matters now


The first wave of AI at work made individuals faster. The next wave is about making the company itself easier to work with.


Less time spent hunting for context, rebuilding history, asking someone to catch you up, or repeating decisions the company already made means we'll all have more time to help customers, build, and move work forward.


That is the story we'll be telling at ZapConnect 2026: How companies are refounding the way work gets done in the AI era by making work queryable, connecting it to action, and keeping humans accountable for the outcome.


These three Zappy winners make that shift concrete. They made context visible, connected it to real workflows, and kept people in control.


[Register for ZapConnect 2026](https://zapier.com/zapconnect) to see where this is going next.


**Zappy Award submissions for our annual prize of $5,000 close July 24th, 2026.**


Monthly winners continue throughout July and August and beyond, with annual award winners announced on stage at ZapConnect 2026.


[Submit your best build today](https://zappy2026nomination.zapier.app/self-nomination) .


Know a builder who should be on this list?[Submit a nomination](https://zapier.com/zappy-awards) .
