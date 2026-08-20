---
schema_version: "1.0.0"
document_id: "57730d2f01cec93aa234dde94aff47c66b7a2f696ed511c092b8ab7921933656"
company_key: "yc-taskade"
company: "Taskade"
source_id: "yc-taskade-rss-a662ed9a0141"
canonical_url: "https://www.taskade.com/blog/agent-harness-history"
published_at: "2026-08-09T00:00:00+00:00"
first_seen_at: "2026-08-09T20:19:57.584347+00:00"
fetched_at: "2026-08-09T20:19:59.874482+00:00"
content_hash: "sha256:37a5b989da53da1457ba194d80e72662dac1fd8c2eea0ab5f171a3a9a01b216d"
---

# The History of the Agent Harness: The Software Around the Model (2026)

[Blog](https://www.taskade.com/blog)


[AI](https://www.taskade.com/blog/ai)


The History of the Agent…


On this page (18)


In May 2024, a team at Princeton took a language model that could resolve **3.8 percent** of real GitHub issues and, without retraining a single weight, pushed it to **12.5 percent** . They did not fine-tune. They did not add data. They rebuilt the *interface* between the model and the computer: a file viewer sized for a model's attention, an editor that lints after every change, a search tool that returns repository structure instead of raw text.


That paper,[SWE-agent](https://arxiv.org/abs/2405.15793) , is the clearest proof of a claim the industry took another two years to accept: **most of what you experience as an AI agent is not the model.** It is the software wrapped around it. That software has a name now. It is called the harness.


This is its history. If you want the concept explained from scratch rather than its origin story, start with[What Is an AI Agent Harness?](https://www.taskade.com/blog/agent-harness-explained) and come back. This post covers where every piece of it came from, and when.


> **TL;DR:** The agent harness is everything around the model: the loop, the tools, the environment, the checks and the permissions. Its parts arrived over sixty years, from the 1964 read-eval-print loop to 2025 context compaction.[SWE-agent](https://arxiv.org/abs/2405.15793) proved the point in 2024 by tripling a model's score through interface design alone.[Build on a ready-made harness free](https://www.taskade.com/create) .


Photo: Wikimedia Commons / ArnoldReinhold / CC BY-SA 4.0. The console typewriter of a DEC PDP-1, the machine on which the first read-eval-print loop ran in 1964.


---


## What Is an Agent Harness, in One Paragraph?


An agent harness is the running software that turns a text-prediction model into something that finishes work. It assembles what the model sees, exposes the actions it can take, executes those actions against a real environment, feeds the results back, checks them, and decides when to stop or ask a human. Anthropic's[Building Effective AI Agents](https://www.anthropic.com/engineering/building-effective-agents) , published 19 December 2024, put the whole thing in one sentence: agents are "typically just LLMs using tools based on environmental feedback in a loop."


The confusion in most coverage comes from four words that get used interchangeably. They are not the same thing.


Layer What it is What it cannot do alone Example


**Model** A fixed function: text in, text out Remember, act, verify, or stop A frontier model API call


**Harness** The loop, tools, environment, checks and permissions around it Reason about your problem Claude Code, Codex CLI,[Taskade](https://www.taskade.com/agents) automations


**Agent** A model running inside a harness on a goal Exist without either half A research agent working a task list


**App** The product a person actually opens Do the work itself The chat window, the dashboard, the[published app](https://www.taskade.com/community)


Read the table top to bottom and the argument writes itself: the model supplies judgement, the harness supplies *everything else* , and the thing you buy is almost always the harness. For a definition page you can cite on its own, see the wiki entry on the[agent harness](https://www.taskade.com/wiki/ai-agents/agent-harness) .


---


## Where the Word "Harness" Came From


The word arrived in AI by way of software testing, not robotics. A **test harness** is the rig that feeds inputs into a program and checks what comes out, a concept older than most working programmers. Machine learning borrowed it for the **evaluation harness** : the wrapper that runs a model against a battery of benchmarks under identical conditions so scores can be compared.


The version that made the word standard is EleutherAI's[Language Model Evaluation Harness](https://github.com/EleutherAI/lm-evaluation-harness) . Its repository opened on **28 August 2020** and its first tagged release, v0.0.1, was cut by Leo Gao on **1 September 2021** (both dates confirmed via the GitHub API). It now covers, in its own words, "over 60 standard academic benchmarks for LLMs, with hundreds of subtasks and variants implemented." When a paper reports an MMLU score, it very often ran through that harness.


So the lineage is short and traceable:


```text
TEST HARNESS          feed a program inputs, check its outputs
|               (software engineering, decades old)
v
EVALUATION HARNESS    feed a MODEL inputs, score its outputs
|               (lm-evaluation-harness, repo opened Aug 2020)
v
AGENT HARNESS         feed a model CONTEXT, run its ACTIONS,
check results, repeat  (2023 onward)


```


The shift is one word: the evaluation harness *scores* , the agent harness *ships* . Everything else, the wrapping, the standardisation, the fact that the wrapper is where the engineering effort goes, carried straight over.


---


## Prehistory: The Loop, the Toolbox, and the Check (1964 to 1973)


Every part of a modern harness existed in some form before anyone had heard of a language model. Three ancestors matter, and each contributed exactly one piece.


The first is the **read-eval-print loop** . Wikipedia's[REPL entry](https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop) traces the expression "READ-EVAL-PRINT cycle" to L. Peter Deutsch and Edmund Berkeley's **1964** Lisp implementation on the PDP-1. Read the input, evaluate it, print the result, wait for more. That is the shape of an agent turn, sixty-two years early, and it is why the featured photo on this article is a PDP-1 console typewriter.


The second is the **Unix toolbox** . Pipes shipped in Version 3 Unix in **1973** , implemented by Ken Thompson and long championed by Douglas McIlroy, per the[Wikipedia pipeline article](https://en.wikipedia.org/wiki/Pipeline_(Unix)) . The result was a philosophy: small programs that do one thing, a shared filesystem, and a way to chain them. Fifty years later Anthropic described the design principle behind its own harness as giving the model "a computer," specifically bash, file read, file write, file edit and search. That is McIlroy's toolbox handed to a different kind of user.


The third is the **test** . Verification, in software, has always been a separate program that runs your program and decides if it worked. Harnesses that skip this step produce agents that confidently finish nothing, a failure mode 2023 demonstrated at scale.


Ancestor Year What it contributed Where it lives in a harness today


Read-eval-print loop 1964 Repetition with state carried forward The agent loop that runs until done


Unix pipes and the shell 1973 Composable tools plus a shared filesystem The tool layer and the working environment


Test harness Long-standing practice Automated pass or fail on results Verification hooks, lint on edit, run the tests


For the wider arc these sit inside, see[The History of Computing](https://www.taskade.com/blog/history-of-computing) and[A History of Primitives](https://www.taskade.com/blog/history-of-primitives) .


---


## 2022: The Loop Gets Its Logic


The modern agent loop dates to **6 October 2022** , when Shunyu Yao and co-authors published[ReAct: Synergizing Reasoning and Acting in Language Models](https://arxiv.org/abs/2210.03629) . The paper's move was to stop treating thinking and doing as separate phases and interleave them: the model writes a reasoning step, takes an action, observes the result, then reasons again with that observation in hand. On ALFWorld it beat comparison methods by **34 percentage points** of absolute success rate, and on WebShop by 10 points, with only a handful of in-context examples.


That is the whole thesis of the harness, stated as a research result. Reasoning alone hallucinates because nothing corrects it. Acting alone flails because nothing plans. The loop between them is where competence lives, and the loop is the harness's job. The pattern is now standard enough to have its own dictionary entry: the[ReAct pattern](https://www.taskade.com/wiki/ai/react-pattern) and the[agent loop](https://www.taskade.com/wiki/ai-agents/agent-loop) .


Eleven days later, on **17 October 2022** , the LangChain repository was created (per the GitHub API), with the first public release following that month. It packaged the loop, the tool interface and prompt templating into something reusable, and it became the default vocabulary for the next two years. Its own story is worth reading separately:[The History of LangChain](https://www.taskade.com/blog/langchain-history) .


---


## 2023: The Year of Wrappers That Did Not Work


2023 was the year everyone built a harness and almost none of them held. The pattern repeated: a wrapper would decompose a goal, spawn steps, call tools, and then run in circles until someone killed the process.


The landmark was **AutoGPT** , whose repository was created on **16 March 2023** and which was released publicly at the end of that month by Toran Bruce Richards of Significant Gravitas. It gave a model a goal in plain language and let it write its own sub-tasks, browse, and write files. It is now at more than **185,000 GitHub stars** , one of the most starred repositories in the platform's history. It also, famously, could not reliably finish an errand.


Two ingredients arrived later that year that explain why. On **13 June 2023** OpenAI shipped[function calling](https://openai.com/index/function-calling-and-other-api-updates/) , letting developers describe functions to a model and get back a structured JSON object naming the function and its arguments. Before that, harnesses parsed tool calls out of free text with regular expressions, and a stray quotation mark broke the run. On **6 November 2023** OpenAI's[Assistants API](https://openai.com/index/new-models-and-developer-products-announced-at-devday/) put threads, code execution and retrieval behind a hosted endpoint. It was the first time a lab sold the harness itself rather than only the model.


2023 experiment What it added What was still missing


AutoGPT Autonomous goal decomposition, self-written sub-tasks Verification, cost bounds, a stop condition


Chain frameworks Reusable loop, tool abstraction, prompt templates An opinionated environment and a permission model


Function calling Machine-readable tool calls Nothing to check whether the call helped


Hosted assistants Managed threads and code execution Access to *your* files, repo and running system


The common gap is the same one the REPL and the shell already understood. A loop with no test is not a loop, it is a spiral. Modern harnesses fix it with verification hooks, turn budgets, and a human checkpoint, and the practice now has a name of its own:[agentic error recovery](https://www.taskade.com/blog/ai-agent-error-recovery) and[human in the loop](https://www.taskade.com/wiki/ai-agents/human-in-the-loop) .


---


## 2024: The Harness Gets Measured


2024 is the year the harness stopped being folklore and became a measurable engineering surface, and the numbers are unusually clean.


In March, Cognition launched **Devin** . Its own[technical report](https://cognition.com/blog/swe-bench-technical-report) , dated 15 March 2024, states that Devin resolved **79 of 570 issues, a 13.86 percent success rate** , on a randomly chosen 25 percent subset of the SWE-bench test set, against a previous best unassisted baseline of **1.96 percent** . Whatever you make of the subset, the product framing was the news: not a chat box but an environment with a shell, an editor and a browser that persisted across a task.


Two months later came the paper that named the thing.[SWE-agent](https://arxiv.org/abs/2405.15793) , submitted 6 May 2024 by John Yang, Carlos Jimenez, Alexander Wettig, Kilian Lieret, Shunyu Yao, Karthik Narasimhan and Ofir Press, introduced the **agent-computer interface (ACI)** and stated the underlying idea plainly: "LM agents represent a new category of end users with their own needs and abilities, and would benefit from specially-built interfaces to the software they use." The result was **12.5 percent pass@1 on SWE-bench against a previous best of 3.8 percent with retrieval-augmented generation** , plus 87.7 percent on HumanEvalFix.


Read that again with the model held constant. Roughly a threefold improvement, produced by redesigning what the model could see and touch. This is the single most quotable fact in the whole history of the harness, and it is why the discipline of[agent experience](https://www.taskade.com/wiki/ai-agents/agent-experience) now exists as a design practice separate from user experience.


The rest of 2024 filled in the environment and the plumbing:


- **22 October 2024** : Anthropic shipped[computer use](https://www.anthropic.com/news/3-5-models-and-computer-use) , letting a model look at a screen, move a cursor and type. On the OSWorld benchmark it scored **14.9 percent** screenshot-only and **22.0 percent** with more steps, against 7.8 percent for the next best system. The harness stopped needing a purpose-built API for every destination. See[computer use agents](https://www.taskade.com/wiki/ai/computer-use-agents) .
- **25 November 2024** : the[Model Context Protocol](https://www.anthropic.com/news/model-context-protocol) gave every harness one way to talk to every tool, replacing the custom implementation each data source used to demand. Background:[our MCP explainer](https://www.taskade.com/blog/mcp) and the wiki entry on the[Model Context Protocol](https://www.taskade.com/wiki/ai/model-context-protocol) .
- **19 December 2024** : Anthropic's[Building Effective AI Agents](https://www.anthropic.com/engineering/building-effective-agents) told builders to "carefully craft your agent-computer interface (ACI) through thorough tool documentation and testing," and disclosed that on its own SWE-bench agent the team "spent more time optimizing our tools than the overall prompt."


More time on the tools than on the prompt. That sentence is the moment prompt engineering stopped being the frontier and[context engineering](https://www.taskade.com/wiki/ai/context-engineering) took over.


---


## 2025: The Harness Becomes the Product


By 2025 the harness was no longer a research artifact or an open-source hobby. It was the thing labs shipped, priced and competed on.


**24 February 2025** : Anthropic released[Claude Code](https://www.anthropic.com/news/claude-3-7-sonnet) as a limited research preview alongside Claude 3.7 Sonnet, describing an agent that can "search and read code, edit files, write and run tests, commit and push code to GitHub, and use command line tools, keeping you in the loop at every step." That last clause is the permission model, stated as a feature. Full story:[The History of Claude Code](https://www.taskade.com/blog/claude-code-history) .


**16 April 2025** : OpenAI shipped Codex CLI, an open-source terminal agent, making harness design a public, forkable artifact rather than a trade secret. See[The History of OpenAI Codex](https://www.taskade.com/blog/openai-codex-history) and, for the editor lineage,[The History of Cursor](https://www.taskade.com/blog/anysphere-cursor-history) .


**29 September 2025** was the densest day in the harness's history. Anthropic renamed the Claude Code SDK to the **Claude Agent SDK** , on the reasoning that the same harness builds agents for far more than code, and published[Effective Context Engineering for AI Agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) . That piece defined context engineering as "curating and maintaining the optimal set of tokens (information) during LLM inference," and named the three techniques that let an agent survive a long job:


The same guidance explained *why* those techniques are needed: models suffer **context rot** , a measurable decline in accuracy as token counts climb, described as "a performance gradient rather than a hard cliff." Dictionary entries for all three:[context compaction](https://www.taskade.com/wiki/ai-agents/context-compaction) ,[subagents](https://www.taskade.com/wiki/ai-agents/subagents) ,[context rot](https://www.taskade.com/wiki/ai/context-rot) .


**16 October 2025** : Anthropic announced[Agent Skills](https://claude.com/blog/skills) , "folders that include instructions, scripts, and resources that Claude can load when needed," portable across apps, the API and the terminal agent. Capability became a file you drop in rather than code you write. Rakuten reported that "what once took a day, we can now accomplish in an hour." See[agent skills](https://www.taskade.com/wiki/ai-agents/agent-skills) .


**9 December 2025** : the Linux Foundation[announced the Agentic AI Foundation](https://www.linuxfoundation.org/press/linux-foundation-announces-the-formation-of-the-agentic-ai-foundation) , anchored by three donated projects: Anthropic's Model Context Protocol, Block's goose, and OpenAI's[AGENTS.md](https://agents.md/) . Harness plumbing stopped belonging to any one vendor. AGENTS.md, an open instruction file now used by over 60,000 open-source projects, is the plainest example of the trend: the harness reads a text file in your repo to learn how your project works. See[AGENTS.md](https://www.taskade.com/wiki/ai-agents/agents-md) .


---


## The Complete Timeline of Harness Capabilities


Here is the definitive version, one row per capability, with the date each one entered the harness.


Year What shipped Capability it added to the harness Why it mattered


**1964** Lisp on the PDP-1 (Deutsch and Berkeley) The read, evaluate, print loop First machine cycle that acted then waited for more. The shape of an agent turn


**1973** Unix pipes, Version 3 Unix Composable tools plus a shared filesystem Small programs became a toolbox. Harnesses hand a model that same toolbox


**Aug 2020** lm-evaluation-harness repository opens The word "harness" applied to language models Made "the code around the model" a named, shareable artifact


**Oct 2022** ReAct paper Interleaved reasoning and acting Gave the loop its logic. Plus 34 points absolute on ALFWorld


**Oct 2022** LangChain repository created A reusable framework for chains and tools Turned one-off scripts into a library others could build on


**Mar 2023** AutoGPT Autonomous goal decomposition Proved a model can self-direct, and that without checks it loops forever


**Jun 2023** OpenAI function calling Structured, machine-readable tool calls Removed the parsing guesswork from every tool invocation


**Nov 2023** OpenAI Assistants API Hosted threads, code execution, retrieval First time a lab sold the harness, not just the model


**Mar 2024** Devin (Cognition) A persistent environment: shell, editor, browser Reframed the product as an environment rather than a chat box


**May 2024** SWE-agent The agent-computer interface, named and measured 3.8% to 12.5% on SWE-bench from interface design alone


**Oct 2024** Computer use (Claude 3.5 Sonnet) Screen, cursor and keyboard as tools The harness stopped needing a bespoke API for every destination


**Nov 2024** Model Context Protocol One protocol for connecting tools Any harness could speak to any tool without custom glue


**Dec 2024** Building Effective AI Agents The working definition plus ACI discipline "LLMs using tools based on environmental feedback in a loop"


**Feb 2025** Claude Code research preview Terminal-native file edit, test run, commit, with checkpoints Put a full harness in front of ordinary developers


**Apr 2025** OpenAI Codex CLI An open-source terminal harness Harness design became public and forkable


**Sep 2025** Claude Agent SDK and context engineering guidance Compaction, sub-agents, structured note-taking Named the long-horizon problem and the three fixes for it


**Oct 2025** Agent Skills Loadable folders of instructions and scripts Capability became a file you drop in, not code you write


**Dec 2025** Agentic AI Foundation (MCP, goose, AGENTS.md) Neutral governance of harness standards Harness plumbing stopped being any single vendor's


**2026** Harness engineering as a named practice The scaffolding treated as the tuned artifact Teams now tune harnesses the way they used to tune prompts


---


## Why the Same Model Behaves Differently in Different Products


Because you are not using the model, you are using the harness. This is now measurable rather than anecdotal:[Terminal-Bench](https://www.tbench.ai/) , a Stanford and Laude Institute collaboration currently on version 2.1, describes itself as a set of benchmarks "to help agent makers quantify their agents' terminal mastery," and its leaderboard scores **agent and model pairs** , not models on their own. That design choice is itself the finding.


The verified anchor remains SWE-agent's 3.8 percent to 12.5 percent. Beyond that, industry write-ups such as Addy Osmani's[Agent Harness Engineering](https://addyosmani.com/blog/agent-harness-engineering/) (19 April 2026) report double-digit swings for a single model across harnesses, including a team moving from roughly top 30 to top 5 on Terminal-Bench by changing harness configuration alone. Treat those specific deltas as reported rather than independently confirmed, but the direction is not in dispute.


Five harness decisions produce most of the difference a person feels:


```text
DECISION                     CHEAP HARNESS            GOOD HARNESS
---------------------------  -----------------------  ----------------------------
What the model sees          Everything, every turn   Curated, compacted, relevant
Which tools exist            One generic "run code"   Purpose-built, documented
How many turns are allowed   Unlimited or one         Budgeted, with a stop rule
Whether results get checked  No                       Lint, test, re-read, retry
When it asks permission      Never, or constantly     Bounded autonomy, clear edges


```


Every row is a choice made by an engineer, not by the model. This is also why[non-determinism](https://www.taskade.com/wiki/ai/non-determinism) frustrates teams: the same prompt through two harnesses is not the same experiment. For how to measure any of it, see[agent evals explained](https://www.taskade.com/blog/agent-evals-explained) and[agent observability](https://www.taskade.com/wiki/ai-agents/agent-observability) .


---


## The Six Parts of a Modern Harness


A 2026 harness has six parts. Miss any one and the agent fails in a predictable way.


Part What it decides Failure mode when it is weak Read more


Context assembly What the model sees each turn The agent forgets its own decisions mid-task[Context engineering](https://www.taskade.com/wiki/ai/context-engineering)


The loop When to call again, when to stop Runs forever, or stops one step early[Agent loop](https://www.taskade.com/wiki/ai-agents/agent-loop)


Tool layer Which actions are possible and how they are described Right idea, wrong call, silently wrong result[Tool use](https://www.taskade.com/wiki/ai/tool-use)


Environment What the tools actually touch Works in the demo, fails on a real repo[Agent environment](https://www.taskade.com/wiki/ai-agents/agent-environment)


Verification Whether a step counts as done Confident output, nothing actually finished[Agent evaluation](https://www.taskade.com/wiki/ai-agents/agent-evaluation)


Permissions What runs without asking Either constant interruptions or an unpleasant surprise[Agent permissions](https://www.taskade.com/wiki/ai-agents/agent-permissions)


Two more pieces sit alongside these and are worth their own entries: the[agent session](https://www.taskade.com/wiki/ai-agents/agent-session) , which is the unit of state a harness carries, and the[agent sandbox](https://www.taskade.com/wiki/ai-agents/agent-sandbox) , which is how the environment is bounded so a mistake stays cheap. In Taskade, that boundary is visible and configurable: see[the agent bash sandbox](https://www.taskade.com/learn/agents/bash-sandbox) .


---


## What the Harness Still Gets Wrong in 2026


The open problems are no longer about capability. They are about durability, cost and trust, and all four have known partial fixes rather than solutions.


Open problem What goes wrong Current partial fix


Context rot Accuracy declines as the session grows long Compaction, sub-agents, external notes


Permission fatigue Users approve everything without reading Bounded autonomy per tool class, not per call


Verification gaps Nothing checks work outside of code, where tests exist Structured outputs, second-pass review, human checkpoints


Harness lock-in Prompts, skills and tool definitions do not port Open formats: MCP, AGENTS.md, portable skill folders


The first of these is structural. Longer context windows help, but the[context window](https://www.taskade.com/wiki/ai/context-window) is not the binding constraint, attention is. That is why the winning designs in 2025 were all about *removing* tokens from the working set rather than adding room for more. The fourth is the reason the December 2025 governance move matters more than it looked at the time.


---


## The No-Code Turn: Who Gets a Harness Now


Assembling a harness by hand is real engineering: the loop, the tool schemas, the sandbox, the verification, the permission model, the observability. **Taskade Genesis** supplies all of it as workspace features, so the people who need agents most are not gated behind that build.


The mapping is direct. **Workspace DNA** is the memory layer, so an agent starts a task already knowing your projects, past work and decisions instead of a blank slate. **Agent tools** are the action layer, covering web search, file analysis, image generation, custom commands and connected apps. **Automations** are the loop, running on triggers and schedules with[100+ bidirectional integrations](https://www.taskade.com/integrations) , where triggers pull events in and actions push data out. The workspace itself supplies the environment: your projects rendered across **7 project views** (List, Board, Calendar, Table, Mind Map, Gantt and Org Chart), all editable by both you and your agents. **Taskade EVE** orchestrates the whole thing across **15+ frontier models from OpenAI, Anthropic, Google, and open-weight providers** , so the model is a setting rather than a rebuild.


Workspace DNA follows the same three-part shape the history above arrives at: **Memory + Intelligence + Execution** . Memory is what the harness carries between turns. Intelligence is the model call. Execution is the tool layer touching a real environment. Sixty years of software converged on that triple, and it is now something you configure rather than construct.


Start at[/create](https://www.taskade.com/create) by describing the outcome in plain language, browse working examples in the[Community Gallery](https://www.taskade.com/community) , or read how the pieces fit in[agent tools](https://www.taskade.com/learn/agents/agent-tools) ,[Workspace DNA](https://www.taskade.com/learn/genesis/workspace-dna) and[automation execution](https://www.taskade.com/learn/automation/automations-execution) . Taskade is free to start, and paid plans begin at **Pro $10/mo billed annually** . Full detail on[pricing](https://www.taskade.com/pricing) .


---


## Frequently Asked Questions


What is an agent harness?


An agent harness is all the software around a language model that turns it into a working agent: the loop that keeps calling the model, the tools it can run, the environment those tools touch, the checks on the results, and the permission rules that decide what is allowed. The model predicts text. The harness makes that prediction land in the real world as finished work.


Where does the term agent harness come from?


From two older ideas. A test harness feeds inputs to a program and checks its outputs. An evaluation harness does the same for models, and EleutherAI's Language Model Evaluation Harness, repository opened 28 August 2020 with a first tagged release on 1 September 2021, made the word standard in machine learning. When the wrapper started running agents in production rather than only scoring them, the community kept the word.


What is the difference between a model and an agent harness?


The model is a fixed function: text in, text out, no memory between calls, no ability to run anything, no way to check itself. The harness supplies all three. Swap the harness and the same model behaves like a different product, which is why benchmarks such as Terminal-Bench score the agent and the model together.


How much does the harness actually change performance?


In the SWE-agent paper of May 2024, a purpose-built agent-computer interface raised the resolution rate on SWE-bench from **3.8 percent to 12.5 percent** with no change to the model. The gain came entirely from redesigning how the model saw and edited files. Later leaderboards report double-digit swings for one model across different harnesses.


When did the first agent harness appear?


Three dates matter. The read-eval-print loop appeared in a 1964 Lisp implementation on the PDP-1. Unix pipes shipped in 1973 and gave programs a shared toolbox and filesystem. The recognisably modern loop arrived with the ReAct paper on 6 October 2022, which interleaved reasoning with tool actions.


What is an agent-computer interface?


An agent-computer interface, or ACI, is the set of tools and views a harness gives a model, designed for the model rather than a human. The SWE-agent team argued that language model agents are a new category of end user, so they built a file viewer, an editor that lints automatically, and repository search sized for a model's attention rather than a person's screen.


What are the main parts of a modern agent harness?


Six: context assembly, the tool layer, the environment those tools touch, the loop, verification, and permissions. Weakness in any one produces a specific failure. Weak context assembly means the agent forgets its own decisions. Weak verification means confident output and nothing actually finished.


What is context compaction and why do harnesses need it?


Compaction is summarising a long working session so the agent can continue after filling its context window. Anthropic documented it in September 2025 alongside sub-agents and structured note-taking. Harnesses need it because accuracy declines as token counts climb, an effect the same guidance calls **context rot** , described as a performance gradient rather than a hard cliff.


Are AGENTS.md and Agent Skills part of the harness?


Yes. Both load behaviour into a harness as plain files instead of code. AGENTS.md is an open instruction file used by over 60,000 open-source projects and now governed by the Agentic AI Foundation. Agent Skills, announced 16 October 2025, package instructions, scripts and resources into folders a harness loads only when relevant.


Do I need to write code to get an agent harness?


No. Building one by hand means writing the loop, the tool schemas, the sandbox and the verification yourself. Taskade Genesis supplies the whole harness as workspace features: Workspace DNA is the memory, agent tools are the action layer, automations run the loop, and role-based permissions bound the agent. Describe the outcome in plain language at[/create](https://www.taskade.com/create) .


Why do two products using the same AI model behave so differently?


Because you are using the harness, not the model. Two products can call the identical model and differ in what context they assemble, which tools they expose, how many turns they allow, whether they verify results, and when they stop to ask. Those five choices produce most of the difference you feel.


What is harness engineering?


Harness engineering is the 2026 practice of treating the scaffolding around a model as the artifact to tune, the way teams once tuned prompts. It covers system instructions, tool definitions, sandbox setup, sub-agent orchestration, verification hooks and observability. The working slogan is that a decent model in a great harness beats a great model in a poor one.


---


## The Sixty-Year Answer


The harness took sixty-two years to assemble and nobody planned it. Deutsch and Berkeley wanted a Lisp that talked back. Thompson wanted programs he could chain. EleutherAI wanted comparable benchmark scores. Yao wanted reasoning that could check itself against the world. Each contributed one piece, and by 2024 a Princeton paper could hold the model fixed, rebuild the interface, and triple the score.


That is the lesson worth keeping. **The intelligence was never the bottleneck. The connection to reality was.** Everything the industry now calls agentic engineering, context engineering, harness engineering, is a different name for the same work: building the software that lets a model touch something real, then checking that it did.


At Taskade the harness is the workspace. Your projects are the environment. Your[agents](https://www.taskade.com/agents) are the loop. Your[automations](https://www.taskade.com/automate) are the execution. Memory, intelligence and execution, arranged so you describe the outcome and the scaffolding assembles itself.


**One prompt. One workspace. A harness you did not have to build.**


[Try Taskade Genesis free →](https://www.taskade.com/create)


▲ ■ ●


---


## Related Reading


### The harness, from every angle


- [What Is an AI Agent Harness?](https://www.taskade.com/blog/agent-harness-explained) : the concept explained from scratch, the companion to this history
- [Agent Harness](https://www.taskade.com/wiki/ai-agents/agent-harness) : the short dictionary definition
- [What Is Agentic Engineering?](https://www.taskade.com/blog/what-is-agentic-engineering) : the discipline that grew around the harness
- [The AI Agent Stack](https://www.taskade.com/blog/ai-agent-stack) : every layer, from model to product
- [Agentic Design Patterns](https://www.taskade.com/blog/agentic-design-patterns) : the shapes harnesses keep converging on
- [AI Agent Tools](https://www.taskade.com/blog/ai-agent-tools) : the action layer in practice
- [Types of Memory in AI Agents](https://www.taskade.com/blog/ai-agent-memory) : what survives between turns


### The histories that feed this one


- [The History of Claude Code](https://www.taskade.com/blog/claude-code-history) : the terminal harness that made the category mainstream
- [The History of OpenAI Codex](https://www.taskade.com/blog/openai-codex-history) : the open-source counterweight
- [The History of Cursor](https://www.taskade.com/blog/anysphere-cursor-history) : the editor as harness
- [The History of LangChain](https://www.taskade.com/blog/langchain-history) : the framework that named the parts
- [The History of Anthropic and Claude](https://www.taskade.com/blog/anthropic-claude-history)
- [The History of ChatGPT](https://www.taskade.com/blog/openai-chatgpt-history)
- [The History of Workflow Automation](https://www.taskade.com/blog/automation-history) : triggers and actions before the model arrived
- [The History of Computing](https://www.taskade.com/blog/history-of-computing)


### Build with one


- [Taskade Genesis](https://www.taskade.com/create) : describe the outcome, get the working app
- [AI Agents](https://www.taskade.com/agents) : agents with tools, memory and multi-model routing
- [Automations](https://www.taskade.com/automate) : the loop, on triggers and schedules
- [Integrations](https://www.taskade.com/integrations) : 100+ bidirectional connections
- [Community Gallery](https://www.taskade.com/community) : clone a working agent app in one click
