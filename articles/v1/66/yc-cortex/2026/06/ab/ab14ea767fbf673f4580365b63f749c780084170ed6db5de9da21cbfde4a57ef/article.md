---
schema_version: "1.0.0"
document_id: "ab14ea767fbf673f4580365b63f749c780084170ed6db5de9da21cbfde4a57ef"
company_key: "yc-cortex"
company: "Cortex"
source_id: "yc-cortex-news-import-d57bc7656b70"
canonical_url: "https://builders.cortex.io/blog/software-factories-are-operating-models/"
published_at: "2026-06-25T15:15:00+00:00"
first_seen_at: "2026-07-24T03:08:40.561120+00:00"
fetched_at: "2026-07-28T21:42:09.561502+00:00"
content_hash: "sha256:2206a30ee8f72b1aee1454b73005cb2585e6a26beebc7d4144f318784a9847fe"
---

# The software factory is an operating model, not a coding agent

## Skeptic Turned Vibe Coder


Earlier this year, after 6 years of continuously building Cortex, I had the opportunity to step away for 3 months — to tend to my *actual* first baby.


In the first month of my paternity leave, between bottle feeding and dodging streams of pee, I found myself with a lot of time on my hands for the first time since college. I had no obligations to write code for work! So naturally, I used the time to write code for fun.


We at Cortex have always been AI forward, being early adopters of Claude Code and Cursor. But models hallucinated, AI authored changes were heavily scrutinized, and productivity gains were modest. It was not until this time that I was coding side projects, without fear of breaking prod, that I fully allowed myself to “let go” and vibe code tens of thousands of LoC.


It certainly did not start that way. This was a completely non-trivial project: it involved low latency digital signal processing and optimizing machine learning inference in WebGPU! Model capability simply was not there 6 months prior, yet I found myself more and more slipping into a trust but verify model with its output, letting the AI write more and more of the code, until it was writing all of it.


And that process of “just letting go” and trusting the machine has quickly bled from hobbyists into the broader industry.


## What Actually Changed?


Starting late 2025, AI models and harnesses seemed to turn a corner.


Several frontier labs had unusually concentrated releases, back to back:


1. 2025-11-18


Gemini 3 Pro


Google claimed substantial gains over Gemini 2.5 Pro across its major reasoning benchmarks, with strong tool use and planning.


2. 2025-11-19


GPT-5.1-Codex-Max


OpenAI’s first model “natively trained” to operate across multiple context windows via compaction, specifically for project-scale refactors, debugging, and multi-hour agent loops.


3. 2025-11-24


Claude Opus 4.5


Anthropic reported a 10.6% jump over Sonnet 4.5 on Aider Polyglot benchmark and significantly better long-horizon behavior, efficiency, tool execution, and instruction following.


4. 2025-12-17


Gemini 3 Flash


Google reported 78% on SWE-bench, while being 3x faster than Gemini 2.5 Pro.


5. 2025-12-18


GPT-5.2-Codex


OpenAI followed with another coding-focused release aimed at realistic repository and terminal tasks.


But it was not just the models that got significantly better, the harnesses around the models improved as well. An early coding assistant mostly responded to the code and context a developer placed in front of it. Harnesses now can search a repository, inspect history, edit multiple files, execute commands, run tests, read the failures, and iterate on that in a tight loop until it achieves its goal.


And the two improvements have lead to a virtuous cycle, with OpenAI and Anthropic both revealing that they do post training reinforcement learning on real software engineering activites, like PR creation, code review, and frontend work. As the models get better the harnesses get better, and vice versa.


This lead to a meaningful shift from the older paradigm of prior years, which was to train a *general* chatbot, expose it to a shell, and hope for the best.


The obvious immediate reaction is to ask just how much code agents can write. If agents are so good then all of software is solved right? And to be clear, this was the case for my small side project with zero users and well bounded scope.


But that’s the wrong abstraction for software at scale — once agents can complete bounded, smaller engineering tasks, the more important question is how those tasks fluidly move through an organization.


## Software Factory ≠ Coding Agents


Software development is not just code generation in a vacuum. It’s one component of a larger SDLC, or *software factory* : filtering product intent and continuous improvement initiatives through triage into small tickets of work to be done by a human or AI.


At Cortex today, our factory roughly looks like this:


-


Plan
-


Build
-


Production
-


Feedback


Production feedback becomes new work. The factory is the loop, not any single coding agent inside it.


Of course it’s not *so* rigid. To take a trivial example, often times customer escalations will be triaged and taken entirely as-is as a ticket to work on for the engineering team, but this is generally how larger projects are built here.


But our view here at Cortex is that every software factory, whether human or machine run, will look slightly different from one organization to another — a large financial institution fundamentally runs differently than a Y Combinator startup.


