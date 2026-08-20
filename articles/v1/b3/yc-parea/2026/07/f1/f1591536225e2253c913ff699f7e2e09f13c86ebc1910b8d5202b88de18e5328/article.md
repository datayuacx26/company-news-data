---
schema_version: "1.0.0"
document_id: "f1591536225e2253c913ff699f7e2e09f13c86ebc1910b8d5202b88de18e5328"
company_key: "yc-parea"
company: "Parea"
source_id: "yc-parea-news-import-2baa56c7e8be"
canonical_url: "https://docs.parea.ai/blog/llm-eval-metrics-for-labeled-data"
published_at: null
first_seen_at: "2026-07-22T08:15:23.356185+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:2e35a1694f242160e5f053de010229bc3a9ea5d3ba8365ad21dbc2f7c72e6bd2"
---

# LLM Evaluation Metrics for Labeled Data

[Joschka Braun](https://joschkabraun.com/) on Feb 13, 2024


We help companies build & improve their AI products with our hands-own services. Request a consultation[here](https://calendly.com/parea-ai/consulting) .


The following is an overview of general purpose evaluation metrics based onfoundational models andfine-tuned LLMs as well asRAG specific evaluation metrics. The evaluation metrics rely on ground truth annotations/reference answers to assess the correctness of the model response. They were collected from research literature and discussions with other LLM app builders. Implementation in Python or links to the models are provided where available.


## ​


General Purpose Evaluation Metrics using Foundational Models


A classical yet predictive way to assess how much the model response agrees with the reference answer is to measure the overlap between the two. This is suggested in[Evaluating Correctness and Faithfulness of Instruction-Following Models for Question Answering](https://arxiv.org/abs/2307.16877) . Concretely, the authors suggest to measure the proportion of tokens of the reference answer which are also part of the model response, i.e., measure the recall. They find that this metric only slightly lags behind using GPT-3.5-turbo (see table 2 from the paper) to compare output & reference answer.


Code:[here](https://github.com/parea-ai/parea-sdk-py/blob/main/parea/evals/general/answer_matches_target_recall.py)


The authors compared more methods by their correlation with human judgment and found that the most predictive metric for the correctness of the model response is to use another LLM for grading it, in this case, GPT-4. In particular, they instruct the LLM to compare the generated response with the ground truth answer and output “no” if there is any information missing from the ground truth answer.


Code:[here](https://github.com/parea-ai/parea-sdk-py/blob/main/parea/evals/general/answer_matches_target_llm_grader.py)


The authors of[LLM-EVAL: Unified Multi-Dimensional Automatic Evaluation for Open-Domain Conversations with Large Language Models](https://arxiv.org/abs/2305.13711) take this method further by prompting an LLM to generate a JSON schema whose fields are scores that assess the model response on different dimensions using a reference answer. While this method was developed for chatbots, it exemplifies using JSON generation as a way to assess the correctness of the model response on various criteria. They compared using scales of 0-5 and 0-100, finding that the 0-5 scale only slightly outperforms.


## ​


Fine-tuned LLMs as General Purpose Evaluation Metrics


An emerging body of work proposes fine-tuning LLMs to yield evaluations assessing the correctness of a model response given a reference answer.


### ​


Prometheus


The authors of[Prometheus: Inducing fine-grained evaluation capability in language models](https://arxiv.org/abs/2310.08491) fine-tune LLaMa-2-Chat (7B & 13B) to output feedback and a score from 1-5 for a given a response, the instructions which yielded the response, a reference answer to compare against, and a score rubric. The model is highly aligned with GPT-4 evaluation and is comparable to it in terms of performance (as measured by human annotators) while being drastically cheaper. They train the model on GPT-4 generated data, which contained fine-grained scoring rubrics (a total of 1k rubrics) and reference answers to a given instruction. The methods were benchmarked on MT Bench, Vicuna Bench, Feedback Bench & Flask Eval.


Model:[here](https://github.com/kaistAI/Prometheus)


### ​


CritiqueLLM


The authors of[CRITIQUELLM: Scaling LLM-as-Critic for Effective and Explainable Evaluation of Large Language Model Generation](https://arxiv.org/abs/2311.18702) fine-tune two versions of ChatGLM-2 (6B, 12B & 66B) to output a score (1-10) and a critique. One fine-tuned version receives as input the user query and the model response, such that it can be used as a reference-free evaluation metric. The other version receives as input the user query, the model response, and the reference answer, such that it can be used as a reference-based evaluation metric.


While their method performs worse than GPT-4, it is interesting as it converts a reference-based evaluation metric into a reference-free one. They achieve this by training the reference-based model on GPT-4 outputs and the reference-free model on GPT-4 outputs that respond to prompts to revise the previous evaluation to not use the reference answer.


### ​


InstructScore


The authors of[INSTRUCTSCORE: Explainable Text Generation Evaluation with Fine-grained Feedback](https://arxiv.org/abs/2305.14282) extend the idea of fine-tuning an LLM to generate feedback & scores given a user query, the model response, and the reference answer. Instead of only giving feedback & scores, they fine-tuned the model to generate a report that contains a list of error types, locations, severity labels, and explanations. Their Llama-7B-based model is close in performance to supervised methods and outperforms GPT-4 based methods.


Model:[here](https://huggingface.co/xu1998hz/InstructScore)


## ​


RAG Specific Evaluation Metrics


In its simplest form, a RAG application consists of a retrieval and a generation step. The retrieval step fetches the context given a query. The generation step answers the initial query after being supplied with the fetched context. The following is a collection of evaluation metrics to evaluate the retrieval and generation steps in an RAG application using labeled data. To see an overview of evaluation metrics for RAG which don’t require labeled data, checkout[this](https://docs.parea.ai/blog/eval-metrics-for-llm-apps-in-prod#rag-specific-evaluation-metrics) .


### ​


Percent Target Supported by Context


This metric calculates the percentage of sentences in the target/ground truth supported by the retrieved context. It does that by instructing an LLM to analyze each sentence in the reference answer and output “yes” if the sentence is supported by the retrieved context and “no” otherwise. This is useful to understand how well the retrieval step is working and provides an upper ceiling for the performance of the entire RAG system as the generation step can only be as good as the retrieved context.


Code:[here](https://github.com/parea-ai/parea-sdk-py/blob/main/parea/evals/rag/percent_target_supported_by_context.py)


### ​


ARES: An Automated Evaluation Framework for Retrieval-Augmented Generation Systems


The authors of[ARES: An Automated Evaluation Framework for Retrieval-Augmented Generation Systems](https://arxiv.org/abs/2311.09476) improve upon the[RAGAS](https://arxiv.org/abs/2309.15217) paper using 150 labeled data to fine-tune LLM judges to evaluate context relevancy, answer faithfulness & answer relevance. Note,[context relevancy](https://docs.parea.ai/blog/eval-metrics-for-llm-apps-in-prod#relevance-of-context-to-query) measures how relevant the retrieved context is to the query,[answer faithfulness](https://docs.parea.ai/blog/eval-metrics-for-llm-apps-in-prod#faithfulness-of-generated-answer-to-context) measures how much the generated answer is based on the retrieved context, and[answer relevance](https://docs.parea.ai/blog/eval-metrics-for-llm-apps-in-prod#relevance-of-generated-response-to-query) measures how well the generated answer matches the query.


Concretely, given a corpus of documents & few-shot examples of in-domain passages mapped to in-domain queries & answers, they generate synthetic triplets of query, passage, answer. Then, they use these triplets to train LLM judges for context relevancy, answer faithfulness & answer relevance with a binary classification loss, and utilize the labeled data as validation dataset. In the last step, they use the labeled data to learn a rectifier function to construct confidence intervals for the model’s prediction (they leverage[prediction-powered inference](https://arxiv.org/abs/2301.09633) ).


When benchmarking their method to rank different RAG systems, they find that their method outperforms RAGAS and a GPT-3.5-turbo-16k baseline as measured by correlation of true ranking with ranking based on the scores of the respective method.


Code:[here](https://github.com/stanford-futuredata/ARES)


## ​


How To Get Started With These Evaluation Metrics


You can use these evaluation metrics on your own or[through Parea](https://docs.parea.ai/welcome/getting-started-evaluation) . Additionally, Parea provides dedicated solutions to evaluate, monitor, and improve the performance of LLM & RAG applications including custom evaluation models for production quality monitoring ([talk to founders](https://calendly.com/joschkabraun/chat) ).
