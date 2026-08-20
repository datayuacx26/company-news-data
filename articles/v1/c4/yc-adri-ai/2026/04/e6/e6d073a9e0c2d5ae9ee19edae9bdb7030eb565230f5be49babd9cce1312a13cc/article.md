---
schema_version: "1.0.0"
document_id: "e6d073a9e0c2d5ae9ee19edae9bdb7030eb565230f5be49babd9cce1312a13cc"
company_key: "yc-adri-ai"
company: "Adri AI"
source_id: "yc-adri-ai-news-import-4d4ea4e66e6f"
canonical_url: "https://docs.getadri.ai/blog/genai/simple-prompting-context-instrumentation"
published_at: "2026-04-19T00:00:00+00:00"
first_seen_at: "2026-07-21T04:52:15.508780+00:00"
fetched_at: "2026-07-28T21:25:38.770750+00:00"
content_hash: "sha256:548c1b2ff1826936028e443f7d8e6b56617bc6794bc50ff9e7e80d01d441711a"
---

# The Return of Simple Prompting in SAP

For the past few years, AI adoption in enterprises has been driven by one idea: write better prompts. In SAP environments, that phase is ending. We are entering a new paradigm: context instrumentation, where systems are designed so agents can build context and take action autonomously.


And interestingly, users are returning to simple prompting.


## The evolution​


### 1. Simple prompting​


At the beginning, users asked things like:


- "Write an ABAP report"
- "Explain this dump"


Results were inconsistent because the model had no awareness of:


- your SAP system
- your configurations
- your custom logic


### 2. Prompt engineering​


Teams improved how they asked: better structure, clearer constraints, and examples. This helped, but only up to a limit.


The model still lacked real context.


### 3. Context engineering​


Teams then started injecting:


- documentation
- schemas
- business rules


Now the model had context, but it was usually:


- static
- manually curated
- incomplete


### 4. Context instrumentation​


Instead of feeding all context manually, we enable the agent to build context itself.


In SAP, this means the agent can:


- read and write ABAP
- query tables
- analyze runtime logs and dumps
- trace execution paths
- generate artifacts (for example forms)
- execute controlled changes


## What this looks like​


### Example: invoice failure​


Traditional approach:


- use ST22 for dump analysis
- jump into SE80
- trace logic manually


With an agent, a user can ask:


> "Why did this invoice fail to post?"


The agent can then retrieve logs, trace execution, inspect dependencies, map to the billing process, and explain root cause.


## The hard problem: meaning​


SAP systems are complex: custom Z programs, implicit logic, and configuration-driven behavior. So the real challenge is not only "Can the agent act?" but also "Can the agent act correctly?"


## Our breakthrough: code-to-process mapping​


A key layer is mapping every code artifact and configuration to a business process.


When the agent encounters code, it:


1. reads the source
2. analyzes dependencies
3. traverses execution paths
4. maps to business processes such as Order-to-Cash, Procure-to-Pay, and Record-to-Report
5. generates living documentation


## What this unlocks​


When a user asks "Why did this invoice fail?", the agent can reason with process context, system logic, and expected behavior.


This is no longer basic debugging. This is business-aware reasoning.


## The new roles​


- Enterprise Architect: designs the capability surface
- Agent: builds context and executes
- SAP system: becomes instrumented
- User: returns to simple prompting


## Final thought​


The shift is clear:


Prompt Engineering -> Context Engineering -> Context Instrumentation.


The core question is no longer "How do we write better prompts?" It becomes:


> How do we make SAP systems understandable and operable by agents?
