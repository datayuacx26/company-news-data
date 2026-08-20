---
schema_version: "1.0.0"
document_id: "2e26c01a6bdc9de6c84fe186c4e069fb1b220d4a12c58bb5f1c211f79f09e3c5"
company_key: "yc-mendral"
company: "Mendral"
source_id: "yc-mendral-news-import-538be81b6878"
canonical_url: "https://mendral.com/blog/how-we-built-our-ai-agent"
published_at: "2026-02-26T00:00:00+00:00"
first_seen_at: "2026-08-10T00:32:33.388808+00:00"
fetched_at: "2026-08-10T00:32:34.981599+00:00"
content_hash: "sha256:6ff2041d51c140792ab1af2ac045434cb13a4c2bc0e983777bd71098d4ab0615"
---

# We use Claude Code daily. We still built our own CI agent.

Last month, a team running 200K+ CI jobs per week asked us why they shouldn't just point Claude Code at their failing builds. Fair question. We use Claude Code every day. After watching Mendral close 16,000+ CI investigations a month autonomously, here's why a specialist agent outperforms a generalist one, even when both run on the same Anthropic models.


## Why we're building this


Coding agents are great for shipping features fast. They're terrible for CI.


Teams adopting AI coding tools are seeing significantly more CI activity. More PRs, more test runs, more failures surfacing. Pipelines are slower because there's more code being tested. Flaky tests that were annoying at 10 engineers become a tax on everyone's productivity at 100. The engineers generating all that code with Copilot and Claude Code aren't the ones debugging the CI failures. They've already moved on.


