---
schema_version: "1.0.0"
document_id: "0b75726ae442a8108b7e373875cfdb4b9115f0af7b4361fc385813244d644dc4"
company_key: "yc-mendral"
company: "Mendral"
source_id: "yc-mendral-news-import-538be81b6878"
canonical_url: "https://mendral.com/blog/same-llm-different-agent"
published_at: "2026-03-30T00:00:00+00:00"
first_seen_at: "2026-08-10T00:32:33.388808+00:00"
fetched_at: "2026-08-10T00:32:34.981599+00:00"
content_hash: "sha256:c454649b0ccbb36c6bcdfddd22b05b21e30a4ff9bae40e1fe34bf9b2643cdda3"
---

# Same LLM, Different Agent: What Changes When You Specialize for CI

The question we get most about Mendral: "Why can't I just run Claude Code on my CI?"


Both run on the same LLMs. Both are coding agents. Both read code, write patches, reason about failures. The difference is everything *around* the model: system prompts, tools, data, context. Every token sent to the LLM is optimized for a different job. Claude Code is optimized for writing software. Mendral is optimized for diagnosing CI failures, fixing flaky tests, and catching regressions before they hit main.


We use Claude Code every day. This post is about why a general coding agent and a CI-specific agent end up so different in practice, even on the same underlying model.


## The token gap


When you send a message to Claude Code, the agent wraps it in a large payload: system prompts, tool definitions for file operations and shell commands, context about your codebase. Every token is designed to make it great at writing code.


Mendral's payload is entirely different. Our system prompts encode patterns from debugging CI at Docker and Dagger for over a decade:


- A test that passes locally but fails intermittently on CI is almost never random; check for resource contention, shared state between parallel suites, and timing-sensitive assertions before blaming the code.
- A sudden spike in failures after a dependency bump is likely a transitive dependency conflict, not a flake.
- A build that's 30% slower but produces the same output is probably a cache invalidation issue, not a code regression.


Our tool definitions expose operations that don't exist in a general coding agent: querying months of CI history, correlating failures across branches, tracing a flaky test to a transitive dependency bump three weeks ago. The context isn't your current file; it's billions of log lines, test execution history, and a living list of known issues in your delivery pipeline.


The model is identical. Everything it sees is different.


## The agent harness


Mendral isn't a wrapper around an LLM API. The agent loop runs on our Go backend with two categories of tools.


Some are native Go functions: querying ClickHouse for log analysis, fetching GitHub metadata, looking up failure history, correlating test results across runs. These are fast, deterministic, and don't need isolation.


Others require a sandbox. When the agent needs to clone a repo, apply a patch, or run tests, it operates inside a Firecracker microVM, with hardware-level isolation between tenants, booting in under 125ms. Between tool calls, the sandbox suspends. When the LLM responds with the next action, it resumes in under 25ms with full state preserved.


This matters for CI specifically. The agent sometimes pushes a fix and needs to wait hours for the pipeline to complete. The sandbox suspends during that wait. No idle compute. When CI finishes, it resumes with full context intact and verifies the result. Without suspend/resume, you'd either burn hours of compute or lose your entire execution state.


## The data layer


The agent is only as good as what it can see. We built a[log ingestion pipeline](https://mendral.com/blog/llms-are-good-at-sql) that processes billions of CI log lines per week into ClickHouse, compressed at 35:1, queryable in milliseconds. The agent writes its own SQL to investigate failures. No predefined query library.


A typical investigation scans 335K rows across 3+ queries. At P95, 940 million rows. The agent pulls up a failing test's pass rate over 90 days, finds the commit that introduced the regression, checks if the same test is flaky on other branches, and cross-references infrastructure conditions at the time of execution.


A general coding agent sees the current failure in the current run. This sees 90 days of history across every branch.


## Static analysis on every tool call


We run static analysis on every tool call the agent makes, both input and output.


On input, we inspect what the agent sends to tools. On output, we inspect what comes back. Some tool calls dynamically modify the agent's context at runtime, injecting guidance based on what the agent finds.


Concrete example: the agent calls our log query tool targeting a specific workflow and time range. The static analysis layer detects the query returned sparse results: 3 data points where we'd normally expect 50. Instead of letting the agent reason on incomplete data, the analysis layer injects a context update: *"Log coverage for this workflow appears incomplete. Consider expanding the window or checking ingestion delay metrics before concluding."* The agent adjusts its investigation without us hardcoding every edge case into the system prompt.


This lets us encode operational knowledge at the tool boundary. Prompts stay focused on reasoning. The tool layer handles domain guardrails.


On the security side, the same layer enforces hard boundaries. The agent can't delete branches, force-push, close PRs it didn't open, or modify CI config destructively. This is enforcement at the tool boundary, not a prompt instruction the LLM could reason around.


We still hit edge cases. The analysis layer occasionally flags legitimate queries as incomplete when a workflow genuinely only runs a few times per week. We're tuning thresholds; it's an ongoing calibration problem, not a solved one.


## Insights: a learning system


Mendral maintains **insights** : a continuously updated list of active issues in your delivery pipeline. Every anomaly becomes an insight: a flaky test, a CI incident, a security alert, a performance regression, a pattern of failures tied to a specific runner type.


Say the agent opens two insights: one for` TestUserAuthFlow` failures and another for` TestSessionExpiry` timeouts. After three investigations, it merges them: both trace to the Redis connection pooling change in January. When someone fixes the issue outside Mendral, the agent detects the resolution and auto-closes the insight. If the problem recurs, the insight reopens with full history.


Over time, this becomes a learning system. After a month on your codebase, the agent knows that` TestUserAuthFlow` has been flaky since the Redis connection pooling change in January, that builds on` staging` fail Tuesday mornings because of a scheduled job competing for DB connections, and that the last three` @testing-library` bumps each broke two E2E suites. Pattern recognition on your specific codebase, built from every investigation the agent runs.


## A team of agents


Internally, Mendral is a fleet. Different agents use different models matched to different cognitive demands:


Tier Tasks Why this tier


Haiku Log parsing, data extraction Fast, cheap; thousands run daily


Sonnet Evidence collection, SQL queries, deduplication Needs reasoning, not deep analysis


Opus Root cause analysis, writing fixes Complex multi-step reasoning required


Using Opus for log parsing wastes tokens. Using Haiku for root cause analysis produces worse results. The full[multi-agent architecture](https://mendral.com/blog/how-we-built-our-ai-agent) (how agents coordinate, how the triager routes work, durable execution) is covered in our previous post.


So: why can't you just run Claude Code on your CI? You can. But it sees one failure in one run. It doesn't have system prompts encoding CI debugging patterns, tools to query months of failure history, or a sandbox that suspends while your pipeline runs and resumes with full context when it finishes.


Same model weights, completely different harness.
