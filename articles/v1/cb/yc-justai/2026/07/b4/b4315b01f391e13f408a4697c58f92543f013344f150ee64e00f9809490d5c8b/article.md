---
schema_version: "1.0.0"
document_id: "b4315b01f391e13f408a4697c58f92543f013344f150ee64e00f9809490d5c8b"
company_key: "yc-justai"
company: "JustAI"
source_id: "yc-justai-news-import-e4de3da632ca"
canonical_url: "https://www.getjust.ai/blog/how-outschool-scaled-personalization-with-multi-agent-ai-decisioning"
published_at: null
first_seen_at: "2026-07-25T10:24:54.291702+00:00"
fetched_at: "2026-07-28T21:35:55.789196+00:00"
content_hash: "sha256:eab482277f7302e4a2f8ea5ac4739de8f2d2bf5332e6e226bc0127b012121733"
---

# How Outschool scaled personalization with multi-agent AI decisioning

## The Challenge: Lifecycle personalization breaks at real-world complexity


Personalization is easy to talk about. Making it work across a real lifecycle is harder. For Outschool, lifecycle performance depended on getting the *right* message to *very* different families, parents with different age groups, buying behaviors, and content affinities, across multiple weekly sends.


The intent was clear:


-


Personalize content meaningfully


-


Improve purchase conversion


-


Learn faster without increasing operational overhead


The problem was scale. Outschool’s lifecycle setup quickly grew complex:


-


Multiple campaigns (Thursday + Sunday sends)


-


Segmentation by buyer status (member, non-member, à-la-carte)


-


Age-based personalization (with age / without age)


-


Topic-level experimentation


-


Manual cadence decisions


What started as “just a few dimensions” turned into **12 parallel tests per cycle** , each requiring setup, monitoring, reporting, and iteration.


At that point, lifecycle wasn’t limited by ideas, it was limited by **human bandwidth** .


## The insight: Lifecycle is a decisioning problem, not a content problem


Outschool didn’t need *more* one-off tests & wins. They needed a system that could:


-


Learn across many strategies simultaneously


-


Adapt in real time as performance signals emerged


-


Optimize content, topic, audience, and cadence together


###### Instead of treating lifecycle as:
“Run an A/B test, pick a winner, move on”


###### They reframed it as:
“Continuously decide the best strategy for each user, across dimensions, at scale.”


That required **multi-agent decisioning** — not static segmentation.


## The approach: Multi-agent personalization across the lifecycle stack


Outschool partnered with JustAI & Verbose to deploy **AI-driven lifecycle decisioning** , powered by multiple specialized agents working together.


###### The agents worked across three layers:


#### 1️⃣ Deeper segmentation (who)


Rather than locking users into rigid segments, the system evaluated performance dynamically across:


-


Transactional vs. subscription users


-


With age vs without age


-


Cross-segment behavioral signals


No hardcoded rules.


No exploding campaign trees.


Just continuous learning at the cohort level.


#### 2️⃣ Topic & strategy optimization (what)


Instead of testing one theme at a time, the system explored **multiple content strategies in parallel** , including:


-


Pop-culture driven topics (e.g., *Taylor Swift* )


-


New creative directions (e.g., changing email sections with a variety of topics)


-


Structural changes (hyperlinked sections, top-loaded CTAs)


Each strategy competed in real time.


Winning topics automatically received more traffic.


Underperformers were deprioritized without manual intervention.


#### 3️⃣ Send time & cadence learning (when)


Outschool expanded beyond content into **cadence experimentation** , testing:


-


Day combinations (Mon–Thu, Tue–Fri, Wed–Sat)


-


First-send vs second-send timing


-


Multi-day vs single-day sequences


The system evaluated not just opens or clicks, but **downstream purchase behavior** , allowing cadence decisions to be optimized alongside content.


## What happened: Learning at scale, not in silos


#### 🚀 Speed & scale of experimentation


###### Within a couple of months, Outschool achieved:


-


**307 email variants tested**


-


**100+ AI decisions (replacing human intervention)** dynamically reallocating traffic


-


**4M lifecycle messages** powered by AI


-


