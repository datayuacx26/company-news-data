---
schema_version: "1.0.0"
document_id: "115e311676749e770fab6152027cbb19c7df51a1a73009cfe6b3b8254635a277"
company_key: "yc-waydev"
company: "Waydev"
source_id: "yc-waydev-rss-a82ef0eb6171"
canonical_url: "https://waydev.co/when-ai-writes-the-code-what-do-we-measure/"
published_at: "2026-02-17T19:10:20+00:00"
first_seen_at: "2026-07-20T23:24:47.561282+00:00"
fetched_at: "2026-07-28T22:19:47.852928+00:00"
content_hash: "sha256:8a3ab895a9bdf0e5381193eb1caaed0247f039102da93545ba7f38a786e8c0d9"
---

# The Engineering Leader’s Paradox: When AI Writes the Code, What Do We Measure?

## Table of contents


- The Measurement Crisis Nobody’s Talking About
- What We’re Actually Measuring Now (Whether We Admit It Or Not)
- The Question That Haunts Me


- 1. Decision velocity matters more than coding velocity
- 2. AI orchestration skill is the new coding skill
- 3. The “why” matters more than the “what”


- What This Means for Engineering Leadership
- The Waydev Perspective: What We’re Building Toward


- 1. From activity metrics to outcome metrics
- 2. From individual productivity to system efficiency
- 3. From lagging indicators to leading indicators
- 4. From human-centric to hybrid-centric measurement


- The Uncomfortable Truth
- What We’re Building for This New Reality
- The Opportunity
- Where This Goes


## Key Takeaways


- AI tools like Codex significantly increase productivity, but traditional metrics fail to capture their true impact.
- The measurement crisis exists because current metrics assume humans write code, while AI does much of the coding now.
- Key focus shifts to decision velocity, AI orchestration skills, and providing context to AI rather than just coding speed.
- Engineering leaders must adapt their metrics to reflect the new reality, focusing on human-AI collaboration and strategic decision-making.
- Future measurement frameworks should prioritize outcomes, efficiency, and hybrid systems to accurately capture productivity in AI-augmented environments.


