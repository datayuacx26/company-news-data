---
schema_version: "1.0.0"
document_id: "e2e00f40a07d7fa47f49edabc95d863e9b2ca53cd1c780634694ae6a3b3fd68c"
company_key: "yc-convictional"
company: "Convictional"
source_id: "yc-convictional-atom-2bd8494c6cc9"
canonical_url: "https://get.convictional.com/posts/benchmarking-collaborative-agent-alignment/"
published_at: "2026-07-13T00:00:00+00:00"
first_seen_at: "2026-08-09T21:02:07.552290+00:00"
fetched_at: "2026-08-09T21:02:10.293363+00:00"
content_hash: "sha256:0fbf3906e48e2b09c765b9b38b090bac840d70a89b296e5091500244fddbd50e"
---

# Benchmarking Collaborative Agent Alignment

#


## TL;DR


- How teams of humans collaborate is changing due to adoption of AI.
- What's more, individual outcome gets amplified by AI. In particular, if they're aligned to what you want, you'll get more of it, but the opposite is also true.
- We want to measure this: how does the collaboration tool impact agents' ability to align? Does the choice of tool impact how aligned the agent's recommendations will be to your team and organization's goals?
- Early observations indicate that teams of agents struggle with alignment as compared to a single agent with a holistic view/control — something in the agent's performance is lost due to collaboration overhead.
- While AI speeds up teams, it may not improve or even worsen alignment issues stemming from collaboration and coordination failures. Teams stepping on toes, missing approvals or working outside of values will become more frequent if not addressed.


## Background


The introduction of AI is changing knowledge work for humans to be less task-level execution, and more execution of judgement and taste. Capable LLMs, increasingly trained on longer horizon tasks, are optimized to operate in 'single player mode', executing a task that a single user oversees.


