---
schema_version: "1.0.0"
document_id: "2020eacdad0ed40da48298a2adbc31fa75c5fdcdafbe86e8287fad81bd7bc31c"
company_key: "microsoft-corporation-common-stock"
company: "Microsoft Corporation"
source_id: "microsoft-corporation-common-stock-rss-0db2ae40c128"
canonical_url: "https://opensource.microsoft.com/blog/2026/05/14/conductor-deterministic-orchestration-for-multi-agent-ai-workflows/"
published_at: "2026-05-14T16:00:00+00:00"
first_seen_at: "2026-07-20T04:34:28.025355+00:00"
fetched_at: "2026-07-28T22:36:38.456755+00:00"
content_hash: "sha256:634f5b9e0d96fd4a91502b8c1090c9f87f0cf09a1f11fc0bf682deabd7d8e904"
---

# Conductor: Deterministic orchestration for multi-agent AI workflows

Multi-agent AI systems are becoming the default approach for complex tasks:


- Code review pipelines.
- Research-then-synthesize workflows.
- Plan-then-implement loops.


These aren’t single-prompt problems. They need multiple specialized agents coordinating in sequence, in parallel, and sometimes in cycles.


Most frameworks approach this by making the orchestrator itself an LLM—an agent that dynamically plans which agents to call, in what order, and with what inputs. That works when the task is exploratory. But for workflows with known structure (and in practice, many of the most useful workflows do have known structure), dynamic orchestration adds cost, latency, and unpredictability that can work against you.


