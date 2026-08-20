---
schema_version: "1.0.0"
document_id: "b89ade032b700d213d238fbd94b6f03f9b5e3e757cdc3ba728ad31258449dba0"
company_key: "natwest-group-plc-american-depositary-shares-each-representing-two-2-ordinary-shares"
company: "NatWest Group plc"
source_id: "natwest-group-plc-american-depositary-shares-each-representing-two-2-ordinary-shares-rss-a9eea26f1824"
canonical_url: "https://nwg.ai/resilience-by-design-a-new-loop-for-a-fragmented-world-b22878cb5d65"
published_at: "2025-10-28T17:03:12+00:00"
first_seen_at: "2026-07-20T04:35:38.759256+00:00"
fetched_at: "2026-07-28T22:01:00.433769+00:00"
content_hash: "sha256:9dea1b3a56b7b59886681d7c861a9aafadde075efb788066bd8115b9934fb0a5"
---

# Resilience by Design: A New Loop for a Fragmented World

# Resilience by Design: A New Loop for a Fragmented World


[NatWest Group](https://medium.com/@NatWestGroup?source=post_page---byline--b22878cb5d65---------------------------------------)


15 min read


·


Oct 28, 2025


--


*By*[Siddharth Pareek (DevSecOps Centre of Excellence, Architecture & Engineering)](https://www.linkedin.com/in/siddharthpareek/)


Press enter or click to view image in full size


## 1. The Strategic Blind Spot: Why Resilience Needs a Systems View


We’ve never had more tooling, talent, or telemetry — yet material outages persist.


For many teams, scalability and observability are now table stakes. The deeper challenge is whether systems can **absorb disruption, adapt in motion, and recover without cascading harm** .


According to *Resilience Engineering in Practice* (Hollnagel et al.), resilience is:


> “The intrinsic ability of a system to adjust its functioning prior to, during, or following changes and disturbances, so that it can sustain required operations under both expected and unexpected conditions.”


In contrast, reliability is about minimising variance during normal operations. Resilience is about **preserving outcomes when conditions are anything but normal.**


Most modern incidents don’t stem from broken services — they arise from misaligned assumptions, fragile integrations, and drifting interfaces. A fallback removed. A retry loop is unchecked. A decision was delayed because ownership wasn’t clear.


Yet, resilience is still often treated as a post-failure concern — something tested in chaotic environments or tuned during incidents. It should be a **design property** embedded across architecture, contracts, and team interaction.


This article explores a model that does just that: a **closed-loop approach** that connects design, validation, operation, and learning — turning resilience from a fragmented concern into a shared, evolving system behaviour.


Press enter or click to view image in full size


The systemic gap in modern resilience


## Section 2: The Six Lanes of Resilience — and the Silos They Create


Every organisation trying to build resilient systems eventually assembles some version of the same six capabilities:


- **Architecture**
- **Customer Journey Mapping**
- **Site Reliability Engineering (SRE)**
- **Chaos Engineering**
- **Observability**
- **InnerSource / Pattern Reuse**


Each discipline delivers clear value. Architecture shapes system boundaries. SRE gives us metrics and thresholds. Chaos tests our assumptions. Observability provides insight. InnerSource enables scale. And customer journeys ground it all in the end-user’s reality.


The problem isn’t these capabilities — it’s how they operate: often **in parallel, but rarely in sync.**


Architecture teams publish patterns that often fail to reach product teams. SREs define SLOs that aren’t tied to real customer journeys. Chaos experiments run in dev, disconnected from platform learning. Observability dashboards track everything except what’s needed during failure. InnerSource efforts launch with energy, but stall without adoption.


Each lane is moving — but horizontally. **What’s missing is the loop that connects them.**


This isn’t a tooling gap. It’s a **coordination gap** , a **feedback gap** , and most of all, a **shared accountability gap** .


Incentives deepen the divide:


- Architects are judged by enablement.
- SREs by incident stats.
- Product teams by velocity.
- Platform teams by uptime.
- InnerSource by contribution count.


Until teams **measure resilience as a system** , local wins can still produce global fragility.


One option: align around **resilience KPIs** that reflect system behaviour, like:


- **Time to Safe State (TTSS)** : How quickly can the system contain harm?
- **Journey Completion Under Failure** : Can users still achieve their goal during degradation?


These aren’t just metrics. They’re reflections of how tightly your six lanes are linked.


To move from **fragmented excellence** to **systemic resilience** , we’ll need more than alignment. We’ll need an **operating loop** — one that connects how we design, how we validate, how we operate, and how we learn.


Press enter or click to view image in full size


The six lanes of resilience


## Section 3: The Resilience Architecture Integration Model (RAIM)


When things go wrong in production — and they always do — it’s rarely one team’s fault. More often, it’s a gap. A missed assumption. A fragile handoff. The **Resilience Architecture Integration Model (RAIM)** is our attempt to close those gaps with intention.


RAIM connects six critical perspectives — **Architecture, Customer Journeys, SRE, Chaos Engineering, Observability,** and **InnerSource** — into a living loop: **Design → Prove → Operate → Learn → Reuse → Design again** .


It’s not a step-by-step framework or maturity ladder. It’s a circular rhythm that organisations can enter from anywhere. Some start with a fresh architecture blueprint. Others begin by making sense of a customer-impacting incident. RAIM meets you where you are — and helps you stitch those efforts together.


Press enter or click to view image in full size


RAIM loop


Let’s break it down, one lens at a time:


## Architecture: Designing for Failure


We often treat architecture like a blueprint: static, polished, abstract. But in RAIM, it’s a pressure map. Architecture is where we get honest about what’s coupled, what’s brittle, and where the real risk lies. It’s where we ask: what will fail together — and who’s going to feel it?


Architecture teams help shape failure domains, encode fallback options into infra (think: AWS zone-awareness, retry budgets), and guide whether systems default to async, bulkhead, or fail-closed patterns. Not just design for scale — but design for uncertainty.


## Customer Journeys: Making Risk Real


It’s easy to say a system is “up.” It’s harder to say if a customer can complete a loan application without timeout. That’s why RAIM puts journeys at the forefront.


When an incident hits, we map the blast radius in terms of customer experience — not backend logs. Where did friction show up? What trust got broken? And when we onboard something like AWS, journey maps help us build SLOs that matter — not just monitor noise.


Resilience becomes real when it’s felt by the people using your product.


## SRE: Holding the Edges Together


SREs in RAIM aren’t just guardians of uptime, they’re boundary keepers. They own the hard-to-navigate areas between teams, like escalation paths, mistake budget conflicts, and ambiguous dependencies.


Their job is to identify drift, reduce time-to-safety during incidents, and incorporate learnings into architecture reviews and journey design. They’re the glue that ensures no lesson stays local.


## Chaos Engineering: Asking the Hard Questions


Chaos Engineering isn’t about breaking things — it’s about testing what we think is true. Does that fallback really kick in under pressure? Does our runbook hold up at 3 AM? Can two retries kill the queue?


In RAIM, chaos is focused, not random. We tie every test to something real: a recent outage, a critical path, a scary assumption. And we make sure what we learn doesn’t stay in a Slack thread.


## Observability: Turning Signals into Stories


Observability in RAIM isn’t a dashboard — it’s a narrative. Can we, within seconds, explain why a customer journey failed? Can we tell the story, not just plot the graph?


Good observability lets us debug, yes — but also helps us design smarter, prove safer, and learn faster. If we can’t see it, we can’t fix it. And if we can’t explain it, we’ll repeat it.


## InnerSource: Making Resilience a Team Sport


No team owns resilience alone. InnerSource is how we scale the wins.


When someone cracks a better way to bulkhead, that pattern should live beyond their repo. When a chaos test teaches us something new, the fallback logic should be portable. InnerSource makes that happen.


It’s how we move from heroics to habits.


## Section 4: From Model to Momentum — Making RAIM Real


The Resilience Architecture Integration Model (RAIM) is only as powerful as the teams make it. It’s not another shiny framework — it’s a working approach that earns its keep by making real problems less painful. You don’t launch RAIM like a program. You show it. In how people fix things. In who gets looped in, and in how often patterns resurface.


## Start With What’s Fractured


Most teams aren’t short on capability — they’re short on connection. You can usually spot the cracks:


- A P1 that keeps coming back but never rewires the design.
- A chaos test that breaks something obvious, again.
- SLOs that don’t map to anything the customer cares about.


We’ve all seen this. The gap isn’t tools — it’s follow-through. Feedback that stops halfway. Learning that doesn’t land. Donald Schön called these “coordination breakdowns.” RAIM starts getting traction when people trace those gaps upstream: where did this start? And who could’ve helped, if only they were in the loop?


## Architecture Topology: Where It Breaks, Not Just How It Works


Conway’s Law states that your system reflects your communication. That means resilience suffers when teams fail to communicate — ownership blurs. Interfaces get mushy. Dependencies spread without bounds.


RAIM pulls resilience back into architecture. Who owns what when things break? What’s async, what’s not? Where’s the blast radius supposed to stop? Good architecture makes assumptions visible — and failure, containable.


Want to dig deeper? *Team Topologies* (Skelton & Pais) and *Site Reliability Engineering* (Beyer et al.) both lay a solid groundwork.


## Org Design and Incentive Misfits


This one’s harder. Resilience breaks when incentives pull teams in opposite directions:


- SREs who inherit fragile systems.
- Architects make rough decisions after the plans are locked.
- Chaos findings that don’t change the roadmap.
- InnerSource that gets applause, but not time.


We’ve seen it: teams trying their best, but no one owns the learning. And when nobody owns it, incidents repeat themselves.


This mirrors Amy Edmondson’s work on psychological safety. If learning from failure isn’t part of the culture, the org optimizes for uptime — even if it means piling up risk underneath.


## Quick Starts and Language Shifts


You don’t “implement” RAIM. You spot it in action. The loop kicks off with something small:


**Try this:**


- Did the fallback show up in the customer journey?
- Did chaos findings lead to an architecture update?
- Did anyone write down the fix for someone else to reuse?


You’ll know RAIM is taking root when team language starts to shift:


- “Does this SLO make sense for that journey?”
- “What did we actually learn from that chaos test?”
- “Is this a one-off fix, or something we want to standardize?”


This isn’t about installing new tooling. It’s about teams thinking differently.


## Leadership That Keeps the Loop Alive


Leaders don’t just fund resilience — they keep it going when it’s not trendy anymore, when the incidents quiet down. When teams move on.


## Get NatWest Group’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


That means:


- Protecting time for post-incident design reviews.
- Making sure telemetry, fallbacks, and runbooks evolve.
- Rewarding fixes that span teams — not just quick closes.


Peter Senge refers to this as “system stewardship.” Leaders who garden the conditions, not just react to the weeds.


## RAIM Adoption Archetypes


Most orgs already show hints of RAIM. But they’re often strong in one area and weak in the loop:


- **Tool-Heavy Org** : Great dashboards, unclear journeys.
- **Chaos-First Org** : Lots of tests, little design feedback.
- **SRE-Centric Org** : Rock-solid ops, brittle upstream assumptions.


The trick isn’t to fix everything. It’s to wire input into what you already do well.


## The Cost of Letting It Slide


When the RAIM loop doesn’t take hold, things quietly regress:


- Teams tackle the same incident twice.
- The fix never makes it past one squad.
- Trust fades — not because people failed, but because no one learned.


Resilience stays local. And when real pressure hits, the system struggles to act like one.


Press enter or click to view image in full size


## Section 5: Beyond the Model — RAIM as an Enterprise Transformation Lens


RAIM (Resilience Architecture Integration Model) wasn’t meant to sit quietly in an engineering wiki. It’s a working model — built for motion. And that motion, over time, doesn’t just influence architecture or reduce incident rates. It starts to reshape how the entire enterprise thinks, reacts, governs, and learns.


Resilience becomes a loop, yes. But in the right hands, it becomes something bigger: a **language** .


## Resilience and the Regulatory Horizon


Resilience today extends beyond SRE dashboards. It’s increasingly scrutinised through the lens of **financial regulators, operational risk committees, and compliance partners** . And that’s a good thing — because RAIM speaks their language too.


Frameworks like the **FCA/PRA’s Operational Resilience rules** , the **Digital Operational Resilience Act (DORA)** in the EU, and newer **cloud concentration risk guidance** all point toward a similar expectation:


- Demonstrate how critical journeys survive real disruption.
- Show who owns that journey — across business and tech.
- Prove that you’re learning and adjusting, not just recovering.


RAIM doesn’t chase compliance. It **earns coherence** , which regulators value more than checklists. When SLOs are tied directly to essential business services, and fallback patterns are maintained as versioned architectural artefacts, audit becomes a walkthrough — not a war room.


## When Engineering and Governance Are Out of Step


If you’ve ever been in a post-incident review that sounded like two parallel conversations — one technical, one regulatory — you’ve felt this gap firsthand. Resilience work happens inside two loops:


- The **engineering loop** — design, test, operate, learn.
- The **governance loop** — risk assessment, policy review, compliance reporting.


These loops don’t naturally talk to each other. They often move at different speeds. And the few people who try to sit in the middle? They burn out, or get ignored.


RAIM gives those loops a shared map. When a chaos experiment uncovers a blind spot, that insight can feed a business continuity plan. When a risk team flags a critical journey, it can shape SLOs, not just sit in a PDF. The trick isn’t to slow down one loop. It’s to create **feedback bridges** between them — so they can move differently, but still move together.


## Synchronizing Loops


Press enter or click to view image in full size


RAIM synhronizes resilience across enterprise boundaries


## Governance as Design


What gets designed gets governed. What gets governed gets safer.


RAIM suggests we treat governance not as policy enforcement — but as a **design exercise** :


- Architecture decisions become reviewable artefacts.
- Incident learnings become inputs to risk frameworks.
- Chaos results are versioned like code — and referenced in controls.


One potential future idea: a **Resilience Coherence Index** — measuring how tightly incidents, architecture, and compliance align. It’s less about scoring and more about **surfacing blind spots** before regulators do.


## Culture Isn’t Soft — It’s the Soil


No version of RAIM works in a fearful culture. If raising a “what-if” makes you look like the blocker, resilience will quietly die — no matter what framework you’ve drawn.


Real resilience is evident in teams that question assumptions early, share uncomfortable learnings openly, and view failure as part of the work, not a deviation from it. That’s not idealism. It’s table stakes.


This isn’t new thinking — Amy Edmondson’s research on **psychological safety** laid it out years ago. However, too often, we focus on the tooling and forget the environment in which it grows. RAIM, at its best, is a system of accountability. But it needs trust to run. And **trust** doesn’t come from policies. It comes from people listening, following through, and making it safe to speak up.


## Signals of a Transformed Org


You’ll know RAIM is taking hold when:


- Design reviews begin with the question, “What happens when this fails?”
- Postmortems turn into **platform-wide learnings** .
- SRE dashboards highlight **customer journey degradation** , not just CPU spikes.
- Risk conversations cite **findings from chaos theory** , not hypotheticals.
- Teams talk in **loops, not lanes** .


That’s when RAIM stops being a framework — and becomes **organisational memory** in motion.


## Where This Heads Next


RAIM isn’t the end. It’s a foundation. The next wave of work? Turning this shared loop into **playbooks, literacy programs, inner-sourced patterns** , and leadership workshops. It’s a systems-thinking lens for **AI safety** , platform governance, resilience engineering, and how regulated institutions build trust at scale.


Because in the end, RAIM isn’t about being resilient for its own sake. It’s about earning the right to move faster, serve better, and recover smarter — together.


## Section 6: The Road Ahead


RAIM (Resilience Architecture Integration Model) isn’t a template. It’s a lens. A way of seeing what’s already happening — and what keeps falling through the cracks. It doesn’t ask for permission to begin. It doesn’t require a playbook or a platform. It starts when one team, anywhere in the system, decides to make the invisible visible.


This is not about launching a new initiative. It’s about changing how we respond to signals — and how we treat learning — not just delivery — as the beating heart of resilience.


## From Playbooks to Patterns


We’ve long relied on documentation and drills. But the future of resilience will be sensed, not scheduled. RAIM pays attention to the subtle shifts:


- A design review begins with “What happens when this fails?”
- A chaos experiment spans developers, SREs, and architects — not just one function.
- A shared library captures fallback patterns, journey breaks, and known blind spots.


These aren’t big bangs. They’re quiet revolutions. Markers that the system is moving — from output to outcome, from separation to synthesis.


## Toward a New Literacy


Every transformation has its language. DevOps gave us pipelines and CI/CD. SRE gave us SLOs and toil. RAIM now adds a new layer of shared understanding:


- **Design-to-Learn loops**
- **Feedback completeness**
- **Journey-informed SLOs**
- **Architecture as failure containment**


This isn’t jargon — it’s fluency. It allows product, platform, security, and risk teams to discuss the same system in the same room.


## Why This Matters Now


Resilience is no longer just technical. It’s reputational. Regulatory. Relational.


The systems we design don’t live in isolation. They shape customer outcomes, economic stability, and institutional trust. From cloud concentration risk to digital operational resilience frameworks, the message is clear: **resilience isn’t optional. It’s expected** .


And that expectation is shifting upstream — from operations teams to boards, from post-mortems to strategy decks.


## What Comes Next


The road from RAIM leads in different directions, depending on where you stand:


- **If you’re a leader** : Ask where learning lives. What happens *after* the fix?
- **If you’re an architect** : Model not just performance, but failure. Who owns it when things go sideways?
- **If you’re an engineer** : Don’t stop at recovery. Trace the insight. Reuse the fix. Teach the loop.


Resilience doesn’t need a charter. It needs a coalition.


## A Final Word


RAIM is not the answer. It’s a better question.


> *“What would it look like if learning was the unit of resilience?”*


That’s a question worth carrying into every incident review, architecture session, and leadership meeting.


Because that’s where the real loop begins.


Press enter or click to view image in full size


The road ahead


## Section 7: Where This Model Came From


RAIM (Resilience Architecture Integration Model) didn’t come out of thin air. It’s a result of years of working across systems, teams, incidents, and conversations — combined with lessons drawn from books, frameworks, and thinkers who’ve shaped how many of us approach resilience, architecture, and culture.


Some of what you’ve read here is my interpretation. Some of it is borrowed, reframed, or extended. This is a short acknowledgement of the sources that genuinely influenced this thinking — not as footnotes, but as foundations.


## Systems and Learning


- **Peter Senge — *The Fifth Discipline*** *,* for laying out what it means to think in systems. The concept of feedback loops, learning organisations, and long-term thinking underlies RAIM — even if it’s not always visible on the surface.
- **Donella Meadows — *Thinking in Systems*** *,* for showing how system behaviour often comes from structure, not people. That shaped how we look at resilience issues as system signals, not isolated failures.
- **Donald Schön — *The Reflective Practitioner*** *,* for introducing the idea of reframing *.* This has been key to how we approach design reviews, learning from chaos, and post-incident analysis.


## Architecture and Team Boundaries


- **Team Topologies — Matthew Skelton & Manuel Pais** helped clarify how team boundaries, ownership, and cognitive load affect everything from system design to recovery under stress.
- **Site Reliability Engineering (Google)** Reinforced the importance of aligning architecture and operations — not just with metrics, but with empathy for failure modes and recovery paths.


## Culture and Chaos


- **Amy Edmondson — Psychological Safety** gave language to what many teams feel but don’t always name: that people don’t learn from failure if they’re afraid of blame.
- **Casey Rosenthal & Nora Jones — Chaos Engineering** For making chaos not just about breaking things, but about forming reasonable hypotheses and building systems that teach us something under pressure.
- **John Allspaw** — for profoundly influencing how we think about incidents, surprise, and systemic learning. His work on adaptive capacity and learning from incidents is foundational to the mindset RAIM encourages.
- **Aaron Rinehart** — for pioneering *Security Chaos Engineering* and expanding resilience thinking into the domain of proactive security, helping teams treat defence as a dynamic system property.
- **Kolton Andrus** — for operationalising chaos engineering at scale and showing how to treat failure testing as a core design activity — not a one-off test, but an ongoing discipline.


## Observability and Storytelling


- **Charity Majors, George Miranda, and Liz Fong-Jones — Observability Engineering** . For reframing observability as a storytelling practice, not just instrumentation. Their emphasis on debugging unknown unknowns, owning production, and enabling fast feedback loops has shaped how RAIM approaches observability: not as tooling, but as a narrative context for resilience.


## Governance and Regulation


- **FCA/PRA Resilience Guidelines (UK)** played a significant role in shaping the need for mapping journeys to accountability. Not just as policy, but as a way to clarify ownership.
- **DORA (Digital Operational Resilience Act)** reminds us that platform decisions, incident learnings, and regulatory language must align — not as a compliance checklist, but to build trust at scale.


## InnerSource and Community Enablement


**James McLeod — Head of Open Source, NatWest Group** For demonstrating how InnerSource and OSPO leadership make resilience participatory — not just technical. His work at NatWest and FINOS helped shape how patterns get shared, teams learn, and feedback becomes part of everyday engineering culture.


None of this work would exist without the contributions of the above thinkers and practitioners. This is my attempt to stand on their shoulders, connect some dots, and build a bridge between technical resilience and enterprise-wide coherence
