---
schema_version: "1.0.0"
document_id: "b56cec4092cb1ab78eab722ebc81e3cc889ece7bf9568a5afd32bca4085ae272"
company_key: "yc-nuanced"
company: "Nuanced"
source_id: "yc-nuanced-rss-6dc59fce0a56"
canonical_url: "https://nuanced.dev/blog/the-AI-coding-app-for-thinking"
published_at: "2026-06-24T00:00:00+00:00"
first_seen_at: "2026-07-20T23:24:05.315691+00:00"
fetched_at: "2026-07-28T21:10:03.278263+00:00"
content_hash: "sha256:4d1302f3caf65ca3485f483c10ccd4d333ce7517cff2888d1b12e70b4e5bf373"
---

# Agents can write code, but engineers still need to think

Today we’re launching Nuanced, a desktop app for building software.


AI has made it easy to generate code. But the hardest part of software engineering has only gotten harder: holding the system in your head, reasoning about how changes fit together, and communicating that clearly. We haven’t found the right abstraction for working with agents at the speed they now make possible.


Right now, you can start with a rough idea, send it to an agent, and get back a lot very quickly. For a bigger change, you might have a plan, maybe a few markdown files, tool outputs, partial implementations, changed files, and a long runaway chat transcript. The speed can feel magical, but the process can also get messy fast.


This is because once the work starts spreading across all of those places, the intent becomes harder to hold onto. If something feels off, or the agent introduces behavior you didn’t mean to ask for, it can be hard to trace where things drifted. This is especially true if you haven’t defined what to build upfront.


You end up wondering: was it the prompt? The plan? An assumption the agent made? A detail you forgot to clarify? Untangling that takes time. You don’t need to understand every detail of what the agent did, but you *do* need to know which details matter. Otherwise, fast code generation can create the illusion of productivity where a lot is happening, but you’re inheriting an expensive investigation instead of making real progress.


It’s easier than ever to explore multiple ideas at once. The harder part is staying oriented, knowing which threads are worth pursuing, which decisions matter, and when the work has started to drift away from your goals.


I kept feeling this friction, and that’s why we built Nuanced.


## A spec-driven workspace for agentic work


Nuanced is a desktop app for building software with agents around a living spec.


Instead of starting with a prompt and reacting to whatever code comes back, Nuanced starts by helping you think through the work. The agent helps you define a spec outlining what should happen, what should not happen, what constraints matter, and where the tradeoffs are.


That spec stays at the center of the workflow. It is not a temporary plan buried in a chat transcript that gets lost in the backscroll. It is the source of truth the agent implements against, and the thing you review against as the code evolves.


This means less time reverse-engineering why something was built a certain way, and more time steering the work at the level where building really happens: architecture, product behavior, assumptions, and scope.


The goal is to let agents handle more execution without losing the deliberate reasoning that good software still requires.


## Why writing matters


My conviction in specs, and in writing things down, comes directly from my own experience. During my seven years at GitHub, the real work often started before any code was written. We would write documents to align on what we were building.


Writing forced me to get very precise and make tacit assumptions explicit. I often only became fully aware of problems once I tried to explain them clearly.


That discipline mattered because at GitHub’s scale, even a small change could ripple through the product in unexpected ways. You had to understand the shape of the system before touching it. You couldn’t just start coding and hope the design would emerge.


Writing things down also made it possible for teammates to challenge ideas or surface blind spots. This helped teams converge on a plan before misunderstandings hardened into implementation and became risky or harder to fix.


This process worked well, but it was slow. Feedback cycles took days. Today, agents can compress that loop dramatically. You can pressure-test decisions in minutes, explore multiple directions with throwaway prototypes, and iterate on the spec while execution happens alongside it.


## Why we need a new interface


The interface for software development has always reflected the bottleneck.


When writing code was slow, the editor became the center of the workflow. When collaboration became the bottleneck, pull requests, issues, and CI became the coordination layer.


Now agents have made execution dramatically faster, but the interface has not caught up.


Agents are getting better at doing the work. They can operate across larger codebases, pull in more context, and run more tasks in parallel. But running more agents is not the hard part. The hard part is knowing what to ask them to build.


As code generation gets cheaper, ambiguity gets more expensive. If you leave gaps, agents will fill them in. Those assumptions do not stay abstract for long. They turn into architecture, product behavior, and surface area you now have to understand or unwind.


Today’s tools show you what changed, but not always why. The reasoning gets scattered across prompts, plans, logs, diffs, and chat history.


What we need is not just more automation. We need an interface that gives the human mind more leverage and keep up with decisions as agents move faster.


## How Nuanced works


Nuanced follows a simple workflow: start with intent, clarify it into a spec, then execute in a way that stays aligned with that intent.


### Start with an idea, not a spec


You open Nuanced, sign in with GitHub. Once you connect ChatGPT, you can add a workspace using` cmd


+


O


` .


You can also open the command palette with` cmd


+


K


` to see all available actions.


You can then start a spec, either by typing directly into the input or hitting` cmd


+


N


` .


You begin to author a spec with a short description of what you want to build or change. This doesn’t have to be a fully formed plan, a rough idea is fine. At this stage, the input is treated as raw intent, not a prompt to immediately generate code. Instead of fanning out into execution, Nuanced opens a clarification space.


### Progressive disambiguation before execution


Before agents start building, Nuanced clarifies intent using progressive disambiguation: an iterative process for defining what you mean before execution begins.


You are not asked to define everything upfront. The system helps you make decisions around goals, constraints, invariants, and what is out of scope, one layer at a time. The spec takes shape as you go, becoming more precise without breaking flow.


### Generate tasks from the spec


Once the spec is clear enough, you can execute tasks derived from it. The agent begins working and shows the tasks in progress.


### Review and commit


Look at files generated from the spec and commit from within Nuanced without having to switch to your terminal.


## Try Nuanced


Nuanced is available for macOS today and currently supports Codex as the execution agent.


We've been building Nuanced with Nuanced and find it significantly speeds up our workflow.


If you’re building with AI and want a clearer way to turn intent into code, we’d love for you to try it. Email me atayman@nuanced.dev with what works, what breaks, and what you want to see next!
