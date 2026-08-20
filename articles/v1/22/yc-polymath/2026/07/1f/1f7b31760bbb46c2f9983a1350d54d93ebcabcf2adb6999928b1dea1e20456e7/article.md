---
schema_version: "1.0.0"
document_id: "1f7b31760bbb46c2f9983a1350d54d93ebcabcf2adb6999928b1dea1e20456e7"
company_key: "yc-polymath"
company: "Polymath"
source_id: "yc-polymath-news-import-682cc202319b"
canonical_url: "https://polymathlabs.ai/blog/horizon-swe"
published_at: null
first_seen_at: "2026-07-25T19:30:35.658468+00:00"
fetched_at: "2026-07-28T21:16:50.994015+00:00"
content_hash: "sha256:c085457f60c1b1c723e9992a48cdcf3f906c3dbdf20b1b000924cc252e091ece"
---

# Introducing Horizon-SWE: Evaluating the Performance of AI Agents on End-to-End Software Engineering Workflows

[Back](https://polymathlabs.ai/blog) February 2, 2026 · Updated February 6, 2026


# Introducing Horizon-SWE: Evaluating the Performance of AI Agents on End-to-End Software Engineering Workflows


## A benchmark for multi-tool, long-horizon software engineering tasks in production grade systems


Claude Opus 4.6


25.5%


±3.6


Claude Opus 4.5


22.1%


±3.0


Claude Sonnet 4.5


20.8%


±3.0


Gemini 3 Pro


19.1%


±3.0


GPT-5.2 Codex


18.0%


±3.0


Gemini 3 Flash


16.7%


±2.9


Qwen 3 Coder 480B


14.7%


±2.7


Kimi K2 Thinking


13.3%


±2.6


Pass Rate


AI agents have become remarkably capable at generating code and executing CLI commands, largely due to the abundance of training data and benchmarks that focus on improving these capabilities. Coding agents initially started off as assistants, but are now showing signs of being able to take on larger tasks autonomously with little or no human supervision.


However, these capabilities are still nascent. Benchmarks for long-horizon, production-grade engineering remain largely unexplored, and training environments for improving these capabilities are scarce.


We're introducing **Horizon-SWE** , a benchmark that evaluates the ability of AI agents to perform *long-horizon tasks in production-grade environments* involving a variety of services and tools. This benchmark measures the ability of AI agents to operate as **autonomous software engineers** , as opposed to coding assistants.


We put an entire software engineering company in a containerized environment, consisting of a running app with live traffic, and a set of 73 software engineering tools. We then asked AI agents to complete large, multi-step tasks: gathering information, implementing features, testing, deploying, monitoring services, and responding to incidents. Finally, we measured the performance of agents in terms of feature correctness, deployment & devops, and engineering quality.


## Principles


Horizon-SWE adopts the following principles:


**Production-grade systems.** Agents implement, deploy, and monitor production-grade systems consisting of multiple components (e.g. frontend, backend, databases, object stores, caches).


**Tool use.** Success depends on coordination across multiple tools (e.g. ticketing system, messaging tools, knowledge base, command line, CI/CD, Sentry, logs, database access, etc.). Agents interact with tools through MCP.


**Long-horizon reasoning.** Solving requires planning, sequencing, and judgment, not just local code correctness. Solutions can appear correct but fail under integration or rollout conditions.


**Stateful, evolving environments.** The environment is stateful and evolving (data, traffic, background jobs, metrics change over time, but are seeded to ensure reproducibility).


**Verifiable outcomes and partial credit.** Agents are assessed based on verifiable outcomes, instead of prescriptive solutions. Partial credit is assigned due to the difficulty of tasks.


**Real interactions.** Traffic and requests flow through real containerized services and tools in order to ensure realism in the environment.


## Environments


Horizon-SWE evaluates coding agents within an environment that reflects the tools, services, and procedures that software engineers interact with. An environment consists of:


**An agent workspace** which contains the monorepo that the agent can read and edit. Within the workspace, the agent can also execute bash commands and call tools.


**An MCP server** which allows agents to interact with the tools that software engineers use.


**Tools and services,** which includes issue trackers, a knowledge base, communication tools, deployment tools, logs, metrics, alarms, and alerting tools, and more.


**A running application,** which is deployed from the service code that the monorepo contains. The monorepo used in Horizon-SWE contains 20,000+ human-authored commits. The code is deployed into a running application that contains a frontend, backend servers, databases, object stores, caches, and queues. The application is running when the agent starts on the task, and the agent can re-deploy after making code changes.


**A traffic generator** which drives synthetic traffic to the running application.


When the environment is initialized, it is seeded with initial data that populates issue trackers, communication tools, and the company knowledge base.


## Tasks


Tasks in Horizon-SWE are designed to involve changes across a large surface area of components in order to evaluate tool use abilities and long-horizon planning. We evaluate agent performance on 50 diverse tasks that measure the ability of agents to implement, deploy, monitor, and debug complex production systems.


Below is a high level description of some of the tasks in Horizon-SWE:


**Multi-service feature rollout.** Implement a feature spanning multiple services or system components, requiring coordinated changes and correct deployment sequencing. The task is complete when the feature works end-to-end and no alarms fire due to incorrect deployment ordering or cross-service incompatibilities.


**Feature flag implementation.** Add a new feature to an existing service and gate it behind a runtime feature flag, without introducing breaking API changes. The task is complete when the flag correctly controls behavior for targeted versus non-targeted users without regression.


**Security incident response.** Investigate suspicious activity, fix the underlying vulnerability, update Sentry ticket and public system status. The task is complete when the vulnerability is patched, exploit attempts are blocked, and an audit report is produced.


**Latency optimization.** Diagnose and resolve a latency issue causing SLO violations. The task is complete when latency metrics return to acceptable thresholds without regression in correctness or error rates.


**Error rate reduction.** Reduce error rates by implementing reliability improvements (timeouts, retries, fallbacks, etc.). The task is complete when error rates decrease to acceptable thresholds and the system handles failure scenarios gracefully.


**API migration.** Deprecate a legacy API while migrating existing traffic to a new version. The task is complete when all traffic uses the new API and the legacy endpoint is decommissioned without errors.


**Flaky test remediation.** Fix intermittently failing tests in the monorepo to achieve consistent, reliable test behavior. If the issue is due to service code, fix the service code. The task is complete when previously flaky tests pass consistently across multiple runs without being skipped or disabled.


## Example


To concretely illustrate Horizon-SWE's evaluation methodology, we trace a single task: replacing a global "allow anonymous browsing" boolean with four granular access controls in a production codebase. The change touches the database schema, admin UI dashboard, API authorization logic, and client-side navigation. Below is a **condensed agent trajectory** .


- The agent tracks usage across the codebase and MCP tools, finding references in backend controllers, config layers, admin UI, serializers, and locale files.
- The agent executes coordinated changes across the entire stack: config defaults, backend controllers, admin models and views, in addition to unit and integration tests.
- The agent queries metrics and logs to establish baseline health.
- The agent deploys, and queries metrics and logs again. However, it encounters failures caused by a missing database migration.


This case reflects a pattern we observe across Horizon-SWE: agents handle application-layer reasoning well but fail to account for the full engineering lifecycle. Migrations, build pipelines, and deployment sequencing require planning beyond code generation, and these failure modes only surface when evaluating agents in production-grade environments.


## Grading


Horizon-SWE evaluates models on 3 dimensions which capture the elements of engineering competency:


**Feature correctness.** Evaluates whether the newly introduced feature satisfies the task specification. Correctness is determined by build success or failure, unit tests, and integration tests. Regressions on existing features are penalized.


**Deployment & DevOps.** Assesses how reliably the agent can test and deploy changes to production. Horizon-SWE penalizes deployments that set off alarms and canaries.


**Engineering Quality.** Evaluates whether the agent maintains high code quality and engineering best practices. An LLM judge assesses commit scope, code maintainability, and documentation. Penalties apply for security issues, non-performant code, and unproductive loops in the agent's trajectory.


Due to the complexity of the tasks in Horizon-SWE, we introduce two scoring frameworks: pass / fail and partial credit.


### Pass / Fail


**Horizon-SWE-PF** uses strict binary grading: a task is marked as passed only if the agent achieves a full score on the feature correctness and deployment verifiers. Engineering quality is excluded from this framework due to its subjective nature.


Claude Opus 4.6


25.5%


±3.6


Claude Opus 4.5


22.1%


±3.0


Claude Sonnet 4.5


20.8%


±3.0


Gemini 3 Pro


19.1%


±3.0


GPT-5.2 Codex


18.0%


±3.0


Gemini 3 Flash


16.7%


±2.9


Qwen 3 Coder 480B


14.7%


±2.7


Kimi K2 Thinking


13.3%


±2.6


Pass Rate


All models are evaluated using the[OpenHands](https://github.com/OpenHands/OpenHands) agent harness in order to compare model performance. Under pass/fail scoring, even the strongest models complete only about one in four runs successfully end-to-end. Claude Opus 4.6 leads at a 25.5% success rate, followed by Claude Opus 4.5 and Claude Sonnet 4.5 at roughly 21–22%, with Gemini 3 Pro and GPT-5.2 Codex close behind.


### Partial Credit


**Horizon-SWE-PC** uses a grading framework that produces a composite score consisting of feature correctness (60%), deployment & devops (30%), and engineering quality (10%). In each category, partial credit is assigned due to the complexity and duration of the tasks.


Claude Opus 4.6


60.4


±1.8


Claude Opus 4.5


54.9


±1.9


GPT-5.2 Codex


50.1


±2.1


Claude Sonnet 4.5


48.5


±1.9


Gemini 3 Flash


45.6


±2.0


Gemini 3 Pro


43.2


±1.8


Grok Code Fast 1


42.1


±1.8


Kimi K2 Thinking


42.0


±1.9


Feature Correctness (60%)


Deployment & DevOps (30%)


Engineering Quality (10%)


With partial credit scoring, Claude Opus 4.6 leads this benchmark with a composite score of 60.4, followed by Claude Opus 4.5 and GPT-5.2 Codex.


An interesting split emerges in the results: the strongest models handle Deployment & DevOps relatively well, while most others score significantly higher on Feature Correctness, as opposed to deployment. This suggests that the ability to plan over long horizons and coordinate across tools, rather than code generation alone, is a key differentiator that separates the leading models from the rest.


By Polymath Team
