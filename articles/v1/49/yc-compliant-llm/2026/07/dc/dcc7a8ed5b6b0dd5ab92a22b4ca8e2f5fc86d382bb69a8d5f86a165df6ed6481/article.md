---
schema_version: "1.0.0"
document_id: "dcc7a8ed5b6b0dd5ab92a22b4ca8e2f5fc86d382bb69a8d5f86a165df6ed6481"
company_key: "yc-compliant-llm"
company: "compliant-llm"
source_id: "yc-compliant-llm-news-import-79fa5784f6e7"
canonical_url: "https://compliantllm.com/blog/llms-going-from-poc-to-production"
published_at: null
first_seen_at: "2026-07-23T06:18:10.598093+00:00"
fetched_at: "2026-07-28T21:20:09.527818+00:00"
content_hash: "sha256:bc2e63376364a97f2e5862ce1c81c3b727a2fe0d549a6627c91e8c86cceb5ae3"
---

# Why enterprise LLMs are stuck in a PoC forever?

Integrating AI into any application is easier than ever — near-human intelligence is just an API call away! But when building an AI data platform and customer support AI at Uber, we faced a number of challenges.


## ‘PoC’: ChatGPT Wrapper + RAG


Write a prompt, make the API call to GPT — an LLM app aka GPT wrapper is ready to go!


Next, hook the LLM to your knowledge base, using a process called retrieval augmented generation or RAG. Grounding the LLM in facts significantly reduces the tendency of your LLM to hallucinate.


When people think GPT wrapper, they think it’s this easy. But the real fun is just beginning!


## Challenges


1.


In many cases, the LLM just does not accurately follow the instructions set in the prompt. Go figure!


2.


Doing RAG with an LLM adds another layer of complexity. Certain information users seek may not be present in the knowledge base, or the retrieval algorithm might retrieve irrelevant data points for certain queries.


## Solutions


With the non-deterministic black box that an LLM is, it is incredibly hard to pin-point the underlying cause of LLM failures. The LLM could be hallucinating due to one of the following reasons:


1.


The prompt is not good enough - use ‘prompt engineering’ to figure out a better version of the prompt. The only way to do this today is through experimentation - tweak the prompt until it works better across a wider set of queries.


2.


The context needed to answer the user query correctly is missing in the knowledge base. In this case, augmenting the knowledge base with relevant information will fix the issue.


3.


The context is present but was not retrieved correctly. The retrieval algorithm needs to be fine-tuned to suit your use case better.


## End State


Once teams iterate enough in this stage, they end up with what Andrew NG calls a mega-prompt — a large prompt that defines the business logic and rules you expect an LLM to follow, with a number of examples that demonstrate good behavior.


## Drawbacks of the end-state


-


It becomes hard to maintain and improve this system. Small changes to the prompt may lead to regressions in core user flows. The process of adding new features or fixing errors often leads to existing features and working queries to fail.


-


Large prompts lead to more hallucinations - as more business logic, examples, and rules are added, the LLM does not always follow the instructions set in the prompt.


-


Cost and latency increases quadratically - more users (n) and more complexity (m tokens in the LLM input) leads to cost and latency proportional to O(n*m). For many real-time use cases (copilots, chatbots, voice bots), increased latency leads to a poor user experience.


## Composition of LLM calls


The logic in a large prompt is split into multiple logical tasks. These tasks are converted into individual prompts. LLMs are called with these individual prompts in series, in what is popularly known as chaining.


Software engineers love the modularity of the new system. But does it work?


### Advantages


-


LLMs do perform much better at small, well-defined tasks. **Note:** This may or may not lead to an improvement in the chain of tasks (more below).


-


Much better maintainability - instead of maintaining a single, large prompt, you now have 4 (or more) much smaller prompts. Adding features typically implies modifying a subset of the prompts. Also, debugging queries that return incorrect responses is more straightforward - you can look at the individual LLM requests and narrow down the issue more easily than with a huge prompt.


### Disadvantages


-


High latencies and cost, similar to the previous solution, caused my making multiple LLM calls in sequence. This can be mitigated by using small LoRA models and tools like vllm which reduce latency to less than 50ms.


-


The errors in the individual LLM calls compound. In some use cases or when not implemented correctly, the system's overall accuracy *reduces* ! This requires careful engineering to ensure that the key LLM requests (for example intent classification that may happen at the beginning of a long chain) perform accurately most of the time.


## Fine-tuning


If low latency and cost is important, fine-tuning is the way to go. With the right tools like LoRA and vllm, fine-tuned LLMs can be made to respond at 25ms latencies at less than 1/10th the cost of using GPT-4, with the same if not better accuracy.


When teams exhaust gains from prompt engineering and prompt chaining/composition, they turn to fine-tuning.


## Challenges


Most teams have found it hard to get this right. Fine-tuning takes significant time — on average a few weeks to get results close to GPT-4.


Over 60% of this time is spent curating a task-specific dataset. The fine-tuning results are almost solely dependent on the quality of the dataset.


Other challenges include:


-


Building and managing infra to train and serve fine-tuned models. There is a significant learning curve to doing this inspite of the deluge of startups and open-source solutions.


-


Managing models and datasets - this is more of a problem for mid-sized to larger teams. Teams need a good way to store and track fine-tuning runs, the associated dataset, the model and its eval scores.