The equivalent of **12 years of one-off A/B testing** compressed into weeks


This wasn’t faster testing. It was **parallel learning across strategies, audiences, and timing** .


## Performance impact: Incrementality that paid for itself


#### 📈 Core conversion lifts


**Email 1**


-


**+33% lift** in purchase membership rate


**Email 2**


-


**+24% lift** in purchase rate


-


**+110 incremental conversions** in under 2 weeks


## How the system learned (and improved)


#### Phase 1: Exploration


Early runs trended negative across some cohorts.


Instead of stopping, the system:


-


Captured signal


-


Recalibrated strategy weights


-


Auto-tuned topic distribution


#### Phase 2: Adaptation


Performance turned positive as:


-


Stronger topic-audience matches emerged


-


Underperforming strategies lost traffic automatically


#### Phase 3: Exploitation + expansion


New topics were introduced.


*K-Pop Demon Hunters* emerged as a standout:


-


**2× higher click rates**


-


**20% lift** in membership conversions


Follow-up runs pitted it directly against prior winners ( *Taylor Swift* ), and it continued to outperform — without a marketer needing to “pick” the winner.


## Segment-level intelligence (without segment sprawl)


###### Key insights emerged organically:


-


**Age-based cohorts consistently outperformed** , especially for high-intent purchases


-


Click-rate lifts were directionally aligned with purchase lifts across *all* segments


-


Certain strategies resonated differently across buyer states — insights that would have required dozens of manual tests previously


These weren’t just results. They became **inputs into future lifecycle strategy** , without adding complexity.


## Operational impact: From campaign chaos to one system


#### Before


-


2 campaigns


-


6 branches


-


12 tests per cycle


-


Heavy setup + reporting overhead


#### After


-


**One unified AI-decisioning setup**


-


No branching logic


-


One test surface across:


-


Audience


-


Topic


-


Strategy


-


Cadence


Lifecycle became **simpler to run** while becoming **more sophisticated in outcome** .


## Early cadence wins


Signals emerged quickly:


-


**Mondays** outperformed for first sends


-


**Tuesdays** (with Thursdays close behind) worked best for second sends


-


**Monday–Thursday** emerged as the strongest overall pairing


This unlocked next-level questions:


-


Single-day vs two-day sends


-


Optimal delay between sends


-


Cadence personalization by cohort


All without rebuilding the system.


## Why this matters for lifecycle marketers


Outschool didn’t win by getting lucky with a few tests. They won by **changing how lifecycle decisions to run on auto-pilot** .


Instead of:


-


Static segments


-


Isolated A/B tests


-


Slow, local learning


They moved to:


-


Continuous cohort-level learning


-


Multi-strategy competition


-


Automatic traffic reallocation


-


Marketers focused on *strategy* , not orchestration


Lifecycle became holistic, spanning **who, what, and when,** without multiplying effort.


### The outcome: A scalable lifecycle operating model


By treating lifecycle as a **decisioning system** , Outschool unlocked:


-


Faster learning


-


Measurable incrementality


-


Operational leverage


-


Reusable lifecycle intelligence


This is what modern lifecycle looks like:


Not more rules.


Not more segments.


But systems that learn as fast as your users evolve.


A huge congrats to[Erica Yamamoto](https://www.linkedin.com/in/ericayamamoto/) **, Head of Marketing at Outschool, along with**[Andrew](https://www.linkedin.com/in/andrewfiore/) **, Head of Data,** for diving head first into AI-innovation in Marketing. Erica and her team set a strong example of what the next generation of lifecycle marketing looks like - where complex multi-faceted strategies scale with AI.


A big shoutout to Verbose, Outschool’s dedicated strategic lifecycle team. While JustAI powered the decisioning layer, Verbose partnered closely with the Outschool team to take a big swing on JustAI’s AI-driven lifecycle personalization. Verbose helped shape a clear success criteria, strong strategic hypotheses, and how early learnings could compound beyond a single experiment into a scalable lifecycle operating model. They helped Outschool move from “we have lots of ideas” to a structured lifecycle system that AI could actually optimize


Combining AI with a strategic framework to lean against, we solved for hyper-personalization in a complex lifecycle setup.
