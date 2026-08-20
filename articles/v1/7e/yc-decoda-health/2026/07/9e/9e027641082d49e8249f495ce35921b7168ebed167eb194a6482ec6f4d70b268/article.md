---
schema_version: "1.0.0"
document_id: "9e027641082d49e8249f495ce35921b7168ebed167eb194a6482ec6f4d70b268"
company_key: "yc-decoda-health"
company: "Decoda Health"
source_id: "yc-decoda-health-news-import-41de34412c0a"
canonical_url: "https://decodahealth.com/blog/enabling-goal-and-loop-for-healthcare"
published_at: "2026-07-29T00:00:00+00:00"
first_seen_at: "2026-07-30T00:24:13.613552+00:00"
fetched_at: "2026-07-30T00:24:15.285130+00:00"
content_hash: "sha256:d6b634def7287c01a452ef666ce3da897b27475aa0bed47d74217b3111c409b5"
---

# Enabling /goal and /loop for healthcare

Where we're going


Software engineering has shifted dramatically since 2022. We now build at the speed of thought, and Jevons paradox is in full swing. For the physical world, this is still in its infancy. We have spent two years building the infrastructure to change that, and we think healthcare is about to get radically more efficient. Here is exactly how.


A note from the team · Decoda Health


---


The thing that changed our jobs as engineers was not the models getting clever. It was two boring facts becoming true at once. Everything an agent needs to know is already in one place, the code. And every action it takes is cheap to check and cheap to undo. You read the change before it ships. Nothing lands without a human saying yes.


A clinic has never had either. The context is scattered across five systems that do not talk to each other. The actions do not undo. You cannot unsend a charge or a message the way we revert a line of code.


So the real work is not "point an LLM at a clinic." It is giving a clinic the same loop that changed software: **everything in one system, and every action reviewed before it is real.** Do that, and you can finally talk to your EMR the way we now talk to our coding tools.


## What you should be able to ask


Not click through seven screens. Not export a spreadsheet at midnight. Just ask, the way you would brief your sharpest operator:


/goal


· achieve an outcome


Adjust my appointments to optimize rebook rate and open blocks for productive providers. You have a budget of $200 in credits.


Build my staff's schedule to optimize our spend. Look at our appointment volume and provider availability.


Get better rates with our suppliers, find deals we can leverage, and run a promotion you see fit.


Optimize our catalog's pricing. Look at what people buy and our margin, then suggest memberships and packaging and market it to the right patients.


Identify our optimal treatment pathway for HRT. Analyze every patient we have ever seen.


/loop


· run it on a schedule


Every day, find everyone who purchased this package and schedule them for a nutrition consult.


None of this needs new data. It all already lives in Decoda. A coding agent only helps because your whole codebase is right there. This only works because your whole clinic is. And as clinics grow and people specialize, the payoff compounds, because no one person can hold the whole picture anymore.


This future is not years away. It is months. It comes down to three things: **solid infrastructure, an incredible MCP, and forward-thinking clinical partners.**


---


## 01


The infrastructure


You must own every patient touch point. Every piece of clinical data, every scheduled appointment, every unit of inventory, every call, every Instagram message, every marketing campaign. This is the foundation, and it already earns its keep. Our clinics collapse five tools into one. Without it, an agent lacks context and hallucinates. Breadth is not sprawl. **It is the context window.**


And the EMR has to move on its own, two ways.


Event-driven


Everything that happens lands as an event the moment it occurs. A booking. A missed call. A failed payment. An agent can respond to it in real time instead of leaving it in an inbox. This is already live: an inbound text or an Instagram DM gets an intelligent auto-response the moment it arrives.


A heartbeat


Every few minutes, around the clock, the platform does its rounds and dispatches curated prompts on a schedule, tuned to each clinic. This is where /loop


lives. And the agent sets these up itself. You ask once in plain English, it schedules the recurring job, and "every day, find everyone who bought this package and book a nutrition consult" then runs for every patient, forever, without anyone remembering to.


Own everything. React to everything. Run on a clock. Both mechanisms already carry real clinics today.


## 02


The MCP


We started with siloed AI, each tool confined to one task, so we could learn to operate at high stakes before asking for more trust. Scribe. The AI Receptionist. Ask About a Patient. AI Analytics. Useful on their own, and nothing next to what they become connected.


Then we built an MCP: a single read-and-write interface an agent uses to actually change a clinic. We put it in the hands of a few Claude-enabled owners to edit their settings. The impact was immediate. **Thousands of uses a day.**


Now we are extending that MCP from settings to operational and clinical data. HIPAA makes this serious, and no one brings their own key. So we are splitting it in two: a config layer that cannot touch PHI, and a fully capable clinical layer embedded in the platform, protected by our agreements with the model providers. Your patients' data never leaves those walls, and none of it trains anyone's model.


The write path uses the same discipline we trust in code. The agent never changes anything directly. It proposes. You see exactly what changes, who it reaches, and what it costs. Then you approve.


Agent · proposed a plan


Needs your approval


Re-book 34 lapsed weight-loss patients into a nutrition consult


What it'll do Text a personal offer & hold an open slot for each


Who it reaches 34 patients · no message before 9am


Est. cost $4.10 in messaging


Reversible Slots auto-release if unbooked in 48h


Approve


Hold


Edit plan ›


A preview of what we're building. This screen doesn't exist yet.


Propose, confirm, apply. The same loop as reading a diff before you merge it. If the clinic moved underneath the plan, the agent throws it out and re-proposes rather than acting on a stale picture. The config layer ships today. The clinical layer is what we are building now, slowly, on purpose.


And when it gets something wrong, you are covered. Every action is logged, so you can see exactly what the agent did and when. Anything it changes can be undone. Nothing runs twice by accident. **The goal is not an agent that never errs. It is an agent whose every move you can see, approve, and reverse.**


## 03


The clinical partners


This is the part we cannot build alone, and the part that matters most. Not the code. Clinical teams with insight. Operators who have run successful businesses for years, who know exactly what needs to happen and have been held back by their tech stack.


The human stays in the loop. You make the decisions. You interact with your patients. You define how your EMR behaves. The agent's job is to know when to act, when to stop and ask, and when to suggest something better than you asked for.


We can build the infrastructure and the MCP. We cannot invent your judgment. That has to come from you, building alongside us and telling us where we're wrong while it is still early enough to shape.


Software already made this jump. The winners were not the ones with the flashiest tools. They were the ones who learned to work with an agent and kept a hand on the wheel. **Clinics are next. Not years out. Months.**


---


If you run a clinic and want to shape this, build it with us.


[Build it with us ›](https://calendly.com/d/cr5z-78z-8zq/decoda-ai-intro)
