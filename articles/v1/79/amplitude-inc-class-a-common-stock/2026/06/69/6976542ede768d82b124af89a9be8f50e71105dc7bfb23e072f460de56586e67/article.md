---
schema_version: "1.0.0"
document_id: "6976542ede768d82b124af89a9be8f50e71105dc7bfb23e072f460de56586e67"
company_key: "amplitude-inc-class-a-common-stock"
company: "Amplitude Inc."
source_id: "amplitude-inc-class-a-common-stock-news-import-1333a773138e"
canonical_url: "https://amplitude.com/blog/ai-evals-for-product-managers"
published_at: "2026-06-05T00:00:00+00:00"
first_seen_at: "2026-07-22T23:44:16.700638+00:00"
fetched_at: "2026-07-28T21:42:44.602232+00:00"
content_hash: "sha256:8cf254747a5563ccb141428671f1f6a9c02a313f4e46fc1f41348ba49064f9a1"
---

# AI Evals for Product Managers: A Beginner’s Guide to Getting Started

For most products, the things you used to measure held still. A user clicked a button, submitted a form, loaded a page, and your analytics recorded the same event every time.


AI agents are a different kind of surface. Instead of clicking through flows you designed, users type their intent straight into a chat box, and the agent responds in ways you might not expect. The inputs are infinite, and the outputs are nondeterministic: Ask the same question, and you’ll get different answers. The product analytics built for clicks and form submits never sees inside the messy middle.


AI evaluations (evals) help you close that gap. Evals are how product teams measure and improve agent quality. Becoming fluent in them will define the product manager craft. If you treat evals as a chore, you’ll ship features you can’t measure, debug, or defend. If you treat eval design as part of your job, you’ll own the loop between agent quality and business outcomes.


This page covers the core concepts of evals, with deep dives linked throughout.


## What is a trace?


