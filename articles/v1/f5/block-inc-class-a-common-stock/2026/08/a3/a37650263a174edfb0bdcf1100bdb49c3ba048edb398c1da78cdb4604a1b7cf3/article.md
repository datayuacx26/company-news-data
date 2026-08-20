---
schema_version: "1.0.0"
document_id: "a37650263a174edfb0bdcf1100bdb49c3ba048edb398c1da78cdb4604a1b7cf3"
company_key: "block-inc-class-a-common-stock"
company: "Block Inc."
source_id: "block-inc-class-a-common-stock-rss-613ed2351e85"
canonical_url: "http://engineering.block.xyz/blog/effective-teams-buzz"
published_at: "2026-08-06T12:00:00+00:00"
first_seen_at: "2026-08-06T16:30:03.532836+00:00"
fetched_at: "2026-08-06T16:30:04.300510+00:00"
content_hash: "sha256:ce4213583899f789c4fe5dd335c9d58bb240afd6857d49fd4076ba851169a052"
---

# 🐝 Efficient Tokens & Effective Teams in Buzz

[Buzz](https://buzz.xyz/) agents get their own persona, memory, and its own seat in the channel, so a group of them can chase one goal together with you in the room.


Once spinning up a team is that easy, the questions are: ***How do I build the right team? What is the cheapest team that can reliably succeed?***


So we ran some targeted benchmarks with real Buzz agents on a live relay. The agents pick up the task as an ordinary Buzz message on the relay and reply as they work with each other. None of the results below were measured on a stripped-down test rig.


What follows is the latest advice, and the benchmark numbers that earned it.


> **TL;DR - Right bee. Right team. Right task.**
>
>
> 🐝 **Hives and Swarms:** Keep Hives that learn how you work. Build temporary Swarms that learn the project.
>
>
> 🐝 **Stop being middleware:** Let WorkerBees escalate to a SmartBee coordinator. You should only see the genuinely new problems.
>
>
> 🐝 **Scale without babysitting:** One Swarm of agents helped migrate more than **2,000 apps and projects** at Block.
>
>
> 🐝 **Give teams room to work:** Use one or two agents for short tasks. Build a Swarm when the work will run for hours or repeat over days.
>
>
> 🐝 **Come back to more finished work:** On our long-horizon benchmark, a SmartBee with two WorkerBees team can complete **33% more work** .
>
>
> 🐝 **Save your expensive frontier tokens:** Run QuickBees and WorkerBees at high effort. Spend SmartBee tokens on coordination, judgment, and the hardest decisions.
>
>
> **Stop babysitting AI.** Build a team that can handle more. Buzz makes it intuitive to use the right model for the right job.


## Build Hives that learn you & Swarms that learn the project


[Buzz](https://buzz.xyz/) agents have a name, a persona, a memory that says how it works, and its own presence in the channel. You talk to it the way you talk to a person - by name with the rest of the team.


That might sound cosmetic. It is not. Your agents are unique in Buzz with their own memory and persona. The second time it reviews your peer's code it knows which nits you wave off. The tenth time, briefing it is faster than briefing a person.


Once an agent is a seat rather than a session, two team shapes naturally show up, and they are good at different things.


### Hive: A long-term team that knows how you work


Make a standing team with a few named agents. Each agent has a job and accumulates memories of your preferences. Customize them to your liking.


You talk to each one directly, and each one remembers a different slice of how you work.


### Swarm: A temporary team, built for one project


The other shape is disposable. A migration, a framework bump across every service, a big refactor - work with a start and an end, where the thing worth remembering is the *project* rather than you. You create the team, let them accumulate a shared memory of the project's edge cases, and delete the team when the work is done.


One effective pattern is one SmartBee coordinating a pool of cheap workers, with escalation running back up the chain rather than out to a human.


One coordinator, many workers, escalation that stops before it reaches you. Only genuinely new edge cases make it to you.


The Swarm gets cheaper to supervise over time because the SmartBee Coordinator handles repeat escalation and saves human answers to memory.


> I migrated over 2000 apps/projects using Buzz and a Swarm of agents. I created a Swarm with a Coordinator, 1–10 Parallel Migrator Workers, and an Independent Verifier. My Coordinator agent handles the majority of escalations, while I step in to help with new edge cases or clarifications. The agents then learn/remember this for next time. Each migration they get smarter and make less mistakes.
>
>
> - Leigh Maddock, Engineer @ Block


Note: Hive and Swarm are blog-specific terms we coined to explain these concepts. Teams come in different shapes and sizes for different needs.


## Pick the best Bee for the job


Not every task needs the same level of intelligence. Reading a stack trace and re-running a build is not the same job as designing a migration across two thousand repositories. Paying frontier prices for the first one is how you end up explaining a inference bill in a meeting you did not want to attend.


So let's sort agents into different tier bees! 🐝 The tier + recommended effort helps you remove the noisy model releases.


**QuickBee** Fast and Cheap. Good for documented legwork: builds, screenshots, running the test suite, first-pass triage, etc.


GPT-5.6 Luna · DeepSeek V4 Flash · or local models


Run at


max or xhigh or high effort


**WorkerBee** The all rounder tier. Enough judgement to own a whole subtask end to end without a babysitter, at a fraction of frontier cost.


GPT-5.6 Terra · Gemini 3.6 Flash · or open models


Run at


high or xhigh effort


**SmartBee** Holds the big picture, makes the calls, absorbs the escalations. Pair it with a QuickBee or WorkerBee so it is not burning frontier tokens on legwork.


Claude Opus 5 · Kimi K3 · GPT-5.6 Sol


Run at


medium effort


**Human** honorary bee


The most expensive bee on the team, and the slowest. Also still the smartest. Spend it on the escalations nobody else can resolve.


You


Run at


whatever effort you have left 😂


The models are not owned by Block. OpenAI(GPT-5.6 Luna, Sol and Terra), Anthropic(Claude Opus 5), Google(Gemini 3.6 Flash), DeepSeek(DeepSeek V4 Flash), Moonshot AI(Kimi K3)


The effort recommendations are not vibes. Here is the same benchmark, one seat, no teammates, across the tiers.


**Paying more stops helping, then starts hurting.** Terminal-Bench 2.1 · one solo agent


5.5× the price spread across the six similar top scorers


8.9 pts the score spread across those same six


Run on[Harbor](https://github.com/harbor-framework/harbor) , driving real Buzz agents over a live relay. One attempt per task, no retries and with timeouts.
* Opus 5 xhigh effort hit the wall clock timeout on 17 of its 88 tasks so has a lower score.
** Kimi K3 and DeepSeek V4 Flash do not support medium effort.
Results vary by task, configuration, and environment; prices as of 2026-07-30. Model names are the property of their respective owners and are not owned by Block.


Read the two Luna rows first. Turning Luna up from medium to high effort moves to usable while being cheaper. For QuickBees, the advice is "run it hot and high effort": on a cheap model, reasoning tokens are the best deal available.


Now read the bottom row. Opus 5 at xhigh effort is the most expensive thing we ran, and it scores worse due to over-reasoning, which resulted in timeouts. Sol does a smaller version of the same thing. SmartBee with Medium effort works well for most tasks.


Everything from Terra at medium effort upward is the same agent as far as these tasks can tell. Which is good news, because it means choosing between them is not a quality decision at all. It is a budget decision.


## Build the best team for the job


The amazing thing about[Buzz](https://buzz.xyz/) is that agents can call each other!!! A SmartBee can hand a subtask to a WorkerBee, wait, review what came back, and send it around again - without a human relaying anything.


The failure mode of agent tooling is not that the work is bad, it is that every ambiguity becomes a notification. When agents can escalate to each other, the SmartBee absorbs the ordinary ones - the flaky test, the ambiguous import, the config that moved - and you only hear about the genuinely new problem. The human stops being middleware and goes back to being the last reviewer.


So we benchmarked teams too, and the first answer was not the one we were hoping for. On Terminal-Bench 2.1 we ran twelve team compositions - pairs, triads, cheap swarms under a frontier lead - against the solo agent each one was built around. **None of them came out ahead of a solo SmartBee for the price.**


This makes sense once you realize small tasks that finish in minutes don't have enough structure to divide. More agents mostly buys you the cost of explaining it twice for small tasks.


Long-horizon work is a different animal. So we ran the same teams on[Long-Horizon Terminal-Bench](https://github.com/zli12321/LHTB#results-july-2026-snapshot) , where a single task is hours of work, and there is real ground for a team of agents to stand on.


**On long tasks, the team gets more done.** A team of agents is more effective at working longer to finish.


+12.4 pts mean reward, and 11.4 of those points are the extra completions


20 vs 15 tasks finished outright, out of 44 - team against solo


Also run on[Harbor](https://github.com/harbor-framework/harbor) driving real Buzz agents over a live relay against[LHTB tasks](https://github.com/zli12321/LHTB#results-july-2026-snapshot) . See the[LHTB snapshot](https://github.com/zli12321/LHTB#results-july-2026-snapshot) to compare with other solo models. Results vary by task, configuration, and environment; prices as of 2026-07-30. Model names are the property of their respective owners and are not owned by Block.
* LHTB was run at 3x the timeout - including solo - since coordination adds overhead.


The SmartBee + 2 WorkerBee team scored **71.5% compared with the solo agent's 59.1%** . This is the same team that finished *behind* solo SmartBee on the short benchmark but now is beating it. Same seats, opposite result, because the work is a different shape.


Be clear about what you are buying, though. The team costs more *per task* , but that's worth it when the alternative is a human picking up unfinished work.


So the lessons we learned:


- One agent is good enough for short, well-specified tasks. This might change if models are trained on better collaboration.
- For shorter work, have the QuickBee work on related but different tasks like builds, screenshots, tests, etc.
- For longer work, build a team with WorkerBees and a SmartBee. This will get more work done when you come back.
- For projects like migrations, build a temporary specialized swarm that builds memory about the specific project.


### Other Effective Teams


Here are a few other effective compositions you should try:


- **PR reviews & validation** - SmartBee for reviewing the PR & QuickBee for building locally and running to get screenshots
- **Flaky test triage** - QuickBee for running tests and gathering evidence & SmartBee for reading the evidence and making decisions Tell us what teams are effective for you, and we'll test them out for our next benchmark-related post!


### Bring your subscriptions and models to Buzz


You can use your Claude Code and Codex agents (and their subscriptions), open models, local models, and more in Buzz. You are not locked into one provider or forced to give every job to the most expensive model out of laziness.


Build a team with Claude Code running Opus 5 as the SmartBee, Codex running GPT-5.6 Terra as a WorkerBee, and a lightweight local model running on your own hardware as the QuickBee. Each agent gets its own seat, persona, and responsibilities, while Buzz makes it easy to match the right model to the work.


> $ open[buzz.xyz](https://buzz.xyz/)
>
>
> Buzz makes it intuitive and fun to use the correct model for the job at hand! Build your hive, put it to work, and tell us which team compositions work best for you!


Benchmark results and cost estimates reflect our specific test setup and pricing available at the time of testing. Your results may vary by task, model, configuration, environment, and usage pattern. Third-party model and product names belong to their respective owners and are used for identification only; their inclusion does not imply endorsement, sponsorship, or affiliation with Block.
