---
schema_version: "1.0.0"
document_id: "85ff4a938373fa1ab76e2044c69b5114d709d39e33716e07239b73a55059c7d6"
company_key: "yc-retool"
company: "Retool"
source_id: "yc-retool-news-import-762698831de2"
canonical_url: "https://retool.com/blog/engineering-mindset-ai-onboarding-strategies"
published_at: "2026-03-25T00:00:00+00:00"
first_seen_at: "2026-07-22T11:44:05.217948+00:00"
fetched_at: "2026-07-28T21:26:23.229623+00:00"
content_hash: "sha256:c256ccccc3d49040443f3b8403a1714d4ebb05f98f8dddf2f5643590a1140e9a"
---

# You don’t have to be an engineer to think like one

01 1. Treat AI like you’re onboarding someone every 10 seconds02 2. Your prompts and docs execute now. Maintain them like it.03 3. “I’ll know it when I see it” is not a spec04 How organizations can function more like engineering teams05 Engineering thinking is the skill nobody’s talking about


If you’ve started building internal tools with AI and felt like the output was almost right but not quite right, congrats. You’re a vibe coder! And you’re not doing it wrong. You’re running into the same problem engineers have been solving for years.


I lead the internal AI engineering group, and a lot of my job right now is helping teams figure out why their[AI workflows](https://docs.retool.com/workflows/guides/generate-with-ai) aren’t working the way they expected. The answer is almost never the model. It's rarely the prompt. Most of the time it comes down to something more fundamental: the people using AI haven't had to define what good work looks like in their job before—not precisely, not in writing—and AI has no way to fill that gap for them. Prompting is the *last* mile. The first 99 miles—context quality, process clarity, knowing what done looks like before you start—matter more.


Attempts to replace human workflows with AI have made it hard to ignore the fact that most people don’t have a precise definition of what good work looks like in their job. That’s worked fine, until now. AI has no access to any of it. Every interaction starts from (or close to) zero, without memory, institutional context, or any lived experience of good output. It has exactly what you give it, and it fills in everything else with an overconfident guess.


Engineers figured this out early, mostly because the cost of getting it wrong showed up in broken code. But the habits we developed go beyond our technical expertise. They’re about how we prepare work, define success, and build systems that hold up over time. Here are three of them that transfer directly to non-engineers, no engineering background required.


## 1. Treat AI like you’re onboarding someone every 10 seconds


Engineers treat every system as something that will eventually be handed off to someone with no prior context, because for us, it will be. Most job functions aren’t documenting their processes in this way. Most engineers have been burned by codebases where critical context lived only in someone’s head, because codebases outlive the people who wrote them.


Engineers working with AI coding agents are onboarding new engineers every 10 seconds instead of every few months. The things that patch a normal onboarding process—a buddy, pairing with a senior person, hallway conversations—don’t scale to that cadence. You need context that works for someone with no prior knowledge, prepared well enough that you don’t need to be in the room.


Most teams outside Eng haven’t built that. They paste in some context, ask the question, move on. That works at low volume. It doesn’t compound.


**Onboarding AI for better software**


Before you prompt, ask “if I were handing this task to an ops manager on my team with zero context about my process, what would they need to know?” That answer is your prompt, and something you should build into a system that makes it easy to include the essential information into future prompts through context engineering and Skill files. Document the process the way you would when onboarding a new employee.


When it comes to building software, be specific about more than just the feature the way you would with a new teammate. What does the app need to connect to? Who's using it and what permissions should they have? What does the edge case look like? The difference between a prompt that produces something useful and one that produces something that can’t go near production is usually the setup, not the ask.


## 2. Your prompts and docs execute now. Maintain them like it.


When the output of an AI workflow is a draft document or a summary, a bad template is an inconvenience. When the output is live software running against production data, it’s a liability. The prompt that generated the app, the requirements doc it was based on, the permissions it was supposed to respect—all of that is now part of the system. If it’s stale or underspecified, you find out the hard way.


Engineers already know this dynamic because we live with it in code. A bug doesn’t sit quietly in a codebase. It executes, causes failures downstream, and compounds. Our response is version control, code review, testing, and retiring things that have gone stale (a payment plan for tech debt).


[Related Article A guide to prompting in Retool](https://retool.com/blog/prompting-best-practices)


Your prompts


, SOPs, and reference docs are code now, too. Every time someone uses that template, it executes. Every time an agent pulls that context file, it executes. Most teams outside of engineering aren’t thinking about their work in terms of execution. They’re running AI on top of context infrastructure built for a world where only humans were reading it. The quality bar is usually “good enough for a human to skim,” when it should be “correct enough to run in production.”


**Generate docs like an engineer generates code**


Version your prompt templates the way an engineer would version their code. We call this creating a walled garden—make it easy for the right people to contribute to your docs and hard for the wrong people to. Review all updates for relevancy and accuracy. Retire them when they’re stale.


Centralize your knowledge in order to prevent drift and compounding maintenance burdens. It’s easier to fix one line in one process doc than it is to search for that same line in five different places.


If a context doc is wrong, treat it like the bug that it is: something actively producing bad outputs at scale. We run incident postmortems and failure analysis to make sure mistakes only happen once. Have a plan for when a bad line of prompting or context creates something bad. Do you update the docs? Write a test or update a checklist of what existed? Factor that in when you’re fixing bugs in your documentation.


## 3. “I’ll know it when I see it” is not a spec


Software has always forced a version of this reckoning. You can’t ship vague. At some point, someone has to decide what the app does,[what it connects to](https://retool.com/resources/videos/how-to-call-resources-in-retool-assist-tab-tutorial) , who can access it, and what happens in every edge case. We’re used to being responsible for the last mile of every feature before it gets out the door. Non-engineers should approach vibe coding this way, too.


Engineers have a structural advantage when working with AI: verification infrastructure. Code compiles or it doesn’t. Tests pass or they don’t. We get an early signal when something breaks, or when the app does something unexpected with data it shouldn’t have touched. Most knowledge work has no equivalent, and AI makes that absence expensive in a way it never was before. Your manager might be the only person that can tell you if your content brief was up to snuff, if you’re lucky.


“Build me an app to track customer requests” is not a spec. Who can see that app? What does it connect to? What happens when a request is closed? Your AI will make decisions about all of that confidently and silently. Whether those decisions match what your team actually needs is entirely up to how well you defined it upfront.


Consider[test driven development (TDD)](https://www.ibm.com/think/topics/test-driven-development) —a popular approach with engineers where you write the tests before you write the code. Treat your processes similarly and ask questions like “What does passing look like?” “What are the edge cases that matter?” “What’s the failure mode we’re specifically trying to avoid?”


**Defining criteria in practice**


Before you prompt, write two or three concrete criteria for what “done” looks like. If you’re building, ask these questions:


- Who has access to what I’m building?
- What systems does it touch?
- Where are the “gotchas”?
- What’s the one thing it must never do?


For software especially, the AI will make decisions about everything you don’t specify, like data permissions, edge case behavior, what happens when something fails. Those decisions won’t always be flagged. They’ll just be in the code. The criteria you write upfront are the only instructions it has.


## How organizations can function more like engineering teams


Individual habits matter, but the organizational version matters more.


When all code used to be written by hand, change was expensive, so we built checks and balances into the system to catch as many bad decisions as possible as scalably as possible. But then, one bad line of code was one bad line of code. Now, because agents move so fast and so much more autonomously, one bad prompt or piece of stale context can result in 100+ lines of bad code *per hour.*


More builders operating without those systems means more things reaching production that probably shouldn’t—or that no one knows how to maintain. At Amazon, senior engineers[now have to sign off on every code change](https://www.businessinsider.com/amazon-tightens-code-controls-after-outages-including-one-ai-2026-3) from junior and mid-level engineers after a series of outages. That’s not an argument against more builders, but an argument for being honest about what breaks when the old guardrails disappear.


When we decided to get serious about AI adoption in our engineering org, the very first thing we looked at was our documentation and test coverage. Not because it’s a best practice that someone put in a style guide, but because we know[agents are only as useful as the context they can access](https://retool.com/agents) and only as effective as the CI/CD systems underpinning their work. A huge amount of knowledge about how our systems behave lives in people’s heads—the “we do it this way because of that one customer edge case” context that currently exists in Slack or Teams threads and institutional memory. That’s fine when humans are the only ones reading it. When agents are making decisions based on it, every gap is a mistake waiting to happen. So the work, increasingly, is making the implicit explicit: writing down *why* the code (or doc or process) is the way it is, not just what it does.


## Engineering thinking is the skill nobody’s talking about


[Related Article Software developers still matter in the age of AI. Here’s why.](https://retool.com/blog/why-software-developers-matter-in-age-of-ai)


Everyone is talking about the death of the software engineering job, but everyone today would benefit from trying to be *more* like engineers .


We have a head start with AI because we’ve spent years being forced to make implicit things explicit: writing down why a system works the way it does, not just what it does, because someone else will have to maintain it later. The cost of *not* having these skills showed up earlier in code than anywhere else. Now that everything is code, it shows up everywhere.


The teams pulling ahead on AI won’t necessarily have the most tools or the biggest budgets. They’ll have the cleanest context, the most explicit processes, and the least amount of knowledge that only lives in someone’s head. You don’t need to write code to build that. You just need to start treating your prompts, your docs, and your workflows the way engineers treat their codebases—as systems that run, and that someone else will have to pick up after you.


If you want to start building yourself (and know you’re doing it safely), check out[our library of walkthroughs and tutorials](https://retool.com/resources) to get started. And if you’re a builder or engineer who wants to work on this problem directly,[we’re hiring.](https://retool.com/careers)


light Reader
