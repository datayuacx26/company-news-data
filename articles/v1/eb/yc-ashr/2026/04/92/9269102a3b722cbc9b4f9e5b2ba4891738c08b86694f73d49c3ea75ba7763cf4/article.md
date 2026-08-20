---
schema_version: "1.0.0"
document_id: "9269102a3b722cbc9b4f9e5b2ba4891738c08b86694f73d49c3ea75ba7763cf4"
company_key: "yc-ashr"
company: "Ashr"
source_id: "yc-ashr-news-import-002a382db1a7"
canonical_url: "https://ashr.io/blog/building-ash"
published_at: "2026-04-14T00:00:00+00:00"
first_seen_at: "2026-07-21T08:00:04.422357+00:00"
fetched_at: "2026-07-28T21:25:38.770750+00:00"
content_hash: "sha256:48b799b38650a039dd911cd0041663cdaaafe273f3d758be94414146be46bdac"
---

# Building Ash: A Background Coding Agent That Fixes AI Agents

We're building ashr, a platform that helps teams build, evaluate, and monitor AI agents, catching errors before production. The same failure patterns kept showing up across our users' repos: broken tool calls, mismanaged context, and agents begging for tools that they didn't have access to.


## The Problem: Building a Good Harness​


As many engineers have noticed, the art behind building a good agent lies in the harness, with the best agent engineers operating under the assumption that any foundation model-level issues will simply be resolved by model providers with time.


The harness is such an elusive part of the agent engineering experience that even model providers struggle to figure it out: Claude Code performs worse on coding benchmarks1 than coding agents that are built on top of the exact same models. Even Anthropic doesn't know what the best Opus coding harness is, and they made the model.


As a testing platform, we saw how our customers tried to solve this problem. The most AI forward of them were running expensive agents overnight, using ashr to test fixes. This process scratched the surface of prompting errors, but was unable to fix broader context management and tooling errors, while also being far too costly to scale.


We decided to solve this problem by building Ash: the on-call AI engineer for your harness, taking into account observability tools, ashr evals, and other information to optimize agents autonomously. The goal is to operate as the AI engineering version of the Weights and Biases sweep2 , which runs parameter optimizations based off Weights and Biases experiments. Ultimately, the best teams treat their harness as an ongoing experiment, and our goal is to make it as easy as possible to test as many high-quality hypotheses as possible.


Ash can either run fully autonomously, picking up subtle signals and regressions from data sources, or go in specific directions based off instructions sent on Slack, Linear, and Github Comments.


## Architecture: Five Tiers of LLM​


Ash runs a tiered architecture where the expensive model thinks and the cheap model works. Each tier has a single job and a constrained tool set.


**Tier 0 — Triager (Haiku).** Every incoming signal — a Slack message, a cron scan, a webhook — first hits a fast classifier. One word: IGNORE, SIMPLE, or COMPLEX. Simple tasks route to Sonnet for planning. Complex tasks get Opus. The triager's system prompt is deliberately aggressive about defaulting to COMPLEX — we'd rather overspend on planning than under-diagnose a hard problem. This single routing decision saved us roughly 40% on LLM costs without sacrificing quality.


**Tier 1 — Planner (Sonnet or Opus).** The planner reads the codebase, queries production data, searches the web for current docs, and produces a fix plan. It doesn't edit files; it thinks. Its output is a structured plan with exact old/new text replacements, or a subtask decomposition for parallel execution.


**Tier 2 — Worker (Haiku).** The worker takes the plan and executes it mechanically by editing files, running commands, and searching code. It doesn't decide *what* to fix; it follows the plan. This separation means the worker model can be cheap and fast. If the worker finishes without making any edits, Ash nudges it: "You haven't made any file edits yet. USE edit_file RIGHT NOW." This sounds crude, but it solved a real problem where cheaper models would describe changes instead of making them.


**Tier 3 — Reviewer (Haiku).** A quick sanity check on the diff. Does this change actually address agent *behavior* , not just code correctness? Is it surgical or did it refactor half the repo? The reviewer responds APPROVED or REVISE with specific instructions that get fed back to the worker.


