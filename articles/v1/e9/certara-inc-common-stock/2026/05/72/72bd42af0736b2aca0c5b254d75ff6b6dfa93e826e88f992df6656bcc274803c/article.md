---
schema_version: "1.0.0"
document_id: "72bd42af0736b2aca0c5b254d75ff6b6dfa93e826e88f992df6656bcc274803c"
company_key: "certara-inc-common-stock"
company: "Certara Inc."
source_id: "certara-inc-common-stock-news-import-6fa26b21f70d"
canonical_url: "https://www.certara.com/blog/best-practices-for-ai-prompt-engineering-in-life-sciences/"
published_at: "2026-05-22T07:18:16+00:00"
first_seen_at: "2026-07-30T08:43:47.512613+00:00"
fetched_at: "2026-07-30T08:43:49.249261+00:00"
content_hash: "sha256:4465293fb07cfa950277f907c1ab498ab255ad1fe79457103c550861b178a401"
---

# Best practices for AI prompt engineering in life sciences

ShareShareShare


Sean McGee, MS Director of Product, Certara


May 22, 2026


Prompt engineering is an emerging skill introduced to the public with the launch of ChatGPT by OpenAI in 20221. Although the hype around prompt engineering becoming the “it” career of the 21st century has died down, prompt engineering is still an essential skill for efficiently using GPTs (Generative Pre-trained Transformers) and LLMs (Large Language Models). ). In this blog, we explore how to follow AI prompt engineering best practices.


## What is AI prompt engineering?


A prompt is a question, task, or other input given to an LLM to get a specific response from the model. Prompt engineering is the crafting of a prompt. It is both an art and a science, requiring creativity and precision.


## Parts of a prompt


A prompt has four basic parts: Context


, Data


, Task


, and Format


. Depending on your use case, you may not need all the different parts in your prompt, nor do they have to be in any order. A key part of prompt engineering is figuring out which parts of a prompt are needed and crafting them to get the response you want.


### Context


Background information, examples, or context that can guide the LLM to a better response.


### Data


Optional, depending on the type of prompt. Provides any data or text that needs to be processed by the LLM.


### Task


The specific instruction to the LLM. This should be concise and clear.


### Format


The structure that you want the LLM to respond with. This can be as simple or as complicated as required.


Here is an example of a prompt with the different parts color-coded:


Classify the following sentences as Positive or Negative.


Only respond with one word. Do not provide any explanation.


Example:
Text: The food at the restaurant was great.
Positive


Text: I thought the movie was too long


The task here is clearly stated and then formatting instructions are given so that the model will only respond with “Positive” or “Negative.” In this prompt an example is given to provide additional context, with the text for analysis following.


Providing an example to the LLM is a highly effective strategy for getting the model to perform the task correctly and to respond in the way that you want. This is called *one-shot prompting* and is a key tool for prompt engineers.


## Basic prompt types


Prompt engineering can be used for several different tasks. The most common tasks are summarization, extraction, and question and answer. Each type of task requires a different type of prompt and often a different ordering of the basic prompt components described above.


The examples below are just two of the AI prompting techniques available.


### Extraction


