---
schema_version: "1.0.0"
document_id: "e220f1684f8e1c264f1209d341244afa050e1ad4b9d7c00a90b2be036bb1d174"
company_key: "yc-retool"
company: "Retool"
source_id: "yc-retool-news-import-762698831de2"
canonical_url: "https://retool.com/blog/ai-observability-stack"
published_at: "2025-12-09T00:00:00+00:00"
first_seen_at: "2026-07-22T11:44:05.217948+00:00"
fetched_at: "2026-07-28T21:58:33.663804+00:00"
content_hash: "sha256:cbd73c09d099e4dce35ab6a16b5d5329745a464813a20ee8128a042a5649efd8"
---

# The 6 Layers of AI Observability: A Guide to the AI Stack | Retool Blog

01 Why AI observability matters right now02 The observability challenge starts with the stack03 Infrastructure layer: Tracking GPU utilization and rate limits04 Data layer: Monitoring RAG retrieval and vector relevance05 Model observability records every LLM interaction06 Evaluating prompts in Retool07 Eval tools for custom implementations08 Agent observability exposes reasoning and tool choices09 Workflow observability traces where execution stalls10 Application layer observability captures when users reject outputs11 Production observability requires purpose-built tools


Observability is a core tenet of software engineering. To understand your software, you must understand its internal state. Is it meeting SLOs? Is it consuming resources as expected? Where are requests failing, or timing out? What changed between this deployment and the last?


This is as true of AI software as it is of anything else, but with a twist. Traditional software is deterministic; for every input, you should always get the same output. Write a simple function to add two numbers, put “2+2” into it, and, unless you’re terrible at coding or math, you’ll always get “4.”


Not so for AI. Large language models are **non-deterministic** , meaning their output is inherently variable. Put “2+2” in and you’ll get “4” one thousand times in a row, but then you’ll get “Paris.” More seriously, if you’re building an AI agent for research that decides to search for “enterprise software solutions” in one run and “business automation tools” in the next, the downstream effects cascade unpredictably. Each variation yields different results, costs, and user experiences.


For AI apps, observability must evolve from simply tracking whether something works to understanding the distribution of outcomes, the reasoning that produced them, and the factors that drive variation.


## Why AI observability matters right now


Observability is about trust; can you trust your software to perform the task required?


Enterprises deploying AI agents and LLM workflows face a trust gap. Before moving these systems into production, they require audit trails that show precisely what was called, with what inputs, and why. That lack of visibility has become a primary barrier to adoption.


The non-determinism makes this worse. Run the same agent twice and watch it diverge. One execution searches Google, the next hits Reddit, and suddenly you’re debugging why user experiences vary wildly. Without stored traces of prompts, tool calls, and decision paths, reproducibility becomes impossible.


Product velocity suffers too. You can’t iterate intelligently on prompts or model choices when you’re flying blind. Does changing “summarize this document” to “extract key points from this document” actually improve output quality? Which retrieval strategy reduces hallucinations? Without observability, you’re guessing. With it, you’re optimizing based on data from actual runs.


The shift from prototype to production-quality software demands this visibility. Basic monitoring tells you something broke. Comprehensive observability tells you why, where, and how to resolve the issue. Without that, AI remains experimental rather than operational. But for builders, it’s hard to observe every part of the agent or workflow in one place.


## The observability challenge starts with the stack


For a single AI-driven app, there are six interconnected layers, each providing visibility for production deployment:


- **Application layer (user interactions and feedback)** : users thumbs down responses, abandon tasks, or rephrase questions when something breaks, but these signals only point to problems without explaining the causes.
- **Workflow/orchestration layer (step-by-step execution)** : Breaks down each operation to identify latency bottlenecks, which steps fail, and retry, and when conditional logic sends execution down unexpected paths.
- **Agent layer (multi-step reasoning and tool usage)** : Logs capture which tools the agent selected, what parameters it built, and how each response influenced its next decision throughout the entire reasoning loop.
- **Model layer (prompts and responses)** : Record every LLM call with full context to compare prompt variants, track costs, and connect specific instructions to specific outputs.
- **Data layer (retrieval and grounding)** : Verify whether your search actually returned relevant documents and whether the model used them, as poor retrieval can corrupt everything downstream.
- **Infrastructure layer (resources and efficiency)** : Distinguish between slow models and **rate limiting and throughput issues** so you know whether to tune prompts or provision more compute.


Let’s go through each of these, starting from the low-level infra layer and working up to the application layer.


