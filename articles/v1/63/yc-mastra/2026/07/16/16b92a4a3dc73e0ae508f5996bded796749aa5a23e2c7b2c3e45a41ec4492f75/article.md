---
schema_version: "1.0.0"
document_id: "16b92a4a3dc73e0ae508f5996bded796749aa5a23e2c7b2c3e45a41ec4492f75"
company_key: "yc-mastra"
company: "Mastra"
source_id: "yc-mastra-news-import-2c9844a44afc"
canonical_url: "https://mastra.ai/blog/software-factory"
published_at: "2026-07-16T00:00:00+00:00"
first_seen_at: "2026-07-24T10:56:34.426085+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:16c72e0bf923184deb01b6eef715b8b287f56d6610ef0104288e2ba45757b67b"
---

# How to Build an AI Software Factory with AI Agents in TypeScript

Your software engineers already run coding agents to fix bugs and build new features. Their job has reduced to triggering new agent sessions, making sure the agent has the right context, permissions, and guardrails, and that every check passes.


An AI software factory takes this further. It’s a system where an orchestrating agent coordinates specialized agents to find bugs, write fixes, validate changes, generate documentation, and monitor production, all autonomously.


Modern engineering teams in startups like StrongDM and Prisma are already running software factories in production.


This guide shows you how to build a software factory on[Mastra](https://mastra.ai/) , a TypeScript framework for AI agents and workflows.


## What Is a Software Factory?


A software factory is an automated development system where AI agents take real work from tools like GitHub, Sentry, CI, Slack, or Linear, turn it into structured tasks, and move each one through the same steps an engineering team already follows, escalating to a human only when a decision needs judgment.


Almost everything an engineering team spends time on falls into six categories:


1.


**Signal detection and triage.** Figuring out what needs to be built or fixed. Reading GitHub issues, Sentry errors, user feedback, and stale PRs, then deciding what is worth doing and in what order.


2.


**Code generation.** Taking a well-understood problem and producing a correct solution. Reading the codebase, matching its patterns, writing the change and its tests.


3.


**Validation.** Verifying the solution is correct, secure, performant, and maintainable. Review, test runs, security scans.


4.


**Release management.** Getting a validated change into production. Version bumps, changelogs, deploy, health checks, rollback when needed.


5.


**Documentation.** Keeping written artifacts in sync with what the code actually does. API references, runbooks, architecture notes, inline comments.


6.


**Production monitoring.** Watching for problems. Detecting anomalies, opening incidents, and routing what they find back into the development loop.


Coding agents made every individual engineer faster, but engineers still spend 50-70% of their time coordinating with each other: standups, handoffs, status updates, review queues. That coordination layer is what a software factory automates.


## From CI/CD to Agentic Software Development


Every process improvement of the last fifteen years automated one step of the software development lifecycle and left the handoffs to humans.


-


**CI/CD (2010s)** automated "does it build, does it pass tests." It left open whether the right thing was built.


-


**Deploy previews and parallel CI** cut the wait from forty minutes to five. Faster previews did not make reviews more thorough.


-


**Feature flags and canary deploys** decoupled deploy from release. A human still watched the dashboard to decide when to advance.


-


**PR-based review** made review asynchronous and durable. It also produced review fatigue, where "LGTM" becomes the default under load.


-


**Inline AI (Copilot, Cursor, and the wave after 2021)** accelerated generation. It added code volume without adding coordination, which means more high-blast-radius changes carrying less context than before.


A software factory is the next step in agentic software development: AI agents own each stage *and* the handoffs between them, and the loop runs continuously instead of waiting on a human to pass work along.


## What is Mastra?


[Mastra](https://mastra.ai/) is a TypeScript framework for building AI agents and multi-step workflows.


A software factory needs specialized agents, typed handoffs, schedules, shared memory, and full traces. Mastra provides each of those as one system:


-


**Agents:** instructions, model, tools, and memory in a single definition


-


**Workflows:** typed multi-step chains with validated inputs and outputs at every handoff


-


**Scheduling:** cron-driven loops, such as the monitoring poll every 15 minutes in this build


-


**Memory and storage:** shared operational state so agents remember what they already triaged or escalated


-


**Observability:** traces for every step, tool call, and model call, inspectable in[Mastra Studio](https://mastra.ai/docs/studio/overview)


## Set Up Your Software Factory in TypeScript


You can find the full source code for this software factory[here](https://manicule.link/mastra-software-factory-repo) .


Start with the scaffolding. Mastra generates a working project with an example agent you can run right away.


```text
npm   create   mastra@latest
cd   software-factory
npm   run   dev    # starts Mastra Studio at http://localhost:4111
```


Open` localhost:4111` and you have[Mastra Studio](https://mastra.ai/docs/studio/overview) : a UI to observe and run your agents and workflows and see full traces of every step, tool call, and token.


This is what Mastra's project structure looks like:


```text
src/mastra/
├── types/
│   └── index.ts          # shared vocabulary: the work item, metrics record, incident
├── storage.ts            # one LibSQL database: agent memory, workflow state, schedule, traces
├── memory.ts             # one shared Memory instance, backed by that database
├── tools/                # the typed actions agents take (return mock data when a token is missing)
│   ├── github.ts         # read issues and PRs, open PRs, comment, read files
│   ├── sentry.ts         # fetch errors with stack traces, list alerts, resolve issues
│   └── metrics.ts        # throughput, backlog, cycle time
├── agents/               # one agent per job: instructions + model + tools + memory
│   ├── triage.ts         # classify and route every incoming signal
│   ├── codeGen.ts        # read the repo, write the change, open a PR
│   ├── validation.ts     # review the PR: correctness, security, tests, performance
│   ├── release.ts        # version, changelog, deploy, verify, roll back
│   ├── documentation.ts  # detect doc drift, open doc PRs
│   ├── monitoring.ts     # poll production health, open incidents
│   └── orchestrator.ts   # supervisor that routes ambiguous work (built last)
├── workflows/            # typed step chains that call the agents and validate every handoff
│   ├── triageWorkflow.ts
│   ├── codeGenWorkflow.ts
│   ├── validationWorkflow.ts
│   ├── releaseWorkflow.ts
│   ├── documentationWorkflow.ts
│   ├── monitoringWorkflow.ts  # the one with a cron schedule, every 15 minutes
│   └── sdlcWorkflow.ts        # the master router across all of them
└── index.ts              # the registry: agents, workflows, storage, logger, observability
```


## Components of a Software Factory


A software factory has five components:


1.


**Input sources.** What triggers the agents, and whether they arrive with enough context to act.


2.


**AI agents.** The reasoning layer. Each agent owns one job, reads what it needs, and calls the right tools.


3.


**Tools.** The actions agents take in the world: fetch error logs, read a file, open a PR.


4.


**Feedback loops.** Orchestrated processes that chain agents together with a validated handoff at every step.


5.


**Outputs.** PRs, releases, docs, incidents, escalations, and the trace data that makes all of it auditable.


### Input Sources


What triggers a software factory? An external signal, with enough context attached that an agent does not have to go hunting.


For example, don't just attach a Sentry error ID. Attach the whole stack trace, the breadcrumbs, the affected user count, when it was first seen versus last seen, and any linked issue. Context quality at the input determines decision quality everywhere downstream.


The factory consumes signals from tools a team already runs:


-


**GitHub issues.** Bug reports, feature requests, community feedback.


-


**GitHub pull requests.** Open or stale PRs that need attention.


-


**Sentry errors and alerts.** Production exceptions, error-rate spikes, active incidents.


-


**Deployment events.** A merge that lands wakes the documentation agent.


-


**Manual work items.** Created directly for work that does not originate from a signal.


### AI Agents


Agents are your floor workers. They are tightly scoped to perform only one function.


Why one agent per function? An agent that does one job can be measured against one clear standard. An agent that does six cannot be reliably evaluated on any of them, because you can never tell which job a given output was trying to do.


The six functions of your software engineering team are mapped to one agent each. These agents run independently in a workflow triggered by the orchestrator and pass only structured output between steps.


Agent Responsibility Triggered by Produces


Triage Signal detection and classification GitHub, Sentry, schedule Prioritized, routed work items


Code Gen Implementation A work item from triage A pull request with tests


Validation The quality gate A PR created A structured verdict per dimension


Release Deployment A PR approved A released version with changelog


Documentation Drift detection and doc updates A PR merged Doc PRs, or triage items for drift


Monitoring Production health A schedule, every 15 minutes Incidents, Sentry resolution, a health report


The validation agent is where[AI agent evaluation](https://mastra.ai/docs/evals/overview) happens in practice: every PR gets a structured verdict per dimension (correctness, security, tests, performance) before anything moves toward release.


In Mastra, an[Agent](https://mastra.ai/docs/agents/overview) is defined by four properties:


-


instructions (the system prompt that defines its job)


-


a model


-


a set of tools it is allowed to call


-


a memory for persistent context across runs.


You can define an agent in a TypeScript file under` src/mastra/agents/` . Here is the triage agent, for example:


```text
import   {   Agent   }   from   '  @mastra/core/agent  '  ;
import   {   anthropic   }   from   '  @ai-sdk/anthropic  '  ;
import   {   agentMemory   }   from   '  ../memory.js  '  ;
import   {   fetchGithubIssuesTool,   fetchGithubPRsTool   }   from   '  ../tools/github.js  '  ;
import   {   fetchSentryIssuesTool,   fetchSentryAlertsTool   }   from   '  ../tools/sentry.js  '  ;
import   {   getFactoryMetricsTool   }   from   '  ../tools/metrics.js  '  ;


export   const   triageAgent   =   new   Agent  ({
id:   '  triage  '  ,
name:   '  Triage Agent  '  ,
instructions:   `  You are the Triage Agent for an autonomous Software Factory.
Classify every incoming signal by type, priority (P0 to P3), and confidence.
Deduplicate against existing work before creating anything new. Route each item:
bugs and errors to codeGen, alerts to monitoring, doc issues to documentation.
[full instructions live in the repo]  `  ,
model:   anthropic  (  '  claude-sonnet-4-6  '  ),
tools: {
fetchGithubIssuesTool,
fetchGithubPRsTool,
fetchSentryIssuesTool,
fetchSentryAlertsTool,
getFactoryMetricsTool,
},
memory: agentMemory,
});
```


### Tools


How do AI agents take actions in the real world? Through tools. Without tools, an agent can only reason and write. With tools, it can read a file, open a PR, post a comment, resolve an incident.


The factory ships three tool groups, each in its own file under` src/mastra/tools/` :


-


` github.ts` fetches issues and PRs, creates PRs, posts comments, updates labels, and reads file contents via the[GitHub REST API](https://docs.github.com/en/rest) . Code gen and validation use these to read the codebase and write back to it.


-


` sentry.ts` fetches issues with full event context, pulls stack traces, resolves issues, and lists active alerts via the[Sentry API](https://docs.sentry.io/api/) . Monitoring uses these to detect and close the loop on production errors.


-


` metrics.ts` reads throughput, backlog, and cycle time. Any agent can check factory health before deciding how to prioritize.


In Mastra, a tool is defined with` createTool` , with four properties: a description that the agent reads to decide when to call it, an` inputSchema` and an` outputSchema` (both[Zod](https://zod.dev/) schemas), and an` execute` function that does the work.


```text
import   {   createTool   }   from   '  @mastra/core/tools  '  ;
import   {   z   }   from   '  zod  '  ;


export   const   fetchGithubIssuesTool   =   createTool  ({
id:   '  fetch-github-issues  '  ,
description:   '  Fetches issues from a GitHub repository, filtered by state and labels.  '  ,
inputSchema: z.  object  ({
owner: z.  string  ().  describe  (  '  Repository owner (user or org)  '  ),
repo: z.  string  ().  describe  (  '  Repository name  '  ),
state: z.  enum  ([  '  open  '  ,   '  closed  '  ,   '  all  '  ]).  optional  ().  default  (  '  open  '  ),
labels: z.  array  (z.  string  ()).  optional  (),
}),
outputSchema: z.  object  ({
issues: z.  array  (
z.  object  ({
id: z.  number  (),
number: z.  number  (),
title: z.  string  (),
body: z.  string  ().  nullable  (),
labels: z.  array  (z.  string  ()),
state: z.  string  (),
url: z.  string  (),
createdAt: z.  string  (),
updatedAt: z.  string  (),
}),
),
}),
execute  :   async   ({ owner, repo, state   =   '  open  '  , labels })   =>   {
// When GITHUB_TOKEN is not set, return realistic mock issues so the
// factory runs locally. Otherwise call the GitHub REST API and map
// each issue onto the output schema above.
},
});
```


### Feedback Loops


How does the factory catch an agent's mistakes before they spread? With feedback loops: orchestrated, multi-step processes that chain agents together with validated data at every handoff. When a step produces bad data, it fails loudly at the boundary, before the next step runs, instead of silently corrupting everything downstream.


In Mastra, a[Workflow](https://mastra.ai/docs/workflows/overview) is a sequence of Steps. Each step takes in structured input and delivers typed outputs, as defined in` inputSchema` and` outputSchema` .


Workflows are defined in` src/mastra/workflows/` with` createWorkflow` , chaining steps with` .then()` .


Here is the monitoring workflow, for example:


```text
import   {   createWorkflow,   createStep   }   from   '  @mastra/core/workflows  '  ;


export   const   monitoringWorkflow   =   createWorkflow  ({
id:   '  monitoring-workflow  '  ,
inputSchema: z.  object  ({
runId: z.  string  ().  optional  (),
windowMinutes: z.  number  ().  optional  ().  default  (  15  ),
}),
outputSchema: HealthReportSchema,
schedule: {
cron:   '  */15 * * * *  '  ,
inputData: { windowMinutes:   15   },
},
})
.  then  (pollHealthSignals)
.  then  (detectAnomalies)
.  then  (createIncidents)
.  then  (routeToTriage)
.  then  (generateHealthReport)
.  commit  ();
```


We are using the` schedule` property in` createWorkflow` to define when this workflow should fire, in this case every 15 minutes.


Now let's look inside a step.` detectAnomalies` is a step that takes the raw health signals and turns them into a structured assessment:


```text
const   detectAnomalies   =   createStep  ({
id:   '  detectAnomalies  '  ,
inputSchema: HealthSignalSchema,
outputSchema: AnomalyDetectionSchema,
execute  :   async   ({ inputData, mastra })   =>   {
const   agent   =   mastra.  getAgentById  (  '  monitoring  '  );
const   response   =   await   agent.  generate  (
// detection rules omitted here for length; the full prompt is in the repo
`  Analyze these health signals and detect anomalies:\n  ${  JSON.  stringify  (inputData)  }`  ,
{ structuredOutput: { schema: AnomalyDetectionSchema } },
);
return   {
anomalies: response.object?.anomalies   ??   [],
healthScore: response.object?.healthScore   ??   100  ,
analysisNotes: response.object?.analysisNotes   ??   response.text   ??   ''  ,
polledAt: inputData.polledAt,
};
},
});
```


With steps and schedules in place, the factory runs three loops at once: three multi-agent workflows registered in the same Mastra instance, all sharing one database.


#### Loop 1: The Primary Development Loop


A signal enters, gets classified, becomes a change, gets validated, gets deployed, and gets documented. This is the factory's core multi-agent workflow.


#### Loop 2: The Monitoring Feedback Loop


Production is watched continuously. When something breaks, it becomes a signal and re-enters the primary loop with no human opening a ticket.


#### Loop 3: The Drift Detection Loop


When code changes, the documentation agent diffs the change against the written docs and opens a triage item if anything fell out of sync. Drift gets caught at the source instead of being discovered months later.


### Outputs


What does a software factory produce? Outputs a human can audit without reconstructing what happened from LLM logs:


-


PRs with their originating signal linked


-


Package releases


-


Updated changelogs and documentation


-


Incident reports with severity and resolution


-


Escalation alerts when a decision needs human judgment


-


Trace data for every run: each step, tool call, and model call


## How Everything Wires Up in Mastra


In Mastra, everything is registered in a single Mastra instance in` src/mastra/index.ts` :


```text
import   {   Mastra   }   from   '  @mastra/core/mastra  '  ;
import   {   PinoLogger   }   from   '  @mastra/loggers  '  ;
import   {   Observability,   MastraStorageExporter,   SensitiveDataFilter   }   from   '  @mastra/observability  '  ;
import   {   storage   }   from   '  ./storage.js  '  ;
// ...agent and workflow imports
export   const   mastra   =   new   Mastra  ({
agents: {
triageAgent, codeGenAgent, validationAgent,
releaseAgent, documentationAgent, monitoringAgent, orchestratorAgent,
},
workflows: {
triageWorkflow, codeGenWorkflow, validationWorkflow,
releaseWorkflow, documentationWorkflow, monitoringWorkflow, sdlcWorkflow,
},
storage,
logger:   new   PinoLogger  ({ name:   '  Mastra  '  , level:   '  info  '   }),
observability:   new   Observability  ({
configs: {
default: {
serviceName:   '  software-factory  '  ,
exporters: [  new   MastraStorageExporter  ()],
spanOutputProcessors: [  new   SensitiveDataFilter  ()],
},
},
}),
});
```


### Storage


[Storage](https://mastra.ai/docs/storage/overview) is the database Mastra uses for runtime state. It is not agent memory by itself, and it is not your application database. It is the place Mastra writes the operational data needed to run the factory. In this build, storage is a LibSQL database:


```text
// storage.ts
import   {   LibSQLStore   }   from   '  @mastra/libsql  '  ;
export   const   storage   =   new   LibSQLStore  ({
id:   '  software-factory  '  ,
url: process.env.DATABASE_URL   ??   '  file:./mastra.db  '  ,
});
```


### Memory


[Memory](https://mastra.ai/docs/memory/overview) is the context an agent can carry across runs. This` agentMemory` is wired into all seven agents. This stops the triage agent from creating the same work item twice, or the monitoring agent from re-alerting on an incident it already escalated.


You can define how many past messages you want the agent to be able to access.


```text
// memory.ts
import   {   Memory   }   from   '  @mastra/memory  '  ;
import   {   storage   }   from   '  ./storage.js  '  ;
export   const   agentMemory   =   new   Memory  ({   storage,   options: { lastMessages:   30   } });
```


The same memory object can be shared across agents because the actual history an agent sees is scoped at call time by` resourceId` and` threadId` . A` resourceId` is usually the repo, user, customer, or project the work belongs to. A` threadId` is the specific conversation or run. You do not need a separate database per agent to keep context separated.


### Observability


Mastra's built-in[observability](https://mastra.ai/docs/observability/overview) tools record what happened when a workflow ran: each step, each agent call, each tool call, each model call, how long it took, whether it succeeded, and what input and output moved through it.


This trace is useful for debugging workflows and finding the source of a hallucination.


In this build, traces are written back into Mastra storage:


```text
observability  :   new   Observability  ({
configs: {
default: {
serviceName:   '  software-factory  '  ,
exporters: [  new   MastraStorageExporter  ()],
spanOutputProcessors: [  new   SensitiveDataFilter  ()],
},
},
}),
```


` SensitiveDataFilter` redacts secrets before they are written.


### Agent Orchestration: Routing Work to the Right Agent


Agent orchestration is how the factory decides what should happen next for every trigger: a Sentry error goes to the monitoring loop, a docs inconsistency goes to the documentation loop.


For that, the factory uses an Orchestrator Agent, whose job is to figure out the right next step for the task.


Why not use a plain switch statement? Some cases need judgement. A feature request might look small until the agent sees that it changes auth, sessions, and database shape. Mastra exposes each entry in agents and workflows to the orchestrator as a callable tool. That means the orchestrator can decide and delegate the work to the right agent or run a workflow as needed.


```text
export   const   orchestratorAgent   =   new   Agent  ({
id:   '  orchestrator  '  ,
name:   '  Orchestrator Agent  '  ,
instructions:   `  Coordinate the software factory. Route each work item to the right place.
Bugs and features run codeGenWorkflow. Docs-only changes run documentationWorkflow.
Production alerts run monitoringWorkflow. Ambiguous signals go to triage first.
Escalate to a human for architectural decisions, breaking changes, or during a SEV1.  `  ,
model:   anthropic  (  '  claude-sonnet-4-6  '  ),
agents: { triage: triageAgent, codeGen: codeGenAgent, documentation: documentationAgent, monitoring: monitoringAgent },
workflows: { codeGenWorkflow, documentationWorkflow, monitoringWorkflow },
memory: agentMemory,
});
```


## What to Build Next


The point of a factory is that your team spends less time writing software by hand and more time writing the agents and workflows that write it. Once the core loop is running, build these next:


-


**Frontend QA.** Agents can write UI code that passes tests and still looks wrong, so add Playwright flows, screenshot checks, visual diffs, and human review for UX-sensitive changes.


-


**Human reviews.** Put humans in place to verify major decisions like architecture, breaking changes, failed validation, or active incidents.


-


**Monitoring.** Detect when a step is taking longer than usual, when an agent gets stuck, when tool calls fail, and when a workflow needs to be retried or escalated.


-


**Bad handoffs.** Pass one typed work item through every step so context does not disappear between triage, codegen, validation, release, docs, and monitoring.


## When Not to Use a Software Factory


A factory optimizes for repeatable work, and it breaks where work is ambiguous by design:


-


**Early-stage products** where requirements change daily. You need exploration, not repeatability.


-


**Codebases without a test suite.** Agents need tests to validate against; add coverage first.


-


**Heavily regulated changes** where every diff requires human sign-off at the code level anyway.


The right human gate is not approval of every PR. It is approval of the agent instructions that govern all of them.


The fastest way to understand a factory is to run one.` npm create mastra@latest` scaffolds a project with a working agent in about a minute. Give one agent one job (triage is a good first pick) and grow the loop from there. The[Mastra docs](https://mastra.ai/docs) cover every primitive used in this guide.


## Frequently Asked Questions


### What is agent orchestration?


Agent orchestration is deciding which agent or workflow handles each piece of work, in what order, with what context. In this factory an orchestrator agent does the deciding: Mastra exposes every registered agent and workflow to it as a callable tool, so it can reason about ambiguous work instead of following a hardcoded switch statement.


### How does a software factory use memory and storage?


Storage is the factory's operational database: workflow state, schedules, and traces. Memory is the context agents carry across runs, scoped by resource and thread, so triage does not recreate the same work item and monitoring does not re-alert on an incident it already escalated. In this build both share one database, which keeps the whole loop auditable from one place.


### How do you evaluate AI agents?


Give each agent one job and a structured verdict to produce, then score every dimension separately. The validation agent here scores each PR on correctness, security, tests, and performance before release, and tracing records every step, tool call, and model call so any decision can be audited after the fact.


### What are some AI agent frameworks?


Mastra, VoltAgent, Google's Agent Development Kit, and Microsoft Agent Framework all ship TypeScript-first agent primitives. This guide uses Mastra because everything a factory needs (agents, typed workflows, scheduling, memory, and observability) comes from one framework, so the whole system shares a single database and trace layer.


### What's the difference between a software factory and AI coding agents like Copilot or Cursor?


AI coding agents accelerate one step: writing code. A software factory automates the loop around it (triage, validation, release, documentation, and monitoring), plus the handoffs between steps, where context normally degrades. You still use a coding agent inside a factory; the code gen agent is one of six.


### Why TypeScript instead of Python?


Python is the default for a lot of AI research tooling, but a software factory runs next to the product it ships. TypeScript types every handoff: tool inputs, workflow steps, and agent outputs are checked at build time and validated with Zod at runtime. It is also the language of most web apps, APIs, and CI pipelines, so the factory lives in the same repo, tooling, and review process as everything else.


### What is a dark software factory?


A dark factory, borrowed from "lights-out" manufacturing, is a software factory where autonomous coding agents run with no human in the loop at all. The Sentry example above is a dark run: detected, fixed, shipped, and resolved entirely by agents. Running every class of change dark is a bad idea; keep human gates on architecture, breaking changes, and releases during active incidents.
