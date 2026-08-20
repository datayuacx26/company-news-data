---
schema_version: "1.0.0"
document_id: "e1d42ba34959d5ded4608e64a9bcdded2954dbe84f045afc883288e96af7206b"
company_key: "yc-driver"
company: "Driver"
source_id: "yc-driver-news-import-217796f58c6f"
canonical_url: "https://www.driver.ai/blog/context-engineering-how-driver-solves-it/"
published_at: null
first_seen_at: "2026-07-21T17:03:14.637812+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:92776aaf3eade5944730ed46ce5e8ecfe338fb9006492fad157d390d2a570ebd"
---

# Context Engineering: Why It is Hard and How Driver is Solving It

# Context Engineering: Why It is Hard and How Driver is Solving It


What is context engineering? Learn the three failure modes that break AI coding agents and how Driver's compiler-inspired architecture solves them.


Jan 22, 2026 — 12 min read


Daniel


Co-founder


Share on XShare on LinkedInShare on Facebook


“Context engineering” is all the buzz right now. But what exactly is it? And what is hard about it? In the first half of this article I’ll share thoughts on these questions, focusing on software source code as context for coding agents. In the second half, I’ll talk about how Driver is built to solve the problems in context engineering.


## What is Context Engineering?


If you have a task where source code unknown to the agent is critical to solving the task, then you have a context engineering problem.


For example: you need to refactor an algorithm split across many files and folders in a proprietary codebase. Work must be done to properly collect and structure the relevant source code content either by the agent itself, using available tools, or presented to the agent by the user when invoked. This is context engineering in its simplest form. More complex definitions will include topics such as management of agent memory implementations and broader tool use, etc.


Compare with: “look at this function right here and update it to combine X and Y values with Z algorithm.” From a context perspective, this is specified and constrained.


Often the most interesting, complex, and high value work is of the former nature, but we often waste significant time steering AI, or AI simply completely fails in these scenarios due to the lack of effective context engineering. Users lose trust and a lot of the potential from today’s powerful models is left untapped in the highest value applications. These phenomena become dramatically more true as the codebase size and complexity grows.


How do agentic systems fail at context engineering?


- The “whiffing” problem.
- The “task-dependent SNR” problem.
- The “shape and abstraction level” problem.


## The “Whiffing” Problem


By this I mean an agentic system fails to find some of the critical source information needed to solve a task. The absence of this context can lead to catastrophic and arbitrary failures, including complete hallucination, irrelevant answers, or partial answers.


It can be useful to consider a continuum between “fully exhaustive” problems on one pole and “needle-in-haystack” problems on the other. An example of the former: “build a single architecture diagram for this huge codebase” and an example of the latter: “update the relevant class somewhere in the bowels of this codebase to do X.”


For exhaustiveness problems, the biggest risk is failing to be completely exhaustive, and most on-the-fly solutions of today are particularly ill-suited for this. For needle-in-haystack problems, the biggest risks are failing to find the needle but also including significant amounts of irrelevant context.


Many real-life tasks lie between these two poles, and failures grow non-linearly with codebase size. A recurring theme is that the nature of the task itself has a huge impact on the nature of the context engineering problem, and by extension, the nature of the optimal solution.


## The “Task-Dependent SNR” Problem


Context engineering isn’t just as simple as ensuring you get all of the relevant material in. You also need to keep irrelevant or incorrect material out.


In signal processing, we know the signal-to-noise ratio (SNR) of true signal to noise (or interference) is critical to performance. With context engineering, the task at hand determines what the signal is, and it is a highly variable and moving target. The same seemingly relevant context may be critical signal for one task and hallucination-inducing interference for another.


For example, explanatory vendor code in a top-level folder of a codebase can be very important for a task requiring an update to the main application along the lines of one of the examples. But for a task that depends only on the particular existing implementation in the mainline application, poorly contextualized inclusion of example vendor code can cause major hallucinations.


