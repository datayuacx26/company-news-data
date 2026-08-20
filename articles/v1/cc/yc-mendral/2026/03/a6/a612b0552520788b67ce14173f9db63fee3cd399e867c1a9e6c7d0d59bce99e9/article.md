---
schema_version: "1.0.0"
document_id: "a612b0552520788b67ce14173f9db63fee3cd399e867c1a9e6c7d0d59bce99e9"
company_key: "yc-mendral"
company: "Mendral"
source_id: "yc-mendral-news-import-538be81b6878"
canonical_url: "https://mendral.com/blog/frontier-model-lower-costs"
published_at: "2026-03-06T00:00:00+00:00"
first_seen_at: "2026-08-10T00:32:33.388808+00:00"
fetched_at: "2026-08-10T00:32:34.981599+00:00"
content_hash: "sha256:6693516a242612d9422bf440fc829ccad033563cac63ed0eaf207e383c6ab002"
---

# We Upgraded to a Frontier Model and Our Costs Went Down

Last week we wrote about[feeding terabytes of CI logs to an LLM](https://mendral.com/blog/llms-are-good-at-sql) . Most of the questions on Hacker News weren't about the logs. They were about the agent: which models, how they coordinate, and how much it all costs.


Today we run Opus 4.6 and pay less than when we ran everything on Sonnet 4.0.


The reason is mostly what Opus *doesn't* do: 80% of failures never reach it, and when they do, it never reads a log line.


The architecture looks like this:


## Let a cheap agent decide if the expensive one is needed


Last week we analyzed around 4,000 CI failures. 818 were new problems. The other 3,187 were a known issue surfacing again: a flaky test, an infrastructure hiccup, a network blip we'd already detected.


It makes no sense to wake up an expensive model when 80% of the time the answer is "it's a duplicate". Unfortunately, we can't deterministically detect duplicates: the same job can fail multiple times for completely different reasons, so you need to actually look at the logs to know if you've seen this before.


We initially used Sonnet for this to balance cost and performance. It worked, but it was the worst of both worlds: still expensive, and the results weren't as good as a frontier model.


We switched to the "triager" pattern: a Haiku agent with a very specific and narrow job. Is this issue already tracked or not? If it is, stop right there. If not, escalate to Opus.


Detecting duplicates with Haiku proved a bit challenging. We needed to make the job as easy as possible, so we attached error messages to previous failures and gave Haiku two search tools: exact matching for known error snippets, and semantic search (pgvector) for similar-but-not-identical errors. RAG is dead, but semantic search is pretty neat.` operator does not exist bigint character varying` and` migration type mismatch on installation_id` are different strings but the same root cause, and semantic search surfaces that.


The Haiku agent reads the logs, searches error messages, tries to match against known failures, and makes a call. When in doubt, it escalates. A false positive costs a little money; a false negative means we miss something real.


4 out of 5 failures never reach Opus. A triager match costs around 25x less than a full investigation.


## Let the agent pull context, don't push it


Several people asked how we handle logs that are 200K+ lines. We don't push them into the prompt. We give the agent a SQL interface to ClickHouse and let it ask for what it needs.


The reason isn't just token cost. If you hand an agent a specific set of log lines, you've already made a judgment about what's relevant before you know what the problem actually is. The agent anchors to what you gave it. If the real cause is somewhere else, you've made it harder to find. It's the same reason you don't want to lead a debugging session by saying "I think the problem is in this file": you've biased the investigation before it started.


We[wrote about the SQL setup in detail last week](https://mendral.com/blog/llms-are-good-at-sql) , but the short version: there's one table with raw data (` github_logs` , one row per log line) and a set of materialized views with pre-aggregated data: failure rates by workflow, job timings, outcome counts. Most investigations start with the materialized views to narrow down the cause, then drill into raw logs when they need to.


We don't tell the agent which table to query. Instead, we use the responses themselves to guide it progressively. If a query returns too many rows, we truncate and suggest a more specific materialized view. If logs aren't ingested yet, we point it to the GitHub CLI. The agent figures out what it needs without us having to anticipate every path in advance.


## Expensive agents plan, cheap agents do the work


Opus looks at what failed, forms a hypothesis, and spawns Haiku sub-agents to do the actual digging. Each sub-agent gets a prompt from Opus: exactly what to search, how to search, what to return. Sub-agents are capped at one level deep; they can't spawn sub-agents of their own. Unbounded fan-out is how you get runaway costs.


A few weeks ago three Storybook CI jobs failed on the same commit, all crashing at` pnpm install` .


Opus started by asking a sub-agent to fetch the error messages from the failing pnpm install step. ClickHouse didn't have the logs yet, so the sub-agent fell back to the GitHub CLI.


**Sub-agent #1** prompt:


> Fetch the CI logs for this run. Return the exact error messages from the pnpm install step, the full error output, especially the last 50-100 lines.


Result:` gyp ERR! not found: make` .[\[email protected\]](https://mendral.com/cdn-cgi/l/email-protection) couldn't compile because` make` wasn't on the runner.


Opus searched existing insights (no match), then queried ClickHouse for the failure trend over 14 days:


```text
Feb 23: 0.2% failure rate
Feb 24: 1.1%
Feb 25: 8.0%  <- inflection point


```


Something clearly changed on Feb 25. Opus spawned **Sub-agent #2** :


> Investigate what changed around Feb 24-25. Failure rate went from 0.2% to 8%. The error is` gyp ERR! not found: make` . Run git log on the workflow file and package.json for that window.


Build dependencies had been removed during an unrelated migration. Correct for that migration, but re2 still needed` make` to compile natively. Opus spawned **Sub-agent #3** to verify the current workflow state, then created the insight with root cause and fix.


The orchestrator never read a line of logs, git history, or code itself.


A few things worth noting:


**Cost.** Haiku handles ~65% of all input tokens but only ~36% of our LLM spend. **The expensive model thinks; the cheap model reads.** Without the model hierarchy, the daily bill more than doubles.


**Opus plans as it goes.** It starts with a hypothesis, but each sub-agent's results shape the next step. In this investigation it got the error, searched history, then asked what changed. Each round informed the next. Over a third of our investigations go multi-round, and new problems need roughly twice the investigation depth of known ones.


**Context hygiene.** The orchestrator's context stays clean: structured summaries from sub-agents, not raw log output. Each sub-agent starts with a clean slate and its context is discarded when it's done. Tool call output accumulates fast, and stale context from earlier in a session degrades decisions later.


**Directed search.** "Return the exact error messages from the pnpm install step" is a very different prompt than "analyze these logs". Opus decides what to look for; Haiku finds it. Haiku's input/output ratio is 86:1 (reads a lot, returns focused extracts), while the orchestrator is around 50:1 (synthesizes and decides). Haiku absorbs the data so Opus doesn't have to.


## This wasn't possible 6 months ago


Six months ago we were on Sonnet 4.0. It struggled to write correct ClickHouse queries: wrong tables, missing filters, reading far too much data. Haiku 4.0 wasn't useful for anything beyond yes/no classification.


Today Opus 4.6 can plan investigations and write precise sub-agent prompts. Haiku 4.5 can handle narrow, directed tasks because the tasks are scoped tightly enough that a fast cheap model can execute them.


Upgrading to a frontier model made costs go down.


## The pattern generalizes


We built this for CI logs but the pattern applies to anything with high event volume: security logs, IoT telemetry, financial data. Most events are noise or repeats, and the expensive model should only see the ones that aren't.


There's a fourth layer we haven't covered: reassessment. The system periodically checks whether what it concluded is still true, closing stale insights, catching false positives, verifying that fixes worked. That's a post on its own.


We're still tuning where the sub-agent boundary sits. Sometimes spawning a sub-agent costs more than doing it inline because the setup overhead outweighs the savings.


The hardest part wasn't making the agent smarter. It was building the layers that stop it from running when it shouldn't.
