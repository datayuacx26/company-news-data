---
schema_version: "1.0.0"
document_id: "42faef6598099f083d19783b6d699e03f5a443a885d824d36d0e8463bb83dc89"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/how-can-you-catch-llm-hallucinations"
published_at: "2023-08-19T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:24.228240+00:00"
content_hash: "sha256:b14f78443beae240d19161d2742574bb555560ad7ebe41ce477397d6f0d1bc25"
---

# How can you catch LLM hallucinations?

Do not index


Original Paper


Blog URL


Most developers building on top of LLMs have 2 primary concerns - **Hallucinations** and **quality of LLM output** .


We all know LLMs have a tendency to produce misleading and made-up responses.


The worst part is that these made-up responses are really hard to catch! These sneaky buggers might even appear correct until you inspect them up close.


These mistakes are very costly - even a single occurrence leaves your users scarred with doubt and gives the impression that your product is unreliable or untrustworthy.


Now this might not be an issue if you only have a few dozen inference calls a day, but if you’re dealing with thousands of LLM calls every day, how do you catch these mistakes?


How do you make sure that your users are not being served with imaginative or factually incorrect information?


###


Evaluation of production responses using Athina


We don’t have a magical solution yet, but we *can* help you detect a lot of these bad outputs.


1. We give you a library ([athina-evals Github athina-evals Owner athina-ai Updated Jun 1, 2025](https://github.com/athina-ai/athina-evals) ) of prebuilt evals and library functions out of the box to catch common LLM mistakes.


1. You can also define your own custom logic depending on your business needs.


**What is an evaluation?**


- An evaluation takes a few inputs: the end user's query, the prompt, the response and your retrieved context, or source documents and converts them into a boolean` pass / fail` result. ( *along with a reason* )


- When you deploy an evaluation to Athina, we start running these evaluations against your production LLM outputs, to help you detect errors, quantify your app’s performance, analyze different categories of errors, and prevent bad outputs.


###


Examples


***Here are few interesting examples of incorrect LLM responses and how our clients use evaluations to catch them:***


**Problem:** The LLM sometimes makes up links that are actually 404s


**LLM response:** Thank you for reaching out to us. Please refer to the following link:[https://www.athina.ai/no-url-exist](https://www.magiklabs.app/no-url-exist)


**Evaluation:**


---


**Problem** : The LLM sometimes says words it shouldn’t (abusive / biased / restricted language)


**LLM response:** Generating return request is a straight forward process. You ought to be dumb for asking this.


**Evaluation:**


---


**Problem:** The LLM sometimes promises refunds to users which is against the refund policy.


**LLM response:** I apologise for your bad experience. As a gesture of apology, I’ll process a full refund to your order


**Evaluation:**


---


**Problem:** The LLM sometimes says it has done things that it cannot do.


**LLM response:** I have updated your shipping address. *(it can’t actually do this)*


**Evaluation:**


####


Explanation


***Why does these evaluations work while the original LLM response is itself inaccurate?***


Your prompt is usually quite long and might contain a lot of conditions, rules, and data needed to provide a good answer.


On the contrary, the LLM evaluation prompt is very simple. The LLM is being asked to solve a *much* simpler question, which is usually very easy for LLMs, and therefore it can be expected to work consistently most of the time.


We can also run the same grading prompt multiple times to detect flakiness and discard flaky results.


###


Getting Started With Athina Evals


We are currently working to solve these problems in more depth.


**We're accepting up to 5 design partners to work with for free for 2 months.**


We would work directly with you, write all your evaluations, and help you detect errors and hallucinations. (Most or all of the work would be done by us)


This would be free for 2 months ($500 value), after which you could decide to proceed or cancel. We would also waive our setup fee.


In exchange, we would just ask you to give us occasional product feedback to help us improve Athina.


Please feel free to reach out or book some time with me at[https://calendly.com/hbamoria](https://calendly.com/hbamoria)


Btw, Athina also offers a monitoring and analytics tool for your LLM outputs. Read more about it[here](https://blog.athina.ai/production-monitoring-and-analytics-for-your-llm-app/) .
