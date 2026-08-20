---
schema_version: "1.0.0"
document_id: "839f29a63763717460dbd92639fb8e343ffb2ae60e7261c70b8416d0012ffef2"
company_key: "yc-emergent"
company: "Emergent"
source_id: "yc-emergent-news-import-fc8f7e880d65"
canonical_url: "https://emergent.sh/blog/introducing-e-3-autonomous-app-building-on-emergent"
published_at: "2026-06-08T00:00:00+00:00"
first_seen_at: "2026-07-21T18:00:00.368523+00:00"
fetched_at: "2026-07-28T21:42:42.932365+00:00"
content_hash: "sha256:249ff14f54ef5f26e77835093ac676f8a383e1d8ee24954a16b701d78d5dcb9f"
---

# Introducing E3: Autonomous app building on Emergent

Building an app with AI today is a bit like hiring a contractor who's great at every individual task but needs you on-site all day telling them what to do next. For simple projects, that's fine. For anything bigger, you end up spending more time directing the work than thinking about what you're actually building.


E3 changes that. It's an autonomous builder available on Emergent's Pro plan, and it works less like a contractor waiting for instructions and more like a project lead who takes a brief and delivers the finished product. You describe what you want, E3 figures out how to build it, and you get a functional build back that you can preview, test, and deploy. The complexity of the project stops being your problem to coordinate.


This post covers how E3 works, what's happening under the hood, and when to use it.


## How E3 works


E3 follows a structured sequence, and every step is designed to keep you in control without keeping you in the loop on every decision. At its core, E3 is an orchestrator. It drives E1 (Emergent's core builder agent) as a sub-agent, coordinating the planning, building, testing, and fixing across a much longer horizon than a single E1 session could handle. Here's what that looks like in practice.


### Start with a brainstorm


When you select E3, you don't jump straight into a build. E3 works by driving E1 (Emergent's core builder agent) on your behalf, so before it starts, it needs to lock down the decisions that would otherwise surface mid-build and stall progress.


E3 opens a brainstorm conversation where it interviews you about the project.


In the screenshot above, E3 has recognized that the project is a 6-phase SaaS, too large to build all at once. Before committing to a plan, it's working through the key questions: which platform to build first (web or mobile), whether billing should be functional or placeholder for the MVP, and whether third-party API credentials are ready or if E3 should use fallback options for now. These are the kinds of decisions that, left unresolved, derail a build three phases in.


### Review the plan


Once the brainstorm is complete, E3 produces a structured build plan broken into numbered phases. You review this plan before anything gets built.


In the screenshot above, E3 has laid out the full phase sequence and is asking the user to confirm before it starts.


The options are specific to the project:


- Approve and start Phase 0 (a proof-of-concept)
- Skip the POC and go straight to Phase 1 (faster, but riskier on the Reddit and OpenAI integration)
- Adjust the scope and ordering


E3 isn't just presenting a plan and waiting. It's flagging the tradeoffs of each path so you can make the call quickly.


### Watch progress from Mission Control


While E3 builds, you get a live Mission Control view in your chat. Progress cards stream in as each phase completes, showing you what was built, what was tested, and what passed.


You can pause the build, resume it, or redirect E3 mid-way if your priorities shift. You're never locked out, but you're also never required to babysit.


### Get the finished product


When the build is complete, E3 delivers a summary with test results, a preview link so you can click through what was built, and a deploy button to take it live.


From brainstorming to deployed apps, without managing a single build step yourself.


## Three capabilities behind E3's autonomy


E3's autonomy rests on three capabilities that directly affect what non-technical builders can take on.


### Self-managing agent orchestration


Until now, for complex multi-phase projects, the coordination between sessions still fell on you: deciding when to test, when to pivot, and how to sequence the next step. E3 takes over that coordination by maintaining long, sticky conversations with E1, working through problems the way a human project lead would rather than handing them back to you.


An E1 session typically runs for 10 to 15 minutes, which is well-suited to focused, scoped tasks where you want direct control. E3 extends that to multiple hours, working through most blockers without needing your input and keeping the build moving across a much wider scope.


### Agentic verification, no human in the loop


This is arguably the biggest change. Emergent already had a robust testing protocol: dedicated test cases for backend logic and automated browser testing that ran through frontend flows after every build. This was more rigorous than what most AI platforms offer. E3 adds a layer on top: agentic testing. E1's existing tests still run, but after they pass, E3's testing agent goes further.


Instead of running pre-written scripts, E3's testing agent tests the way a human would, clicking through the app, checking if things work as expected, and flagging what doesn't. The difference is that no human is actually needed.


When E3 finds a problem, it doesn't just report it back to you. It routes the issue to the appropriate agent, gets a fix, and re-verifies. In most cases, the verification loop is closed without you needing to step in. For complex builds where the gap between 'code works' and 'product works' is wide, this removes one of the biggest bottlenecks: waiting for a person to test and confirm.


### Monorepo: Web and mobile from one codebase


E3 supports building a full-stack web application and a mobile app simultaneously within a single repository. On Emergent, web apps use React, Python, and MongoDB; mobile apps use Expo (React Native) with FastAPI and MongoDB. Previously, these were separate agent modes with separate sessions. E3 can coordinate both in parallel, which means you can describe a product that works across web and mobile and have both built in the same run.


## When to use E3 vs. E1


E3 isn't built for everything, and it doesn't need to be.


**Use E1 or E2** directly for well-defined builds where you already know what you want: a landing page, a client portal, a single-feature tool. E1's conversational loop gives you direct control over every step, which is ideal when you want to steer the build in real time and iterate as you go.


**Use E3** when the build is complex enough that coordinating the process becomes a project in itself. A CRM with multiple modules. A SaaS product with auth, billing, and admin dashboards. Anything where you'd normally need to break the build into multiple sessions, manage context across them, and spend significant time testing between each stage. E3 is designed for exactly these cases: complex, multi-feature apps where the planning and verification overhead is what slows you down.


One practical note on cost: E3 consumes approximately 30% more tokens than E1 for comparable tasks, which is the natural overhead of running a longer, more autonomous process with built-in verification and agent coordination. For projects where the alternative is hours of manual oversight and multi-session debugging, that tradeoff is straightforward. E3 also builds phase by phase, not all at once. Each phase is tested before the next one begins, so the process is methodical rather than instantaneous, but what you get at the end actually works.


## Availability and pricing


E3 is currently in beta and available exclusively to[Pro plan](https://emergent.sh/pricing) users ($200/month). If you're already on Pro, you can select E3 from the agent selector and start a brainstorm conversation immediately.


## What this means for builders


The promise of AI-powered app building has always been that non-technical people can ship real software. E3 pushes that further by removing the last piece of technical work that still fell on you: managing the build itself.


You don't need to know how to sequence a multi-phase build. You don't need to write test cases or know when something should be re-verified. You don't need to keep the agent on track across a complex project. You describe what you want, approve a plan, and get a working app back.


If you've had a product idea sitting in a document because the build felt too complex to manage, E3 is built for exactly that.[Start a brainstorm](https://emergent.sh/) on Emergent's Pro plan and see what E3 produces.
