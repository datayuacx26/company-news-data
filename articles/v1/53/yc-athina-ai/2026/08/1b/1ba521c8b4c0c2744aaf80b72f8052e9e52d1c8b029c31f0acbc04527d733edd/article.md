---
schema_version: "1.0.0"
document_id: "1ba521c8b4c0c2744aaf80b72f8052e9e52d1c8b029c31f0acbc04527d733edd"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/how-a-5b-insurance-provider-uses-llms-for-risk-underwriting"
published_at: "2026-08-20T00:55:52.946+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:1bf362f1edd324298195c6d1445bb764a9c35524bea19d665c432c87b4760f86"
---

# How a $5B Insurance Provider Uses LLMs for Risk Underwriting

Do not index


Original Paper


Blog URL


In the competitive landscape of the insurance industry, speed, convenience, and consistent control over risk selection and decision-making are critical.


Insurers are increasingly investing in AI and digital risk processing technologies to streamline their workflows, enhance underwriting productivity, and improve broker responsiveness.


Risk processing is a manual, time-consuming and siloed operation. Here’s how their risk assessment team uses LLMs to transform their underwriting process.


###


Problem


The mentioned insurance provider works with many insurance brokers to get leads. Each broker provides **Risk submissions** in a different format (PDFs, spreadsheets, emails, broker APIs) and varying schema.


####


Here are the challenges that they face:


- Converting all the submissions to a machine-readable structured format.


- Various brokers often ask the same question to the end client but in a differently phrased manner. For e.g:


Extracting information and understanding the meaning of these submissions is difficult and time-consuming.


- Evaluating these submissions as per their internal risk guidelines and taxonomy requires manual effort.


###


Solution


The team decided to leverage LLMs to overcome these challenges.


For this, they needed a Model and Prompt that could Evaluate each submission based on their internal risk assessment questionnaire.


The team chose[Athina IDE](https://www.loom.com/share/8910d2ea58f34d38bbe41e4a446f711f) to experiment with their datasets and quickly find out the best model and prompt for the task.


####


Experiment: Evaluation of Submissions


To evaluate a submission, each question in their Risk assessment criteria had to be answered using the information available in the Risk Submission form.


Since the information available in the submission is only semantically similar to the Risk assessment criteria, they used an LLM to answer every question using the Risk Submission form.


They created 3 datasets for each Risk submission form using the models - GPT 4o, Llama 3.1 and Claude 3.5 Sonnet respectively


####


Dataset Setup & Response Generation


*(Note: This is only an indicative dataset)*


` RiskAssessmentQuestions` - List of questions from their Risk assessment criteria


` GroundTruthAsPerdocument` - Human labelled Ground truth


` Model Responses` (Dynamic Columns) - Response generated from **GPT-4o** and 3 different versions of Fine tuned models


- The model generated responses for each question by traversing through the information provided in the Risk Submission form.


- Dynamic columns in Athina IDE allowed them to execute complex prompt techniques like - COT reasoning and reference data from existing columns like a spreadsheet.


*(Note: This is only an indicative prompt)*


####


Response Evaluation


[Athina IDE](https://www.loom.com/share/8910d2ea58f34d38bbe41e4a446f711f) contains[50+ preset evaluators](https://docs.athina.ai/evals/overview) for immediate use and gives a flexibility to write custom evals as well.


Here’s an example how they used one of our evaluators to compare model’s answer with the Ground truth labels.


This helped them benchmark performance of their fine-tuned models to GPT-4o. For e.g. performance of their V2 model was a comparable to GPT 4o.


####


Conclusion


[Athina](https://athina.ai/) helped them fast-track the process of model development that can evaluate the incoming Risk submissions. Here are some of the ways in which Athina IDE was useful:


- **Rapid Experimentation:** The team was able to experiment with their pipelines quickly and figure out the best model and prompt.


- **Collaboration:** Technical and non-technical members of the team were able to collaborate on the experiments independently


- **Visibility:** The team had the visibility of the impact of changing the parameters over the entire pipeline. ****


💡


**[Athina](http://athina.ai/)** **can help you 10x your AI development →** If you’re working on LLM applications that require experiment, prototyping and evaluation of your AI pipelines, then check out[Athina AI](https://athina.ai/) or reach out to us at himanshu@athina.ai
