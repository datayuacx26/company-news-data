---
schema_version: "1.0.0"
document_id: "0ee8d70b60c0ed5c69a4fd642428167cfe83efa505a0f3c30c47fccaf0088acb"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/text-summarization-llm-failure-cases-and-detection-methods"
published_at: "2023-09-20T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:23.018316+00:00"
content_hash: "sha256:150fc9d10f124e7ca064d6758c1ed8cc4143dff7f0a5b404542ecfd612d5ac35"
---

# Text Summarization: LLM Failure Cases and Detection Methods

Do not index


Original Paper


Blog URL


Large Language Models (LLMs) excel at sifting lengthy text to extract key insights, transforming documents into concise summaries.


An increasing number of companies are using LLMs for text summarization—be it to condense massive datasets or enhance data quality.


Yet, ensuring the reliability and consistency of LLMs in production remains a challenge.


At Athina, we provide tools for monitoring and evaluating LLMs effortlessly in real-time, fostering confidence in text summarization and promptly detecting failure cases.


###


Commonly used Metrics


Textual similarity metrics, such as ROUGE and BertScore, are widely used for evaluating text summarization \[Stanford\]. These metrics measure text overlap or semantic similarity using human-generated reference summaries.


However, the reliance on such summaries introduces complications in both developmental and production stages.


In the development phase, data scientists aim to measure the LLM's performance against a dataset with human-generated reference summaries. These datasets, even with detailed annotation on failure cases might not capture the nuances tailored to each use-case.


Yet, in the production phase, a unique challenge arises due to the frequent absence of reference summaries, especially with unpredictable outputs.


###


Failure cases in text summarization


*Original Text*


> "Athina is a testing framework and production monitoring platform for your LLM app. We started this project because we discovered that reliability of output is one of the biggest challenges for people trying to use LLM apps in production. LLM responses are non-deterministic by nature. This makes it very hard to measure how good the output is. Eyeballing the responses from an LLM can work in development, but it’s not a great solution.


*Correct summary*


> "Athina is a testing and monitoring platform designed for LLM apps, addressing the challenge of output reliability.


The categorization of failure cases in text summarization is an active research area. Drawing inspiration from Google Research \[paper\], we have identified the following categories.


1. **Non-informativeness**


The omission of essential information or the generation of overly generic summaries is a common failure case in text summarization.


> Response: "Athina is a platform for LLM apps.”


In the above response, the generated summary is overly generalized, failing to truly capture the core essence of the original content.


**2. Contradiction**


Another frequent issue with LLM-generated summaries is when the summary contradicts the original document.


These contradictions can arise from the LLM misunderstanding the provided information or failing to accurately interpret the relationships between sentences.


> Response: "Athina is a platform for eyeballing the responses from an LLM ”


In the above response, the generated summary incorrectly describes Athina as a company designed for eyeballing.


However, Athina is actually a testing framework and production monitoring platform that eliminates the need for manually inspecting LLM responses as described in the original text.


**3. Hallucination**


A notable challenge with LLMs is their propensity to introduce information in summaries not found in the original documents, a phenomenon termed "hallucination.”


The hallucinated information can either be factual, stemming from the LLM's pre-existing knowledge, or entirely fabricated. Regardless of its origin, when an LLM's summary diverges from the original content, it fails to summarize the original document.


> Response: "Athina is a YC company for testing and monitoring LLM apps, addressing the challenge of output reliability.”


While Athina is indeed a YC company, the original document did not specify this. Yet, the LLM incorporated this information based on its prior knowledge.


###


Our Approach for detecting summarization failure cases


We utilize the capabilities of the question-answer generation **(QAG)** framework, which allows us to pinpoint failure cases in production without the necessity for human-annotated reference summaries.


By generating questions from the source text, we can assess the informativeness of the summary \[MQAQ\].


Conversely, by generating questions from the summary, we can evaluate whether the LLM generated summaries with inaccurate or hallucinated information.


**Here is a breakdown of our approach:**


1. **Question Generation:** The LLM formulates closed-ended (Yes/No) questions drawing from both the summary and the main document.


1. **Summary-based Answers:** An LLM answerer generator responds to these questions using only the summary as a reference. The potential responses include "Yes," "No," and "Unknown."


1. **Document-based Answers:** Similarly, the LLM answerer generator answers the same set of questions, but this time, it references the primary document. Possible responses remain "Yes," "No," and "Unknown."


1. **Evaluation Metrics:** The evaluation metrics assessing the consistency between the summary-based and document-based summaries are computed to draw conclusions.


To detect the type of failure cases, we compute the following evaluation metrics:


- Hallucination Score: This metric captures the percentage of questions that received a 'Yes/No' summary-based answer and an 'Unknown' document-based answer. A high score suggests the summary might include content absent from the original document.


- Contradiction Score: This metric captures the percentage of questions that received a 'Yes' summary-based answer and a 'No document-based answer, and vice-versa. A high score suggests the summary might include content that contradicts the original document.


- Non-informativeness Score: This metric captures the percentage of questions that received a 'Unknown' summary-based answer and an 'Yes/No' document-based answer. A high score indicates that the summary may miss details from the document or be very generic.


We use various other approaches at Athina to detect inaccuracies in "Text Summarization" use cases. We'll try to cover more about them in our next posts.


*If you are building an LLM app that requires text summarization or you're facing challenges with LLM response evaluation, please reach out to us at hello@athina.ai or book a 1:1 meeting with our team here -*Calendar *.*