Even if irrelevant context is likely to be benign, it can dilute the true signal in the context window/memory implementation of the AI. You always want the highest SNR you can get!


## The “Shape and Abstraction Level” Problem


Optimal context engineering for agentic systems should take into account their iterative nature and ability to decompose tasks into smaller, more constrained pieces solved separately.


For coding agents specifically, we are seeing increased success with schema-driven paradigms and using significant upfront planning and design prior to implementing major coding solutions. This means the context engineering problem itself can and should be decomposable to match.


Consider the task of updating an entire backend REST API implementation. The best context at first is likely high level and total in terms of what the REST API is for and where throughout the codebase it is implemented. Any further details may obscure or overly bias the agent’s early planning. In subsequent steps focused on specific endpoints, we need all the gory details but for just the subsets in question.


The context optimal at different stages of solving a complex task changes. We need tools that give us and agents the ability to shape context to the needs of each stage, not merely to fetch it from the source and combine it in rudimentary ways. This includes deriving or pre-computing new content on top of the core source.


## The Outcome


Today, agents fail and sometimes spectacularly so due to a lack of effective context engineering solutions. These are acute with large proprietary codebases and failures rise nonlinearly with the size and complexity of a codebase as well as the complexity of tasks.


And even as agent systems and workflows get more and more capable, failures from an inability to get the right context can poison the entire process.


## Our Approach at Driver


We view context engineering as a fundamental information theory problem. As such, it is a first-class problem now and will be in the future even as models and agents advance. To solve it at scale requires systems that can deliver high SNR context for agents as they work on highly varied tasks in real time.


Like we learned to build signal conditioning and preprocessing systems in electronics, pre-computing context for non-trivial source code is important. Then we must provide interfaces for runtime agent systems to shape and augment this context in the right ways at the right time.


At a high level: Pre-computing and deriving information on top of source material ahead of time is critical. So we built an ahead-of-time compiler architecture that we call our transpiler.


With effectively no latency constraints, we are free to optimize content creation in the transpiler:


- by systematically breaking up the analysis of arbitrarily sized and shaped code into many granular, constrained, and highly structured units of processing.
- by building and optimizing different kinds and types of content over multiple passes, as in traditional compilers.
- to tailor content to solve expected downstream runtime (e.g., coding agent or human) tasks.


In the ideal scenario, we distill all of the complex and important context via the transpiler, such as that which requires exhaustive review and synthesis of the whole source. An example is a high quality architecture description of a huge codebase.


We then marry the powerful content of the transpiler to runtime agent systems via interfaces in that domain. Today those are MCP tools, skills, sub-agents, etc. Blending a static ahead-of-time compiler and runtime agentic systems allows us to solve many of the problems that plague purely runtime or naive ahead-of-time methods such as indexing and RAG.


## The Transpiler


Our core technology is the transpiler. It is built with a mix of traditional static analysis and parsing tools along with LLM-based content generation.


Many passes are made, as in a traditional compiler, wherein every line of code is touched and we progressively build intermediate representations (IRs) as we progress through the steps of the transpiler. Early stages are concerned with parsing syntax trees, creating symbol tables, and generating symbol-level explanatory content, while middle and later passes generate content at higher levels of abstraction and isolate different kinds of information.


The transpiler emits a complete articulation of an input codebase: from symbol-complete tech docs to documents describing aspects of the codebase as a whole. Examples of the latter include a number of always applicable “deep context” documents on architecture, onboarding to the codebase, and the history of development of the codebase.


We have built significant infrastructure around the transpiler to take in codebases of any size and shape, generate this context, and automatically maintain it up-to-date at all times. The latter is accomplished via integrations with all major version control system (VCS) providers and a transpiler flow for efficient updates based on code diffs associated with pushed commits and merged pull requests.


## Optimization of Pre-Computed Content


We continue to develop the later stages of the transpiler to develop content tailored for each codebase and specialized for what is needed in downstream applications.


