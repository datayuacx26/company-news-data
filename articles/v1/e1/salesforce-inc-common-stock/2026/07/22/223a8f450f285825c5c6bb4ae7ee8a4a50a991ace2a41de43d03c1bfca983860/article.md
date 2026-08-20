---
schema_version: "1.0.0"
document_id: "223a8f450f285825c5c6bb4ae7ee8a4a50a991ace2a41de43d03c1bfca983860"
company_key: "salesforce-inc-common-stock"
company: "Salesforce Inc."
source_id: "salesforce-inc-common-stock-rss-5706a1a21901"
canonical_url: "https://engineering.salesforce.com/building-reliable-production-ai-with-durable-workflows/"
published_at: "2026-07-27T22:26:18+00:00"
first_seen_at: "2026-07-27T23:30:35.959996+00:00"
fetched_at: "2026-07-28T20:31:35.420648+00:00"
content_hash: "sha256:296eb1d062b6356e3b7c42b1f7135c204e1ca8e5458a8919d1777b5ddb50d30a"
---

# Building Reliable Production AI with Durable Workflows

Building an AI prototype has never been easier. You send a prompt to a model, receive a response, and present the result to a user. As long as each request is independent, the interaction is straightforward to reason about, test, and debug. That is why so many early AI applications feel deceptively simple.


