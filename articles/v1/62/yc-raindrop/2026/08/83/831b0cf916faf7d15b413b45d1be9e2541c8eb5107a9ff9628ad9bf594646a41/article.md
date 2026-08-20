---
schema_version: "1.0.0"
document_id: "831b0cf916faf7d15b413b45d1be9e2541c8eb5107a9ff9628ad9bf594646a41"
company_key: "yc-raindrop"
company: "Raindrop"
source_id: "yc-raindrop-news-import-6a6050810bab"
canonical_url: "https://www.raindrop.ai/blog/signals-2-frontier-classification/"
published_at: "2026-08-11T00:00:00+00:00"
first_seen_at: "2026-08-11T13:30:44.838293+00:00"
fetched_at: "2026-08-11T13:30:46.638698+00:00"
content_hash: "sha256:4947a7d0e9deffd5033c9da718d3cf738f07c475a901437ca239f0bab025c1fc"
---

# rd-signal-2: Frontier Classification at Production Scale

Today we're launching Signals 2.0, powered by


rd-signal-2,


our new model pipeline for building task-specific binary classifiers from production traces.


rd-signal-2


approaches


**GPT-5.6 Sol xhigh accuracy** while costing


**1600x less, and 260x less than GPT-5.6 Luna xhigh** .


This model is available to all Raindrop customers today (at no additional cost).


We are also releasing Signal Builder: a platform for training and hosting custom classifiers with Zero Data Retention. Signal Builder brings the power of Signals to the strictest of enviroments (including healthcare).


### Precision


Claude Sonnet 5 adaptive


78%


Raindrop


71%


GPT-5.6 Sol xhigh


71%


GPT-5.6 Luna xhigh


67%


### Recall


GPT-5.6 Sol xhigh


91%


Raindrop


87%


GPT-5.6 Luna xhigh


83%


Claude Sonnet 5 adaptive


75%


### Relative cost


linear scale, per classified trace


Raindrop


1×


GPT-5.6 Luna xhigh


260×


GPT-5.6 Sol xhigh


1,600×


Claude Sonnet 5 adaptive


1,650×


## Everything is a binary classification problem


Last year, OpenAI published a paper called


