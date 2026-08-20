---
schema_version: "1.0.0"
document_id: "ae992f354d5bd56e5e266a575b8d6355900d65048db726b72a7e2dd43f65e011"
company_key: "yc-minded"
company: "Minded"
source_id: "yc-minded-news-import-42e7450e1631"
canonical_url: "https://www.minded.com/blog/adlc-loop"
published_at: "2025-12-07T12:00:00+00:00"
first_seen_at: "2026-07-22T04:25:07.261888+00:00"
fetched_at: "2026-07-28T21:58:33.663804+00:00"
content_hash: "sha256:bac39ca339966dba4a2de256cef3ae8d0e0779b86145ca1f16be57f6d82a7825"
---

# The Agent Development Lifecycle Loop

Software ate the world. Now agents are eating the rest, they interact with humans, they can talk, they read images, they make sense of messy files, but they are surprisingly hard to build.


For traditional software, we have the SDLC. It gives us a repeatable playbook: design, code, test, deploy. Inputs are clean, outputs are predictable, and bugs are reproducible.


Agents don't live in that world. They operate in language, not logic trees. They reason instead of following rules. So we needed a new kind of lifecycle.


At Minded, we built the Agent Development Lifecycle, a loop that deals with ambiguity and turns it into a process that gets you faster to production.


## The ADLC Loop


Agents are meant to bridge the gap between fancy ChatGPT demos and the messy reality where APIs return strange values and users ask things you didn't expect. Actually, in our work with public companies like eToro and Amex, we've found that most of the work happens in the "last mile" of development, where unexpected edge cases pile up.


ADLC begins with the end in mind, starting with an end-to-end annotated examples that the agent is meant to solve including API responses and ideal tone of voice. You can think of it as Eval-Driven Development, the agentic equivalent to TDD (Test Driven Development).


The ADLC kicks off with Design where Agent Product Manager defines what the agent should do, how and where it should respond, and what a successful interaction looks like.


Then the development loop begins:


- Evaluate: Look at real chats and follow what happened step by step. Watch how humans click through screens, handle odd user questions, and recover when an API breaks. These full flows show what the agent needs to do from start to finish.
- Build: Use the eval as guidelines to building. This might mean updating the prompt, adding new logic, or improving the data the agent sees.
- Release: run your tests and make sure the agent still handles what it's supposed to. Then push it into the wild and see how it does.


And you keep looping until it works well enough to trust.


Agents live at the intersection of traditional software and machine learning. You build them like software with prompts, logic, and integration, but you improve them like ML models, using labeled interactions to refine behavior.


To see the ADLC in action, take JustEat. They needed agents that could resolve missing order issues reliably. During the Design phase, the team mapped out how the agent should chat with customers to understand the issue, and then call the restaurant to confirm if an item was actually missing. They worked from real conversations and had to connect past call logs to current chats to make sense of what actually happened. For example, in one case, a restaurant was out of stock on an item. The agent confirmed this directly during the call, informed the customer with a clear and polite message, and logged a credit request with no human help needed. It handled the whole thing end to end. These kinds of cases helped define what agents could fully automate, and when they were ready to run solo.


## Release Agents as Code


Software used to be messy to deploy. Then teams started writing everything down from servers, databases to configs and storing it all in version control. Now if something breaks, you can roll it back. If you need more capacity, you just scale it out. It's called infrastructure as code, and it changed how software gets shipped.


Agents need the same thing. They change often with new prompts, new custom code, guardrails that keep chaging, new integrations. If you can't track exactly what changed, you're flying blind.


At Minded we invented a new concept call Agent as Code where every part of an agent including prompts, tools, orchestration, is code. It all lives in a Git repo. Every release is versioned and frozen, so you can test it, roll it back, or run it side-by-side with another version. Same principles, new kind of software.


## Continuous Human-in-the-Loop QA


Most agent mistakes are actually driven by unknown unknowns, because users said something unexpected or a 3rd party system responded with never seen before datapoint. Minded employs Agent Experts to audit daily interactions, annotating mistakes and these feed directly into improving the agent.


These annotated conversations can be saved and turned into future test cases. That way, once the agent learns to handle a situation, it won't forget how.


Minded's Quality Hub makes it easy to find the most important cases to review and helps Agent Developers replay tricky moments while improving the agent.


## Build The Lean Agent, One Conversation at a Time


Startups once wrote long business plans before talking to a single customer. Agents used to follow the same path—long design docs, written before a single conversation had run. The Lean Startup taught us to build, measure, and learn from real users. Agents need that same rhythm. Not just polished docs, but real-world loops that ground theory in truth.


At Minded, we are trying to build that loop: from evaluation to release, always learning. Try it. Break it. Tell us what worked for you.