We[spent a decade building and scaling CI systems at Docker and Dagger](https://www.mendral.com/blog/ci-at-scale) . The work was always the same: stare at logs, correlate failures, figure out what changed. Mendral is the agent we wished we'd had.


## Specialist vs. generalist


Claude Code is a generalist software engineer. Mendral is a specialist. Despite running on the same Anthropic models, Mendral consistently outperforms Claude Code at diagnosing and fixing CI failures, because the useful signal isn't in the code.


When a CI job fails, the signal is in the logs from this run, the logs from the last 50 runs, the test execution history, the failure patterns across branches, and the infrastructure conditions at the time of execution. Claude Code doesn't have access to any of that.


We built a[log ingestion pipeline](https://www.mendral.com/blog/llms-are-good-at-sql) that processes billions of CI log lines per week into ClickHouse, compressed at 35:1 and queryable in milliseconds. Our agent writes its own SQL queries to investigate failures. A typical investigation scans 335K rows across 3+ queries. At P95, it scans 940 million rows. The agent can trace a flaky test back to a dependency bump three weeks ago by correlating across hundreds of CI runs at once, something no human would have the patience to do.


The whole implementation is ours, from the system prompt to every tool. Our agent can grab specific logs from a run, query historical failure rates across months, trace which commit introduced a regression, check if a test has been flaky on other branches, and cross-reference all of this in seconds. Claude Code can't, because it doesn't have the tools or the data.


## One agent to the customer, a team of agents behind the scenes


From the outside, Mendral is one agent. You install a GitHub App, it joins your Slack, and it starts investigating CI failures. Internally, it's a team of specialized agents coordinating through our Go backend.


We use all three Anthropic tiers (Haiku, Sonnet, Opus). Using the wrong model for a task is either wasteful or insufficient.


Opus handles root cause analysis and implementation. When the agent forms a hypothesis about why a test is failing, reasons about complex interactions between test suites, or writes a non-trivial fix that touches CI configuration and test code at once, Opus takes over. The cost is higher. For root cause work, the quality justifies it.


Sonnet collects facts and deduplicates issues. It reads logs, writes SQL queries, gathers evidence from the repository, and correlates failures with code changes. Sonnet is the right balance of intelligence and cost for structured, evidence-gathering work.


Haiku handles log parsing and data extraction: classifying failure types, formatting structured output, extracting relevant snippets from raw logs. The solution space is constrained and we need throughput. We process thousands of these per day.


Routing is something we keep iterating on. Work that required Sonnet six months ago sometimes runs fine on Haiku today, so we re-evaluate model assignments regularly. A full investigation might involve a dozen sub-agent calls across all three tiers.


## The agent loop


Our agent loop runs on our Go backend. We don't use LangChain, LangGraph, or any off-the-shelf agent framework. We need full control over execution, concurrency, and failure handling.


The core loop is straightforward: the agent receives a trigger (a CI failure, a Slack message, a scheduled analysis), assembles context, makes an LLM call, processes tool calls, and iterates until it reaches a conclusion or exhausts its budget.


Some tools are pure Go functions. Querying ClickHouse, fetching GitHub metadata, looking up repository structure, checking PR status. These are fast, deterministic operations that don't need isolation. They run in-process.


Some tools require a sandbox. When the agent needs to clone a repository, run tests, apply patches, or execute arbitrary code to validate a fix, it needs an isolated environment. We provision Firecracker microVMs on[Blaxel](https://blaxel.ai/blog/mendral-builds-the-first-24-7-ai-dev-ops-engineer-using-blaxel) for this. Each sandbox is a lightweight VM with its own kernel, with hardware-level isolation between tenants. The sandbox boots in under 125ms, the agent operates on it, and when the session ends, the sandbox is destroyed.


Between tool calls, the sandbox is suspended. The agent doesn't hold compute while it's thinking. When the LLM returns the next tool call, the sandbox resumes in under 25ms with full filesystem and memory state preserved. A single investigation can involve 10+ tool calls with LLM reasoning in between, and paying for idle compute during inference would be wasteful.


There's another pattern specific to CI: the agent sometimes needs to wait hours for a pipeline to complete after pushing a fix. The sandbox suspends during that wait. When CI finishes, the sandbox resumes with full state intact. Without suspend/resume, you'd either pay for hours of idle compute or lose the entire execution context and start over.


## Plan for messy LLMs


Models are the easy part. Everything around them is hard.


LLM APIs are slow. A single Sonnet call takes 2-10 seconds depending on context size. Opus can take 30+ seconds for complex reasoning. Tool calls hit external APIs (GitHub, Slack, ClickHouse) that have their own latency and failure modes. A single CI investigation involves 10-20 LLM calls and 30-50 tool executions. The whole chain takes minutes, and any step can fail.


An LLM call that fails costs you the entire accumulated context if you have to start over. A GitHub API call that times out after you've already spent 30 seconds on an Opus reasoning step is expensive to retry from scratch. The failure modes compound: rate limits, network timeouts, API errors, malformed LLM output, context window overflows.


We solve this with durable execution. Both our agent loop and our data ingestion pipeline run on[Inngest](https://www.inngest.com/) . Every meaningful operation is a step that can be retried independently. If a GitHub API call fails on step 7 of a 15-step investigation, we retry step 7, not the entire investigation. The state of all previous steps is persisted and memoized.


Without durable execution, you build your own retry logic, state recovery, and deduplication for every function. Every interrupted operation needs to be reconciled. With Inngest, a rate limit response from GitHub is just a pause. We read the` Retry-After` header, add jitter to avoid thundering herd, and suspend execution. When the wait is over, the function resumes at exactly the point it left off. No re-initialization, no duplicate work.


We break agent functions into steps at every boundary that can fail: LLM calls, API calls, database writes, sandbox operations. Each step is individually retried with configurable backoff. The agent doesn't crash on transient failures, doesn't redo expensive LLM calls because a downstream API hiccupped, and doesn't lose state when infrastructure restarts.


## A decade of CI expertise, encoded


A powerful model plus the right tools doesn't equal good performance. Prompts, tools, and the data you feed the model all work together. The expertise isn't in any one piece. It's in how they combine.


We spent a decade debugging CI at Docker and Dagger. We know the patterns: race conditions in parallel test execution, shared state between test suites that causes order-dependent failures, infrastructure variance that makes timing-sensitive tests fail on slower runners, dependency resolution differences between CI and local environments, cache invalidation bugs that only show up under specific build orders.


All of that is encoded in Mendral: in the prompts, the tools, the SQL queries, and the order in which the agent retrieves data during an investigation. The agent knows that a test failing intermittently on CI but passing locally is almost never "random." It knows to look at resource constraints, concurrent test execution, and shared state before blaming the code. It knows that a sudden spike in failures after a dependency bump is likely a transitive dependency issue, not a flake.


Every Mendral session, the customer gives a thumbs up or thumbs down on the result. We track these signals across all sessions and use them to find where the agent's reasoning breaks down. When we see a pattern of negative feedback on a specific failure type, we update the prompts and tools. PostHog alone runs 575K CI jobs per week and 1.18 billion log lines through Mendral. Each customer pushes the agent in different ways and surfaces edge cases we couldn't anticipate.


We're not done. Routing between tiers is still partly heuristic, and we don't always know in advance when an Opus call will be worth the latency. The model assignments shift every few months as the underlying models improve. Some failure types we still misclassify. The system gets better with feedback, but it's not finished.


## Observability


You can't improve what you can't see. Every LLM call, every tool execution, every decision point is traced. We log the full prompt, the response, the tool calls, the results, and the time each step took. When a session produces a bad diagnosis, we replay the exact sequence of decisions and find where the reasoning went wrong.


"The agent gave a bad answer" is not actionable. "The agent queried failure rates for the wrong time window at step 4, which caused it to misclassify a regression as a flake at step 7" is.


We version our prompts and tools together. A prompt change ships with corresponding tool changes and evaluation results. If a new prompt version causes a regression in diagnosis quality, we pin it to the exact change and roll back.


## Back to the question


When that team asked us why they shouldn't just point Claude Code at their CI, the honest answer is: Claude Code is a great generalist, but it doesn't have the data. Our database holds 1.18 billion log lines from one customer alone. Our agent loop has been retrying around real-world API failures for a year. Our prompts encode CI patterns we learned over a decade at Docker and Dagger. Pointing a generalist at the same problem won't get the same result, even with the same models underneath.
