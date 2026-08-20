---
schema_version: "1.0.0"
document_id: "ce58963862cdba6badb8d87b07e5564116df5815479792da7814d8b9fd8cfff8"
company_key: "yc-vibrant-labs"
company: "Vibrant Labs"
source_id: "yc-vibrant-labs-news-import-1f38412d73f2"
canonical_url: "https://vibrantlabs.com/research/ragas/hard-earned-lessons-from-2-years-of-improving-ai-applications"
published_at: "2025-05-07T00:00:00+00:00"
first_seen_at: "2026-07-27T06:02:47.548224+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:70be987cff46d445522836cc64976882992ba5f82b0165d1e2c4e1075fbce50d"
---

# Hard-Earned Lessons from 2 Years of Improving AI Applications

### Introduction


You’ve launched your AI app. Users are interacting with it. The responses aren’t broken, but they’re not great either. Feedback is vague, satisfaction scores are middling, and every new prompt tweak or model swap feels like fumbling in the dark. You try different workflows, chase edge cases, and still can’t tell if anything actually improved. Eventually, you stop tracking, start guessing, and ship anyway.


It doesn’t have to be this way. By the end of this post, you’ll have a clear playbook to systematically improve your AI app using evals from setup to robust, automated eval loops that exceed user expectations. These insights are distilled from our experience improving LLM applications at various enterprises and dozens of early-stage startups, along with lessons from building[Ragas](https://github.com/explodinggradients/ragas) itself.


### TL;DR


Improving AI apps isn’t art—it’s science. Set clear metrics, build realistic test sets, gather human feedback, and scale with automated LLM evaluations. This playbook walks you through the entire evaluation loop, from test dataset creation to analyzing errors and integrating user feedback, so you can stop guessing and start systematically improving your AI.


### What are evaluations?


Evaluations measure how effectively your AI system achieves its intended goals.


Evaluations in AI apps are often confused with related ideas like AI observability or guardrails. Evaluations specifically quantify system performance against clear objectives.


There are three key steps to evaluation:


1.


Curate a realistic test set: Collect or generate expected inputs to your system.


2.


Define success clearly: Specify exactly what a “good” outcome looks like and why its good.


3.


Choose precise metrics: Measure each outcome (pass/fail, scores, rankings).


Another common misconception is around evaluations and benchmarks.


Are evaluations same as benchmarks? No they aren’t. LLM benchmarks focus on evaluating different models on publicly available datasets based on academic metrics. The data or metric in which these models are evaluated does not have anything to do with your system, data or task in most cases. This is also why public benchmarks or datasets can’t help you evaluate and improve your system.


### Why AI engineers should spent time on it?


Scientific experiments / evaluations are the only reliable way to systematically improve your AI apps. You might wonder, why not just eyeball your responses and deploy changes quickly?


Here’s why evaluations outperform vibe checks:


-


**Scale:** You can’t manually eyeball hundreds of edge cases. Real-world inputs are messy, and models fail in subtle ways random checks won’t uncover.


-


**Objectivity:** Your “gut feeling” might differ from your teammate’s. Evals turn subjective impressions into consistent, repeatable metrics.


-


**Communication:** Saying “it feels better” doesn’t help teams align. Evals give concrete numbers: “The new prompt increased task completion from 50% to 70%.”


Evaluation isn’t just good practice—it’s essential infrastructure.


### Where to start?


With LLM based systems, iteration is fast, but measuring impact is slow. Unlike traditional ML workflows where training and data preparation consumed most of your time, AI application development often boils down to changing prompts, tools, or routing logic. These changes take minutes to implement but evaluating the effect of this change on the outcome takes more time as often a human needs to go through a set of sample responses and grade/vibe check them.


In fact, we’ve seen evaluations take up ~30% of a team’s iteration loop, especially when prompt tweaks or retrieval changes need to be tested across many edge cases. And we’re not alone: teams like GitHub Copilot, Casetext, and Notion have also written about the increasing role of evals in their workflows.\[3\]


So where do you begin?


Start by deciding **what to evaluate** :


-


**End-to-end evaluation** : are like integration or end-end tests. They tell you whether the full pipeline, when stitched together, actually delivers the outcome your users care about.


-


**Component-wise evaluation** : are like unit tests. They help you debug specific parts of your pipeline, such as retrievers or rerankers, to ensure each one works in isolation.


Start with **end-to-end** . It’s what users see. Once you’re confident in the output, go deeper into most important components or units that are less likely to be replaced.


### Dataset Curation


If you’ve made it this far, you’re serious about improving your AI system. Step one: build a good test dataset.


-


Pre-production


No users yet? No problem. Start by writing 10–30 realistic inputs to your AI system that represent what you expect in production. You’re designing for intent diversity, not volume.


Synthetic data can be of great use here, once you have the initial set of inputs you can use synthetic data generation pipeline to extrapolate and generate more variants on it. Details of principles to keep in mind are described below.


-


Post-production


Already have users? Great. Sample 30–50 real system inputs. Review them manually—look for quality, diversity, and edge cases. Your goal is a set of inputs that represent different scenarios you see in production.


Even though this is a good start it may not represent system inputs with enough diversity. Here is one of the simplest techniques you can use here to improve your test data


-


sample x% of system inputs from production. Ideally 1000 samples or more.


-


Deduplicate based on system input


-


Embed these system inputs using an embedding model.


-


Cluster the embeddings using KNN or DBSCAN.


-


Sample one or more from each cluster. Inspect them, and add it to your test data.


In both cases, start small, make them realistic, and gradually expand as needed. What you don’t want to do is to get nerd sniped - spiraling into designing the perfect test set before you’ve even run your first eval.


### Techniques for generating high quality synthetic test data


If you’re short on real inputs or want variations of an existing ones LLMs can help.


Use LLMs to generate high-quality synthetic data by conditioning them on a few key variables. These variables can be different for different applications, the principle here is to find variables you can condition the LLM on so that it yields inputs with enough diversity.


Here is an example for the common AI chatbot application


-


Persona: Who’s asking? (e.g. “A junior researcher studying cancer treatment”)


-


Topic: What’s it about? (e.g. “Genomics and personalized medicine”)


-


Query complexity: Short vs. long, ambiguous vs. specific


Prompt structure + example + few real queries → excellent synthetic data.


Use LLMs to extrapolate, use humans to validate. Never skip the review.


You can boost quality further by retrieving past real queries from a vector DB and using them as examples in your generation prompts.


```text
# Define some representative personas
personas   =  [
"A PhD student early in their research journey, seeking help with literature reviews and forming hypotheses."  ,
"An experienced academic looking for synthesis across papers and advanced insights."  ,
"A specialist doctor (e.g. oncologist) using the assistant for rare case support and new treatment options."
]


# Define query complexities and topics
query_complexities   =  [
"long_form_summary"  ,
"comparative_analysis"  ,
"query_with_ambiguity"
]


topics   =  [
"cancer_treatment"  ,
"clinical_trials"  ,
"genomics_and_personalized_medicine"
]


# Prompt template to generate synthetic test queries
prompt_template   =  """
You are helping build a test dataset for a healthcare AI assistant.


Generate a user query based on:
- Persona: {persona}
- Query Complexity: {complexity}
- Topic: {topic}


Instructions:
- Write a **realistic query** this persona might ask.
- Reflect the specified complexity and keep it grounded in the topic.
- Output only the query. No explanations or metadata.
"""


# Example usage (you would loop through combinations in practice)
print  (  prompt_template  . format  (
persona  = personas  [  0  ]  ,
complexity  = query_complexities  [  1  ]  ,
topic  = topics  [  2  ]
)  )
```


```text
# Define some representative personas
personas   =  [
]


# Define query complexities and topics
query_complexities   =  [
"long_form_summary"  ,
"comparative_analysis"  ,
"query_with_ambiguity"
]


topics   =  [
"cancer_treatment"  ,
"clinical_trials"  ,
"genomics_and_personalized_medicine"
]


# Prompt template to generate synthetic test queries
prompt_template   =  """
You are helping build a test dataset for a healthcare AI assistant.


Generate a user query based on:
- Persona: {persona}
- Query Complexity: {complexity}
- Topic: {topic}


Instructions:
- Write a **realistic query** this persona might ask.
- Reflect the specified complexity and keep it grounded in the topic.
- Output only the query. No explanations or metadata.
"""


# Example usage (you would loop through combinations in practice)
print  (  prompt_template  . format  (
persona  = personas  [  0  ]  ,
complexity  = query_complexities  [  1  ]  ,
topic  = topics  [  2  ]
)  )
```


We used spreadsheets/.csv files to organize and maintain such datasets but gradually moved to Notion databases, which offers better UX and API for reading and writing datasets from code.


### Human review and annotation


This is where many teams go wrong.


They underestimate how crucial human review is—and overestimate the effort required to set it up. Let’s be clear: if your goal is to align your AI system with human expectations, you must first define those expectations clearly. LLMs can’t read your mind. If you don’t show them what “good” and “bad” looks like, it’ll probably never learn.


**Step 1: Define what to measure**


Ask: *What matters in a good response for your use case?*


Pick 1–3 dimensions per input–response pair. Common examples:


-


For a RAG system: focus on response correctness and citation accuracy.


-


For a coding agent (SWE agent): focus on syntactic correctness, runtime success, and code style.


**Step 2: Choose metric type**


For each dimension, choose a metric that’s both easy to apply and actionable. You generally have three types:


1.


Binary (pass/fail) : Use when there’s a clear boundary between acceptable and unacceptable. *Example: Did the agent complete the task? Yes or no.*


1.


Numerical (e.g. 0-1): Use when partial credit makes sense and you can clearly define what scores mean. *Example: Citation accuracy of 0.5 = half the citations were incorrect.*


1.


Ranking: Use when outputs are subjective and comparisons are easier than absolute judgments. *Example: Summary A is better than Summary B.*


No matter which metric you use, always collect justification alongside it.


“Fail” without a reason isn’t helpful. Add context like *“the response hallucinated facts”* or *“missed key points.”* These justifications become essential training data for your LLM-as-judge later.


**Tip : Custom UIs for making annotation/human review frictionless**


Looking at the data can be tedious, but with this simple trick you can make it less tedious and more efficient. The best way to do it is customize the data viewer for the use-case.


Your data viewer should be:


-


tailored to your use case (RAG, summarization, agents, etc.)


-


fast to label


-


structured enough to store data and feedback consistently


###### A simple annotation UI for a RAG use-case


we use lightweight web UIs built with Claude Sonnet—it’s surprisingly good at generating custom interfaces fast. Once built, reuse this UI whenever you need new annotations from domain experts.


### Scaling with LLM as judge


Once you’ve built a solid test set with human annotations, the next challenge is **scaling** your evaluation. Every time you change a prompt, retriever, or reranker, it’s painful to re-label everything manually. This makes human review a bottleneck—and slows down iteration.


That’s where **LLMs-as-judges** come in.


The goal is simple: replace repeated manual review with automated evaluation, powered by LLMs prompted to act like your domain expert. Ideally, your LLM judge should agree with your expert labels **at least 80% of the time** .


An LLM as judge with 66.6% agreement


**Step 1: Setup**


You’ve already defined what to measure (e.g., correctness, citation accuracy). Now, write prompts that instruct the LLM to score or classify responses on those dimensions.


**Step 2: Align with human judgement**


Run your LLM judge on the annotated test set. Compare its outputs with expert labels. Iterate on prompt quality and few-shot examples until you consistently see high agreement.


What works best in practice:


-


few-shot prompting with high-quality examples


-


retrieval-based prompting: fetch similar reviewed samples from your dataset and feed them to the judge


-


include the verbal feedback you wrote during annotation — it helps the judge reason better


With a strong feedback loop, your LLM-as-judge improves over time. You can use correlation analysis methods like Spearman’s rank correlation or Cohen’s kappa to measure alignment between LLM-as-judge outputs and human evaluator ratings.


```text
def    judge_response  (  input  ,    response  )  :
# Embed the input + response to find similar, labeled examples
embedding   =  embedding_model  . embed  (  f"  {  input  }   -   {  response  }  "  )
examples   =  get_similar  (  embedding  ,    labeled_index  )


# Construct a few-shot prompt
prompt   =  f"""
Check if the response is relevant to the input. Output: Pass or Fail.


### Examples
{  examples  }


### Input
{  input  }


### Response
{  response  }
"""


return    llm  . generate  (  prompt  )
```


```text
def    judge_response  (  input  ,    response  )  :
# Embed the input + response to find similar, labeled examples
examples   =  get_similar  (  embedding  ,    labeled_index  )


# Construct a few-shot prompt
prompt   =  f"""
Check if the response is relevant to the input. Output: Pass or Fail.


### Examples
{  examples  }


### Input
{  input  }


### Response
{  response  }
"""


return    llm  . generate  (  prompt  )
```


You can also try out Dspy-like optimisation or finetuning evaluator model if you like. That said, in most cases, this isn’t required. Handcrafted examples + retrieval-based prompting is usually enough.


If you have a good system for reviewing results from llm as judges, over time, your LLM-as-judge will get better—and closer to human judgment—without constant human supervision.


### Error analysis


Evaluations tell you where your system fails. Error analysis helps you understand why it fails and what to do next.


*Evaluations = Measurement*


*Error Analysis = Diagnosis*


Let’s say your test set shows 30% of your samples are failing. What now?


Before you fix anything, you need visibility. That means inspecting inputs, intermediate outputs, and component logs data, retrievers, rerankers, llm calls, code logic, external tools, etc.


This is where observability tools come in. You can use existing observability stack like[Sentry](https://docs.sentry.io/product/insights/ai/llm-monitoring/) ,[Datadog](https://www.datadoghq.com/product/llm-observability/) , or adopt a specialised LLM observability tool \[5\] to trace and log everything you need.


Error analysis fundamentally involves two steps:


-


**Step 1: Error hypothesis**


Manually inspect the logs for each of these underperforming samples, then ask what do I think went wrong here? Write a one-line hypothesis in plain language.


-


**Step 2: Error categorisation**


Group these hypotheses into categories so you can prioritize and fix them. You can do this manually or prompt an LLM to cluster them for you:


```text
error_hypothesis   =  [
"document was not retrieved"  ,
"document had paywall"  ,
"retrieved doc had a paywall"  ,
"doc not retrieved"  ,
"paywall in document"
]


prompt   =  f"""
Given the following error hypotheses, generate a set of error categories.
Then classify each hypothesis under one of these categories.


{  error_hypothesis  }
"""


# Expected output:
{
"retrieval_issues"  :  [  "document was not retrieved"  ,    "doc not retrieved"  ]  ,
"paywall_issues"  :  [  "document had paywall"  ,    "retrieved doc had a paywall"  ,    "paywall in document"  ]
}
```


```text
error_hypothesis   =  [
"document was not retrieved"  ,
"document had paywall"  ,
"retrieved doc had a paywall"  ,
"doc not retrieved"  ,
"paywall in document"
]


prompt   =  f"""
Given the following error hypotheses, generate a set of error categories.
Then classify each hypothesis under one of these categories.


{  error_hypothesis  }
"""


# Expected output:
{
}
```


Now, **sort your categories by frequency** . Tackle the most common ones first. This gives you a clear roadmap for debugging and iteration.


###### Error categories ordered by number of occurrences


Now that you’ve identified failure points, it’s time to experiment—make changes, run evals, and measure what actually improves your pipeline. That’s what we’ll cover next.


### Experimentation


Since you have now you have identified error patterns that causing your system to underperform, you may have many ideas to improve it. This is where experimentation comes in.


What is an experiment?


*An **experiment** is a structured change made to your AI system with the goal of observing its impact on one or more evaluation metrics.*


A good experiment looks like this:


-


**Change:** tweak a **single** component in your pipeline (e.g. prompt tweak, retriever swap, reranker added).


-


**Evaluate:** run it using your existing test data and record new response, and metrics.


-


**Compare:** benchmark it against your baseline. Review and compare changes manually.


-


**Decide** : if the metrics and responses improve meaningfully, ship it or try different solution.


Let’s say you have five ideas to improve your system. Try each one, record the results, evaluate and compare. This process removes guesswork from iteration and gives you the confidence to ship the best version.


But now does it end with this? If you have deployed an AI application in production you will definitely know that however you try there will be new edge cases that pops up in production each week that effects the customer experience. That leads us to our next section.


### ML feedback loop


So you’ve run experiments, shipped improvements, and deployed your AI system. Is that the end of the loop? Not quite.


*AI problems are long-tailed. The real challenge isn’t the first 80%—it’s the last 20%.*


In production, new edge cases will surface regularly. That’s because your system now encounters a wide variety of rare, unpredictable scenarios. And the best AI systems are the ones that learn from them—quickly.


Imagine a customer support bot that only handles FAQs. That’s table stakes. The real differentiator? Handling the weird, unexpected, unstructured 20%.


###### The long tail problem in AI


**Step 1: Capture signals from production**


To catch failures in the wild, you need a signal pipeline. These can be:


-


Explicit feedback – direct input from users (e.g. thumbs down, 1-star rating)


-


Implicit feedback – behavioral signals (e.g. user doesn’t copy output, retries same input, abandons session)


Example: In a text-to-SQL system, the user doesn’t copy the result. That’s an implicit sign something went wrong—even if they didn’t downvote it.


**Step 2: Interpret feedback carefully**


Not all feedback means what you think it does.


-


A thumbs-down might be due to latency, not quality.


-


A user might not copy output because they were just double-checking their own SQL.


So: don’t take raw feedback at face value. Build a lightweight review process, either manual or with LLM-as-judge to filter noise from true signal.


**Step 3: Close the loop**


When you identify a real failure:


1.


Add it to your test dataset


1.


Run experiments specifically targeting that scenario


1.


Ship an improved version


### Conclusion


The best AI products—the ones users truly love—are built by teams that focus deeply on experimentation, evaluation and iterations to improve its reliability. That’s what sets them apart from the competition.


At[Ragas](https://app.ragas.io/) , we are building the infra to replace vibe checks with eval loops. We are turning all these learnings into building the next iteration of Ragas. We would love to talk to you if you’re interested in partnering with us to solve this problem.


🔗 Book a[slot](https://bit.ly/3EBYq4J) to chat with us.


##### Resources


1.


[Grading notes for LLM evals](https://www.databricks.com/blog/enhancing-llm-as-a-judge-with-grading-notes)


1.


[Hamels blogs](https://hamel.dev/)


1.


[Ragas blogs](https://blog.ragas.io/)


1.


[Data distribution shift and monitoring](https://huyenchip.com/2022/02/07/data-distribution-shifts-and-monitoring.html)


1.


List of open-source tools that provides llm observability.


1.


[Langfuse](https://langfuse.com/library)


2.


[Pheonix](https://blog.ragas.io/d833444236144a7597fc08ff584071c5)


3.


[Helicone](https://www.helicone.ai/)


4.


[Laminar](https://lmnr.ai/)


5.


[traceloop](https://traceloop.com/)


6.


[OpenLit](https://blog.ragas.io/d833444236144a7597fc08ff584071c5)


1.


Stories from top AI product builders


1.


[Evaluating Github copilot](https://github.blog/ai-and-ml/generative-ai/how-we-evaluate-models-for-github-copilot/)


2.


[The secret of Casetext making GPT-4 work as a legal associate with zero error](https://x.com/garrytan/status/1842210460638253498)


3.


[How we built ellipsis](https://www.ellipsis.dev/blog/how-we-built-ellipsis)


4.


[Paradigm Shifts of Eval in the Age of LLMs](https://medium.com/data-science/paradigm-shifts-of-eval-in-the-age-of-llm-7afd58e55b29)


5.


[How notion iterates on Notion AI](https://www.braintrust.dev/blog/notion)


6.


[OpenAI’s AI in enterprise report](https://cdn.openai.com/business-guides-and-resources/ai-in-the-enterprise.pdf)


1.


[Error analysis by Andrew Ng](https://www.youtube.com/watch?v=S-f7zr_2cHQ)