**Tier 4 — Validator (Sonnet).** The final gate. The validator can run tests, score before/after states, and inspect the codebase. It returns VALIDATED, FAILED, or UNCERTAIN — a FAILED blocks the PR entirely. It chooses its own verification strategy: code logic changes get the test suite, prompt changes get the scorer, config changes get a parse check.


## The Signal Pipeline​


Ash doesn't start from a user typing "fix this." It starts from signals — normalized observations from multiple sources. A GitHub` @ash` mention, a Slack message, a scheduled cron scan that queries` trace_metrics` for error spikes, a proactive scan that looks for degradation patterns. Every signal gets normalized into an` AshSignal` with a problem description, evidence array, tenant ID, repo, and source metadata.


Before the agent runs, the context builder assembles everything it needs: past fix patterns from project memory, repo metadata, and per-repo agent config. Each data source is fetched independently — a single failure never blocks the pipeline.


The loop: normalize → build context → run coding agent → create PR → log the run → record the fix pattern to memory for future runs.


## Sandboxes and the Warm Pool​


Every Ash run executes inside a Daytona sandbox, a full VM with the repo cloned, dependencies installed, and a database running if the repo needs one. Setup is automatic: detect package manager, install deps, run migrations, inject env vars, reset git state.


Cold-starting a sandbox takes 30–60 seconds, which is too slow for a tool triggered from Slack. So we built a warm pool that maintains pre-provisioned sandboxes per repo. When a run grabs one, the pool resets it and kicks off background replenishment. This cut our median start time to under 5 seconds.


The trickiest part was orphan cleanup. When a Modal container dies mid-run, the` finally: sandbox.destroy()` block never fires. We run a cron every 10 minutes that lists all sandboxes and nukes anything older than 30 minutes that isn't actively in use. When disk quota fills up, we force-clean everything except the youngest sandboxes before retrying.


## Durable Execution via Checkpointing​


Long-running agent sessions (some complex fixes take 15+ minutes and hundreds of tool calls) are vulnerable to container restarts. We solve this with Postgres-backed checkpointing: every 5 tool calls, the full conversation history is serialized to JSON and upserted into` ash_run_checkpoints` , keyed by Slack thread timestamp. The serializer handles SDK message objects, content block arrays, and nested tool results — anything that isn't a plain dict gets` model_dump()` 'd or` __dict__` -extracted.


On restart,` run_coding_agent()` checks for a resumable checkpoint before doing anything else. If one exists and is under 30 minutes old, the agent restores the message history, cost accumulator, and tool call counter, then continues from the exact tier (planner or worker) where it left off. Stale checkpoints are discarded. Successful runs clear their checkpoint. The whole system is non-fatal, if checkpoint save or load fails, the run continues normally.


## Parallel Workers and the Experiment Loop​


For complex tasks, the planner can decompose work into subtasks with dependency declarations using an XML format:` <subtask id="1" type="write" depends_on=""> ... </subtask>` . The orchestrator parses this, builds execution waves via topological sort (cycle detection falls back to a single wave), and fans out workers across isolated Daytona sandboxes created in parallel via` create_sandbox_pool()` . Write subtasks get isolated sandboxes; read subtasks share the parent. Each worker gets its own budget and timeout.


Failed workers retry once with their full conversation history injected as context. If they fail again, the subtask degrades gracefully — it gets spawned as a completely separate Ash run via the API. Diffs from successful isolated workers are merged into the parent sandbox using` git apply --3way` , with merge conflicts tracked and reported.


When a repo has a scorer configured, Ash enters experiment mode. It scores a baseline, executes the plan, scores again, and compares. If the delta is below a noise threshold it reverts all changes with` git checkout . && git clean -fd` and asks the planner for a different approach. The best-scoring variant's file contents are stashed before revert and re-applied at the end. The experiment history (approach description, score delta, kept/reverted) is fed back to the planner so it doesn't repeat failed strategies.


## Conversation Compaction​


Both planner and worker conversations can grow past 50+ messages during complex runs. We compact them: keep the first message (task description) and last 3 exchanges, summarize everything in between with a Haiku call, and replace the middle with the summary. A subtle bug we hit: after compaction, assistant messages sometimes contained citation blocks referencing web search results that were in the now-removed middle. The API would reject these with "Could not find search result for citation index." We now strip all` citations` ,` server_tool_use` , and` web_search_tool_result` blocks from the tail messages after compaction.


