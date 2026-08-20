---
schema_version: "1.0.0"
document_id: "e2d7ce3c0705e0d6154e2cd520c17821b6475f2759f07d33af329a92f4231e6f"
company_key: "yc-taskade"
company: "Taskade"
source_id: "yc-taskade-rss-a662ed9a0141"
canonical_url: "https://www.taskade.com/blog/ai-agents-history"
published_at: "2026-08-06T00:00:00+00:00"
first_seen_at: "2026-08-06T00:36:28.550226+00:00"
fetched_at: "2026-08-06T00:36:30.904396+00:00"
content_hash: "sha256:4ec2dc95ae8b05f81f989ac2d395c615b3b7075fae2e6a4b65a47b5067b1aa6a"
---

# The History of AI Agents: From SHRDLU to the Agent Loop (2026)

[Blog](https://www.taskade.com/blog)


[AI](https://www.taskade.com/blog/ai)


The History of AI Agents:…


On this page (25)


Ask most people when AI agents were invented and they will say 2023. The real answer is that a robot called Shakey was already perceiving a room, planning a route and pushing a box across it in 1971, and a program called SHRDLU was already taking English instructions and answering questions about its own past actions the same year.


What changed in 2023 was not the idea. It was the substrate. Fifty years of agent research produced a stable shape, the loop of perceive, plan, act and reflect, but every generation could only build part of it. Symbolic planners could plan and could not perceive messy reality. Reactive robots could perceive and act and could not plan. Learning agents could master one game and nothing else. Large language models were the first substrate that could hold all four arrows at once.


This is the complete history, checked against primary sources: the papers, the technical notes, the release announcements and the repositories. If you want the definition rather than the history, read[what AI agents are and how they work](https://www.taskade.com/blog/what-are-ai-agents) first. This article is the story of how we got here.


> **TL;DR:** AI agents are 60 years old, not 3. Shakey ran a perceive, plan, act loop at SRI in 1971 using the STRIPS planner. The modern era started on 6 October 2022 when the[ReAct paper](https://arxiv.org/abs/2210.03629) showed a language model could interleave reasoning with tool calls. Everything since, from AutoGPT to[MCP](https://www.taskade.com/wiki/ai/model-context-protocol) , is that loop getting better plumbing.[Build one in Taskade free](https://www.taskade.com/create) .


*Shakey, Stanford Research Institute, with its sensors and effectors labelled. Every agent since has the same anatomy: something that senses, something that decides, something that acts. Photo: SRI International via Wikimedia Commons, CC BY-SA 3.0.*


---


## What Is an AI Agent, Historically Speaking?


An AI agent is a system that perceives an environment, decides what to do, acts on that environment, and uses the result to decide again. That definition was formalised by Michael Wooldridge and Nicholas Jennings in[Intelligent Agents: Theory and Practice](https://www.cs.cmu.edu/~motionplanning/papers/sbp_papers/integrated1/woodridge_intelligent_agents.pdf) , published in *The Knowledge Engineering Review* in 1995, thirty years before anyone typed a prompt into a chat box. Their "weak notion of agency" required four properties: autonomy, social ability, reactivity, and pro-activeness.


Notice what is not in that definition. Not language. Not neural networks. Not a chat interface. The[agent loop](https://www.taskade.com/wiki/ai-agents/agent-loop) is an architecture, and the whole history of the field is the story of which arrow each generation could finally close.


Read the edge labels as a history. The plan and act arrows were closed in 1971 at SRI. The fast act-back-to-perceive arrow was Brooks's contribution in 1986. The pink node is the one that took fifty years: reflect, meaning an agent that reads its own failure and adjusts without being told, only became practical in 2023.


---


## The Master Timeline of AI Agents


The single most useful thing in any history is the timeline. Here is the pre-language-model half, covering the sixty-five years from the founding of the field to the eve of ChatGPT. Every date is drawn from the original paper, technical note or announcement.


Year Milestone What it could do What it changed


1956 Dartmouth Summer Research Project Named the field "artificial intelligence" Set the agenda: machines that reason about the world


1956 Logic Theorist (Newell, Shaw, Simon) Proved theorems from *Principia Mathematica* First program that searched a problem space


1959 General Problem Solver Means-ends analysis on formal problems Separated the reasoning engine from the domain


1966[ELIZA](https://dl.acm.org/doi/10.1145/365153.365168) (Weizenbaum, *CACM* 9:1) Pattern-matched a therapy conversation Proved people project agency onto anything conversational


1968 A* search (Hart, Nilsson, Raphael) Optimal pathfinding with a heuristic Gave robots a way to choose routes, still used today


1971[SHRDLU](https://dspace.mit.edu/handle/1721.1/7095) (Winograd, MIT AI TR-235) Followed English commands in a blocks world, explained its own actions Language, memory and action inside one system


1971[STRIPS](http://ai.stanford.edu/~nilsson/OnlinePubs-Nils/PublishedPapers/stripsrevisit.pdf) planner, SRI Technical Note 43R Turned goals into ordered action plans The planner that drove Shakey; still the ancestor of task decomposition


1972 Shakey project concludes at SRI Sensed a room, planned, moved objects First end-to-end perceive, plan, act robot


1980[Hearsay-II](https://dl.acm.org/doi/10.1145/356810.356816) ( *ACM Computing Surveys* 12:2) Independent knowledge sources cooperating on a shared blackboard Invented shared-state multi-agent coordination


1980[Contract Net Protocol](https://www.reidgsmith.com/The_Contract_Net_Protocol_Dec-1980.pdf) (Smith, *IEEE Trans. Computers* C-29:12) Nodes announce tasks, other nodes bid Delegation and negotiation between agents


1986[Subsumption architecture](https://www.robolabo.etsit.upm.es/asignaturas/irin/papers/Brooks86RobustLayeredControl.pdf) (Brooks, *IEEE J. Robotics* ) Robots that reacted with no world model at all Rejected central planning; behaviour-first design


1987 BDI theory (Bratman, *Intention, Plans, and Practical Reason* ) Modelled beliefs, desires and intentions separately The mental-state model behind decades of agent design


1994[Interface agents](https://dl.acm.org/doi/10.1145/176789.176792) (Maes, *CACM* 37:7) Learned a user's habits, filtered mail and news Agents as personal assistants, not robots


1994[Internet softbots](https://homes.cs.washington.edu/~etzioni/papers/cacm.pdf) (Etzioni, Weld, *CACM* 37:7) Used ftp, telnet, mail and search as sensors and effectors Software tools as an agent's body


1995[BDI Agents: From Theory to Practice](https://cdn.aaai.org/ICMAS/1995/ICMAS95-042.pdf) (Rao, Georgeff, ICMAS) Deployed BDI reasoning in real systems Made a philosophy of intention into an engineering pattern


1995 *AI: A Modern Approach* (Russell, Norvig) Reframed all of AI around rational agents The agent became the field's unit of analysis


1996 FIPA founded in Geneva Standard agent communication language First serious attempt at agent interoperability


1997 Deep Blue defeats Kasparov; first RoboCup Superhuman search in one closed domain Proved narrow mastery, and its limits


2015[DQN](https://www.nature.com/articles/nature14236) (Mnih et al., *Nature* 518) Learned 49 Atari games from pixels with one architecture One agent, many environments, no hand-coding


2016 AlphaGo defeats Lee Sedol Beat a world champion at Go Search plus learning beat search alone


2017[Attention Is All You Need](https://arxiv.org/abs/1706.03762) The transformer architecture The substrate everything after 2022 runs on


2020 GPT-3 Few-shot task performance from a prompt General capability without task-specific training


2021[WebGPT](https://arxiv.org/abs/2112.09332) (OpenAI) Answered questions by browsing the live web An LLM using an external tool, before it was called agentic


Now the language-model half. Note how compressed it is. Six decades in the table above, four years in the one below.


Date Milestone What it could do What it changed


Jan 2022[Chain-of-thought prompting](https://arxiv.org/abs/2201.11903) Models reasoned step by step when asked to Made the plan arrow visible and steerable


Oct 2022[ReAct](https://arxiv.org/abs/2210.03629) (Yao et al.) Interleaved reasoning traces with tool actions The template for every modern agent loop


Oct 2022 LangChain repository created Chained prompts, tools and memory First widely used agent construction kit


Nov 2022 ChatGPT launches Put a capable model in front of everyone Turned a research pattern into a market


Feb 2023[Toolformer](https://arxiv.org/abs/2302.04761) (Meta) Taught itself when to call an API Tool use as a learned skill, not a prompt trick


Mar 2023 AutoGPT repository published Self-prompted toward a goal without a human The viral proof that autonomy was possible, and fragile


Apr 2023 BabyAGI;[Generative Agents](https://arxiv.org/abs/2304.03442) (Stanford) Task lists that rewrote themselves; 25 agents with memory and reflection Memory and reflection as first-class components


Jun 2023[OpenAI function calling](https://openai.com/index/function-calling-and-other-api-updates/) Model emitted structured arguments for real functions Tool use became reliable enough to ship


Oct 2023[SWE-bench](https://arxiv.org/abs/2310.06770) 2,294 real GitHub issues as a benchmark Gave agents a hard, honest scoreboard


Oct 2024[Computer use](https://www.anthropic.com/news/3-5-models-and-computer-use) (Anthropic) Moved a cursor, clicked and typed on a real screen Every app became a tool, with or without an API


Nov 2024[Model Context Protocol](https://www.anthropic.com/news/model-context-protocol) One open standard for connecting tools and data Ended per-vendor integration sprawl


Dec 2024[Building Effective Agents](https://www.anthropic.com/engineering/building-effective-agents) Named the workflow and agent patterns Gave the industry shared vocabulary


Jan 2025[Operator](https://openai.com/index/computer-using-agent/) (OpenAI) Browsed and completed tasks in its own browser Consumer-facing computer-using agents


Feb 2025 Claude Code research preview Delegated engineering tasks from a terminal The[agent harness](https://www.taskade.com/wiki/ai-agents/agent-harness) as a product category


Apr 2025[Agent2Agent](https://developers.googleblog.com/en/a2a-a-new-era-of-agent-interoperability/) (Google, 50+ partners) Agents from different vendors discovering each other Interoperability between agents, not just tools


Aug 2025[AGENTS.md](https://agents.md/) One instruction file every coding agent reads A README for agents, now in 60,000+ repositories


Dec 2025[Agentic AI Foundation](https://www.linuxfoundation.org/press/linux-foundation-announces-the-formation-of-the-agentic-ai-foundation) formed MCP, AGENTS.md and goose under the Linux Foundation Agent standards moved to neutral governance


### The six eras at a glance


Sixty years of agent research divide cleanly into six eras. Each one solved a real problem and hit a wall that defined the next.


Era Years Defining system What it could do What broke


Symbolic 1956 to 1979 SHRDLU, Shakey with STRIPS Reason about goals, plan ordered actions, act in a hand-specified world The world had to be encoded by hand; scale killed it


Coordination 1980 to 1989 Hearsay-II, Contract Net Split work across many specialised knowledge sources Rule bases lost consistency past a few thousand rules


Reactive 1986 to 1999 Subsumption architecture React to the real world in real time, no world model No long-horizon goals; could not be told what to do


Software agents 1994 to 2005 Internet softbots, BDI systems Use software tools as a body, hold intentions over time Could not understand instructions in ordinary language


Learning 2000 to 2021 DQN, AlphaGo Learn a policy from reward, master a closed domain No transfer between environments, no natural-language goals


Language-model agents 2022 to now ReAct, harnesses, MCP Read an instruction, plan, call tools, notice and fix its own errors Reliability over long horizons; cost; honest evaluation


---


## Era 1: Symbolic Agents in Toy Worlds, 1956 to 1979


The first agents reasoned beautifully and perceived almost nothing. The defining constraint of this era was that the world had to be simplified until a program could represent it in logic, which meant a tabletop of coloured blocks or a handful of rooms.


The field was named at the Dartmouth Summer Research Project in 1956, and within months Allen Newell, Herbert Simon and Cliff Shaw had the Logic Theorist proving theorems from Whitehead and Russell's *Principia Mathematica* . Their follow-up, the General Problem Solver, introduced means-ends analysis: compare where you are to where you want to be, and pick the operator that shrinks the difference. That is still, structurally, what a modern[planning and reasoning](https://www.taskade.com/wiki/ai/planning-reasoning) step does.


Then in 1966 Joseph Weizenbaum published[ELIZA](https://dl.acm.org/doi/10.1145/365153.365168) in *Communications of the ACM* . ELIZA was not an agent. It had no goals, no memory of the world and no ability to act. It matched keywords and reflected sentences back. Its historical importance is what happened next: people confided in it, and Weizenbaum spent the rest of his career alarmed by that. The first thing the field learned about agents is that humans grant them agency far too readily, a lesson that is fully relevant to 2026.


### SHRDLU: the first program that could explain itself


Terry Winograd's SHRDLU, published as[MIT AI Technical Report 235](https://dspace.mit.edu/handle/1721.1/7095) in February 1971, is the single most misunderstood system in agent history. People remember the blocks. What mattered was everything around them.


SHRDLU accepted typed English, resolved pronouns against context, executed physical actions in a simulated world, remembered what it had done, and answered questions about its own reasoning. Ask it why it moved a block and it could tell you. That is an agent with an[agent session](https://www.taskade.com/wiki/ai-agents/agent-session) , working memory, tool execution and a rudimentary audit trail, in 1971.


It also contained the era's fatal flaw in its purest form. SHRDLU worked because the blocks world was small enough to be fully specified. Everything the program needed to know had been written down by a human in advance. Scale the world and the hand-written representation collapses. That failure is the reason the phrase "toy domain" became a criticism.


### Shakey: the first robot to close the loop


While Winograd worked in a simulated world, Stanford Research Institute was closing the loop in a real one. Shakey, built between 1966 and 1972, had a television camera, a range finder and bump detectors for perception, drive motors for action, and a radio link to a room-sized computer for thinking.


Its planner was[STRIPS](http://ai.stanford.edu/~nilsson/OnlinePubs-Nils/PublishedPapers/stripsrevisit.pdf) , published by Richard Fikes and Nils Nilsson as SRI Technical Note 43R in 1971 and in the journal *Artificial Intelligence* the same year. STRIPS took a goal state and a set of operators with preconditions and effects, and searched for an ordered sequence of actions that got from here to there. The A* algorithm, published by Peter Hart, Nils Nilsson and Bertram Raphael in 1968, came out of the same programme and is still in production in every route planner on earth.


*Shakey, preserved at the Computer History Museum. The camera is at the top, the drive wheels at the bottom, and the thinking happened on a computer in another room, connected by radio. Photo: The wub via Wikimedia Commons, CC BY-SA 4.0.*


Shakey's real limitation was time. A single plan could take a very long time to compute, and by the time it finished, the world might have moved. That gap between deliberation speed and world speed is the problem the next era attacked.


---


## Era 2: Blackboards and Contracts, 1980 to 1989


The second era's contribution was coordination. When one agent cannot solve a problem, the answer is several agents that share state and divide labour, and both canonical mechanisms for that were published in 1980.


The[Hearsay-II speech understanding system](https://dl.acm.org/doi/10.1145/356810.356816) , described by Lee Erman, Frederick Hayes-Roth, Victor Lesser and Raj Reddy in *ACM Computing Surveys* in 1980, introduced the blackboard architecture. Independent knowledge sources, one for acoustics, one for syntax, one for semantics, all watched a shared data structure. When a knowledge source saw something it could improve, it wrote its contribution back. Nobody was in charge. The solution emerged from the shared state.


If you have ever given several AI agents a shared project and let each one contribute what it knows, you have used a blackboard. Taskade's[multi-agent teams](https://www.taskade.com/wiki/ai-agents/multi-agent-teams) are the same shape with better plumbing: the project is the blackboard, and each agent watches it.


The same year, Reid Smith published the[Contract Net Protocol](https://www.reidgsmith.com/The_Contract_Net_Protocol_Dec-1980.pdf) in *IEEE Transactions on Computers* . Where blackboards shared state, contract nets negotiated. A node with work to do announces the task. Nodes that think they can do it bid. The announcer awards the contract. This is delegation with a market mechanism attached, and it is precisely the shape that modern[agent orchestration](https://www.taskade.com/wiki/ai-agents/orchestration) frameworks rediscovered in 2023 when they started routing subtasks to specialist agents.


This era also produced the first AI winter's aftermath. Expert systems had promised that encoding human expertise as rules would industrialise intelligence. They worked in narrow slices and collapsed under maintenance, because rules interact combinatorially and nobody could keep thousands of them consistent. Funding contracted twice, in the mid-1970s and again in the late 1980s. Agent research did not stop, it went quieter and got more rigorous.


---


## Era 3: The Reactive Revolt and the Software Agent, 1986 to 1999


The third era split the field in two, and the split still defines agent design in 2026. In March 1986 Rodney Brooks published[A Robust Layered Control System for a Mobile Robot](https://www.robolabo.etsit.upm.es/asignaturas/irin/papers/Brooks86RobustLayeredControl.pdf) in the *IEEE Journal of Robotics and Automation* , and argued that the entire sense, model, plan, act pipeline was a mistake.


Brooks proposed decomposing a robot by behaviour rather than by function. Layer one avoids obstacles. Layer two wanders. Layer three explores. Each layer runs continuously and asynchronously, higher layers can suppress the outputs of lower ones, and there is no central world model and no planner. His robots walked out of the lab and into unmodified corridors while planner-driven robots were still computing. He later put the position in the title of a paper: intelligence without representation.


Modern agents are the hybrid at the bottom. They do not build a complete world model and they do not plan the whole task up front, because both go stale. They plan one step, act, look at what came back, and plan again. That is Brooks's tight sensing loop wrapped around a symbolic planner, which is the synthesis the 1990s argued about and nobody could build until the substrate arrived.


### Beliefs, desires and intentions


Running in parallel, the deliberative camp got more sophisticated. Michael Bratman's 1987 book *Intention, Plans, and Practical Reason* argued that a resource-bounded agent needs intentions as a distinct mental category, because reconsidering every option at every moment is ruinously expensive. Commitment is an efficiency mechanism.


Anand Rao and Michael Georgeff turned that philosophy into engineering in[BDI Agents: From Theory to Practice](https://cdn.aaai.org/ICMAS/1995/ICMAS95-042.pdf) at the first International Conference on Multi-Agent Systems in 1995. Beliefs are what the agent thinks is true, desires are what it would like, intentions are what it has committed to pursue. Their Procedural Reasoning System ran in real deployments, including air traffic management.


The BDI insight is underrated today. An agent that re-derives its goal from scratch at every turn wanders. An agent that holds an intention across turns finishes. That is exactly the problem[context compaction](https://www.taskade.com/wiki/ai-agents/context-compaction) and persistent[agent memory](https://www.taskade.com/wiki/ai-agents/agent-memory) solve in a 2026 harness.


### Agents escape the robot body


Then the internet arrived, and agents stopped needing wheels. The July 1994 issue of *Communications of the ACM* carried two papers that between them created the software agent as a category.


Pattie Maes described[interface agents](https://dl.acm.org/doi/10.1145/176789.176792) that learned an individual user's habits and then filtered their mail, news and calendar. Oren Etzioni and Daniel Weld described the[Internet softbot](https://homes.cs.washington.edu/~etzioni/papers/cacm.pdf) , whose effectors were ftp, telnet, mail and file commands, and whose sensors were the internet search facilities of the day. Read that sentence again and swap in modern names. The softbot was doing[tool use](https://www.taskade.com/wiki/ai/tool-use) in 1994.


By 1996 the Foundation for Intelligent Physical Agents was founded in Geneva to standardise how agents talk to each other, producing the FIPA agent communication language. It is worth knowing that the industry attempted agent interoperability standards thirty years before[Agent2Agent](https://www.taskade.com/wiki/ai/agent-to-agent-protocol) , and that the attempt largely failed for want of agents worth connecting.


The decade closed with two demonstrations of what narrow agents could do. Deep Blue beat Garry Kasparov in 1997, and the first RoboCup was held the same year. Both proved mastery inside a closed domain. Neither transferred one inch outside it.


---


## Era 4: Learning Agents, 2000 to 2021


The fourth era's contribution was that agents stopped being programmed and started being trained. In February 2015 DeepMind published[Human-level control through deep reinforcement learning](https://www.nature.com/articles/nature14236) in *Nature* , describing a single network architecture that learned to play 49 different Atari games directly from screen pixels, without per-game tuning.


That is a genuinely different thing from Deep Blue. Deep Blue was hand-built for chess. DQN was built for nothing in particular and learned each game from reward. In March 2016 AlphaGo beat Lee Sedol, combining learned evaluation with tree search, and the combination beat either alone.


[Reinforcement learning](https://www.taskade.com/wiki/ai/reinforcement-learning) gave the field the vocabulary it still uses: environment, state, action, reward, policy. But it had a hard ceiling. An RL agent learns a policy for one environment through millions of episodes, and cannot read a manual, cannot be told what to do in words, and cannot transfer to a task it never saw. It closed the perceive and act arrows brilliantly and could not accept an instruction.


Meanwhile the assistant thread continued commercially. SRI's DARPA-funded CALO project spun out the technology that became Siri, which shipped on the iPhone in 2011. Siri and the voice assistants that followed had exactly one agent property: they could act. They could not plan, could not chain steps and had no memory across turns.


And in June 2017 the[transformer](https://www.taskade.com/wiki/ai/transformer) arrived, in a paper about machine translation that said nothing at all about agents. By 2020 GPT-3 could perform tasks it was never trained on, given only a prompt. By December 2021 OpenAI's WebGPT was answering questions by browsing the live web. The pieces were on the table.


---


## The LLM Turn: Why October 2022 Is the Real Hinge


The modern agent era began on 6 October 2022, when Shunyu Yao and colleagues submitted[ReAct: Synergizing Reasoning and Acting in Language Models](https://arxiv.org/abs/2210.03629) to arXiv. The idea is almost embarrassingly simple, and it is the reason every agent framework in existence looks the way it does.


Before ReAct, you could prompt a model to reason (chain of thought, January 2022) or you could prompt it to produce an action. ReAct interleaved them in one stream. The model writes a thought, takes an action, reads the observation that comes back, then writes the next thought informed by what it just learned. The paper reports that grounding reasoning in real observations helped with "overcoming issues of hallucination and error propagation".


Here is what a ReAct trace actually looks like, which is the clearest way to see why this was the unlock:


```text
STEP  CHANNEL      CONTENT
----  -----------  ---------------------------------------------------
1    Thought      The user asked for last quarter's churn number.
I do not have it. I should look in the CRM.
2    Action       crm.query(metric="churn", period="Q2")
3    Observation  ERROR: field "churn" not found
4    Thought      Wrong field name. Let me list available metrics.
5    Action       crm.list_metrics()
6    Observation  ["retention_rate", "logo_churn", "net_revenue_churn"]
7    Thought      "logo_churn" is what they meant.
8    Action       crm.query(metric="logo_churn", period="Q2")
9    Observation  0.041
10    Thought      4.1%. I have the answer and can write it up.
11    Answer       Logo churn for Q2 was 4.1%.


```


Steps 3 to 7 are the whole story of AI agent history. The system made a mistake, saw the error text, diagnosed it, and recovered without a human. Fifty years of research could perceive, plan and act. This is the first substrate where the reflect arrow closed by itself. Everything called "agentic" since is a variation on this loop, which is why we wrote a whole wiki entry on[the ReAct pattern](https://www.taskade.com/wiki/ai/react-pattern) and another on[error recovery](https://www.taskade.com/blog/ai-agent-error-recovery) .


ChatGPT launched on 30 November 2022. Within four months, the pattern had a market.


---


## Era 5: The Agent Explosion of 2023


2023 was the year the industry tried everything at once, and most of it broke productively. The pattern was consistent: someone wrapped a language model in a loop, it did something startling in a demo, and then fell over on real work in a way that taught the field something specific.


In February, Meta researchers published[Toolformer](https://arxiv.org/abs/2302.04761) , which trained a model to decide for itself when to call a calculator, a search engine or a translator. Tool use stopped being a prompt trick and became a learned capability.


In March, Toran Bruce Richards published AutoGPT. Its repository was created on 16 March 2023 and it became one of the fastest-starred projects in GitHub history, standing at over 185,000 stars by July 2026. AutoGPT gave a model a goal and let it prompt itself indefinitely: generate tasks, execute them, generate more. In demos it was spellbinding. In practice it looped, forgot what it had already tried, and burned budget on the same failed approach. BabyAGI, published by Yohei Nakajima in April 2023, was the minimal version of the same idea, a task list that rewrote itself.


Both projects failed at exactly the thing the previous fifty years predicted they would fail at. A planner with no persistent state and no commitment mechanism wanders. Bratman wrote that down in 1987.


April also brought[Generative Agents](https://arxiv.org/abs/2304.03442) from Stanford, in which twenty-five simulated agents with memory streams, reflection and planning lived in a small town. Starting from one agent's intention to throw a Valentine's Day party, the agents autonomously spread invitations and coordinated attendance over two simulated days. It was the most convincing demonstration to date that memory plus reflection produces behaviour that looks like intent.


The infrastructure caught up fast. On 13 June 2023 OpenAI shipped[function calling](https://openai.com/index/function-calling-and-other-api-updates/) , so a model could emit a structured JSON object matching a declared function signature instead of hoping a regular expression would parse its prose. This is the single change that made agents shippable, and it is why[function calling](https://www.taskade.com/wiki/ai/function-calling) and[tool calling](https://www.taskade.com/wiki/ai/tool-calling) are foundational vocabulary now.


In October,[SWE-bench](https://arxiv.org/abs/2310.06770) arrived with 2,294 software engineering problems drawn from real GitHub issues, and the honest scoreboard it produced was brutal. The paper reports that the strongest model they evaluated, Claude 2, "is able to solve a mere 1.96% of the issues". That number is worth remembering whenever someone claims agents were a solved problem in 2023.


### Agent frameworks by year


Framework Repository created Stars, Jul 2026 Core idea


LangChain 17 Oct 2022 142,000+ Chain prompts, tools and memory into pipelines


AutoGPT 16 Mar 2023 185,000+ Self-prompting loop toward an open-ended goal


BabyAGI 3 Apr 2023 22,000+ Minimal self-rewriting task queue


LangGraph 9 Aug 2023 37,000+ Agents as explicit graphs with durable state


AutoGen (Microsoft) 18 Aug 2023 59,000+ Conversation between multiple specialised agents


CrewAI 27 Oct 2023 55,000+ Role-playing agents with assigned jobs in a crew


MCP servers 19 Nov 2024 88,000+ Reference tool servers for the open MCP standard


Star counts read from the GitHub API on 23 July 2026. They measure attention, not quality, but the shape of the curve is the story: the 2023 cohort optimised for autonomy, and the 2024 cohort optimised for structure and standards, because autonomy without structure did not survive contact with production.


---


## Era 6: Harnesses, Standards and the Boring Part, 2024 to 2026


The current era's contribution is that agents got infrastructure instead of ambition. The turning point was the recognition that the interesting engineering is not in the model, it is in the software wrapped around it: the[agent harness](https://www.taskade.com/blog/agent-harness-explained) .


On 22 October 2024 Anthropic shipped[computer use](https://www.anthropic.com/news/3-5-models-and-computer-use) , letting a model look at a screen, move a cursor, click and type. On the OSWorld benchmark it scored 14.9% in the screenshot-only category against 7.8% for the next best system, rising to 22.0% when allowed more steps. Those numbers are low and the announcement said so plainly, calling the capability "at times cumbersome and error-prone". They mattered anyway, because they meant every application became reachable, API or no API. See[computer-use agents](https://www.taskade.com/wiki/ai/computer-use-agents) for where that led.


A month later, on 25 November 2024, Anthropic released the[Model Context Protocol](https://www.anthropic.com/news/model-context-protocol) as an open standard for connecting assistants to data sources, shipping the specification, SDKs, local server support and pre-built servers for systems like Google Drive, Slack, GitHub and Postgres on day one. This is the most consequential piece of plumbing in the whole modern era, and the reason is arithmetic.


Six custom integrations on the left, five standard connections on the right, and the gap widens as either side grows. That is the N times M to N plus M collapse, and it is the same argument that justified USB, HTTP and every other successful protocol.


Then in December 2024 Anthropic published[Building Effective Agents](https://www.anthropic.com/engineering/building-effective-agents) , which gave the industry the distinction it had been missing. Workflows are "systems where LLMs and tools are orchestrated through predefined code paths". Agents are "systems where LLMs dynamically direct their own processes and tool usage, maintaining control over how they accomplish tasks". The post also named the patterns most teams actually need: prompt chaining, routing, parallelisation, orchestrator-workers and evaluator-optimizer. Most production "agents" are workflows, and that is usually the right call.


2025 was the year of standards. OpenAI shipped[Operator](https://openai.com/index/computer-using-agent/) on 23 January, a browsing agent whose underlying Computer-Using Agent model scored 38.1% on OSWorld and 58.1% on WebArena, still well below human performance but a large jump in a single year. Anthropic released Claude Code as a research preview on 24 February. Google announced[Agent2Agent](https://developers.googleblog.com/en/a2a-a-new-era-of-agent-interoperability/) on 9 April with more than fifty partners, and contributed it to the Linux Foundation in June. In August, OpenAI, Google, Cursor, Factory and Amp converged on[AGENTS.md](https://agents.md/) , a plain instruction file that any coding agent reads, now present in more than 60,000 open-source projects.


The era closed with governance. On 9 December 2025 the Linux Foundation announced the[Agentic AI Foundation](https://www.linuxfoundation.org/press/linux-foundation-announces-the-formation-of-the-agentic-ai-foundation) , with MCP, AGENTS.md and Block's goose as founding projects. Thirty years after FIPA tried to standardise agent communication in Geneva and found there were not enough agents to connect, the standards finally have something to standardise.


---


## What Each Era Got Wrong, and What Survived


The most useful way to read sixty years of agent research is as a list of specific failures, each of which produced a component that is still in use.


Era What it could not do What survived into 2026


Symbolic, 1956 to 1979 Perceive an unstructured world; everything had to be hand-encoded Goal decomposition, preconditions and effects, A* search


Blackboard and contract, 1980s Scale rule bases; consistency collapsed past a few thousand rules Shared-state coordination, task announcement and bidding


Reactive, 1986 to 1999 Pursue a long-horizon goal without a plan Tight sense-act loops, layered behaviour, replan-per-step


BDI and software agents, 1990s Understand instructions given in ordinary language Persistent intention, commitment, agents with tools as a body


Learning agents, 2000 to 2021 Transfer across environments or accept a spoken goal Environment and reward vocabulary, learned policies, search plus learning


Autonomous LLM loops, 2023 Stay on task; no durable memory, no commitment, no stop condition Self-prompting, task decomposition, the reflect arrow


Harness era, 2024 to 2026 Still open: reliability over long horizons, cost, evaluation Standard protocols, scoped permissions, human checkpoints


Read the right-hand column top to bottom and you have the architecture of a modern agent. Nothing was thrown away. The blackboard became the shared project, the contract net became task routing, subsumption became replanning after every step, BDI became persistent memory, and RL gave us the words we use to describe all of it.


---


## What Is Still Unsolved in 2026


Three problems remain open, and every one of them has a fifty-year-old ancestor.


**Long-horizon reliability.** An agent that is 95% reliable per step is 60% reliable across ten steps and 36% across twenty. This is the same arithmetic that made Shakey's long plans go stale, and it is why[human-in-the-loop](https://www.taskade.com/wiki/ai-agents/human-in-the-loop) checkpoints are not a temporary crutch. They are the design.


**Context management.** A model's[context window](https://www.taskade.com/wiki/ai/context-window) is finite, so long-running work must be summarised, discarded or stored elsewhere. What to keep is a judgement call, which is why[context engineering](https://www.taskade.com/wiki/ai/context-engineering) became a named discipline and why[parametric knowledge](https://www.taskade.com/wiki/ai/parametric-knowledge) alone is never enough. The blackboard architecture faced the identical question in 1980 and answered it with shared external state, which is still the best answer.


**Evaluation.** SWE-bench started at 1.96% resolution in 2023 and rose fast, and the field promptly discovered that benchmark scores and production usefulness diverge.[Agent evaluation](https://www.taskade.com/wiki/ai-agents/agent-evaluation) is now its own subfield, and[agent observability](https://www.taskade.com/wiki/ai-agents/agent-observability) is how teams see what actually happened in a run.


---


## Where Taskade Fits in This History


Taskade builds the harness, not the model. The historical read is that the substrate problem is solved and the surrounding software is where the remaining value sits, which is exactly the bet[Taskade Genesis](https://www.taskade.com/create) makes: describe what you want in one prompt and get a working app with agents, automations and data already wired together.


Concretely, the components this history produced map to things you can use today. The blackboard is your project, shared by everyone and every agent in it, across all 7 project views including List, Board, Table, Mind Map and Gantt. The contract net is[multi-agent teams](https://www.taskade.com/learn/agents/multi-agents) , where you assign specialist agents to specialist jobs. The softbot's body is[100+ bidirectional integrations](https://www.taskade.com/integrations) , where triggers pull events in and actions push data out. Persistent intention is[agent memory and knowledge](https://www.taskade.com/learn/agents/knowledge) , where an agent reads your own documents rather than guessing. The reflect arrow is[autonomous agents](https://www.taskade.com/learn/agents/autonomous) that keep working across steps, and Taskade EVE is the meta-agent that builds the rest for you.


Under the hood you get 15+ frontier models from OpenAI, Anthropic, Google and open-weight providers, so a task can be routed to a model that suits it rather than whichever one you signed up with. Paid plans start at $10 per month billed annually.


If you are choosing where to start,[AI Agents](https://www.taskade.com/agents) is the place to build one,[Automations](https://www.taskade.com/automate) is the place to trigger it, and[Taskade Community](https://www.taskade.com/community) has working examples you can clone.


---


## Frequently Asked Questions


What was the first AI agent?


Shakey, built at Stanford Research Institute between 1966 and 1972, is the system most historians point to. It perceived a real room with a camera, range finder and bump detectors, planned with STRIPS, and executed physical actions. Earlier programs such as the Logic Theorist in 1956 reasoned but had no environment to sense or act on.


What is SHRDLU and why does it matter to AI agent history?


SHRDLU was Terry Winograd's natural language program, published as MIT AI Technical Report 235 in February 1971. It took English commands to manipulate blocks in a simulated world and could answer questions about its own past actions. It matters because it was the first system to hold language, memory, reasoning and action together in one program, which is precisely the combination modern agents rely on.


When did the modern AI agent era begin?


October 2022. The ReAct paper, submitted to arXiv on 6 October 2022, showed a language model interleaving reasoning traces with tool actions in a single loop. ChatGPT followed on 30 November 2022. The 2023 wave of agent projects is downstream of that one pattern.


What is the agent loop?


The agent loop is the repeating cycle of perceive, plan, act and reflect that an agent runs to complete multi-step work. Symbolic AI closed the plan arrow in 1971 and reactive robotics closed the perceive-act loop in 1986, but the reflect arrow, where an agent reads its own error and corrects course unprompted, only became practical with large language models.


What is the difference between an AI agent and a workflow?


A workflow follows a route you drew in advance. An agent chooses the route. Anthropic's December 2024 engineering post defines workflows as systems where models and tools are orchestrated through predefined code paths, and agents as systems where models dynamically direct their own processes and tool usage. Most production systems are workflows, and that is usually correct.


Who invented the subsumption architecture?


Rodney Brooks, in a paper published in the IEEE Journal of Robotics and Automation in March 1986. It decomposed robot control by behaviour instead of by function, with layers that suppress each other and no central world model. Its modern descendant is the practice of replanning after every single step instead of committing to a long plan up front.


What was AutoGPT and did it work?


AutoGPT was an open-source self-prompting agent published in March 2023. It became one of the most-starred repositories on GitHub, passing 185,000 stars by July 2026, and it genuinely demonstrated that an autonomous loop was possible. It also looped, lost track of what it had tried and overran budgets, because it had no durable memory and no commitment mechanism, a failure mode BDI research had characterised in 1987.


What is the Model Context Protocol and why was it a turning point?


MCP is an open standard, released by Anthropic on 25 November 2024, for connecting AI assistants to tools and data. It turned an N times M integration problem into an N plus M one: instead of every assistant writing custom glue for every tool, both sides speak one protocol. In December 2025 it became a founding project of the Agentic AI Foundation under the Linux Foundation.


What were the AI winters and how did they affect agent research?


Funding collapsed in the mid-1970s and again in the late 1980s after symbolic systems failed to scale past toy domains. Agent research narrowed rather than stopped, and that period produced blackboard systems, the Contract Net Protocol, belief-desire-intention architectures and the subsumption architecture. All four are visible in agent frameworks today.


What is a multi-agent system?


A multi-agent system is several agents coordinating on a problem that one agent handles poorly. The pattern is decades old: the Contract Net Protocol described task announcement and bidding in 1980, and blackboard systems described shared-state cooperation the same year. Frameworks like AutoGen and CrewAI rediscovered both shapes in 2023. Read more in[multi-agent systems](https://www.taskade.com/wiki/ai/multi-agent-systems) .


What is AGENTS.md?


AGENTS.md is an open format that gives coding agents project-specific instructions, the way a README serves humans. It was formalised in August 2025 by OpenAI, Google, Cursor, Factory and Amp, and by 2026 it appears in more than 60,000 open-source projects. It is now stewarded by the Agentic AI Foundation.


How do I build an AI agent today?


Define the trigger, the tools it may use, the knowledge it can read and the point where a human approves. Taskade lets you assemble all four without writing code, then run the agent on a schedule or from an event in any of 100+ integrations. Start at[Taskade AI Agents](https://www.taskade.com/agents) .


---


## Sixty Years, One Loop


Terry Winograd's blocks and Shakey's radio link look primitive now, and they were solving the same problem you are solving when you ask an agent to reconcile an invoice: read the state, choose a step, do it, check what came back. The loop has not changed. Only the substrate has.


What is different in 2026 is that the loop no longer needs a research lab. It needs a workspace where memory, intelligence and execution live in the same place, which is exactly what[Taskade Genesis](https://www.taskade.com/create) is for. Describe the job once, and the agents, automations and data that carry it out are generated together instead of assembled by hand.


Fifty years of research to close one arrow. Now go use it.


▲ ■ ●


[Build your first AI agent free →](https://www.taskade.com/create)


---


## Further Reading


### Start Here


- [What Are AI Agents?](https://www.taskade.com/blog/what-are-ai-agents) : the definition sibling to this history, covering components and how agents differ from chatbots
- [What Is an Agent Harness?](https://www.taskade.com/blog/agent-harness-explained) : the software wrapped around the model, which is where the 2024 era put its effort
- [AI Agent Taxonomy](https://www.taskade.com/blog/ai-agents-taxonomy) : the five agent types, from simple reflex to learning


### Related Histories


- [The History of LangChain](https://www.taskade.com/blog/langchain-history) : the framework that defined the 2023 agent stack
- [The History of ChatGPT](https://www.taskade.com/blog/openai-chatgpt-history) : the launch that turned a research pattern into a market
- [The History of Claude Code](https://www.taskade.com/blog/claude-code-history) : the terminal agent that made harnesses a product category
- [The History of Workflow Automation](https://www.taskade.com/blog/automation-history) : the parallel track that agents eventually merged with


### Go Deeper


- [Agentic Design Patterns](https://www.taskade.com/blog/agentic-design-patterns) : reflection, tool use, planning and multi-agent, as patterns you can apply
- [How Do Large Language Models Work?](https://www.taskade.com/blog/how-do-llms-work) : the substrate, explained plainly
- [Agentic Workflows: Path Towards AGI](https://www.taskade.com/blog/agentic-workflows-towards-agi) : where the loop goes next


### Build Something


- [Taskade Genesis](https://www.taskade.com/create) : one prompt, one living app with agents and automations included
- [AI Agents](https://www.taskade.com/agents) : custom agents with your knowledge, your tools, your rules
- [Automations](https://www.taskade.com/automate) : triggers, actions and 100+ bidirectional integrations
- [Taskade Community](https://www.taskade.com/community) : clone a working agent and edit it
