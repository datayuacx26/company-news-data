---
schema_version: "1.0.0"
document_id: "4442ac1ed3b88cfdb0aa50250bdaf5e999c465ca3383d1559844766d687a12c5"
company_key: "yc-ellipsis"
company: "Ellipsis"
source_id: "yc-ellipsis-news-import-97656c4ed8a9"
canonical_url: "https://www.ellipsis.dev/blog/lessons-from-building-llm-agents"
published_at: "2025-01-30T00:00:00+00:00"
first_seen_at: "2026-07-24T05:57:47.052865+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:dce017b404acfdbe4be38d78d4fe348755e8c4c15348722e994c5a3751e25beb"
---

# Lessons from 15 Months of Building LLM Agents

We have been building production LLM agents since late 2023. In that time, we have shipped agents that review code, generate pull requests, fix bugs, answer questions about codebases, write reports, and run analytics queries. They run autonomously, 24/7, across hundreds of repositories.


Here is what we have learned. None of this is theoretical — these are lessons from production systems handling real workloads.


## Sandboxing is non-negotiable


An autonomous coding agent needs to execute code. It needs to run tests, install dependencies, invoke build tools. If you give it access to your production environment, you are one hallucinated` rm -rf` away from an incident.


Every agent invocation in Ellipsis runs in an isolated sandbox — a fresh container with a cloned copy of the repository, network access restricted to approved endpoints, and a time and resource budget. The agent can read and modify the cloned repo. It cannot touch production infrastructure, access secrets outside its scope, or persist state between invocations.


We run these sandboxes on Modal. The cold start overhead is acceptable (a few seconds), and the isolation guarantees are strong. The alternative — running agents on shared infrastructure with permission scoping — is fragile and eventually breaks.


## Context management is the entire game


The quality of an agent's output is determined almost entirely by the quality of its context. A code review agent that only sees the diff will produce shallow, generic comments. A code review agent that sees the diff, the full files, the related test files, the style guide, the git history, and the PR description will produce reviews that read like they came from a senior engineer who knows the codebase.


We spent more engineering time on context assembly than on any other component. The system that determines which files to include, how to chunk them, what metadata to attach, and how to order them within the prompt is the most important system in the stack.


Key lessons on context:


- **Retrieval matters more than generation.** The model is good at reasoning over information it has. It is bad at reasoning about information it does not have. Getting the right files into context is more impactful than prompt engineering.
- **Static analysis is underrated.** Language servers, AST parsing, import graph traversal — these tools are old, boring, and extremely useful for determining which files are relevant to a given change.
- **Context windows are large but not infinite.** Even with 200k-token context windows, you cannot include an entire monorepo. Prioritization and truncation strategies matter. We rank files by relevance and include them in priority order until the budget is exhausted.


## Cost control requires architecture, not just optimization


LLM API calls are expensive. A naive implementation — send the entire repo context on every invocation — will produce accurate results and astronomical bills. Cost control is an architectural concern, not an optimization pass.


We use three strategies:


- **Tiered model selection.** Not every task needs the most capable model. Routing simple tasks (formatting checks, style lint) to faster, cheaper models and complex tasks (architectural review, bug detection) to more capable models reduces cost by 60-70% with negligible quality impact.
- **Incremental processing.** When a PR is updated, we do not re-review the entire PR. We identify which files changed since the last review and only re-process those files. This reduces token usage by 40-50% on iterative PRs.
- **Caching and deduplication.** Repository metadata, style guides, and common file contents are cached across invocations. The same file does not get re-tokenized on every review.


## Reliability is about retries, not accuracy


LLM outputs are non-deterministic. The same input will sometimes produce a great review and sometimes produce a mediocre one. You cannot fix this by making the model more accurate. You fix it by building reliability at the system level.


Our approach: every agent invocation goes through a structured output pipeline. The agent produces structured JSON (not free-form text). A validation layer checks the output against a schema. If the output is malformed or incomplete, the system retries with a corrective prompt. If it fails after three attempts, it falls back to a simpler analysis.


In production, the first attempt succeeds about 92% of the time. After retries, the success rate is 99.7%. The remaining 0.3% falls back gracefully — the user gets a partial result or a notification that the agent could not complete the task, rather than a broken comment or a silent failure.


## Testing LLM agents is hard but possible


You cannot write a unit test that asserts an LLM will produce a specific output. The output is non-deterministic and the quality is subjective. But you can build an evaluation framework that catches regressions.


Our testing strategy has three layers:


- **Deterministic tests** for everything outside the LLM call — context assembly, output parsing, integration posting. These are standard unit and integration tests.
- **Snapshot evaluations** for agent output quality. We maintain a corpus of 200+ test cases — real PRs with known issues — and run the agent against them on every deploy. We measure recall (did the agent find the known issues?) and precision (did it flag things that are not issues?).
- **Production monitoring** for real-world quality. We track which agent comments get resolved (accepted as useful) vs. dismissed. This gives us a continuous quality signal from actual users.


## The things that actually matter


After fifteen months, here is what we believe matters most for production LLM agents:


1. **Context quality.** What information the model has access to determines the ceiling for output quality. No amount of prompt engineering overcomes missing context.
2. **Structured output.** Free-form text output is unparseable and unreliable. Structured JSON with schema validation and retry logic is the baseline for production systems.
3. **Isolation.** Autonomous agents that can execute code must be sandboxed. There is no middle ground.
4. **Configurability.** Users need to control what the agent does, when it runs, and what it flags. Default behavior will never be right for everyone.
5. **Cost architecture.** Design for cost from day one. Model routing, incremental processing, and caching are not optimizations — they are requirements.


The models will keep getting better. The infrastructure around them — context, reliability, cost, safety — is what determines whether an agent is a demo or a production system.
