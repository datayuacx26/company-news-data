---
schema_version: "1.0.0"
document_id: "8d4effafee7e6245dd33db2f60d095a46e2fa3c4e26e1a2fa35dd9ad70660f8e"
company_key: "yc-speak"
company: "Speak"
source_id: "yc-speak-news-import-4988501dda0c"
canonical_url: "https://www.speak.com/blog/agentic-engineering"
published_at: "2026-02-11T00:00:00+00:00"
first_seen_at: "2026-07-22T14:26:08.696382+00:00"
fetched_at: "2026-07-28T22:20:59.045830+00:00"
content_hash: "sha256:b6d09d121119252850a564627ca7ae41ee5d18345f8adf5732dd23232929b733"
---

# Embracing the agentic engineering era at Speak

Speak has been AI-native and singularly focused on our vision of building a superhuman AI teacher, since day one. We’ve always focused on making something people want and bringing real value to our learners. As a company and team, we value truth-seeking, intellectual curiosity, and we care about adapting and moving quickly when new truths emerge.


That’s why it was so exciting when it became clear to us in December 2025 that agentic AI coding had crossed a capability threshold such that the era of software engineers writing code by hand was over. We realized that this was a fundamental change to the way we develop software, and we needed to immediately and urgently embrace it.


This post shares a very-much-in-flux snapshot of what we’re observing and learning, and where we think things are going. Some of this thinking may seem very wrong or outdated later this year, which is sort of the point - we can’t wait to learn which part.


### **What is happening?**


Throughout most of 2025, AI coding agents were popular for vibe-coding prototypes and side projects, but arguably still supplementary for professional software engineers building production systems at scale. Most were using Cursor and AI autocomplete as a productivity boost on the order of 25%.


That changed meaningfully in December 2025, with the release of Claude Code with Opus 4.5. This combination of new model with upgraded harness meant that the day-to-day work of a software engineer flipped from mostly typing by hand into a code editor, to directing and orchestrating AI coding agents using natural language.


This didn’t feel just like a decent productivity boost - it felt more like a totally different activity. We're now in what Andrej Karpathy calls the era of "agentic engineering."


### **Some observations**


**The pace of change is unintuitive and often disorienting, even for the best engineers** ‍


Many opinions formed in January 2026 are already stale, and those formed in November 2025 are largely invalid. This is genuinely disorienting and often even more so for experienced engineers with hard-won engineering instincts and battle-tested lessons about how to build great software.


Engineers have to hold two realities simultaneously: build with today's capabilities *and* anticipate the next step function, which may arrive next month.


**Signal is hard to find in the noise**


There's an enormous amount of hype and online discourse about new techniques, tools, and workflows, and it's easy to feel like you're falling behind. The tricky part is that a subset of the hype is real, but it’s very hard to tell which subset.


We’ve found that the most reliable filter is to (1) stay focused on building and specialization within our own domain and problems, and (2) actually spend meaningful hours trying it for yourself on real projects.


Coding model capabilities are uneven and jagged, and some models "feel" better to work with but make more mistakes. Benchmarks and evals are directional at best. You have to develop your own intuition through use.


**The meta is still evolving fast**


As of Feb 2026, the current frontier is converging around the concept of "agent teams" - teams of multiple AI coding agents parallelizing work across different segments of a project and coordinating with each other. This was barely a concept six months ago.


There's a large and growing gap between teams that stay near the progress frontier and those that don't. Most teams won't be able to keep up, but the ones that do will have a compounding structural advantage.


**People and culture shift**


We’ve always hired engineers that view code and models as tools to solve user problems and deliver business value, so in general our engineering team has embraced this new reality with huge excitement. But engineers that tie their identity to the code and systems they've built, rather than to the outcomes they deliver, will struggle to rethink deeply held assumptions and adapt.


On the other side, we’re seeing AI leaders emerge organically who are particularly skilled at learning and extracting leverage from the tools, and evangelizing usage to help level up the team around them.


### What we're doing at Speak


**Commit explicitly and make room to learn**


We've explicitly committed to transforming how we work and talk about it openly. This is a deep cultural shift and we’re leaning into the change and giving our team permission to experiment, break things, and take time to learn.