## Infrastructure layer: Tracking GPU utilization and rate limits


Infrastructure problems often disguise themselves as quality issues. A perfectly configured agent can still fail if the underlying systems can't keep up. Without this layer, you might waste time tuning prompts when the real culprit is queue depth or rate limiting.


This layer tracks the mechanics that keep everything running:


- GPU utilization and queue depths
- Request throttling and rate limits
- Compute resources consumed per run
- Cost per interaction


[High-level dashboards](https://docs.retool.com/agents/guides/monitor) can display aggregate metrics, including total token usage, estimated cost, average runtime, and total runs. If the average runtime increases from 30 seconds to 3 minutes, you know something has changed. If the estimated cost spikes, you know exactly when.


That’s monitoring. It alerts you to problems but doesn’t explain why. When costs rise, observability tools enable you to drill into specific runs, examine which tools consumed the most resources, and trace resource consumption back to specific decisions in your agent’s execution path. Monitoring shows the symptom. Observability diagnoses the cause.


This matters most when scaling. Resource contention affects response consistency, throttling impacts user experience, and inefficient allocation burns budget. A 200ms latency increase might be attributed to queue depth, rather than model performance. Understanding[billing and usage](https://docs.retool.com/support/billing-usage#agent-usage) patterns helps you determine whether to optimize prompts or provision additional compute resources.


On the Retool platform, infrastructure-layer observability is abstracted away, handled automatically with[AWS tooling](https://docs.aws.amazon.com/wellarchitected/latest/management-and-governance-guide/aws-observability-tools.html) . But a healthy infrastructure only proves the lights are on. It doesn't tell you if the information flowing through the system is actually accurate. That brings us to the data layer.


## Data layer: Monitoring RAG retrieval and vector relevance


But a healthy infrastructure only proves the lights are on. It doesn't tell you if the information flowing through the system is actually accurate.


This is the defining challenge for Retrieval-Augmented Generation (RAG) workflows. In these systems, retrieval fails silently. An agent can execute flawlessly, call the right tools, construct perfect prompts, and still produce garbage because the vector database returned irrelevant documents. You need visibility into the quality of what gets pulled, not just that *something* got pulled.


Poor retrieval quality cascades throughout the entire system, contaminating outputs even when prompts, models, and infrastructure are functioning correctly. If your agent retrieves five documents, are they relevant and did the model actually use them?


This layer monitors what gets retrieved and whether it grounds the response. It watches for things like:


- Document relevance scores from vector searches
- Which retrieved chunks did the model actually reference
- Semantic drift in search results over time
- Stale or outdated sources returning in queries


Measuring retrieval quality requires comparing what you retrieved to what you needed. Context recall metrics, such as those in[Ragas](https://docs.ragas.io/en/stable/concepts/metrics/index.html) , evaluate whether retrieved documents contain the necessary information to answer the question. Here’s how to set up both retrieval tracking and evaluation:


```text
1
2
3
4
5
6
7
8
9
10
11
@braintrust.traced
async def fetch_top_k_relevant_sections(input: str) -> List[str]:
embedding = await embed_text(input)
results = table.search(embedding).limit(TOP_K).to_arrow().to_pylist()
return [result["markdown"] for result in results]


@braintrust.traced
async def generate_answer_e2e(question: str):
retrieved_content = await fetch_top_k_relevant_sections(question)
answer = await generate_answer_from_docs(question, retrieved_content)
return dict(answer=answer, retrieved_docs=retrieved_content)


```


```text
Python
```


Run evaluations that track both retrieval quality and answer quality:


```text
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
from autoevals import AnswerCorrectness, ContextRecall


async def context_recall(output, **kwargs):


return await ContextRecall().eval_async(


output=output["answer"],


context=output["retrieved_docs"],


**kwargs


)


async def answer_correctness(output, **kwargs):


return await AnswerCorrectness(model="gpt-4").eval_async(


output=output["answer"], **kwargs


)


eval_result = await EvalAsync(


name="RAG Pipeline Evaluation",


data=qa_pairs,


task=generate_answer_e2e,


scores=[context_recall, answer_correctness],


)
```


```text
Python
```


Context recall compares retrieved documents against ground truth answers, revealing when your retrieval strategy misses critical information. A score of 95% indicates that your vector search accurately identifies the relevant documents. A score of 60% suggests that you’re retrieving too few documents, using poor embeddings, or that your chunking strategy splits relevant information across multiple pieces.


To evaluate this layer, track which documents appear most frequently in successful responses. If the same ten documents answer 80% of queries, your retrieval strategy works. If every query pulls different random documents, something’s broken. Monitor relevance score distributions, too. Consistent scores above 0.8 suggest good retrieval. Scores clustering around 0.3-0.5 mean your embeddings aren’t discriminating well.


[Retool Vectors](https://docs.retool.com/data-sources/quickstarts/retool-vectors) offer semantic search with built-in relevance scoring, enabling you to track retrieval quality without building vector infrastructure from scratch.


## Model observability records every LLM interaction


Even with perfect retrieval, the logic can still fail. Once your data reaches the LLM, you need to see exactly how the model interprets that context and what it decides to generate.Without visibility into the model layer, you are effectively debugging in the dark. When a user complains about an incorrect output, you need to know exactly which prompt variant they were on and what instructions guided that specific response. Otherwise, prompt engineering becomes nothing more than guesswork.


The model layer captures the raw interaction between your system and the LLM, so you can understand what the model actually does versus what you expected it to do. The model layer should record:


- All prompts with their full context and instructions
- Complete model responses
- Token usage per interaction
- Which prompt version produced which output


Model layer observability lets you systematically compare prompt variants and correlate specific phrasings with output quality..


## Evaluating prompts in Retool


[Retool’s Eval framework](https://docs.retool.com/agents/concepts/evals) handles prompt evaluation directly in the platform. Create test cases with expected inputs and outputs, then run your agent against them to see how prompt changes affect performance.


Retool provides two types of reviewers to score agent outputs. First **programmatic reviewers** use code to score based on predefined rules. Use these when the expected output is clearly defined:


- Exact match for binary comparisons
- Levenshtein distance for string similarity
- JSON/XML validation for structured outputs


You can also use LLM-as-a-judge reviewers. These use a second model to evaluate quality when the expected output isn’t precisely defined.


These reviewers let you define custom evaluation criteria. The “Match Expected Answer” reviewer, for example, prompts an LLM to assess how closely the output matches expectations, returning scores such as 0 (no match), 0.5 (somewhat), or 1 (yes). You control the evaluation model, temperature, prompt template, and scoring thresholds.


Compare two agent runs side by side to examine how different prompts or configurations affect behavior. Each eval captures the full prompt, response, and scoring across multiple test cases, making it clear whether your iteration improved things or just changed them.


## Eval tools for custom implementations


Outside Retool (and under the hood at Retool), several tools provide prompt evaluation and versioning:


- [Braintrust](https://www.braintrust.dev/) : Tracks prompt versions, runs A/B tests, and offers detailed scoring breakdowns with LLM-as-judge evaluators
- [Langsmith](https://www.langchain.com/langsmith) : Monitors every LLM call with full prompt and response logging, plus dataset-based evaluation
- [Weights & Biases](https://wandb.ai/site/weave/) : Offers prompt tracking and comparison across experiments with visualization tools


The key is systematic comparison. Intuition about prompt quality fails at scale. Track every version, measure results against test cases, and let data drive your iteration.


## Agent observability exposes reasoning and tool choices


Optimizing single prompts is essential, but most AI apps aren't just one-off calls; they are agents acting as autonomous loops. This introduces a new layer of complexity: non-deterministic reasoning chains.Agents are non-deterministic. Run the same agent twice on identical input, and it may query your CRM in one execution but retrieve data from your support ticket system in the next.


These different data sources cascade into distinct contexts, different insights, and ultimately, different outputs. Without visibility into the agent’s reasoning and tool selection, debugging becomes impossible. This layer shows you where and why your agents diverged.


By[monitoring agents](https://docs.retool.com/agents/guides/monitor) , you can capture every step of agent execution in the logs:


The logs record shows:


- Which tool the agent selected at each step
- The parameters it constructed for that tool
- The agent’s reasoning before making the decision
- What the tool returned
- How that output influenced the next decision


Click into any tool call to examine the full input and output. If your agent called a web search tool, you can see exactly what search terms it used. If you obtain a particularly good result, drill into the logs to identify the query that led to it, and then update your prompt to suggest those types of searches.


The monitoring view provides a different perspective, showing tool usage patterns across multiple runs:


The agent graph visualizes which tools get called. The activity panel displays tool calls in real-time during execution. The tool usage statistics at the bottom reveal which tools the agent relies on the most. If one tool accounts for 80% of calls, that’s your critical path. If tool usage is evenly distributed but results are inconsistent, your agent might be choosing randomly rather than strategically.


Looking into this layer can help you optimize prompts. Seemingly minor prompt changes often have significant effects on agent behavior. Slight variations in phrasing, instruction order, or examples can dramatically shift which tools the agent selects and how it constructs parameters. You won’t discover these patterns through intuition. Systematic observation of actual agent behavior reveals what works and what doesn’t.


Tracking decision patterns over time, you’ll start to see what needs to improve. Does your agent consistently choose the right tools for similar queries? Or does it try different approaches randomly? Consistent tool selection for similar inputs suggests good prompt design. Random selection suggests the agent can’t distinguish between scenarios and needs more precise instructions.


## Workflow observability traces where execution stalls


While the agent handles the reasoning, the workflow handles the plumbing. Agents rarely exist in a vacuum; they are usually just one step in a larger orchestration of APIs, databases, and conditional logic.When a multi-step workflow slows down, aggregate metrics tell you nothing useful. You know the whole thing takes 45 seconds, but is that because one API call is timing out, a retry loop is stuck, or because the context window is full? You need to see each step to find the bottleneck.


Complex AI applications involve sequences of operations: retrieving data, processing it, calling an LLM, validating the output, and taking action based on the result. Each step depends on the previous one. When the whole process takes too long or produces unexpected results, you need visibility into each step to diagnose the problem. When you run a[workflow](https://docs.retool.com/workflows/) , it's essential to understand what happens at every step.


For observability, each step needs to show:


- Execution time
- Input parameters
- Output data
- Success or failure status
- Retry attempts


Say you look and see that the LLM call took 4.2 seconds. The database query took 0.3 seconds. The validation step failed initially but was retried successfully. This granularity pinpoints bottlenecks that aggregate metrics miss.


Conditional logic adds complexity. If your workflow branches based on LLM output, which path executed? Did it retry because of an error or because the condition triggered multiple times? The execution trace shows the actual path taken, not just the possible paths defined in your workflow.


This matters for optimization. Aggregate metrics show your workflow averages 30 seconds per run. The step-by-step view reveals that 25 of those seconds happen in one API call that frequently times out and retries. Now you know what to fix. Without execution visibility, you’d waste time optimizing fast steps that don’t matter.


## Application layer observability captures when users reject outputs


Ultimately, all this backend logic culminates in one place: the user experience. You can have perfect infrastructure and valid logic, but if the user isn't getting value, the application is failing.Users abandon your AI feature without saying why. They give up mid-task, rephrase the same question three times, or just never come back. Without capturing these signals, you're missing the clearest indicator that something's broken.


Application-level feedback captures user experience:


- Explicit signals like thumbs up/down or star ratings
- Task completion or abandonment rates
- User corrections or rephrasing patterns
- Time spent reviewing outputs before accepting them


In Retool, this layer is entirely up to you to build. The[Agent Chat component](https://docs.retool.com/apps/guides/forms-inputs/agent-chat) displays agent responses but doesn’t include built-in feedback mechanisms. If you want to know whether users found an agent’s response helpful, you need to add those elements yourself, akin to how it looks in ChatGPT or Claude:


Some things you could build into your app layer include:


- Adding a thumbs-up/thumbs-down component after agent responses.
- Tracking whether users accept the agent’s suggestion or modify it.
- Logging when users abandon a conversation midway through.


These data points connect to the observability layers below, letting you trace user dissatisfaction back to specific prompt versions, tool selections, or retrieved documents.


Once you’ve deployed your agent, user feedback becomes your most important signal. A spike in negative ratings tells you something’s changed. Did you update the prompt? Switch models? Modify tool descriptions? The monitoring and logging layers below help you answer these questions, but user feedback triggers the investigation.


## Production observability requires purpose-built tools


AI systems fail without observability. You can’t debug what you can’t see, can’t optimize what you can’t measure, and can’t trust what you can’t trace.


The six layers outlined here work together. User feedback points to problems. Workflow traces reveal bottlenecks. Agent logs expose decision patterns. Model records enable prompt comparison. Retrieval metrics catch silent failures. Infrastructure monitoring separates resource issues from quality issues.


Retool provides observability across the stack by default. Logs capture every agent decision. Monitoring dashboards track costs and performance. Evals measure prompt quality systematically. The infrastructure layer runs behind the scenes. You build the agent, Retool builds the visibility.


[Start building with Retool Agents](https://retool.com/agents) to deploy AI systems you can actually trust in production.


light Reader