Many of these users are operating within a team of other humans, aligning around a single mission, and as teams rely more on agents for task execution — including strategic and tactical advice — the ways in which they collaborate change, with[studies](https://economics.mit.edu/sites/default/files/2026-02/AI%2C%20Human%20Cognition%20and%20Knowledge%20Collapse%2002-20-26.pdf) pointing to decreasing shared knowledge. What's more, agents like[Claude Tag](https://www.anthropic.com/news/introducing-claude-tag) are entering the multi-player arena and may collaborate directly with your team while influencing how they collaborate with their colleagues.


With agents now more broadly entering multi-player team environments, along with the already broad adoption of individual agents across team-members, we decided to explore and develop a benchmark for how agents collaborate in long-horizon environments involving tactical and strategic decision making. In particular, we are interested in agents collaborating where there are differences in their tasks and information, across typical human collaboration models such as chat, channels or more structured tools.


## Motivation


Tools such as email, documents and chat were designed with humans in mind who have ever-growing context as well as experience. These tools focused on breaking down communication barriers, while doing little to codify or structure the content. Emails use subject and senders, chat uses members and channel title/description, and documents are as well structured as the team maintaining them.


An agent assisting a team-member does so as an amnesiac. *Everything* they need to know about the task at hand has to be given to them as part of their prompt. System prompts stuffed with memories, tools, additional retrieved context, and more reasoning tokens do a good job of solving this today at the expense of[variability](https://get.convictional.com/posts/butterfly-effect-applied-ai/) and resources.


Our hypothesis is that traditional flat and unstructured group-oriented tools such as chat are ill-structured for multi-player human and agent collaboration. Specifically, due to fixed search and context budgets, these tools don't afford efficient or complete understanding of organizational decisions, outcomes and players. In multi-player collaboration, many problems can span or implicate multiple of these, and the LLM's reluctance-to-refuse means they can happily plow away on tasks without stopping to check in, question assumptions or seek out needed information or approvals.


> Active research is taking place in the area of LLM assumptions and lack of refusals when not enough information is present.[Studies such as this](https://arxiv.org/html/2502.13069v3) show that LLMs in no-human-in-the-loop modes struggle to recognize an under-specified problem.


As users automate more of their agents, or trust them with larger tasks, under-specification of problems becomes compounded by unstructured tools. What's more, this behaviour in under-specified problems can manifest in collaborative environments as failures to coordinate or collaborate with other team-members. To overcome this, humans have to fill this gap by both being aware of needed collaboration, and being able to intervene in their agent's tasks or design/adopt systems to do so.


Organizations feeling pain in collaboration will see it increase as teams hand more task execution to agents who simultaneously speed up the human's output while not being able to help the humans close those collaboration gaps.


> Some of our more technical readers may be protesting along the lines of, "this is already being solved in coding — teams of people use individual agents and have found real productivity gains", to which I would agree. However, you might be hard-pressed to find a more-structured corpus of information than a well-structured production code base. This problem remains largely unsolved for meaningful work in most domains other than technical STEM applications.


At Convictional, our mission is to keep teams mission-aligned by design, so having a way to measure how this manifests is important in achieving that mission. We purposefully design Convictional's collaboration surfaces in a way to encourage focused collaboration amongst humans, and a benchmark helps us understand how that design, along with our own communication norms, impacts alignment when agents are involved.


## GoalBench


Specifically, our benchmark looks to capture a scenario in which multiple agents, each with partial shared information, are forced to collaborate through fixed communication channels/models, and the impact on their ability to effectively align towards shared and individual goals. In short, we look to capture what would be observed in a real-world team of knowledge workers: each individual has shared and private information, each individual has their own individual goals as well as responsibility towards shared team or organizational goals, and each individual collaborates through some human collaboration tool.


In order to build a benchmark which can isolate the collaboration layer, we chose to build a seeded stochastic turn-based software startup "game" that teams of agents and/or humans could play, including single-player modes. We chose the startup scenario given our familiarity, but future work to extend to other scenarios would be an interesting follow-up.


**Game Structure**


The game is structured around five functions in a software startup: *Engineering* , *Sales* , *Marketing* , *Customer Support* (CS), and *Operations* . A baseline single-player mode in which a single agent or human controls all five functions is also included — otherwise, a single agent (or human) controls a single function. Going forward, I'll assume the more interesting multi-player scenarios unless otherwise specified.


**Game Scenario**


The scenario that the players are dropped into is a ' *well-funded seed-stage startup* ' that has only Sales, Engineering and Marketing functions to begin with. The players share a starting budget that is burned down each turn through capacity cost, maintenance and marketing spend. At game start, the team has only one MVP feature and no customers. The team starts with no CS or Ops functions, so must hire if they want to add them.


**Capacity Constraints**


Each function has limited capacity and has to decide how to spend it each turn on unique actions available to their function (e.g. Eng can work on a feature in their feature tree, Marketing and Sales can collaborate on an Event). Agents can also submit 'hire' actions allowing them to hire for their own function (increasing their own capacity), or for another function. Agents have to coordinate their actions in order to successfully find, close and retain customers.


**Goals**


The goals that the players share are` total MRR` ,` churn rate` , and` runway remaining` . Three goals were chosen to force players to make decisions potentially compromising one for another. The game is scored on normalized progress towards each of these goals.


In addition to shared goals, each function has their own individual goal as well, which aligns with one or more of the shared goals (e.g. Sales targets` pipeline velocity` , CS →` customer health` , Eng →` feature maturity` , Marketing →` awareness` , and Ops →` project completions` ). These are introduced to reflect real organizations in which tension may exist between individual goals and one or more shared organizational goals.


**Determinism**


Some actions are deterministic in outcome, but many are stochastic, determined by seeded random number generation allowing for controlled repetitions to be run across conditions. The probabilities and rates used were drawn from our own internal experience and data to keep mechanics and decisions as rooted in reality as possible given our design constraints around determinism.


### The Collaboration Layers


Players are only able to communicate with each other via the provided collaboration layer — for agents, these are presented as skills over a CLI. We build three distinct collaboration layers/CLIs for teams, plus a single-player baseline for comparison, and treat these as the conditions being tested:


1. **Single Player Baseline** : A single player controls all five functions, so has an 'oracle' view of the game, whereas the multi-player conditions below have to choose to share their private information via the collaboration layer.
2. **Single Chat Room** : Players can only communicate via a single chat room, similar to teams relying on a Signal or Whatsapp group. The CLI only contains commands to read message history or write a new message. Of the multi-player conditions, this is considered the 'baseline'.
3. **Multiple Channels** : Each function starts with their own chat along with an 'everyone' channel. Each chat is public, so all agents can read and message in any. Agents can also create new channels at will. This is our flat, largely unstructured model and mimics Slack with the exception of private channels and threaded replies. With a team of only five players, we felt this was adequate to capture multi-channel dynamics. The initial channels were chosen to be generic, but players are free to create more specific channels.
4. **Convictional** : Players have access to an 'everyone' chat along with the ability to add updates to goals (any of the starting three shared + five individual), add new goals, create posts, comment and log decisions on those. While Convictional offers humans much more, for a team of five players this balanced the core feature thesis (goals, topic-centric content, decision logging) with the team's size.


## Early Observations


Full benchmarks, along with a preprint and code, will be coming. However, we thought it would be helpful to share some of our early observations and learnings from the development process.


### Single vs Multi-Agents


During development and testing, we have seen evidence of our single-player 'oracle' outperforming teams of agents. A gap due to how, and what, agents choose to collaborate and share seems to exist.


If this observation holds, then teams will rely on humans to both be aware of the needed collaboration and be able to intervene in the agent's task. Teams already suffering from alignment pain will feel more. Agents will simultaneously speed up human tasks while failing to prevent the increase in the amount of coordination failures amongst teams.


This is reinforced by other studies ([HiddenBench](https://arxiv.org/html/2505.11556) ,[SiloBench](https://arxiv.org/pdf/2603.01045) ) on single-agent vs. multi-agent performance on various sets of tasks. Our work looks to expand on findings such as these by testing the impact of shared goals across collaboration layers modeled on real-world tools and decision making.


Across our development, we have seen similar behaviour. While the game has gone through tuning and increases in complexity, we have seen the single-player baseline outperforming a naive team of agents working in a single chat room. While the single-chat-room condition is our baseline multi-player scenario, it does represent the agent's most natural collaboration tendencies. Testing is still ongoing for other conditions, but we would expect the gap to close.


Below, you can see these trends across the three shared game goals: runway, MRR and churn, where each point represents single versus multi-player outcomes for a single configuration. Points are coloured by the state of the game code at the time, with the last *commit* id on the codebase used as the identifier. When broken down across models (see the table below) we observe similar behaviour although to varying degree by model. Points below the dashed line represent a commit where the single-player runs outperformed their multi-player counterpart.


We see similar, but less pronounced, behaviour in MRR across development:


The same pattern holds for churn, represented as its inverse, *retention* , to align chart axis directions with the above:


**By Model**


Model n Final MRR (W/T/L) Final runway (W/T/L) Retention (W/T/L)


Opus 4.6 3 1/1/1 2/0/1 2/0/1


Opus 4.8 10 5/3/2 7/3/0 6/1/3


Sonnet 4.6 21 13/1/7 8/8/5 11/4/6


DeepSeek v4 1 0/1/0 1/0/0 0/1/0


All models 35 19/6/10 18/11/6 19/6/10


- Tie bands: MRR ±$1,000/mo, runway ±2 turns, retention ±0.02
- **W** = single-player outperforms · **T** = within tolerance · **L** = single-player loses


### Benchmarks as Games are Hard


In working on this benchmark, there have been some more meta-observations related to designing, testing and running robust benchmarks. A core tension has existed throughout development between speed, cost and intelligence. The multi-player conditions use up to five parallel agent sessions, with each agent able to use any of the tools available to them. Testing with frontier models introduces real cost, with average runs costing $25–$50 USD on Sonnet and Opus (same 'high' effort). Open-source models such as Z.ai's GLM5.2 cut this cost by about a third to a half, and smaller, cheaper models such as DeepSeek's v4 Flash make runs in the single-digit-dollars range, but struggle to successfully complete games, leading to limitations in what mechanics can be tested.


Compounding this is the variability in LLMs. Games are run according to a seed, and even on the same seed and model, two game sessions can have large differences in their outcomes. Putting this together, in developing a robust benchmark you are forced to try and account for this by running more seeds and repetitions within seeds, which comes with more time and cost.


So, a word of caution to all readers of benchmarks: your mileage may vary, and this applies to the real world as well. The same varying and costly behaviour of agents will emerge in how they execute tasks. The same repetitive task, with the same or similar input, could lead to vastly different outcomes when there is no human in the loop to intervene.


## Conclusion


GoalBench represents our attempt at understanding how teams of agents and humans can best collaborate in order to align on their goals.


We believe that while AI is an incredible tool, capable of massive productivity gains, it is fundamentally an output scaling function. If that output is misaligned, or built through misaligned biases, then these only become amplified through AI adoption.


Convictional is built for humans first, but acknowledging how work is changing — and the impact of agents — also shapes our design towards ensuring that the output being amplified is going in the right direction.


## Appendix


### Related Work


Benchmarks for LLMs and agents are becoming increasingly common as teams look to quantify the 'spikiness' of these models' capability. Two areas which are related to this work are the area of multi-player collaboration of agents, as well as benchmark design as a simulation/game. What's more, the choice of a 'software startup' as the scenario for said simulation is also related to other long-horizon single-player benchmarks.


Our work looks to add to the quickly growing corpus of multi-player benchmarking by focusing on the question of alignment to shared goals by a team of agents collaborating through tools in use by humans today.


**Multi-Player or Collaborative Agents**


In preparing this work, we researched the state of simulation-based benchmarks and found most focused on single-agent task execution over long horizons. Examples of this are[VendingBench](https://andonlabs.com/evals/vending-bench-2) (now version 2), or[METR's work on long-horizon benchmarking](https://metr.org/blog/2025-03-19-measuring-ai-ability-to-complete-long-tasks/) . Each of these focuses on a single agent executing tasks, and doesn't consider the collaborative nature that they are now being extended to.


The multi-player environment lacked a more realistic and grounded simulation of tactical and strategic trade-offs. More multi-player benchmarking and research is now emerging. For example,[SiloBench](https://arxiv.org/pdf/2603.01045) (April 2026) looks at a similar environment of multiple agents, each working with partial information, forcing them to collaborate to solve a problem. The problems they are tasked with, however, are numerical in nature — each agent has a subset of an array of numbers, and their shared tasks relate to summing, sorting, global maximums, etc.


[CollabBench](https://arxiv.org/pdf/2606.05793) (June 2026) proposes measurement of a similar problem to the one that we do, particularly that agents struggle with multi-player human collaboration. Their paper introduces benchmarking and game-based environments for the training of agents in multi-player human collaboration. This work is the most closely related to our own.


**Games as Benchmarks**


Games have been used to study the collaborative and competitive nature of group behaviour as they give fine-grained control over mechanics and conditions being tested, while allowing players to strategize between turn gates. An early example that served as partial inspiration for this work is the "[Beer Game](https://en.wikipedia.org/wiki/Beer_distribution_game) ", in which players play in a beverage supply chain coordination problem, allowing for the study of market dynamics and coordination failures.


Increasingly often, games are now being used to act as controllable simulations, used as a harness to test LLM capability. CollabBench, mentioned above, uses this exact setup to propose a training gym for agents in multi-player environments.


Another benchmark highly related to our chosen scenario is[CEOBench](https://ceobench.com/) (June 2026), in which a single-player agent is tasked with growing a software startup by playing in a turn-based game. Their simulation environment is also mechanistic with stochastic outcomes (outcome changes according to a probability) in which a single agent reasons about actions to take during each turn.


### Technical Details


Full technical details accompany the pre-print. The high-level setup is described below.


### How a game runs


Every game runs inside an isolated virtual machine that operates as a sandbox, providing an easy way to configure the agent's access and tools while allowing it to take fully autonomous actions safely. A host-side driver launches each run, monitors it, and persists results when the game ends.


Inside the sandbox, three components work together:


- **Game engine (Python).** Holds all game mechanics and state, as well as resolving each turn. Outcomes are a mix of deterministic rules and *seeded* stochastic draws, so a given seed reproduces the same world across runs. This lets us hold the scenario, model and config fixed, varying only the collaboration layer. The engine is never touched by agents directly and is blocked by hooks preventing access to its separate directory.
- **Collaboration orchestrator (an API service).** Sits between the agents and the engine. It exposes the engine through API endpoints, hosts the collaboration layer being tested, and enforces the turn gate. We use the same orchestration model across conditions, with changes only in the endpoints available, while the engine holding the game mechanics and task underneath stays identical.
- **Agents.** Each player is a coding agent in its own harness, and currently all players in a given game use the same model and harness. For development purposes we have used **Pi** as our primary harness, as we can use it with most open and frontier models. We use **Claude Code** for fallback and in some testing. Each player (model and harness) is walled inside its own run directory, which holds everything the agent sees: its instructions (` CLAUDE.md` and skills), scratch notepads, and turn logs. Hooks stop an agent from navigating out of its run directory or reaching the engine/mechanics underneath, so each player only ever sees information for their role.


### The turn loop and the turn gate


Agents interact with the orchestrator through a small game CLI, surfaced to them as skills. Reading and writing the collaboration layer (messages, posts, goal updates, etc.) goes through API endpoints, and there is no limit on how often an agent can read or write within a turn, allowing them to catch up on activity as well as revise as much as they want before committing to their move.


When ready, an agent writes its moves for the turn to an actions file and runs a *submit* command. Agents are then held (or resumed if they exit) until every player has submitted. One coordination nudge is built in: if an agent has unread activity in any of its collaboration channels, the first submission attempt is refused and the agent is notified, forcing it to read what its teammates said before the turn can resolve. However, they can still choose to ignore this and submit again, at which point it is accepted, unless newer activity arrives in that window.


The single-player baseline is the same machinery with the collaboration layer removed: one agent controls all five functions and drives the engine directly through the same function-level CLI, giving it a full "oracle" view of the game that the multi-player conditions never have.


### The game and its mechanics


The scenario is a *seed-stage software startup* , run over a fixed horizon of turns, across five functions: *Engineering* , *Sales* , *Marketing* , *Customer Support* , and *Operations* . The team begins with only Engineering, Sales, and Marketing; Support and Ops don't exist yet and must be hired into being. On turn one the company has a single MVP feature, no customers, and a shared budget that burns down every turn.


Each turn, every function is given a limited pool of capacity, represented as a numerical budget, and must decide how to spend it on function-specific actions. Engineering builds and hardens features, squashes bugs or pays down technical debt; Sales works prospects through the pipeline; Marketing runs campaigns; Support tends customer health; Ops runs process projects and analysis; and any function can spend capacity on hiring (for itself or, cross-functionally, for another function). Some actions resolve deterministically; many are stochastic, drawn from the seeded RNG so that repeated runs under different collaboration layers face the same luck.


Collaboration is encouraged through the interplay of various systems, meaning optimal play in multi-player scenarios requires group collaboration and decision making:


- **Economy & runway.** Revenue is recurring (MRR from active customers); costs are the team, overhead, upkeep on shipped features, and one-time costs such as marketing events. The budget nets out each turn, and runway is the solvency constraint the whole team shares.
- **Sales pipeline.** Prospects advance through stages (roughly: lead → engaged → in-deal → customer) via the right actions at each stage. Whether a step lands is a stochastic roll shaped by how well the product meets that customer's needs, awareness in that customer's market, how warm they are, pricing, and the team's recent sales momentum.
- **Product, debt & bugs.** Features live in a dependency tree; building and upgrading them raises how well the product satisfies customers. Lower-tier features are faster to ship, but also accrue technical debt faster, which injects bugs, which drag down customer health and spread CS capacity thinner.
- **Health & churn.** Customers left unattended drift down in health and eventually churn, so growth that outruns Support quietly turns into lost revenue. Customers can develop emergent feature needs based on the team's current feature tree, which can only be caught in time with active support check-ins. Unmet needs lead to churn, forcing a team decision on roadmap.
- **Goals & scoring.** The team shares a small set of company goals in tension (revenue, churn, and runway), and each function also carries its own goal that only partially aligns with the shared ones. A run is scored on normalized progress toward these goals based on targets set as part of the scenario. Targets are chosen based on realism and game mechanics; however, the targets act more as anchors for the LLM to push for more as scores > 1.0 are allowed.