A[trace](https://amplitude.com/explore/analytics/what-is-an-ai-trace) is the complete record of an agent interaction, capturing everything the agent did between a user’s request and the agent’s response, including every tool call, retrieved context, and model response. Each step in that record is a span, such as a single tool call.


**Example** : A user asks your support agent, “Why was I charged twice?” The agent calls a billing lookup tool, retrieves the transaction history, calls a policy lookup tool, and generates an answer. The trace captures everything: what the user typed, what the agent retrieved, which tools fired, what they returned, and what the agent said back. When the user says the answer was wrong, you open the trace and see that the billing tool returned a duplicate row the agent didn’t catch, a fixable retrieval bug that your session recording would never surface.


A typical trace includes:


- The user’s input and detected intent
- The model’s response
- Each tool the agent called and what it returned
- Retrieved context, such as RAG documents and prior conversation history
- Latency and cost
- The final outcome or completion state
- User feedback signals, such as a thumbs up or thumbs down, a follow up question, or an abandonment


With the trace, you can see the exact interaction the user complained about and figure out what the agent saw, what it called, and where it went wrong. Traces can also help you get ahead of complaints if you review them regularly to catch bad interactions before users report them.


For PM teams working on agents, the trace is the new source of truth for what users experience.


## What is trace analysis?


[Trace analysis](https://amplitude.com/explore/analytics/what-is-trace-analysis) is the practice of inspecting traces, individually or in aggregate, to understand agent behavior and identify what needs to change.


Trace analysis happens at two levels:


- **Single trace inspection** is the unit of debugging. You open a trace and read through it to understand a specific failure, the same way a developer reads a stack trace or a researcher watches a session recording.
- **Aggregate trace analysis** looks for patterns across many traces: which intents fail most often, where tool calls error out, which queries cost the most, and which kinds of sessions correlate with happy users.


Trace analysis produces two outputs. The first is a fix that goes straight into the system, like a prompt change, a tool change, or a context retrieval change. The second is an eval that captures the failure so it can be tracked over time. Trace analysis is manual work, but you only have to find the failure by hand once before the eval starts checking for it on every change.


**Example:** Aggregate trace analysis over a 7-day window shows that 34% of traces in which the agent calls the inventory lookup tool end with a user follow-up question. Drilling in, you find that the tool returns stock counts but not restock dates, so the agent gives a technically correct but incomplete answer. That pattern becomes two evals: one that checks for a restock date mentioned when inventory is low, and one that flags any trace with a same-session follow-up on the same topic.


## What is an AI evaluation (eval)?


An[AI evaluation](https://amplitude.com/explore/analytics/what-is-an-ai-evaluation) , or eval, is a repeatable test that measures whether an agent’s output meets defined quality criteria for a given input. You run it over and over as the system changes, and it returns a score you can track.


Evals serve the same purpose for agents that unit tests and integration tests serve for deterministic software. They define what good looks like, run repeatedly as the system changes, and produce a score you can track over time. The difference between unit tests and evals is what they check.


A unit test asserts that the output exactly matches the expected answer. An eval often can’t rely on that because the agent might phrase the same correct answer ten different ways, and whether it’s correct at all is a judgment call. So instead of checking for an exact match, an eval checks the output against criteria you define.


The two dimensions that matter most are how an eval is scored (code-based or LLM-as-a-judge) and where it runs (offline in development, or online against live traffic). The sections below cover each.


Code-based eval


LLM-as-a-Judge eval


Scoring method


Deterministic logic (regex, JSON schema, exact match)


A second model scores against a natural language rubric


Speed


Fast, runs in milliseconds


Slower, each grading call takes seconds


Cost


Very low


Higher, each graded case consumes tokens


Best for


Verifiable properties (valid JSON, correct tool call, required disclaimer present)


Subjective quality (helpfulness, tone, groundedness)


Limitation


Can’t evaluate open-ended quality


Nondeterministic; requires calibration against human reviews


Offline eval


Online eval


When it runs


In development, before a change ships


Continuously, against live production traffic


Dataset


Fixed, curated set of cases


Real user traffic


Primary use


Catch regressions before release


Monitor live behavior; surface new failure modes


Limitation


Only tests inputs you anticipated


Expensive at full scale; sampling required for LLM judges


Feeds into


CI gate, blocks bad changes from merging


Offline eval set, new failures become new test cases


## Code-based evals


A[code-based eval](https://amplitude.com/explore/product/what-is-a-code-based-eval) scores an agent’s output using deterministic logic written in code: a regex match, a JSON schema check, an expected tool call, a SQL row count, or an exact string equality.


Code-based evals are fast, cheap, and reproducible across runs. You can run them on every change during development and on all live traffic in production without worrying about cost. They work well for verifiable properties: Did the agent return valid JSON? Did it call the correct tool? Did it return the expected number of rows? Does it include the required legal disclaimer?


**Example:** Your agent is supposed to call a get_account_balance tool before answering any billing questions. A code-based eval checks every trace in the billing intent bucket and flags any where that tool call is absent. When an engineer ships a prompt change that accidentally removes the billing routing logic, the eval catches it in CI before the PR merges.


The limitation is that code-based evals can’t capture anything subjective or open-ended. An agent’s response can be valid JSON and still useless, or miss a tool call and still give the right answer. For these cases, you need an LLM judge or a human.


## LLM-as-a-Judge (LLMaaJ) evals


An[LLM-as-a-judge (LLMaaJ) eval](https://amplitude.com/explore/product/llm-as-a-judge) uses a second language model to score an agent’s output against a rubric written in natural language, handling quality questions that code-based evals can’t answer.


Was the response helpful? Was the tone right? Did it actually address what the user asked? Was the answer grounded in the retrieved context, or made-up? You hand the judge a rubric, the input, and the output, and it returns a pass/fail or a score.


**Example:** You want to catch responses where the agent makes claims not supported by the retrieved documents (hallucinations). You write a rubric, “Given the context below, does the response contain any claim that cannot be verified from the provided documents? Answer YES or NO and cite the specific claim if YES.” The judge runs this check against a 10% sample of production traces each day. Any trace that returns YES gets routed to a human reviewer queue.


The limitation is that an LLM judge is itself nondeterministic. It can score the same output differently across runs, and its scores shift if you change the judge model. That’s why you calibrate a judge against human judgment before trusting it at scale. A judge that gives every answer a passing score is worse than no judge at all because it creates false confidence. Mature teams sample judge decisions, compare them to a human reviewer’s decisions, and adjust the rubric until the two align. Anthropic’s[engineering guide on evals](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents) is a useful reference on judge design.


## Offline evals


[Offline evals](https://amplitude.com/explore/analytics/what-is-an-offline-eval) run in development against a fixed dataset of inputs before you ship a change, making them the AI equivalent of pre-deployment regression testing. You curate cases from real failures, edge cases, customer scenarios, and known good examples, then run each input through the system and score the result with a mix of code-based and LLM-as-a-Judge evals.


Because the dataset is fixed, you can run the same eval suite on every change and see exactly what broke. In practice, this lives in your CI pipeline: a step runs the suite on changes that touch the agent, so a regression is caught before it merges. LLM judges cost tokens, so most teams only run the full eval suite on changes that actually affect the agent.


**Example:** After shipping a new system prompt, your offline eval catches that the agent now fails 4 of 12 edge cases involving ambiguous date ranges, cases you added to the dataset after a wave of user complaints three months earlier. Without the offline eval, that regression would have reached production. The eval blocks the PR, and the engineer re-prompts until the eval passes.


Offline evals are useful for fast iteration and catching regressions before they reach users. The limit is that a fixed dataset can only test what you thought to include. Real users will always type things you didn’t anticipate, which is what online evals are for. Amplitude’s blog on[eval-driven development](https://amplitude.com/blog/eval-driven-development) walks through how the Amplitude team built its first offline eval set.


## Online evals


[Online evals](https://amplitude.com/explore/analytics/what-is-an-online-eval) run continuously against real production traffic, scoring traces as they arrive rather than against a curated fixed dataset.


The difference is that offline evals tell you whether the agent handles the cases you prepared. Online evals tell you whether it handles what users actually type, including the long tail of phrasings, intents, and edge cases that no dataset anticipated. Online evals also give you a live quality signal you can monitor, alert on, and segment by feature or user cohort.


The scoring works the same way: Code-based evals and LLM judges are just pointed at live traffic instead of a fixed set. The constraint is cost. Judging every production trace adds up fast, so most teams run code-based checks broadly and sample a slice of traffic (typically 5–15%) for the LLM judge.


Most production AI teams run both offline and online evals. Offline evals gate releases. Online evals monitor live behavior and surface new failure modes, which then feed back into the offline set.


**Example:** Your online eval detects that the pass rate on “helpfulness” drops from 81% to 64% on Monday morning across a specific intent cluster, specifically questions about account migration. Tracing back, you find a backend change deployed Sunday night that altered the context the agent receives for that intent. The online eval caught a production regression in six hours. The failure pattern gets added to the offline dataset, so it’s caught at the CI level going forward.


## Connecting AI evals to product engagement


Eval scores are most useful when they connect to the product metrics your team already tracks, including retention, conversion, and feature adoption. This is how you can make the case for whether agent quality is influencing business outcomes.


A high pass rate tells you the model performs on a test eval set, but it doesn’t tell you whether successful agent interactions drive retention, whether failure modes concentrate in high-value segments, or whether your most expensive query types are also your lowest-converting ones. Answering those questions means joining trace data and eval scores to product engagement data under the same user identity.


Amplitude[Agent Analytics](https://amplitude.com/blog/agent-analytics) was built for exactly this, treating agent interactions as events in the same product event stream where retention, conversion, and adoption are already measured. Once eval scores and product metrics live together, you can ask the questions that actually matter for your business.


Glossary


**Agent.** An AI system that can take actions on behalf of a user, typically by calling tools, retrieving context, and producing responses through multiple model turns.


**Agent session.** A continuous interaction between a user and an agent, usually bounded by the start and end of a conversation. One session can contain many traces.


**AI evaluation (eval).** A repeatable test that measures whether an agent produces output meeting defined quality criteria for a given input.


**Code-based eval.** An eval scored by deterministic logic written in code, such as regex matches, JSON schema checks, or expected tool calls.


**Eval-driven development.** A practice in which evals are written before the system can pass them, then used to guide iteration. Analogous to test-driven development in software engineering.


**Failure mode.** A specific, named way an agent fails, such as a hallucination, incorrect tool call, or unsupported claim.


**Failure taxonomy.** A structured set of named failure modes used to classify and triage agent failures across many traces.


**Grounding.** The degree to which an AI response is supported by retrieved context or verifiable sources rather than generated from the model’s parametric knowledge alone.


**Hallucination.** An AI response that asserts something not supported by retrieved context, training data, or verifiable sources.


**Intent.** The user’s underlying goal in an agent interaction, often detected and classified by the system to route the request appropriately.


**LLM as a judge (LLMaaJ).** A separate language model used to score open-ended agent output against a rubric defined in natural language. Also sometimes referred to as LLM judge.


**Offline eval.** An eval run in development against a fixed dataset of inputs, before the system ships or a change is deployed.


**Online eval.** An eval run continuously against real production traffic, scoring traces as they happen.


**Pass rate.** The percentage of eval cases that meet the defined success criteria. The most common headline metric for eval suites.


**Precision.** In an eval context, the fraction of detected outputs that were correct.


**Recall.** In an eval context, the fraction of expected outputs the system actually found.


**RAG (retrieval augmented generation).** A pattern in which the system retrieves relevant context from a knowledge source and includes it in the prompt before generating a response.


**Rubric.** A structured set of criteria used by a human reviewer or an LLM judge to score agent output.


**Span.** A single operation within a trace, such as a tool call, model response, or retrieval step. A trace is composed of multiple spans.


**Tool call.** A request from the agent to an external function, API, or data source. Tool calls and their results are captured in the trace.


**Trace.** The complete record of a single agent interaction, including user input, model response, tool calls, retrieved context, and user feedback.


**Trace analysis.** The practice of inspecting traces, individually or in aggregate, to understand AI behavior and identify what to improve.


## Frequently asked questions


###### **What is the difference between an eval and a unit test?**


A unit test asserts that the output exactly matches the expected answer. An eval measures whether an agent’s output meets defined quality criteria for a given input, where the output may vary between runs and the criteria may be partially subjective.


###### **What is the difference between a trace and a log?**


A log is a record of system events, usually optimized for engineers who are debugging failures. A trace is a structured record of a single agent interaction, optimized for understanding what the agent did and why. Logs are infrastructure observability. Traces are product observability for agents.


###### **Do I need offline evals if I run online evals?**


Yes. Offline evals gate releases by catching regressions before they reach users. Online evals monitor live behavior and surface new failure modes after release. Each catches what the other misses. Most teams run both, with online failures feeding back into the offline set.


###### **How many evals do I need to start?**


Twenty to fifty real failures from manual testing, bug reports, and early user feedback are enough to produce a useful first signal. The quality of the cases matters more than the count. Each case should be unambiguous enough that two reviewers would independently reach the same pass or fail verdict.


###### **How is an LLM judge different from a human reviewer?**


A human reviewer is slower and more expensive, but also more accurate on subjective questions. An LLM judge handles more cases but is nondeterministic and requires calibration against human reviews to be trusted. Most teams use humans to define the rubric and calibrate the judge, then let the judge run at scale.


###### **Can you A/B test an agent without evals?**


You can, but you will optimize for shallow proxies. A/B tests on agents without eval coverage tend to measure response length, latency, or user reactions in isolation, without knowing whether the variant actually produced higher quality output. Pairing experiments with evals lets teams measure both quality and business outcomes for each variant.


###### **What is the difference between an eval and a benchmark?**


A benchmark is a public, shared measure used to compare models or systems across organizations. An eval, in the product sense, is usually private and specific to a single product’s quality bar. Benchmarks answer “Which model is best in general?” Evals answer “Is this system good enough for our users?”


###### **Who owns evals on a product team?**


In most mature product teams, PMs and engineers share ownership of evals. PMs define what counts as success and contribute eval cases from real user behavior. Engineers implement the eval infrastructure and integrate it into CI. Many teams treat evals as a shared artifact that both functions can edit, the same way both functions edit product specs.
