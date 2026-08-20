---
schema_version: "1.0.0"
document_id: "a74e93935a09e4b64333d576a69839e7a0beabca89f9e6240ed89c8b7dbfd2b9"
company_key: "yc-cloudcruise"
company: "CloudCruise"
source_id: "yc-cloudcruise-news-import-0286802dbbee"
canonical_url: "https://cloudcruise.com/blog/a-better-way"
published_at: "2025-02-05T00:00:00+00:00"
first_seen_at: "2026-07-23T05:51:05.060428+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:5a9c2b6c14d41e0133a6d731b9f8f22c8232c49ca93b6a1f16e9f9607037a1aa"
---

# A Better Way to Build Browser Agents | CloudCruise

Everyone’s talking about browser agents. There’s been a wave of demos showing language models clicking around on websites, taking multi-step actions, supposedly reasoning their way through a task.


But once you look past the video, most of these systems fall apart on even basic reliability tests. They're slow, expensive, and impossible to debug. And it’s not surprising – these agents don’t actually “understand” the task or have any reusable logic. They rely on recursively prompting a language model over and over again at runtime, hoping it figures out the right thing to do at each step.


This approach doesn’t scale. Not in cost, not in performance, not in maintenance.


We’re building CloudCruise because we think there’s a better way. It starts by framing computer automation as a code generation and maintenance problem – not as a multi-step reasoning problem.


#### A Domain-Specific Language for Automation


At the core of our system is a domain-specific language (DSL) for automation. Instead of generating raw Python or JavaScript, we generate a structured representation of the workflow – a directed graph of nodes and edges.


Each **node** in the graph represents an atomic or high-level action:


-


UI actions like click and input_text


-


Control flow like if and loop


-


Data operations like extractText or uploadFile


**Edges** define the execution order and conditional flow between nodes.


This abstraction does two things:


1.


It allows us to separate “what” the workflow should do from “how” it’s implemented in code.


2.


It gives us a structure that’s easy to generate, validate, and maintain over time.


The DSL is declarative enough to express complex workflows – like looping through a table, clicking into each row, downloading a file, extracting metadata, and conditionally branching depending on what’s found – but constrained enough that LLMs can output it reliably.


We’ve designed it so the builder agent never has to write low-level code. It only has to populate the graph.


#### Agent Architecture


There are two main agents in the system:


###### 1. Builder Agent


This agent is responsible for generating the first version of the automation. It interacts with the user (or another AI) and translates the high-level intent into a valid DSL graph.


It operates more like a compiler frontend – it doesn’t run code, it generates a clean, structured plan.


The graph includes enough metadata to allow downstream rendering into real executable code, with variables, retries, browser hooks, etc. But the LLM never touches that part. We have strict separation between generation and execution.


###### 2. Maintenance Agent


This agent is only invoked when something goes wrong during execution. It observes the failure context, classifies the issue, and chooses one of several strategies:


-


If it’s a missing element or selector change, it can attempt to regenerate that node


-


If it’s an authentication failure, it might prompt the user to re-authenticate


-


If the workflow logic itself needs adjustment, it might suggest a structural update


-


If it's transient, it can simply requeue the job with retries


All of this is based on a formal error taxonomy. Not everything goes through an LLM again – sometimes it’s just deterministic routing logic based on error codes, logs, and historical outcomes.


The goal isn’t to blindly “retry until it works.” It’s to treat failures as structured events that can be observed, learned from, and addressed in a maintainable way.


#### Why We Generate Code, Not Run Prompts


Generated workflows are deterministic. You can lint them, diff them, version control them, and inspect exactly what they do.


That’s not possible with runtime LLM reasoning, where every execution path is probabilistic. You can’t build a test suite for it. You can’t do CI. And when it breaks, you’re stuck scrolling through prompt logs.


With our approach:


-


Latency is much lower (you run native code, not inference)


-


Costs are predictable and amortized


-


Debugging is possible


-


Recovery is systematic


We still use LLMs – but only where they’re helpful. They are not the runtime engine. It’s traditional software and LLMs working together in an agentic system.


#### Where This is Going


We’re not building just a better automation framework. It’s infrastructure for agentic systems to map and automate any workflow on a computer.


Our builder and maintenance agents handle generation, execution, and repair – interacting with users or other agents to clarify intent, adapt to failures, and maintain workflows over time. Other agents can hand off high-level tasks like “download the patient report” or “reconcile this invoice,” and the CloudCruise system will generate the appropriate graph, ask follow-up questions, and execute the workflow.


We’re building for a future where LLMs aren’t isolated reasoning engines – they’re components in distributed agentic systems. These agents collaborate by interacting through well-defined tool interfaces, not by guessing each other’s intent.
