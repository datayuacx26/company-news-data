---
schema_version: "1.0.0"
document_id: "f234a6411565c200165227426f9e34fe3897e2c65105168d841d3c021def864a"
company_key: "yc-verbiflow"
company: "Verbiflow"
source_id: "yc-verbiflow-news-import-392b5e1f9020"
canonical_url: "https://verbiflow.com/blog/benchmark-agent-fanout-vs-sdk"
published_at: "2026-06-09T00:00:00+00:00"
first_seen_at: "2026-07-22T18:39:30.762214+00:00"
fetched_at: "2026-07-28T21:42:42.932365+00:00"
content_hash: "sha256:8e39bc2f219801d2f266302e844fd8c242f4c7512a04806766bd3e3c0a66cdd5"
---

# We tested our SDK against 10 Claude subagents. Our SDK ran 1,165% faster.

There are two reasonable ways to run batch outbound work from a coding agent. You can have Claude Code write one job using the Verbiflow SDK. Or you can have Claude Code send the work to 10 Claude subagents and let each one handle one company. We ran the same task both ways: find every security certification claimed on the trust pages of 10 well-known SaaS companies, with the same SDK, model, and task. Here’s what we saw.


## Wall clock


Total time to extract certifications across 10 companies


Our SDK


31 s


10 Claude subagents


392 s


The SDK ran 1,165% faster: 31 seconds instead of 392. Same AI model, same SDK calls, same 10 companies.


## Retries needed


Retries needed before all 10 companies completed


Our SDK


0


10 Claude subagents


4


3 of the 10 agents refused the first dispatch as “suspected prompt injection.” Same prompt, same workload as the 7 that ran it without issue.


The SDK kept the repeated work in one batch. The Claude subagents turned it into ten isolated runs, which added retries.


## Same answer


Security certifications found across all 10 companies


Our SDK


85


10 Claude subagents


87


Within run-to-run search noise. The 2-cert delta came from slightly better trust-page URLs for Figma and Snowflake in the Claude subagent run. Re-run the SDK job and the gap can flip directions.


## What the test shows


If you are analyzing one company, asking an agent to do it directly is probably fine. The setup cost of building one job may not be worth it for a one-off task.


Outbound work is rarely one company. Once the same operation needs to run across 10, 100, or 1,000 accounts, the tradeoff flips. You pay the setup cost once, and every additional company after that runs faster and with fewer failures.


The agent version treated 10 companies like 10 separate jobs. The SDK version treated them like one batch.


## The principle


The SDK version still uses LLM calls for extraction, classification, and reasoning where needed. The difference is that those LLM calls run inside one structured job with concurrency, caching, retries, and shared state instead of inside 10 separate agent loops.


> Use the agent to design the workflow. Use the SDK to run the repeated work, including the LLM steps.