For example, we can detect the nature of the code in various senses (e.g., a frontend web application vs. an embedded system algorithm) and then dispatch creation of deep context documents particularly suited for that kind of codebase, such as an exhaustive table of all endpoints for a codebase that implements an important REST API.


We also provide users the ability to tap into components of the transpiler to generate their own standalone documents or context that can plug into the always kept up-to-date context layer.


Finally, we want to pre-compute the best information up front, especially that which is complex and time-consuming to create. But we don’t need to, and obviously cannot, solve for the ideal context package for every possible downstream query or task in its particulars. But we can pre-compute a solid base so that an agentic system downstream can always use our context to quickly and effectively build that particular context, and then go on to solve the problem. This is the system-wide goal and requires us to think about how to jointly optimize what the transpiler does and what runtime agents are empowered to do.


## Use in and Synergies with Runtime Agent Systems


The final piece of the puzzle are the integrations for day-to-day use. Our primary targets today are engineers as well as managers and product folks working to plan and execute software development. This is increasingly in the form of software development life cycles (SDLCs) with AI significantly in the loop and rigorous schema-driven AI coding workflows.


A primary body of work here is building the right interfaces for agent systems to be able to take advantage of the pre-computed content. This has a few components:


**Context primitives:** A discrete set of tools provides the foundational interface to our context layer. Thoughtful design of the set of core tools is critical for success. Considerations include choosing the information exposed by each tool, the shape of the input and the output, and choosing some tools that provide immediate read access to pre-computed transpiler content and others that trigger on-the-fly generation of new content. Today, tools are often provided via the model context protocol (MCP) or as standard API endpoints.


**Intelligent orchestration:** This means making sure agents are aware of and choose to use the core tools in the right ways and at the right times. This is an area of rapid development currently, with many agent system-specific implementations. This includes how individual MCP tools and MCP services are documented and treated by agent systems on installation, building workflow information into global components such as CLAUDE.md and AGENTS.md, and new implementations of triggerable bundles of behavior such as in skills and sub-agents.


**Modular implementations:** We want to build all of this as a context interface layer independent of the particular agent or client systems that will use it. Interface standards, such as the MCP adopted by the majority of the agentic industry, are very important here.


**Context window optimization:** Individual agents today must manage a finite context window. Compaction processes, required when a context window is nearing complete usage, are a source of fear and frustration in longer running flows. There are many ways to alleviate this situation, and major advancements are incoming. But this is related to a core value in dense, high SNR context. Beyond the core value of better, more consistent, and more robust output, Driver’s pre-computed content can significantly reduce agent context window token usage by obviating on-the-fly discovery.


Today, our flagship coding agent MCP service is available, complete with 7 core tools, split between those that provide powerful “deep context” documents and those that provide more granular navigation tools. This can be used with a wide variety of agentic systems out of the box. We’re actively working on more advanced runtime implementations using skills, sub-agents, and generative MCP tools.


## The Results


Let’s revisit the three failure modes from before:


**The “whiffing” problem:** By providing holistic context (deep context docs) and granular navigational tools, agents are immediately onboarded to any codebase. For arbitrary tasks, they either immediately know what is relevant or know where and how to go investigate further. As a result, they don’t miss context and don’t require steering from human users.


**The “task-dependent SNR” problem:** Driver’s pre-computed content has pre-distilled information on important topics and provides the clarity agents need to quickly assemble the task-specific context they need.


**The “shape and abstraction level” problem:** Driver’s context layer has pre-computed context from the symbol-level to the codebase-wide level on various topics. This can be fetched and augmented at runtime according to their needs.


Customers today are getting great results using Driver’s context layer. By pre-solving much of the source context problem and providing effective interfaces for agents, Driver allows them to work faster and much more confidently. We’re working internally on benchmarks that articulate and quantify the benefits of Driver’s context layer, looking at both well-known existing agentic benchmarks but also custom benchmarks that are better focused on scope, complexity, and context challenges in software development.
