---
schema_version: "1.0.0"
document_id: "bd417a0e5980700c109af4d30ab87c2eaa7ce4e2358042b643d40257f67e7b8c"
company_key: "yc-corelayer"
company: "Corelayer"
source_id: "yc-corelayer-rss-a6386e25c52b"
canonical_url: "https://www.corelayer.com/blog/production-data-eng-is-hard"
published_at: "2026-01-09T16:34:24+00:00"
first_seen_at: "2026-07-20T23:20:24.241390+00:00"
fetched_at: "2026-07-28T21:58:18.576112+00:00"
content_hash: "sha256:5b7ca2bb733ab7eee40fcb88cefb0ea0b679b39bbc6776a0020773369cd2bbf5"
---

# Why Production Data Engineering is Hard and Where AI Agents Can Help

Data pipelines are particularly painful to debug. They don’t always fail loudly. Data correctness issues are sneaky and subtle, and you may not even know there’s an issue until your users start complaining.


The root cause is often some change in an upstream dependency that’s out of your control. Complexity compounds because every incremental node in your DAG explodes the number of failure paths. Data engineers have to live with this complexity, and somehow still find time to actually ship new products.


What if AI agents could do the worst parts of your job for you? You’d spend significantly more time building cool, useful data products and less time on support. We built a multi-agent system with this as our North Star.


High-level multi-agent system


Here’s how an end-to-end investigation works:


**1. Detect → filter → group**


- We watch logs + DAG runs + metrics + datasets for failures, spikes, exceptions, and statistical anomalies.
- New signals go through noise filtering (e.g., known-benign patterns/flaky retries) using runtime context (job metadata, surrounding logs) with Gemini 2.5 Flash Lite.
- We collapse events into issue candidates using deterministic fingerprinting (stable signatures like error + task + resource) + semantic similarity (near-duplicates across open issues).


**2. Orchestrate the run**


- A candidate spins up the main orchestration agent (Claude Sonnet 4.5).
- First step is fetching a system “map” (your DAG/service graph + where relevant tables, logs, code, and docs live).
- Then it queries issue memory via semantic search over closed issues (patterns + outcomes) to bootstrap hypotheses and known fixes.
- The agent maintains a lightweight scratchpad (plan + hypotheses + what nodes have been checked in the “map”) and iterates as evidence arrives.


**3. Gather evidence with subagents**


- Data subagent (Gemini 2.5 Flash Lite): targeted sampling + queries across upstream/downstream tables; checks invariants like null rates, uniqueness, ranges, and “what changed” diffs.
- Logs, code, and metrics subagents (Gemini 2.5 Flash Lite): correlate failures with deploys/config changes, recent commits, exceptions in logs, and spikes in metrics.


**4. Produce a decision + next steps**


- The orchestrator returns a structured report: what happened, impacted assets, severity, the most likely root cause, confidence + evidence, and recommended steps to fix + prevent.


**5. Code changes + transparency**


- Users have a “Create PR” button for issues that require a code fix.
- This triggers a PR-creation subagent (Claude Sonnet 4.5) which opens a PR from a new branch with the change.
- Every investigation writes a case file: steps taken, queries run, logs inspected, and code referenced with citations so you can verify or continue the investigation yourself.


We think this is all pretty intuitive. When in doubt, we ask “what would a good human engineer do in this situation?” Now, getting a multi-agent system like this to work reliably in production is a separate challenge.


We attribute the success of our agent to these three principles:


**1. Evals**


This should be obvious if you’ve built with LLMs. We built our eval suite on top of promptfoo, and the big lesson was: any step can be the weak link in a multi-agent system.


- Write evals for every LLM call in the system.
- Start with a small set of basic evals. Whenever you see unexpected behavior in prod, turn it into an eval.
- All prompt or model changes should be grounded in eval performance.


**2. Guardrails**


You need controls that prevent the agent from spiraling, especially over longer time horizons and in edge cases.


- Limit the total number of iteration steps per agent.
- Every prompt needs an “out” — explicit instructions on how the model should respond when it doesn’t know the answer.
- Track token spend per step/tool and review it regularly. This has helped us surface issues in the agent.


**3. Optimize for a great human-in-the-loop experience**


This one is the most important. The output of the system has to be useful even when the agent only gets you 80% of the way there.


- Expose the agent’s steps and reasoning trace.
- Cite sources (links to logs, code, queries) so you can verify or continue the investigation yourself if necessary.
- Keep everything read-only by default, and require human approval for anything that changes code or state.


***Bonus* : temperature. This was counterintuitive for us.**


We started with temperature 0 everywhere because we thought we’d get less hallucination and more consistent output. We discovered that the main agent/subagent models actually perform better in function calling, error recovery, and what you might call “flexibility” with temperature set closer to 1 (0-2 scale).


Even with all of this, end-to-end agent evaluation and realistic environment simulation, especially with data, is very hard. AI agents can’t remove the inherent complexity of production data engineering, but given the right context and harness, they can manage it for you so you can get back to building.