Agentic software development is a genuine skill with a real learning curve. It takes dozens of hours before you start to feel comfortable using it professionally. We've been explicit with our team that transforming how we work is the top priority in Q1, even if some things break and take more time.


We're starting with our engineering team as the experts and tip of the spear, but the agentic development vision extends across the full product development lifecycle - product, design, content, and more.


**Use forcing functions to accelerate learning**


We ran a hack week in January with one rule: write as little code by hand as possible. We thought about it as a forcing function to carve out time and space from the regular sprint cycle to learn agentic engineering. It was unsurprisingly super energizing for engineers to stretch their wings and work across traditional boundaries and platforms (and feel the pain of long mobile build times!).


We ran a detailed survey afterwards to understand primary tooling and workflows, what the team learned, and how much their day-to-day had actually changed. This was hugely valuable “AI state of the union” signal across the engineering team, and helped us deeply understand which parts of the work have already climbed the abstraction ladder, and which parts remain frustrating and need focused infrastructure buildouts. For example, it exposed the shared need for sandboxed full Speak development environments for background coding agent execution, which we’re actively building out.


**Invest in shared infrastructure**


We're focused on repo readiness - making our codebase, workflows, and documentation legible to AI agents. This includes building a shared agent skills repo, reusable repo-level context, and learnings that level up the whole team rather than just individuals.


**Make something people want**


You can spend days and weeks optimizing your Claude Code local config, MCP setup, skill hooks, and subagent panel. But we always try to come back to the question, did you make something people want? What did you ship? We think the best way to learn here is to learn by doing and by building something real. This is exactly Speak’s ethos of learning to speak by speaking out loud.


### Where we think things are going


**Organizational changes to support execution velocity**


Decisiveness and clarity of direction for teams is becoming even more critical, and getting this right will allow teams to move radically faster. We're organizing all of our pods to be more self-sufficient, with leadership focused on ensuring cohesion across the product rather than gatekeeping what gets built.


**Context engineering becomes the core skill**


The best mental model for understanding agentic capabilities is that agents are bounded by access to the right context. They can't work on what they don't know about, and they don't know about what isn't written down or logged. This principle generalizes far beyond coding to every company workflow. The teams that get great at structuring and surfacing context will get disproportionate leverage.


**Functional boundaries are blurring**


The most impactful people will be the ones who stretch across traditional roles. We don’t think functions will go away, but PMs and designers who can build prototypes and fix bugs, or engineers who can build end-to-end without waiting for specs, will be dramatically more productive.


**Every function climbs the abstraction ladder**


What's happening to engineering is happening to all functions. As an example, our content team used to write every single lesson script in Speak’s curriculum by hand, just like our engineers used to write every line of code by hand. This is all changing and every function is abstracting from doing the work directly, to directing AI that does the work and designing the systems and tasteful evals and guardrails that ensure the work is done well.


**The engineering frontier continues to push outwards**


There’s debate over whether software engineers today spend 75% of their time in ‘agent mode’, or 85%, or 95%, but there’s no debate that the number is moving ever closer to 100%.


This means that we're approaching a world where engineers don’t manually write or edit code at all. Code review will shift from line-by-line inspection to AI summarization, automated reviews, improved testing infra, and fast rollback. Some things will definitely break, but the AI agent will become the technical tool of first resort.


This also means we’re deeply rethinking our engineering interview process, because the skills that define a world-class engineer are shifting in real time.


**Transform or die**


We believe the best teams in technology will use agentic development to dramatically accelerate their velocity and expand the scope of what’s possible for a team to ship. The best teams are already doing this, and the gap between those who adapt and those who don't will only widen.


We originally started Speak when we felt an inflection point with AI progress unlocking the superhuman AI teacher. It’s clear that 9 years later, we’re at another key inflection point, and being in the arena while all this is happening is the most fun and energizing time in Speak’s history.


If this resonates and you want to be part of a team thinking this way, reach out at[\[email protected\]](https://www.speak.com/cdn-cgi/l/email-protection#fc9d9b999288959fbc8f8c999d97d29f9391) .
