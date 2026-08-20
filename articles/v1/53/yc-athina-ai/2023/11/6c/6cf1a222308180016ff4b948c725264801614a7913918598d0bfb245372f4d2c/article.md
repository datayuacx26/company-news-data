---
schema_version: "1.0.0"
document_id: "6cf1a222308180016ff4b948c725264801614a7913918598d0bfb245372f4d2c"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/what-we-learned-from-speaking-to-50-llm-developers-building-rag-apps"
published_at: "2023-11-29T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:21.272352+00:00"
content_hash: "sha256:94569bed4768be62892e1516fa6ac7525d6b027e2015e062ba2d8b25c11cf903"
---

# What we learned from speaking to 50+ LLM developers building RAG apps.

Do not index


Original Paper


Blog URL


We’ve spoken to over 50 developers building RAG-based applications last month.


##


Here are 5 Common Mistakes an LLM might make:


1. [Bad Retrievals](https://docs.athina.ai/evals/preset_evals/ccei) **:** This normally happens when the context provided is irrelevant to the query.


1. [Unfaithful](https://docs.athina.ai/evals/preset_evals/faithfulness) **Responses:** Often, the response doesn't stay true to the context provided.


1. **Not knowing which piece of data to use to answer the query:** This situation is when your RAG pipeline passes 2 documents to the LLMs context. Document A contains the information the LLM should use, but the LLM prefers to use the information in Document B and ends up ignoring Document A.


1. **Verbose Answers / Rambling /**[Irrelevant Answers:](https://docs.athina.ai/evals/preset_evals/draq)


1. The LLM might give an answer that is tangential or irrelevant to the user’s query.The LLM will give the correct answer, but then add unnecessary, extraneous information on top (often completely made up)


1. **Not staying within the bounds of your prompt instructions:**


1. Your prompt says “NEVER mention customer support, " yet the LLM will occasionally mention customer support.You can detect such issues with a[custom LLM-graded eval](https://docs.athina.ai/evals/custom_evaluators)


##


Now that we know what goes wrong with LLMs, let's see how to improve yours:


Here's what the best teams we spoke to were doing to improve their RAG performance.


1. [Log your data for visibility](https://docs.athina.ai/quickstart) : It’s impossible to improve definitively if you don’t log your data somewhere for visibility. Ideally, this should be a platform where you can also filter, search and view traces to debug.


1. **Improve your data and retrieval pipeline:** Your LLM app will only perform as well as the data/context you provide it.


There’s a lot you can do to make this better, so this is usually where you’ll find the best scope for model performance boosts.


Here are some ways you can improve your retrievals:


- Improve your data quality (sanitize, pre-process, add metadata tags)


- Use filters in addition to similarity to fetch more relevant results


- Introduce a re-ranking step


- Fine-tune your embedding model


- Experiment with different chunking strategies


1. **Configure a suite of evals to measure performance:** Looking at a number that tells you your model performance is a hell of a lot easier than eyeballing responses trying to look for mistakes.


> ”If it doesn’t get measured, it doesn’t get managed”


[Athina’s Preset Evals](https://docs.athina.ai/evals/preset_evals) can help you with this by giving you a **pass rate** , and a breakdown of the different types of failures.


1. **Fine-Tune Your Model:** Fine-tuning doesn’t always work, but when it does the results can be great. In the best case, you will get equivalent or better performance than GPT-4 at a fraction of the cost and latency.


Fine-tuning will work very well when you are trying to generate certain types of output, but it isn’t a replacement for your RAG system.


It usually works best along *with* your RAG system.


> Pro Tip:


We’ll share more about this in the coming weeks.


1. **Split your inference step - don’t overload a single prompt:** One of the most effective techniques we’ve seen people implement is to split the inference step so the response generation happens using a much more targeted prompt.


For example: a simple app’s flow looks like this: *Query → Retrieve Context → Generate Response (Generic Prompt that has to handle lots of scenarios)*


In a simple pipeline like this, the inference step has to handle a lot of different potential scenarios.


It’s often a lot better to have something like this: *Query → Match Intent → Retrieve (more specific) context → Generate Response (using a more specific Prompt)*


1. **Force JSON output:** Instead of directly letting the LLM respond to your users, you can force the LLM to return a JSON output instead, which you can then validate and sanitize before finally responding to your users.


OpenAI recently introduced a new JSON Mode in their API.


1. **Implement a Cache:** Having a caching system will naturally improve your latency and cost, but it can also improve your response quality **** because you can return a fixed, template response (or ask an LLM to return an appropriate variant of such a template response).


At the end of the day, building a reliable conversational AI is hard work.


But being aware of the common pitfalls and reliability issues, and having a game plan to address them puts you way ahead of the curve.
