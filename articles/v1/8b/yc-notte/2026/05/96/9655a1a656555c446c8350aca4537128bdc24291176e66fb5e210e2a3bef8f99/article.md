---
schema_version: "1.0.0"
document_id: "9655a1a656555c446c8350aca4537128bdc24291176e66fb5e210e2a3bef8f99"
company_key: "yc-notte"
company: "Notte"
source_id: "yc-notte-news-import-7238aba58520"
canonical_url: "https://www.notte.cc/blog/agent-execution-tax"
published_at: "2026-05-19T09:00:00+00:00"
first_seen_at: "2026-07-22T06:23:42.732156+00:00"
fetched_at: "2026-07-28T21:43:30.232286+00:00"
content_hash: "sha256:5df60113cb36fa580e0d0eb4d5b273cfdeba67aaebc05ffdd056ae81bc8448b2"
---

# What Is the Agent Execution Tax?

**The Agent Execution Tax is the share of language model inference an agent wastes on malformed output that has to be retried.** It is the gap between the inference you pay for and the inference that actually moves a task forward. In a single chat call it is invisible. Inside a multi-step agent loop it compounds into real cost, latency, and failed tasks.


This page defines the term, gives the formula, and explains how to measure and reduce it. For the full empirical study across four models and 720 runs, see the benchmark linked at the end.


### Definition


The Agent Execution Tax is the ratio of wasted inference to productive inference across an agent run:


code.txt


```text
Agent Execution Tax = (total_inference_calls - productive_calls) / productive_calls
```


A productive call is one that returned valid, well-formed structured output on the first attempt. Every other call is overhead: the model returned something the agent could not use (malformed JSON, a missing field, an invalid action), the framework caught the error, and the call was retried. The tax is the price of that retrying, expressed as a fraction of the useful work done.


Note the denominator. This is not the raw retry rate (retries divided by total calls). Because wasted calls are removed from the productive base, an 18.6% retry rate becomes a 22.9% Execution Tax. The metric is non-linear by construction: a small per-call unreliability does not stay small.


### Why the Agent Execution Tax exists


An agent task is not one model call. It is a loop: observe the page, decide an action, return it as structured output, execute it, observe again, repeat. A typical task runs around ten steps, and every step is a call that must return a valid structured object.


When a model returns malformed output, the failure happens inside the inference layer, before the agent framework ever sees a usable result. The framework simply retries. From the outside the task still looks like it succeeded. The retry never appears in a task success rate or a reasoning benchmark. It only shows up if you instrument the engine itself and count how many calls were thrown away.


That is why standard benchmarks miss it. MMLU, HumanEval and similar evaluations measure capability in a single shot. None of them measure whether a model can sustain valid structured output across a ten-step loop. The Agent Execution Tax is the metric that does.


### Why it matters: it compounds


A per-call reliability gap does not stay a per-call problem. Across a loop it compounds into three costs at once:


- **Cost.** Every retry is a full inference call. The entire context is re-sent, billed again, and produces nothing. In our benchmark the worst model paid a 22.9% Execution Tax. At a modest production volume of 10,000 agent tasks per day, that is over $40,000 a year of inference that produces no value.
- **Latency.** Every retry adds a full model round trip, roughly 2 to 3 seconds. Across a task that is dead time the user waits through.
- **Task success.** A retry mid-task can desync the agent's state, so later steps misread the page and fail outright. This is the hardest cost to measure and the most damaging at scale.


> Token pricing and leaderboard rank are not what you actually pay for in an agent system. Cost per successful task is.


The procurement consequence is the part that surprises people. The model with the lowest looking token price in our benchmark was 2.3x more expensive per successful task than the winner, once retries compounded.


### How to measure the Agent Execution Tax


You cannot measure it from the outside. You have to instrument the model engine and separate first-attempt-valid calls from retried calls:


1. Log every call the engine makes, not just the ones the agent framework sees as successful.
2. Mark a call productive only if it returned schema-valid structured output on the first attempt.
3. Count total calls and productive calls across a representative run.
4. Apply the formula above.


[Notte](https://notte.cc/) is an open-source, model-agnostic browser agent framework that records this by default: per-call latency, parse retry counts, and token usage for every run. Change the model string and the same pipeline runs identically across providers, which is what makes a clean cross-model comparison possible.


### How to reduce the Agent Execution Tax


Three levers, in rough order of impact:


- **Pick models on structured-output reliability, not reasoning score.** The models that win agent workloads are the ones that reliably return valid output every step, not the ones highest on a reasoning leaderboard.
- **Hold the serving layer constant.** Tail-latency consistency and clean retry recovery across very different model architectures are properties of the serving stack, not the model. A stable serving layer keeps the tax low even when a model occasionally slips.
- **Constrain the output.** Strict structured-output modes, schema validation close to the model, and tight prompts reduce the rate at which malformed output is produced in the first place.


### Related metrics


The Agent Execution Tax sits alongside three metrics worth tracking together:


- **Parse retry rate.** The share of calls that returned invalid structured output and were retried. The raw input to the Execution Tax.
- **Reliability-Adjusted Accuracy.** Task success rate discounted by execution overhead: Task Success x (1 - Execution Tax). It separates a model that succeeds cleanly from one that succeeds expensively.
- **Cost per successful task.** Total cost divided by successful tasks. The number procurement should lead with, because it absorbs retry waste, step efficiency, and success rate in one figure.


### FAQ


**What is the Agent Execution Tax?**


It is the ratio of wasted inference to productive inference in an agent loop: the share of model calls that produced nothing because the output was malformed and had to be retried.


**How is the Agent Execution Tax calculated?**


(total_inference_calls - productive_calls) / productive_calls, where a productive call returned valid structured output on the first attempt.


**Why don't standard benchmarks show it?**


Because the retry happens inside the inference engine, before the agent framework sees a result. Single-shot benchmarks like MMLU never observe it. You only see it by instrumenting the engine across a multi-step loop.


**How do you reduce the Agent Execution Tax?**


Select models on structured-output reliability, keep the serving layer consistent, and constrain output with strict schemas. In our benchmark the best models held the tax between 0 and 1.6%, while the worst paid 22.9%.


### The full benchmark


We measured the Agent Execution Tax across four language models and 720 browser agent runs. The full study, including the per-model numbers, the cost-per-successful-task ranking, and the methodology, is here:[the 720-run browser agent benchmark](https://fireworks.ai/blog/agent-execution-tax) .


Notte coined the Agent Execution Tax as a procurement metric for agent systems. If you are building agents in production, the framework and the instrumentation that produced these numbers are open source at[notte.cc](https://notte.cc/) and on[GitHub](https://github.com/nottelabs/notte) .
