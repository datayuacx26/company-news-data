---
schema_version: "1.0.0"
document_id: "6bfc65adb16c1f416219f95f096feab9819ae395d57c1c6c18cebb2f713906aa"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/how-to-use-a-custom-grading-criteria-to-evaluate-llm-responses-llm-as-a-judge"
published_at: "2024-04-17T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:25:56.819128+00:00"
content_hash: "sha256:09be451d498c6475965680520fb6795a22ca08494bd5193736c7eb6a86bd01ad"
---

# How to Use a Custom Grading Criteria to Evaluate LLM Responses (LLM-as-a-Judge)

Do not index


Original Paper


Blog URL


In the rapidly evolving field of language models, ensuring the accuracy and relevance of responses is crucial. This blog post will guide you through setting up a custom grading criteria to evaluate responses from large language models (LLMs), using a simple conditional evaluation system.


####


What is it?


A custom grading criteria is a method used to evaluate the responses of language models based on predefined conditions.


It operates on a simple principle: if the response meets a certain condition X, it fails; otherwise, it passes.


This evaluation is integrated into a Chain of Thought (CoT) prompt, ensuring that the output is a structured JSON containing the pass/fail status along with a reason.


####


Why do you need it?


For developers working with LLMs, ensuring that the model's responses meet specific standards or criteria is essential.


This tool is particularly useful for applications where responses need to adhere strictly to certain guidelines or quality standards. It simplifies the process of assessing whether the responses from an LLM are adequate, based on the conditions you define.


Some examples:


*“If the response contains a financial figure, then fail. Otherwise pass”*


*“If the response contains a phone number, then fail. Otherwise pass”*


*“If the response says something like I don’t know, then fail. Otherwise pass”*


*“If the response claims to have taken an action, then fail. Otherwise pass”*


*“If the response mentions a refund, then fail. Otherwise pass”*


*“If the response says to contact support, then fail. Otherwise pass”*


####


How it works


The evaluator wraps the custom grading criteria inside a CoT prompt.


It checks if the response from the LLM meets the specified conditions.


If a response does not meet the condition, it is marked as a fail, and the reason for failure is recorded. This system is particularly effective for straightforward, conditional evaluations.


###


Tutorial


####


Step 1: Set Up Your Environment


First, import necessary libraries and set up your environment variables. Ensure that your API keys for OpenAI and Athina are loaded correctly.


```text
import os
from athina.evals import GradingCriteria
from athina.loaders import ResponseLoader
from athina.keys import OpenAiApiKey, AthinaApiKey
import pandas as pd
from dotenv import load_dotenv


load_dotenv()


OpenAiApiKey.set_key(os.getenv('OPENAI_API_KEY'))
AthinaApiKey.set_key(os.getenv('ATHINA_API_KEY'))


```


####


Step 2: Initialize Your Dataset


Load your dataset using the` ResponseLoader` class. This class ensures that the data is in the correct format with a "response" field, suitable for the` LlmEvaluator` class.


```text
# Create batch dataset from list of dict objects
raw_data = [
{"response": "I'm sorry but I can't help you with that query"},
{"response": "I can help you with that query"},
]


dataset = ResponseLoader().load_dict(raw_data)
pd.DataFrame(dataset)


```


####


Step 3: Configure and Run the Evaluator


Configure the evaluator using the` GradingCriteria` class by specifying your custom grading criteria. Optionally, you can also select the model you wish to use for grading.


```text
# Checks if the LLM response answers the user query sufficiently
eval_model = "gpt-3.5-turbo"


grading_criteria = "If the response says it cannot answer the query, then fail. Otherwise pass."


GradingCriteria(
model=eval_model,
grading_criteria=grading_criteria
).run_batch(data=dataset, max_parallel_evals=2).to_df()


```


This setup will evaluate each response in your dataset. If a response indicates that it cannot help with the query, it will fail; otherwise, it will pass.


As always, you can reach out to us for help anytime athello@athina.ai or using the chat on[https://athina.ai](https://athina.ai/) .
