---
schema_version: "1.0.0"
document_id: "142d329ac8dfaa626ad367d70fd00ddabf3728e7e3e3af14e2d78372b7e445eb"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/10-rules-for-llm-evaluation-what-we-learned-after-a-year-of-building"
published_at: "2024-02-06T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:00:19.733210+00:00"
content_hash: "sha256:fe66440e76457280ffcf96309440ff35e5ff5ff13d6616970b969af88eb0d793"
---

# 10 Rules for LLM Evaluation: What we learned after a year of building

Do not index


Original Paper


Blog URL


*This post is a compilation of notes about creating effective evaluators for your LLM pipelines.*


**Background:**


We started our journey 8 months ago when we discovered that evaluating and measuring LLM pipelines is really hard for everyone.


Since then, we’ve learned a LOT about this problem.


We spoke to hundreds of users, read dozens of papers and ran countless experiments.


We built (and rebuilt time and again) a[platform](https://bit.ly/athina-sandbox) for evaluating LLM pipelines, and run hundreds of thousands of evals against real production logs.


Here’s what we’ve learned about how to effectively evaluate LLM responses in production *(without a reference response to compare against)*


**Disclaimer: this is a long post. But here are the TLDR notes:**


1. Eval metrics unlock 10x faster iteration than eyeballing responses.


1. Other than human evaluators, LLMs are the most effective evaluators for most tasks.


1. LLM graded evaluators are NOT perfect (but they can be quite good).


1. The key is to break down evaluations into very simple subtasks - ambiguity, unclear instructions, or complex tasks will lead to poor eval performance.


1. Use evals to measure your retrievals as well, not just your responses.


1. LLM graded evals can serve as effective quality metrics, but rarely as guardrails.


1. Enforce a consistent output format.


1. Use GPT-4 for complex eval tasks. Switch to cheaper models for simpler tasks


1. Run the same evaluations in development and production.


1. Your eval metrics should be closely track your product metrics


####


**Why do you need evals?**


- Iteration: Eval metrics unlock 10x faster iteration cycles than eyeballing responses


- Production: Understand model performance in production - production queries are not always the same as the queries in your development dataset.


####


**Why do LLM evals work?**


Other than human evaluators, LLMs are the best tool we have for handling reasoning tasks on large pieces of text.


LLM evaluators can perform complex and nuanced tasks that require human-like reasoning.


*A common question that follows is:*


####


**“Why would LLM evaluation work if my own inference failed?”**


Fundamentally, the evaluation task is designed to be very different from the task you are asking your main LLM to perform.


A simple example:


- your application’s prompt might be: *“Answer this question: {question} based on this information: {context}”*


- But your evaluation prompt might be something like: *“Can you infer this response {response} from this context {context}. Answer in Y/N with a reason”*


Note that in the evaluation prompt, we are not even *including* the original question.


But there’s another reason too:


**Your application's inference task might be quite complex.** It likely includes a lot of conditions, rules, and data needed to provide a good answer. It might be generating a long response with a fair degree of complexity.


**But your evaluation task should be very simple.** The evaluator LLM is being asked to solve a *much* simpler problem, which is usually easy for powerful LLM models.


####


**How to think about LLM evals**


- **LLM evals are not perfect.** But they are the state-of-the-art for many use cases and the closest thing we have to human evaluators.


- **LLM evals can serve as effective quality metrics, but not as guardrails** For most applications (especially chat apps), the latency of LLM evals (a few seconds) is too high to run in real time. So you need to think of these evals as quality metrics, not as real-time guardrails.If latency is not an issue, then theoretically you could run them in real time, but keep in mind that LLM evals will never be perfect, so you’d be adding an additional layer of non-determinism into your product.


- **LLM eval metrics are especially good for comparisons:** The eval might mess up from time to time, but the overall metrics will give you a very good signal of how things are changing over time, and an objective way to compare performance across different models / prompts / retrieval strategies.


####


**Rules for effective, high-performing evaluators**


1. **Break the evaluations down into simple subtasks:** The simpler the subtask, the more effective the evaluator. The most effective ones will break down the task into very small, discrete subtasks (something that's very hard for an LLM or a 5th grader to get wrong).Example: Suppose you want to measure groundedness (or faithfulness) of a response to the context.For example: instead of providing it the entire response and context at once, you can pass it one sentence at a time and ask it to determine if that particular sentence has evidence in the context.Then Groundedness score = # of sentences with context / total # of sentencesHere’s a link to an[evaluator](https://docs.athina.ai/evals/preset_evals/groundedness) that uses this approach.[Ragas metrics](https://docs.athina.ai/evals/preset_evals/ragas_evals) are particularly good at this.


1. **Enforce a consistent output format:** Force the LLM to return a structured output (ex: JSON). This will create standardized eval results that are much easier to compare.Try OpenAI JSON mode, or Marvin, (or just ask for JSON and write a simple extract function).


1. **Use chain-of-thought prompting wherever reasoning is required.** This is an easy win to improve evaluation performance.


1. **Measure your retrievals as well, not just your prompts and responses** Bad retrieval ⇒ bad outputMake sure to set up some evals that measures your retrieval accuracy. For example: Athina’s[Context Sufficiency](https://docs.athina.ai/evals/preset_evals/ccei) eval, or Ragas’[Context Relevancy](https://docs.athina.ai/evals/preset_evals/ragas_evals#context-relevancy) eval.


1. **Use GPT-4 for complex tasks. Use cheaper models for simpler tasks.** There might be a slight bias in the eval from using the same model, but since the evaluation task is fundamentally different from your main inference prompt, it’s still fine to use the same model.


1. **Consider multi-step evals** Just like your LLM inference pipeline has multiple steps, an effective eval might also require multiple steps to draw a meaningful conclusion.For example, for a Summary Accuracy eval metric, here’s a multi-step eval.


1. Step 1: Generate a list of closed-ended (Y/N) question and answer pairs from the summaryStep 2: Ask an LLM to answer these questions based ONLY on the source documentThen compare the responses. You can read more about this technique[here](https://docs.athina.ai/evals/preset_evals/summarization_eval) .


1. **Run evals in production** To help you understanding how your model performs in the wild, run evals in the wild.You don’t need to run evals on every production log - running on a representative sample would still give you good indicators of model performance and response quality.


1. **Use the same evals in development AND production** This helps you identify how your production data is different from your dev dataset, and make better improvements to your model and dataset.


1. **Start with preset evals. Then add your own custom evals.** Preset evals / metrics are great to set up quickly and detect common issues in RAG applications.Ultimately, evals should be as closely aligned to your quality criteria as possible. This usually requires you to write your own custom evals.Using a[custom eval](https://docs.athina.ai/evals/custom_evals#llm-evaluator-with-a-completely-custom-prompt) is likely the best way to tailor your eval to work perfectly for your use case.


1. **Your eval metrics should closely track your product success metrics** Eval metrics are best if they can directly help you improve your product.Pick evals that are closely related to your product success metrics and – if possible, join eval results with your product analytics tool to get the complete picture.


---


**Plug:** Did I mention that[Athina](https://athina.ai/) does most of this for you, and supports custom evals? :)


Feel free to try out our sandbox to see how we run evals against production logs to provide granular performance metrics for your model:[Athina Sandbox](https://bit.ly/athina-sandbox)


Feel free to reach out anytime (shiv@athina.ai )