And indeed, when we were still a burgeoning four person startup in 2019, our software factory looked more like this:


In 2019, our factory was a straight line from idea to deploy.


Maybe your factory has load testing, or a pass through the legal department, or a signoff ritual with the CEO where you get publicly berated.


The point is that different businesses will fundamentally have different software practices, and you need to decide what it is you’re optimizing for.


Astute readers will ask “but isn’t that just the SDLC?” And yes that’s exactly my point!


Users of coding harnesses like Claude Code with “plan” features have likely experienced this first hand with AI. When you take time to upfront plan the general approach and work through potential blockers and design decisions up front, writing the code becomes smoother, to the point of being nearly mechanical.


Is that not a lesson organizations have collectively learned through tech spec culture?


## Every Organization Already Has a Software Factory


We all already have a software factory and it’s called the SDLC. It may be inefficient, inconsistent, or sloppy, but it is there.


With coding agents, there was first a hyperfocus on just writing as much code as possible, with the largest scope of work ending at the “Build + PRs” phase. Then, as unreviewed pull requests piled up in our inboxes, a slew of “code review bot” companies sprouted up to solve *that* bottleneck.


The next frontier is companies like Factory and Ona, which can continually pull work items off of a queue like Jira, and self drive larger roadmaps. This is where the category of “software factories,” as it relates to AI, is currently at.


But I think the category is not complete until software factory products, and AI in general, can fluidly participate in every stage of the SDLC.


## The Gradual Automation of the Factory


The world of software is moving fast and organizations that do not fully utilize AI in their SDLC will get left behind by their speedy competitors.


But in the ideal state, humans are not replaced by machines but operate either exactly where they currently sit in the SDLC, meta management of the factory itself, or both. Management of the factory can look like optimizing competing levers for speed of delivery, reliability, and cost, analogous to DevOps, Platform Engineering, and SRE today.


And this change will not and should not happen overnight! A responsible organization should strategically replace or reorient human-in-the-loop within the factory as is appropriate for their business.


## Autonomy Is Earned, Not Enabled


Much like I experienced during my side project, autonomy in the factory is an earned privilege, only after repeatedly hitting its mark, at an accuracy indistinguishable from (or even better than) a human.


Thinking through the historical progression of AI in software:


- The earliest examples include GitHub Copilot, which could autocomplete snippets of code
- Once you trust that, then being able to write entire functions, or even files, with a prompt
- Once you trust that, it graduates to being able to take Jira tickets and create entire pull requests that are approved by an engineer.
- Once you trust that, then being able to effectively break down a tech spec into Jira tickets
- And so on


Every step of the way the progression looks closer and closer to “trust but verify,” much like a manager would do with their reports.


If you go full throttle on AI and fully go[dark software factory](https://www.danshapiro.com/blog/2026/01/the-five-levels-from-spicy-autocomplete-to-the-software-factory/) autonomous mode, how can you be sure that you’re still producing software responsibly and sustainably?


My cofounder Ganesh recently published a framework to think about this, called[DRIVE](https://www.cortex.io/report/drive-framework?utm_source=builders.cortex.io) , that captures effectiveness of an engineering org, especially as the SDLC is accelerated with AI, that can be a helpful place to start.


## Cortex’s Software Factory


At Cortex, we’re focused on and deeply care about making engineering organizations operate at their highest level.


We also believe that the future of software is software factories, and we want to help our customers get to a level of operational excellence to truly go all in on AI to accelerate their business.


Inspired by companies like[Stripe](https://stripe.dev/blog/minions-stripes-one-shot-end-to-end-coding-agents) and[Ramp](https://builders.ramp.com/post/why-we-built-our-background-agent) , we’ve been developing our own software factory, called` engrams` , and will be publishing a series of devlogs as we build it in the open.


An engram in the brain is a theoretical connective trace in the brain that represents a persistent memory —` engrams` the product, at scale, represents the collective organizational memory where more and more context lives in, through AI-driven work. Also we are *Cortex* , so our internal naming schemes are thick with brain analogies, obviously.


We decided to build our software factory completely from scratch, for a few reasons:


1. Primarily, we wanted to deeply understand every component of software factories, starting from effective cloud dev environments, up to harness engineering and agent coordination, so that we can help our customers build their own
2. We wanted our factory to minimize cost, run fully in our private cloud, and be purpose built for agentic workflows — for example, making AI harnesses a first class part of the underlying dev environment affords both performance optimizations and an overall better product experience
3. It’s nerdy and it’s fun


## Devlogs to Come


The` engrams` devlogs will be a mix of tutorials and technical learnings, each article focusing on a single facet of the software factory in gruesome detail.


Here’s a quick demo of what we’ve built so far, and what we’ll slowly build towards over the next few articles:


0:00


Running Claude Code in a sandboxed cloud environment