I came across something very interesting last week.[Sherwin Wu](https://www.linkedin.com/in/sherwinwu1/) just[pulled back](https://www.lennysnewsletter.com/p/engineers-are-becoming-sorcerers) the curtain on engineering at OpenAI, and what he revealed changes everything we thought we knew about[measuring productivity](https://waydev.co/measure-developer-productivity/) .


**95% of their engineers use Codex** . Engineers who **embrace AI tools open 70% more pull requests** than their peers. The **average PR review time dropped from 10-15 minutes to 2-3 minutes** . Engineers are managing 10-20 parallel AI coding threads instead of writing code themselves.


As someone who’s spent the last seven years building engineering analytics, here’s what keeps me up at night: **the metrics we’ve been measuring are answering yesterday’s questions.**


And honestly? That’s the most exciting problem I’ve ever faced.


## **The Measurement Crisis Nobody’s Talking About**


Let’s be direct about what’s happening.


When an engineer at OpenAI manages 20 parallel Codex threads and **opens 70% more PRs than their peers** , what are we actually measuring?


- **Lines of code?** The AI wrote those.
- **Commit frequency?** The AI commits.
- **PR velocity?** Inflated by AI-generated submissions.
- **Code review time?** Codex reviews before humans see it.
- **Cycle time?** Compressed by automation, not human improvement.


Every traditional engineering metric assumes **humans write the code** . That assumption just broke.


*We’re trying to measure productivity with tools designed for a world where engineers typed into IDEs for 6 hours a day. That world ended sometime in 2023, and most engineering leaders haven’t noticed yet.*


## **What We’re Actually Measuring Now (Whether We Admit It Or Not)**


Here’s what Sherwin’s observations reveal: the job description changed, but the measurement framework didn’t.


**The old job:** Write code, review code, deploy code.


**The new job:** Orchestrate AI agents, steer parallel workstreams, make judgment calls AI can’t make, review AI output for strategic alignment.


The engineers opening 70% more PRs aren’t “more productive” in the traditional sense. They’re **better at managing AI agents** . **They’re better at prompt engineering** . They’re better at knowing which problems to delegate and which require human judgment.


That’s a completely different skill set. And we’re measuring it with the wrong instruments.


**It’s like measuring a Formula 1 driver’s performance by tracking how hard they press the gas pedal. The pedal pressure doesn’t matter, the lap time does. But we’re still reporting on pedal pressure because that’s what our dashboards were built to show.**


## **The Question That Haunts Me**


If **AI writes 95% of the code at OpenAI** , and that percentage is rising across the industry, **what should engineering leaders actually measure?** Here’s what I’m seeing with our customers who are furthest along this curve:


### **1. Decision velocity matters more than coding velocity**


The bottleneck isn’t “ **how fast can we write code?** ” anymore. It’s “ **how fast can we decide what to build, validate it’s the right thing, and course-correct when it’s not?** “.


The engineers thriving in AI-augmented environments make faster, better decisions. They kill bad ideas earlier. They validate assumptions quicker. They iterate on product direction, not syntax.


Traditional metrics don’t capture this. PR count doesn’t show decision quality. Commit frequency doesn’t show strategic thinking.


### **2. AI orchestration skill is the new coding skill**


Sherwin mentions engineers running 10-20 parallel Codex threads. That’s not about typing speed, it’s about managing complexity.


The best engineers know how to:


- Decompose problems into AI-delegatable chunks
- Prompt effectively for the outcome they need
- Review AI output for correctness, edge cases, and architectural alignment
- Integrate AI-generated code into coherent systems


This is a learned skill. Some engineers pick it up immediately. Others struggle. The productivity gap Sherwin mentions (between AI-embracing engineers and others) isn’t about who types faster. It’s about who adapts faster.


And right now, most engineering orgs have no visibility into who’s adapting and who’s falling behind.


### **3. The “why” matters more than the “what”**


When AI can generate the implementation, the engineer’s value shifts to defining the problem correctly.


Bad prompt: *“Write a user authentication system.”*


Good prompt: *“ **Write a user authentication system that handles edge case X, integrates with our existing session management, follows our security standards Y, and accounts for the scaling constraint Z we hit last quarter.** “*


The difference is **context** , **judgment** , and **institutional knowledge** . AI doesn’t have that. Humans do.


So the question becomes: **How do we measure whether engineers are providing the right context to AI agents?**


Is it quality of AI-generated code after human review? Is it defect rates in AI-assisted PRs vs. human-written PRs? Is it time from prompt to production-ready code?


## **What This Means for Engineering Leadership**


Here’s where this gets uncomfortable for most CTOs and VPs of Engineering.


**Your current dashboards are measuring the wrong things.**


If you’re still reporting to your board on:


- Story points completed
- Lines of code written
- Individual engineer commit counts
- PR volume


You’re reporting on theater. **You’re measuring the appearance of productivity, not the reality of value creation.**


Worse, you’re potentially optimizing for the wrong behaviors. If engineers know they’re measured on PR count, and AI lets them open 70% more PRs, guess what happens? PR count goes up, but business outcomes might not.


**The scary part:** Most engineering leaders know this, but don’t know what to measure instead.


The frameworks we’ve relied on were all designed for human-written code. They’re not wrong, exactly. They’re just incomplete for an AI-augmented world.


## **The Waydev Perspective: What We’re Building Toward**


At Waydev, we’ve been having versions of this conversation with engineering leaders for the last 18 months. The questions keep evolving:


- **“How do I measure productivity when AI writes the code?”**


- **“How do I know which engineers are adapting to AI tools and which are resisting?”**


- **“How do I demonstrate ROI on engineering when traditional metrics are inflated by automation?”**


- **“What do I tell my board when they ask why our velocity is up 50% but time-to-market hasn’t improved?”**


Here’s our thesis on where engineering analytics needs to go:


### **1. From activity metrics to outcome metrics**


Stop measuring how much code gets written. Start measuring how quickly validated ideas reach customers.


The relevant metrics:


- **Time from decision to deployment** (not time from commit to merge)
- **Iteration velocity on strategic initiatives** (how fast can you test and learn)
- **Decision quality** (% of shipped features that drive intended outcomes)


AI compresses the implementation phase. So measure the phases AI can’t compress: problem definition, strategic alignment, outcome validation.


### **2. From individual productivity to system efficiency**


The engineer managing 20 AI threads isn’t “ **20x more productive.** ” They’re leveraging a different system architecture.


What matters:


- **How efficiently does your org translate intent into working software?**
- **Where do AI-assisted workflows create bottlenecks?** (Spoiler: often in review, validation, and integration — the human steps)
- **How quickly can teams absorb and act on AI-generated output?**


This shifts measurement from “how productive is Engineer X?” to “how efficient is the human-AI system?”


### **3. From lagging indicators to leading indicators**


Traditional metrics tell you what happened. In an AI-augmented environment, you need metrics that predict what’s about to happen.


Sherwin mentions that top performers become disproportionately more productive with AI. The gap is widening.


**Can you identify your top AI-augmented performers before they leave for a startup that will 10x their leverage?** Can you see which teams are adopting AI tools and which are resisting? Can you predict which engineers will thrive in the next 2-3 years?


These are leading indicators. Most dashboards show lagging indicators.


### **4. From human-centric to hybrid-centric measurement**


Your engineering org is no longer just humans. **It’s humans + AI agents.**


Eventually, you’ll need to measure:


- **Human contribution:** Strategic decisions, context provision, quality oversight
- **AI contribution:** Code generation, test coverage, initial review
- **Human-AI interaction quality:** How well do engineers orchestrate AI agents?


The winning orgs will be the ones who optimize the collaboration between humans and AI, not just one or the other.


## **The Uncomfortable Truth**


Here’s what I believe, even though it makes our product roadmap significantly harder:


**Most engineering metrics will need to be reinvented in the next 3 years.**


The frameworks we built at Waydev, well, the frameworks the entire industry built, were optimized for measuring human engineering teams. They worked because the assumptions held: **humans write code** , **humans review code** , **humans deploy code** .


**Those assumptions are breaking.** Fast. At some companies, **AI already writes the majority of code** . At OpenAI, it’s 95%. Within 2-3 years, **Sherwin believes we’ll see one-person billion-dollar startups** .


If that’s true, and I think it is, then measuring “how many PRs did your team open this sprint?” is like measuring how many horses your company owns in 2024. Technically measurable. Completely irrelevant.


## **What We’re Building for This New Reality**


**At Waydev, we saw this shift coming. Not because we’re clairvoyant, but because our customers at the frontier started asking questions traditional metrics couldn’t answer.**


For the past 18 months, we’ve been working with engineering orgs where 50%+ of code is AI-generated, where engineers orchestrate AI agents more than they write functions, where the old playbook has already broken down.


What we’ve learned is shaping the next evolution of engineering analytics:


**New measurement frameworks we’re deploying:**


- **AI adoption impact tracking:** Correlating tool usage with actual delivery outcomes, not vanity metrics
- **Decision velocity vs. implementation velocity:** Separating strategic thinking time from execution time
- **AI-augmented performance indicators:** Identifying which engineers excel at human-AI orchestration
- **Hybrid workflow efficiency:** Measuring the effectiveness of human-AI collaboration, not just human productivity


These aren’t experiments anymore. **They’re becoming core capabilities in Waydev** – because the companies winning in AI-augmented environments are the ones measuring the right things.


**The shift is clear:** Engineering Intelligence needs to evolve from measuring coding activity to measuring strategic leverage. **We’re building for that future, not retrofitting old frameworks.**


## **The Opportunity**


Here’s the exciting part, and why I’m more energized about Waydev’s mission now than ever:


**Engineering leadership is about to get way more strategic.**


When AI handles the implementation, the human contribution shifts entirely to judgment, strategy, and orchestration. That’s a higher-leverage role.


CTOs won’t spend board meetings defending story points. They’ll discuss:


- How effectively their org translates strategy into shipped value
- Where human judgment is most critical in the AI-augmented workflow
- How to identify and empower engineers who excel at AI orchestration
- What systemic inefficiencies exist in their human-AI collaboration model


This is a better conversation. More strategic. More tied to business outcomes.


But it requires different data. Different metrics. Different dashboards.


That’s what we’re building.


## **Where This Goes**


Sherwin says the next 2-3 years will be the most exciting in tech history. I believe him.


For engineering leaders, the challenge is this: **your job is changing faster than your measurement tools.**


**You can’t manage what you can’t measure.** And right now, most of what matters in an AI-augmented engineering org is invisible to traditional dashboards.


The orgs that figure out the new measurement framework first will compound advantages faster than their competitors. They’ll identify top AI-augmented performers earlier, optimize human-AI workflows better, and make faster strategic decisions.


The orgs that keep measuring lines of code and PR velocity will optimize for the wrong things and wonder why their “ **productivity improvements** ” **don’t translate to business outcomes** .


**At Waydev, our job is to help engineering leaders see clearly in this transition** . To measure what actually matters, not what’s easy to measure. To provide visibility into the human-AI system, not just the human part.


Because here’s what I know for sure: **the future of software development isn’t about writing more code. It’s about making better decisions about what code to write, and orchestrating AI agents to write it.**


And if that’s the future, then the measurement frameworks need to evolve too.


[Get a demo call](https://waydev.co/demo/) with Waydev to explore the new frameworks we’re creating for AI-augmented teams.


Let’s build that future together,


*Alex*