[Conductor](https://github.com/microsoft/conductor) is an open-source CLI (MIT license, Microsoft org) that takes a different approach: you define your multi-agent workflows in YAML, and the routing between agents is deterministic. Jinja2 templates and expression evaluation handle conditions and branching. The orchestration layer consumes zero tokens. The structure is fixed at definition time—and that’s the point.


[Dig into Conductor today](https://github.com/microsoft/conductor)


## The problem with multi-agent workflows today


We kept building multi-agent workflows—code review pipelines, design document generation, research assistants—and writing the same glue code every time: Python scripts stitching prompt chains, ad hoc retries, manual state between steps, no good way to version-control the workflow itself.


We looked at other tools, such as Microsoft Agent Framework (MAF), Microsoft’s primary SDK for building agents in code, which covers many of the same primitives.[Conductor](https://github.com/microsoft/conductor) is a different surface for similar patterns: a YAML-first CLI for teams who want to compose agents and tools without writing SDK code. Declared, diffable, and as readable as a CI/CD pipeline.


We also wanted to separate concerns that keep getting mashed together in multi-agent systems:


- Orchestration should be deterministic and inspectable. Not an LLM making routing decisions.
- Execution should support multiple providers and models, so you can put a cheap model on triage and a capable one on reasoning.
- Context flow between agents should be explicit. No implicit conversation bleeding.
- Human oversight should be a built-in workflow step, not something you bolt on later.


[Conductor is the result](https://github.com/microsoft/conductor) : YAML workflows, isolated agents, and a routing graph you can see before anything runs.


## Key capabilities of Conductor


### YAML-defined workflows


Every Conductor workflow is a YAML file that declares agents, their prompts, models, inputs, outputs, and routing logic. Workflows are version-controlled, diffable, and reviewable, the same way you’d treat infrastructure-as-code or CI/CD pipelines.


` workflow:
name: design-review
entry_point: architect


agents:
- name: architect
model: claude-opus-4.6-1m
prompt: |
Create a design document for: {{ workflow.input.purpose }}
output:
file_path: { type: string }
routes:
- to: reviewer


- name: reviewer
model: claude-opus-4.7
prompt: |
Review the design at {{ architect.output.file_path }}
output:
score: { type: number }
approved: { type: boolean }
routes:
- to: $end
when: "{{ output.approved }}"
- to: architect`


## Deterministic routing, zero token overhead


Routing between agents uses Jinja2 templates and expression evaluation. First matching condition wins. A workflow can loop hundreds of times through an evaluator-optimizer cycle without the routing layer consuming any tokens. This is what separates Conductor from dynamic orchestration: the workflow topology is declared, not discovered at runtime.


## Mix providers and models per agent


[Conductor](https://github.com/microsoft/conductor) supports GitHub Copilot and Anthropic Claude as providers, with per-agent model overrides. You can mix them in a single workflow: run claude-haiku-4.5 for classification, gpt-5.2 for research with MCP tool access, and claude-opus-4.6-1m for complex reasoning. Each agent gets its own session with no shared conversation state.


### Parallel execution


Static parallel groups run multiple agents concurrently with configurable failure modes (fail_fast, continue_on_error, all_or_nothing). Dynamic for each groups process variable-length arrays in parallel with batched concurrency. Results are aggregated and available to downstream agents through template expressions.


` parallel:
- name: researchers
agents: \[academic, web, technical\]
failure_mode: continue_on_error
routes:
-to:synthesizer`


### Script steps


Not every step needs an LLM. Script steps run shell commands directly, capturing stdout, stderr, and exit codes into the workflow context. A code review workflow can run pytest between the “implement” and “review” steps. Routes can branch on exit codes. No model invocation, no token cost.


### Human gates


Human gate steps pause execution, present options in a Rich terminal UI or the web dashboard, and route based on the response. Approval workflows, review checkpoints, interactive decision points: they’re part of the workflow graph, defined the same way as any other step.


### Web dashboard


Conductor includes a web dashboard that visualizes execution in real time. An interactive DAG shows the workflow topology with animated edges for execution flow. Each node is clickable, showing the agent’s prompt, model, token usage, cost, activity stream, and output. Human gates work directly in the browser. Background mode (–web-bg) starts the dashboard, prints the URL, and returns control to the terminal.


### Context control


Three context modes control what each agent sees: accumulate (all prior outputs), last_only (just the previous step), and explicit (only named dependencies). The default is accumulated, but for larger workflows, explicit mode cuts token consumption significantly. Being deliberate about what each agent sees turned out to matter more than we expected.


### Plugins and workflow registries


Plugins follow the[Agent Skills](https://agentskills.io/) open standard, bundling reusable skills and MCP server configurations that agents can use. Reference them from Git repos or local paths. Workflow registries let teams share and version workflows: configure a registry once, then run workflows by short name.


### Safety limits


Max iteration limits and wall-clock timeouts prevent runaway execution. Dry-run mode previews the execution plan without calling any models.` conductor validate` catches schema errors, missing references, and unreachable agents before anything runs.


## Works with your existing tools


[Conductor](https://github.com/microsoft/conductor) doesn’t replace your editor, CI system, or agent framework. It’s a CLI that reads YAML, calls models, and produces structured output. It plugs into what you already have:


- MCP servers give agents tool access: web search, documentation lookup, code analysis, anything with a[Model Context Protocol](https://modelcontextprotocol.io/) server.
- Shell commands run directly as workflow steps, so your scripts, linters, test suites, and build tools participate without modification.
- Structured output with JSON schemas means downstream tools and scripts can consume results programmatically.
- A Claude skill ships in the repository. Point your coding agent at it and it can build workflows for you.


## What we learned


### 1. Determinism is a feature


The most common pushback is “what about dynamic orchestration?” Fair question. If your task needs to restructure itself based on what it discovers, let the LLM decide what comes next. But the workflows we keep reaching for (review loops, research pipelines, plan-then-implement) have known structure. We’d rather have predictability, cost control, and auditability than replanning flexibility. Conditional routing and loop-back patterns cover more ground than you’d expect.


### 2. Agent isolation pays for itself


Each agent gets its own session, system prompt, model, provider, and temperature. No shared conversation bleeding. This seems like overhead until you’re debugging a workflow where step 4 is mysteriously influenced by step 2’s output. Explicit context flow makes multi-agent systems tractable.


### 3. Events over logs


The engine uses a pub/sub event system for all output. The terminal renderer, web dashboard, and any future consumers subscribe independently. More work upfront than printing to stdout, but it decoupled the execution engine from the presentation layer in a way that keeps paying off. Adding the web dashboard required zero changes to the workflow engine.


## 4. YAML is the right level of abstraction


We considered Python APIs, JSON schemas, and visual builders. YAML hit the sweet spot: readable, structured, diffable in pull requests, and familiar to anyone who’s written a GitHub Actions workflow or a Kubernetes manifest.


## Open source and ready to use


MIT-licensed, developed in the open from day one.


- pytest test suite covering the engine, CLI, config validation, providers, and integration scenarios.
- Ruff for linting and formatting, ty for type checking, both enforced in CI.
- Runs on macOS, Linux, and Windows.
- One-line installers (curl | sh and irm | iex) with SHA-256 checksum verification.
- Self-update via conductor update.


Contributions welcome: provider integrations, workflow examples, plugins, docs, bug reports.


## How to start using Conductor today


**Install:**


*# macOS / Linux*
curl -sSfL https://aka.ms/conductor/install.sh | sh


*# Windows (PowerShell)*
irm https://aka.ms/conductor/install.ps1 | iex


**Run your first workflow:**


conductor run workflow.yaml –input question=”What is Python?”


**Visualize it:**


conductor run workflow.yaml –web –input topic=”AI in healthcare”


Conductor requires Python 3.12+ and works with GitHub Copilot or Anthropic Claude. The[repository](https://github.com/microsoft/conductor) has documentation, example workflows, and a getting-started guide.


Multi-agent workflows are becoming infrastructure: repeatable, versioned, shared across teams. We chose deterministic orchestration because for the workflows we build most often, known structure is the whole point.


## Start using Conductor


If you’re stitching together agent pipelines with glue code, give Conductor a look.


[Explore more](https://github.com/microsoft/conductor)


*Conductor is open source under the MIT license at*[github.com/microsoft/conductor](https://github.com/microsoft/conductor) *.*
