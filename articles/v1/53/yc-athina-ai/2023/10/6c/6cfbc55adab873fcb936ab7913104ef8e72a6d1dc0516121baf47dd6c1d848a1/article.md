---
schema_version: "1.0.0"
document_id: "6cfbc55adab873fcb936ab7913104ef8e72a6d1dc0516121baf47dd6c1d848a1"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/detecting-common-mistakes-in-rag-based-llm-applications"
published_at: "2023-10-10T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:22.552077+00:00"
content_hash: "sha256:0045086183c4ac7fa0511ee7ef48f48bd8c313b861304942f46fc8609bea2de2"
---

# Detecting Common Mistakes in RAG-based LLM Applications

Do not index


Original Paper


Blog URL


If you’re building an AI app powered by an LLM, chances are you’re using retrieval-augmented generation to get some data from a vector DB and use that to power your LLM prompts.


But as you probably know by now, the responses still tend to have mistakes in them. In this article, we will explore several common pitfalls in RAG-based Language Model (LLM) applications and discuss methods to detect these issues to improve the accuracy and reliability of your RAG application.


####


**Insufficient Information in Context**


Often, the provided context lacks sufficient information to answer a user query accurately.


If the context is insufficient, the answer is probably unreliable.


**Example 1:**


**Query** : "What was the name of the spaceship used for the moon landing in 1969?"


**Context** : "In 1969, Neil Armstrong became the first person to walk on the moon."


**Example 2:**


**Query** : "Who invented the linux operating system?"


**Context** : "Bjarne Stroustrup invented C++"


In these examples, the LLM does not have enough information to answer the user's query, so we cannot expect it to have a correct response.


####


**Response Misinterpretation or Omission**


Another issue is when the response misinterprets or omits relevant information from the context.


**Example 1:**


**Query** : "Which team collaboration software does John use?"


**Context** : "John is deciding between Microsoft Teams and Slack"


**Response** : “John’s team uses Slack for team collaboration”


Is the response logically consistent with the context? No.


####


**Lack of Contextual Faithfulness**


In some cases, an LLM-generated response might introduce information that cannot be substantiated by the provided context.


This lack of faithfulness to the context can lead to inaccurate or misleading answers.


**Example:**


**Query** : “How much does Y Combinator invest in startups”


**Context** : "Y Combinator is a startup accelerator launched in March 2005. It has been used to launch more than 4,000 companies"


**Response** : "$125,000"


Is the response faithful to the context? No.


**Reason** : The context does not contain any information to substantiate the response. Most likely, the LLM is using knowledge from it’s training data instead of sticking to only information provided in the context.


####


**Failure to Address the Query**


Sometimes, the generated response might not directly answer the question posed by the user.


**Example 1:**


**Query:** "What are the dimensions of the smartphone's screen?"


**Response:** "The smartphone has cutting-edge camera capabilities that redefine mobile photography."


**Example 2**


**Query:** "Are there any rest stops along the trail?"


**Response:** "Hikers on the trail are treated to stunning natural vistas and landscapes."


###


How to e **nsure accurate evaluation**


The most effective way to identify these issues is to use an LLM.


Breaking down the problem into manageable subproblems and utilizing classification tasks can yield very reliable results.


On top of that, you can also improve your evaluation reliability by following ways:


- **Repeat Evaluations:** Running evaluations multiple times helps eliminate inconsistent or flaky results.


- **Confidence Score:** Introduce a confidence score to indicate the reliability of the response. This can assist in identifying responses that might require further scrutiny.


- **Incorporate User Metrics:** Consider factors like tone, sentiment, and user feedback to determine whether the conversation concluded successfully. This approach prioritizes likely issues and identifies potential concerns.


Detecting and rectifying common mistakes in their responses requires careful evaluation and a multi-faceted approach.


A comprehensive setup with automatic evaluations is one of the best ways to ensure your LLM application is reliable and accurate