Production AI systems tell a different story. Imagine generating AI output across 10,000 records. Thousands of model calls begin executing simultaneously. Some finish immediately, while others encounter rate limits, transient failures, validation errors, or long-running tool invocations. Then, halfway through execution, a worker restarts or a deployment rolls out. Suddenly the question is not whether the model generated the correct response. It is whether your system knows what completed, what is still running, and what can be safely retried without duplicating work. At that point, you have stopped[debugging](https://engineering.salesforce.com/reducing-agentforce-ai-debugging-from-two-weeks-to-same-day-with-query-driven-observability/) AI behavior and started debugging distributed execution.


This is the transition every successful AI platform eventually makes. The challenge goes beyond producing intelligent responses: it is ensuring that intelligence survives long-running execution, partial failures, deployments, retries, and changing inputs without forcing users to start over. The lesson from building Agentforce Grid is that production AI is not fundamentally a prompting problem. Reliable production AI depends on durable workflows that preserve execution across failures.


Production AI systems differ from prototypes because they rely on long-running AI workflows rather than isolated model calls. Those workflows must coordinate execution state, retries, recovery, and progress across thousands of independent operations. Designing those workflows correctly can have a greater impact on reliability than choosing the model itself.


An[AI workflow](https://engineering.salesforce.com/agentforces-agentscript-building-deterministic-control-for-enterprise-ai-workflows/) can take many forms. It might be a multi-step agent that reasons across tools and records, a chain of prompts that classifies, summarizes, validates, and writes back output, a batch evaluation across test cases, or a Grid column generating AI output across thousands of rows. What these patterns have in common is that the work no longer fits inside a single prompt-response interaction. Once AI execution spans multiple steps, external systems, retries, or user-visible progress, it becomes a workflow that needs durable orchestration.


##### **Why do successful AI prototypes become fragile production AI systems?**


Most AI systems begin with the same mental model: a request comes in, the application builds a prompt, the model returns a response, and the application moves on. That model works because each interaction is independent.


It starts to break down once work spans thousands of operations. Consider a user generating AI output across 10,000 rows. Each row may depend on source data, previous outputs, application data, external tools, or other model responses. Some rows finish quickly. Others encounter latency, transient failures, or validation errors. What originally appeared to be one AI request has quietly become a long-running AI workflow composed of thousands of coordinated pieces of work. The same pattern shows up in long-running agents, chained prompt pipelines, tool-using automations, and evaluation workflows.


It is tempting to treat these as isolated operational problems solvable with better logging, monitoring, or retry logic. In reality, they are symptoms of the same underlying assumption: the application is still behaving as though it is processing one request, even though it is coordinating a long-running execution. Once that distinction becomes clear, the failures stop looking random and start pointing to a more important question: **what exactly is the unit of work your system is trying to execute?**


##### **Is the failed row really the problem?**


Imagine a background job successfully processes 8,431 rows before a worker crashes while handling row 8,432. At first glance, the failed row appears to be the problem.


Look more closely. How does the system know whether row 8,432 actually failed? What if the model generated a response successfully but the worker crashed before recording completion? What if the output reached the database but the status update did not? Can the application safely retry the row, or will it duplicate work that already finished? These questions aren’t really about the row. They are about the execution that produced it.


This is exactly the situation encountered while designing Grid. A straightforward implementation loads every row, resolves its inputs, calls the model, writes the output, and repeats until every row has finished. The design is easy to understand because the operation is treated as one large background job. The difficulty appears only after something interrupts that execution. Once the process disappears, the system has to reconstruct progress from logs, database state, and partial outputs, often without enough information to determine what actually happened.


The failed row is not the real problem. It is simply where the uncertainty becomes visible. The deeper challenge is that the application has no explicit representation of the execution itself. If recovering an entire job feels too expensive and recovering an individual model call feels too granular, the next question naturally follows: **what should the system actually recover?**


##### **What is the right unit of work?**


Finding the right recovery boundary is one of the most important design decisions in a production AI system. Treat the entire execution as one unit of work and a retry may repeat thousands of successful model calls. Break every tiny operation into its own workflow and orchestration overhead quickly becomes difficult to manage. Neither extreme reflects how engineers or users actually think about progress.


*From one large job to recoverable execution units.*


One way to solve this is to model execution as a hierarchy, which is the approach Grid took. A column run represents the user’s overall request. Rows or batches become meaningful execution units. Within each unit, smaller activities resolve inputs, build prompts, call models or tools, validate responses, write outputs, and update progress. This structure isolates failures without forcing unrelated work to restart, while keeping execution aligned with how users understand the operation they initiated.


The model itself didn’t become more reliable. The execution became easier to reason about because the system had clear boundaries around what could fail, what could be retried, and what had already completed. That leads to a broader engineering principle that applies well beyond Grid: the right retry boundary is usually the smallest meaningful unit of recoverable work.


Solving retry boundaries exposes an even more fundamental question. **If work is divided into smaller, recoverable pieces, where does the system remember their state after the original process disappears?**


##### **Where should AI workflow execution state actually live?**


Breaking a large execution into smaller units solves only part of the problem. If a worker restarts halfway through a run, how does the next worker know which units completed, which failed, and where execution should resume?


Many systems answer that question by reconstructing progress from logs, database state, and timestamps. That approach works while failures are rare, but it becomes increasingly fragile as executions grow longer and more complex. Every new failure mode requires additional recovery logic, making the system harder to reason about over time.


The breakthrough came from reframing the problem. Instead of treating execution as something a running process temporarily manages, Grid treats execution itself as durable state. Durable workflows make that possible by preserving execution history outside the lifecycle of any single worker. Rather than asking a worker to remember where it left off, the execution records its own history. The worker becomes replaceable but the execution does not.


Grid implements this by persisting execution history outside the lifecycle of any single process. In Grid, that durable execution layer is built on[Temporal](https://temporal.io/blog/very-long-running-workflows) , which allows long-running work to be modeled as workflows and activities with persisted history, retry policies, and recovery boundaries. A column execution becomes a parent workflow, while row and batch-level operations become child workflows and activities with their own retry behavior. If a worker disappears, another worker resumes from durable state instead of reconstructing execution from scattered evidence.


*Durable workflows preserve execution across failures.*


The impact showed up clearly in internal performance testing. With ten concurrent users and 200 rows per worksheet, columns still running on the pre-migration synchronous path failed roughly 90% of the time under load. The retry storm lasted more than ten minutes before surfacing a permanent failure, and each attempt reprocessed work the system had no durable record of. In the same test, columns that had migrated to Temporal-backed durable orchestration completed with 0% failure. For the Prompt Template migration specifically, P95 completion time improved by roughly 60% while eliminating that failure class. These numbers come from an internal performance environment and should be reviewed before external publication.


The broader lesson extends well beyond Grid. Production failures are often difficult not because the business logic is complex, but because the system no longer knows the history of its own execution. Treating execution as durable state eliminates that uncertainty.


##### **Why isn’t retrying an AI workflow a sufficient recovery strategy?**


Once execution becomes durable, retries can be aligned with the same boundaries that define recovery. Consider a model call that times out while processing a single row. Retrying the entire column execution repeats thousands of successful operations, consumes additional model quota, and risks overwriting correct outputs. Retrying every tiny instruction independently creates unnecessary orchestration overhead. Neither approach reflects how the work is actually organized.


Grid instead assigns retry behavior to meaningful execution boundaries. Activities define retry policies, exponential backoff, maximum attempts, and non-retryable error types. A transient model timeout retries only the affected activity. Validation failures remain isolated to the row that failed, while idempotent writes prevent completed work from being duplicated if a worker disappears before recording success.


The mechanism underneath is simple but important: each batch checkpoints after every successful row, and retries up to ten times with exponential backoff. When a retry fires, execution resumes at the first row that has not yet completed. Rows that already produced a valid output are not re-executed, and their model quota is not spent twice.


The principle applies to any production AI system. Before implementing retries, decide what your system should be allowed to repeat. The safest retry strategy is the one that matches the natural boundaries of your execution model.


##### **How do users learn to trust long-running AI?**


Recovering correctly is only part of reliability. Users also need confidence that the system understands its own progress. A single “Running…” status reveals very little about a long-running AI operation. Users cannot tell whether work is advancing, retrying, or stalled, and engineers face the same uncertainty during production debugging.


Grid addresses this by exposing execution at multiple levels. Cells report the status of individual generated values, rows aggregate work for a single record, columns represent the user’s requested execution, and worksheets summarize overall progress. This layered model allows both users and engineers to distinguish completed, failed, and retryable work without reconstructing execution manually.


The implementation is specific to Grid, but the lesson is universal. Progress should mirror the same execution boundaries that define recovery. When those models stay aligned, failures become understandable instead of mysterious.


##### **What should you design before choosing a model?**


One of the biggest lessons from building Grid is that the most important architectural decisions often have nothing to do with the model itself. Once AI becomes part of a production workflow, it inherits the same distributed systems challenges every large-scale platform faces: state management, concurrency, retries, observability, deployments, idempotency, and recovery.


That realization changes how you evaluate an AI architecture. Instead of starting with questions like “Which model should we use?” or “How should we write the prompt?”, start with different ones. What is the smallest meaningful unit of recoverable work? What state must survive a worker restart? Which operations are safe to retry? How will users know what completed successfully and what still requires attention? Those questions determine whether your AI application behaves like a production platform or a sophisticated demonstration.


The next time you’re designing an AI system, don’t start by choosing the model. Start by imagining that execution reaches row 8,432 before something unexpected happens. If your first instinct is to rerun everything, you’re still designing around model execution. If your first instinct is to ask what state survived, you’ve started designing for production.


Durable workflows make production AI reliable because they preserve execution when individual processes fail. The model generates intelligence. Your execution model determines whether that intelligence can be trusted.


##### **Learn More**


- Stay connected by joining our[Talent Community](https://flows.beamery.com/salesforce/eng-social-2023) .
- Explore our[Technology and Product](https://www.salesforce.com/company/careers/teams/tech-and-product/?d=cta-tms-tp-2) teams to see how you can get involved.