LLMs can be used to extract data from text. Using[Certara AI](https://www.certara.com/certara-ai/) , you can extract data from scientific papers, clinical studies, and other documents.


For an extraction-type prompt, you need to tell the model what to extract and where to extract it from.


**Prompt:**
Text: 10% of patients given the study drug had adverse effects. 5% of patients who had adverse effects were positive for anti-drug antibodies.


In the above text, what percent of patients had adverse effects?


**Response:**
According to the text, 10% of patients given the study drug had adverse effects.


There are several ways to improve the prompt. For example, if you include an additional formatting instruction to only return the percent, then the model would return only 10%.


**Prompt:**


In the above text, what percent of patients had adverse effects?


Only return the percentage. Do not include any explanatory or additional text.


**Response:**
10%


### Question and answer


For Question-and-Answer-type tasks, you can either ask the LLM a question directly or you can ask it a question about some provided text or data. With direct questioning, the LLM will try to answer based on the large set of data that it was trained on. This is good for general knowledge or questions about public data. But it is also more prone to hallucination, and thus, responses should be verified.


For example, we can ask about the pharmaceutical industry relying on publicly available data:


**Prompt:**
What is the largest pharmaceutical company in Europe.


Include public data such as market cap and revenue numbers for 2024.


Be concise.


**Response:**


Based on publicly available data, here are the answers to your questions:


Roche Holding AG (ROG)


* Market Cap (2024): approximately €243 billion EUR


* Revenue (2024): approximately €63.8 billion EUR


Please note that market capitalization and revenue figures may fluctuate and are subject to change. These numbers are based on publicly available data as of 2024 and may not reflect the current market situation.


Sources:


* Market capitalization: Yahoo Finance, Bloomberg


* Revenue: Company reports, Statista


On demand webinar


## [Explore Practical Applications of GenAI in Medical Writing](https://www.certara.com/on-demand-webinar/to-generate-or-not-where-gpts-are-most-valuable-across-your-csr/)


Discover where generative AI can have the greatest impact across clinical study reports and regulatory content creation.


[Watch now](https://www.certara.com/on-demand-webinar/to-generate-or-not-where-gpts-are-most-valuable-across-your-csr/)


## Tips and tricks for prompt engineering


If you’re just getting started with prompt engineering, here are a few simple things you can do to help craft an effective prompt.


### Start small and work iteratively


You don’t need to craft the perfect prompt on your first try. Prompt engineering is an iterative process. So, it can help to start with a small prompt and then build on those results until you get the response you want.


It’s also important to consider if what you’re doing is a one-time query or if you will be doing the same thing multiple times. If you’ll be reusing the prompt repeatedly, then it is worth the extra effort to refine the prompt. If not, maybe your first or second try is just fine.


### Constrain the Large Language Model


Using phrases like


Be precise.


and


Do not make things up if you don’t know. Say “I don’t know” instead.


can help limit the amount of hallucination.


Providing extra guidance can also help you get better responses. Instead of saying


Summarize this paper.


you may get a better response by being more specific, such as


You are a smart and friendly assistant that reads scientific papers on medicinal chemistry and clinical outcomes.


Summarize the drug-drug interactions found in this paper.


Put each drug-drug interaction found in a separate bullet point.


It can also be helpful to repeat an instruction to the LLM but phrase it differently. For example, instead of just saying


Respond with only one word.


You can say


Respond with only one word. Do not add any additional text.


### Providing examples/one-shot


We saw an example above how providing an example of the output you are looking for, called *one-shot prompting* , will help the LLM. In this example, we are making a statement. But it is unclear, even to a human, what we are looking for as a response. The LLM does its best and comes up with an answer:


**Prompt:**
The odd numbers in this group add up to an even number:


15, 32, 5, 13, 82, 7, 1.


**Response:**


The odd numbers in the group are 15, 5, 13, 7, and 1.


By providing the LLM with an example of the output we are looking for, the LLM can come up with the correct response:


**Prompt:**
The odd numbers in this group add up to an even number: 8, 5, 3, 2, 5, 1.
A: True. The odd numbers are 5, 3, 5, and 1. They add up to 14, which is an even number.


The odd numbers in this group add up to an even number:


15, 32, 5, 13, 82, 7, 1.


A:


**Response:**


False. The odd numbers are 15, 5, 13, 7, and 1. They add up to 41, which is an odd number.


For more complex prompts, it may be useful to provide multiple examples, such as a True and False case. This is called *few-shot prompting* , but the idea is the same as one-shot prompting.


## Summary and next steps - Certara’s AI Solutions


You can find additional prompt engineering guidance on[Certara.AI’s developers pages](https://developer.vyasa.com/docs/prompt-considerations) .


Implementing and using AI has become a top priority for many companies. Some estimates say that there is between $5-7 billion worth of value to be unlocked by AI in the life sciences2. Despite this investment, 74% of companies struggle to achieve their AI goals3.


Building AI tools since 2017, Certara can help you and your company achieve its AI goals and master Prompt Engineering.[Certara.AI’s CoAuthor™](https://www.certara.com/coauthor/) provides an interactive environment for you to test your prompts and have the LLM extract, summarize, and glean insight from documents and scientific data.


CoAuthor is a tool designed with Medical Writers in mind. It comes with a library of pre-designed prompts to help write reports for regulatory submissions, but you can also write your own prompts and build your own prompt library, so you can be confident that you are following medical AI prompt engineering best practices and getting the most out of the LLM.


On demand webinar


## [To Generate or Not? Where GPTs Are Most Valuable Across Your CSR](https://www.certara.com/on-demand-webinar/to-generate-or-not-where-gpts-are-most-valuable-across-your-csr/)


Ready to start using GenAI in your medical and regulatory writing? Watch this webinar for everything you need to know.


[Sean McGee, MS](https://www.certara.com/teams/sean-mcgee/) Director of Product, Certara


Sean McGee is currently the Director of Product at Certara, working within the Certara artificial intelligence (AI) group. Throughout his career, Mr. McGee has supported the strategy and go-to-market motions of various software technologies, including Benchling’s laboratory informatics platform and the AI and molecular modeling and simulation offerings for Dassault Systèmes BIOVIA brand. In his role with Certara, Mr. McGee guides the development of new AI-focused use cases which maximize the benefits of the Certara AI and broader company portfolio.


Mr. McGee completed his Master of Science at the University of Notre Dame exploring the scientific and commercial applications of medical devices designed to aid in the identification of child abuse.


References


1 https://openai.com/index/chatgpt/


2 https://www2.deloitte.com/us/en/pages/life-sciences-and-health-care/articles/value-of-genai-in-pharma.html


3 https://www.bcg.com/press/24october2024-ai-adoption-in-2024-74-of-companies-struggle-to-achieve-and-scale-value


*This blog was originally published in October 2025 and has been updated for accuracy.*


## FAQs


### What are the biggest risks when using AI prompts in life sciences?


Because outputs may influence research, clinical decisions, or regulatory submissions, the main risks are hallucinations (incorrect claims), bias (misrepresentation of under-studied populations), and safety/regulatory noncompliance. Use constrained prompts, require citations, and set guardrails (e.g. “If uncertain, respond ‘I don’t know’” or “If nothing is found, respond with ‘None’”) to mitigate risk.


It is also important that, when AI is used in these and other critical areas, a human always reviews, checks, and verifies the response from the AI.


### How do I evaluate if my prompt is “good enough” in scientific tasks?


You can assess prompts by metrics such as accuracy (correctness vs. reference), consistency (stable outputs across runs), reproducibility, and error rate (number of hallucinations). In critical tasks, maintain a “prompt testing dataset” of known inputs and expected outputs. Using this dataset, you can compare performance after changes to the prompt and underlying models.


### Can I reuse prompts across life sciences subdomains (e.g. oncology, immunology)?


Yes – with caution. While many prompting principles are shared (clarity, providing examples, adding constraints), you’ll need to adapt the context, add domain-specific vocabulary, and modify or set additional constraints (e.g. safety thresholds, biomarker ranges). Always test the reuse of prompts on domain-specific validation cases to ensure reliability.


## Experience the future of regulatory writing with CoAuthor


Book a no-obligation demo to see how CoAuthor can revolutionize your regulatory writing processes.


Accelerate submission timelines with generative AI


Ensure compliance and consistency with built-in QA tools


Leverage secure, organization-specific AI for peace of mind


## You May Also Like


AllRegulatory


[Assessing Cross-Version Simcyp Simulator Prediction Performance Using a Bioequivalence Framework to Support EMA Qualification](https://www.certara.com/poster/assessing-cross-version-simcyp-simulator-prediction-performance-using-a-bioequivalence-framework-to-support-ema-qualification/)


[Assessing Cross-Version Simcyp Simulator Prediction Performance Using a Bioequivalence Framework to Support EMA Qualification](https://www.certara.com/poster/assessing-cross-version-simcyp-simulator-prediction-performance-using-a-bioequivalence-framework-to-support-ema-qualification/)[Poster](https://www.certara.com/category/poster/)


### [Assessing Cross-Version Simcyp Simulator Prediction Performance Using a Bioequivalence Framework to Support EMA Qualification](https://www.certara.com/poster/assessing-cross-version-simcyp-simulator-prediction-performance-using-a-bioequivalence-framework-to-support-ema-qualification/)


[EANM 2026](https://www.certara.com/conference/eanm-2026/)


[EANM 2026](https://www.certara.com/conference/eanm-2026/)[Conference](https://www.certara.com/category/conference/)


### [EANM 2026](https://www.certara.com/conference/eanm-2026/)


[CHI Webinar: From Dosimetry to Dose Decisions How Clinical Pharmacology and Model-Informed Approaches Are Shaping TRT Development](https://www.certara.com/webinar/from-dosimetry-to-dose-decisions-how-clinical-pharmacology-and-model-informed-approaches-are-shaping-trt-development/)


[CHI Webinar: From Dosimetry to Dose Decisions How Clinical Pharmacology and Model-Informed Approaches Are Shaping TRT Development](https://www.certara.com/webinar/from-dosimetry-to-dose-decisions-how-clinical-pharmacology-and-model-informed-approaches-are-shaping-trt-development/)[Webinar](https://www.certara.com/category/webinar/)


### [CHI Webinar: From Dosimetry to Dose Decisions How Clinical Pharmacology and Model-Informed Approaches Are Shaping TRT Development](https://www.certara.com/webinar/from-dosimetry-to-dose-decisions-how-clinical-pharmacology-and-model-informed-approaches-are-shaping-trt-development/)