## Tool Analysis at the Boundary​


Every tool call passes through a static analysis layer (` analyze_input` before execution,` analyze_output` after) that returns one of three actions: pass, inject, or block. This catches things prompt engineering alone can't.


**Blocked inputs:** destructive git operations (` git push --force` ,` git reset --hard` ,` rm -rf /` ,` git branch -D main` ), reading a directory path with` read_file` (redirected to` list_files` ). **Injected input guidance:** package installations during work phase get a warning that lockfile changes will pollute the diff; placeholder-looking scorer inputs get flagged.


**Injected output guidance:** empty` query_data` results get debugging suggestions (table empty? filters too strict? no eval data?); empty` search_code` results suggest trying different terms; file reads over 15,000 characters warn about context usage; test failures get "read the error, fix the root cause, don't just re-run" guidance; score results get "record this, compare before/after, >5% delta is meaningful" instructions.


The insight was that guardrails belong at the tool boundary, not in the system prompt. The model can ignore prompt instructions. It can't ignore a blocked tool call.


## The Knowledge Layer​


Ash maintains a knowledge layer so the planner doesn't start from zero every time. A knowledge layer, especially in an evolving field with many parameters, is often very hard to get right. Working with quick-moving teams guarantees that knowledge gets stale quickly, and adding in the complexity of different prompting guidelines, agent frameworks, and development patterns makes the knowledge layer one of the most vital features we needed to develop for ash.


**Ecosystem knowledge.** A scraper indexes documentation for every tool in our catalog. It fetches pages, strips boilerplate, and distills them into structured sections — quickstart, API reference, configuration, troubleshooting. Content hashing avoids redundant re-indexing. The planner gets current docs for every tool it's reasoning about, not whatever was in the training data six months ago.


**Project memory.** Every successful PR records a fix pattern: what the problem was, what worked. The next time Ash sees a similar issue in that repo, the planner gets that history in context. Over time, Ash develops repo-specific intuition about what tends to break and how to fix it.


**Global patterns.** Successful fixes are abstracted across the fleet (with tenant isolation) so Ash learns from everyone. Insights such as "when you see a` tool_arg_error` on a search tool, tightening the schema's enum constraints worked 73% of the time." enrich ash to truly be the best AI engineer across organizations.


**Insights.** Persistent, evolving hypotheses about a repo's agent behavior. Created from proactive scans, auto-closed when fixes land, reopened if the problem recurs. Each insight carries its evidence trail and what's already been tried, so the planner doesn't waste cycles repeating failed approaches.


## What We Learned​


**Research before you touch anything.** Ash's planner is instructed to web search before every prompt edit, tool schema change, or behavior modification. Training data is months old. The field moves fast. This single instruction — "search first, implement second" — was the biggest quality improvement we made.


**The expensive model should never touch a file.** The moment your planning model starts editing files, it gets distracted by implementation details and loses the thread on strategy. Strict separation of planning and execution made both tiers better at their jobs.


**Nudge, don't hope.** Cheaper models need explicit behavioral corrections at runtime, not just better prompts. The worker nudge ("you haven't edited anything — do it NOW"). Runtime steering is infinitely more valuable than subtle prompting fixes for the dumber worker agents.


**Dog-food everything.** Ash emits observability traces into the same ashr observability system it monitors for users. Its own runs show up in the dashboard with per-step timing, token usage, and cost breakdowns. When Ash's quality degrades, Ash can theoretically detect and fix itself. We haven't fully closed that loop yet, but the infrastructure is there.


## Footnotes​


1.


[Terminal Bench 2.0 Leaderboard](https://www.tbench.ai/leaderboard/terminal-bench/2.0) — an independent coding agent benchmark where third-party agents built on the same foundation models routinely outperform the model providers' own coding agents.↩


2.


[W&B Sweeps](https://docs.wandb.ai/guides/sweeps) — Weights and Biases' automated hyperparameter optimization tool that runs parameter searches based on experiment results.↩