["Why Language Models Hallucinate"](https://arxiv.org/abs/2509.04664) . Hallucination, they said, is simply a binary classification problem: any statement is either true or not.


The LinkedIn headlines were immediately victorious: "OpenAI solved hallucination."


And, if being binary made classification easy, they would have. But, as it would turn out, everything in life is just binary classification too. You either should or should not get married; the UI is either good or not.


...and a given agent behavior is either good or bad.


Binary classification is a hard alignment problem. You need to align the humans within a company on the definition of good/bad, and then explore all of the edge cases, and then - and only then - you must align a model or pipeline to that definition.


Raindrop covers the entire journey of training accurate classifiers for agent behavior.


## Classifiers for Agents


In June 2025, we launched


[the first version of Signals](https://x.com/benhylak/status/1935794106493722702) : an automated pipeline for training tiny classification models.


At the time, our classifiers evaluated a single input-and-output pair for a given behavior. That worked for the chatbot era, when the relevant evidence was contained within a single turn. Our competitors still have that limitation.


But agent failures now unfold across multiple turns, tool calls, and subagents, sometimes spanning hundreds of thousands of tokens. These failures are often nuanced and sparse. Finding them requires both deterministic filtering to assemble the relevant evidence and semantic judgment to interpret it.


Running a frontier model over every trace is the obvious solution, but, among other problems, it becomes prohibitively expensive and slow at production scale.


Smaller models are affordable, but they struggle with complex behaviors and are often limited by how much context they can consume.


*rd-signal-2* solves this through an automated research loop. For each behavior, it studies production traces, writes code to assemble the context that matters, and uses that context to train a task-specific model.


## How


rd-signal-2


builds Signals


When an agent produces this trace:


Agent trace


4 events · 90.2s


1


update_record


` ({ id: 42, status: "resolved" })` timed out · 30.0s


2


update_record


` ({ id: 42, status: "resolved" })` timed out · 30.0s


3


update_record


` ({ id: 42, status: "resolved" })` timed out · 30.1s


Final response


“The record has been successfully updated.”


rd-signal-2 · claims success after repeated failures


Matched


The failure is not contained in any single step. It is the relationship between the repeated tool failures and the assistant's final response.


*rd-signal-2* can express that relationship:


```text
export     function     run  (  event  )     {     1      const     attempts     =     findToolCalls  (  event  .  trace  ,     "  update_record  "  )  ;         2      const     repeatedWithoutChange     =           attempts  .  length     >  =     3     &  &           haveIdenticalInputs  (  attempts  )     &  &           attempts  .  every  (  call     =  >     call  .  failed  )  ;         3      if     (  !  repeatedWithoutChange  )     {           return     {     matched  :     false     }  ;         }         4      return     classify  (           formatForReview  (  attempts  ,     event  .  finalResponse  )  ,           "  The assistant claims the operation succeeded  "  ,           {     spanIds  :     attempts  .  flatMap  (  call     =  >     call  .  spanIds  )     }         )  ;     }
```


The code finds the relevant tool calls, compares their inputs, and checks whether they failed. If those conditions are not met, the Signal returns a non-match without calling a model.


If they are met, the Signal extracts the failed attempts and the assistant's final response from the trace. The remaining semantic judgment goes to a combination of a "task-specific classification head" and our in-house semantic reasoning model, optimized for binary classification.


This was an easy example, constrained to a single turn. Signals are built to efficiently and dynamically collect context from sessions that span days and weeks.


## Reasoning at build time


*rd-signal-2* separates the reasoning required to


**construct** a classifier from the computation required to


**execute** it.


Traditional LLM Judges waste reasoning tokens rediscovering the same evidence and making slightly different judgments each time.


```text
Prompt classifier cost = traffic × full-trace reasoning
```


*rd-signal-2* pays a one-time cost that can be spread across every future trace:


```text
rd-signal-2 cost = build once + deterministic execution
+ ambiguous candidates × compact context
```


By iterating and enforcing strict self-verification, the system can try several approaches before finding one that matches the customer's intent. This pattern is similar to the advisor-executor pattern used by frontier coding harnesses, where a smarter planner constructs a narrow task that a smaller model can perform efficiently.


**Model calls should scale with uncertainty, not traffic.**


As a result, Signals are inexpensive enough to include with the Raindrop platform and run across billions of traces per month. This stands in stark contrast to platforms like Braintrust and Langchain that require customers to pay out-of-pocket for inference.


## Binary classification is an alignment problem


The hardest part of building an effective classifier is often discovering what the user actually means. We quickly realized that this is as much a product problem as it is a machine-learning problem.


Return to the repeated tool-call example. Should the Signal match if:


- the arguments changed slightly but the strategy did not?


- the first three attempts failed but the fourth succeeded?


- the agent admitted that the operation failed?


- the agent handed the task to a subagent that completed it?


Each decision is rooted in our customers' judgment. Translating their nuanced requirements into a faithful classifier is a non-trivial product problem.


### Match rate per policy interpretation


Loose identity


4.6%


Strict


3.1%


Rate-limit exempt


1.4%


Eventual-success exempt


0.9%


67% of matched traces were matched by at least one interpretation and cleared by another.


To illustrate how challenging finding this line can be, we wrote four variations of the same one-sentence behavior and ran all four over the same 2,000 production traces. Match rates ranged from 0.9% to 4.6%, and 67% of matched traces were contested by at least one variation. Human-model alignment will remain an active area of research at Raindrop.


## A Signal is never truly finished


Building an accurate classifier is just the first step. Raindrop helps keep it accurate.


As models and harnesses change, the shape of traces changes too. Customers also discover specification errors as they see their Signal operate on more data. A Signal can become less accurate even when its code has not changed.


After deployment, we continue watching. Each day we randomly sample production Signals using frontier models. When these evaluations uncover drift or a regression, we rerun prompt optimization and retune.


This creates a continuous loop:


```text
deploy → sample → evaluate → find regressions → retune → re-evaluate
```


We also give customers the power to tune all this themselves. They can directly inspect and tweak the code and prompt that power their Signal.


## Running Signals safely at scale


Evaluating 10,000 traces is easy. Running more than two million a day requires careful model serving, queueing, retries, and isolation.


Every generated Signal is treated as untrusted by default. Each customer's Signals run in an isolated environment with no credentials or internet egress. An organization's runtime can access only that organization's data.


Trace context is fetched once and cached close to the evaluation workers, so multiple Signals can reuse it instead of repeating expensive queries. This keeps the hot path focused on deterministic execution and the small set of cases that need model judgment.


At production scale, the median trace is classified in 100 ms. This infrastructure currently evaluates over 20 billion traces per month.


## How


rd-signal-2


builds a better Raindrop


*rd-signal-2* already powers more than user-created classifiers inside Raindrop.


Our Issue Detection system shares the same architecture to identify, track, and monitor emerging failure modes in our customers' data distributions. This allows our customers to insantly turn any issue detected into a long-standing Signal. From there, they can refine the policy, use Experiments to A/B test, and more.


Signals can be created by a user, constructed through our Triage Agent, generated by your coding agent via MCP, or used by systems like Raindrop Issue Detection.


Once deployed, they all run through the same evaluation and monitoring infrastructure.


Creating a Signal with a coding agent


## Signals 2.0 API


Signals aren’t limited to just the Raindrop platform. Developers can now call the Signals API directly to build domain-specific detection into their own systems.


For teams with stricter data requirements, ZDR Signals supports training and running task-specific classifiers without retaining production data.


## Try Signals 2.0


Signals 2.0 is available today in Raindrop.


Create your first Signal in Raindrop:


[Get started](https://app.raindrop.ai/signup) , or


[schedule a call with our team](https://cal.com/team/raindrop-ai/chat-15-min) to build one together.
