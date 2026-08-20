---
schema_version: "1.0.0"
document_id: "63dfba0457d3754364cf6f7df9f1b392c56b3ef432d8d4b3ef853f97b7b467e5"
company_key: "yc-parea"
company: "Parea"
source_id: "yc-parea-news-import-2baa56c7e8be"
canonical_url: "https://docs.parea.ai/blog/eval-metrics-for-llm-apps-in-prod"
published_at: null
first_seen_at: "2026-07-22T08:15:23.356185+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:ed92b8b515d23d5f7bed5b25ee099dcefc0a21b0ac642f67ff19c12b9496bf29"
---

# Evaluation Metrics for LLM Applications In Production

[Joschka Braun](https://joschkabraun.com/) on Oct 12, 2023


We help companies build & improve their AI products with our hands-own services. Request a consultation[here](https://calendly.com/parea-ai/consulting) .


The following is an overview ofgeneral-purpose as well asRAG ,chatbot andsummarization specific evaluation metrics which do not rely on ground truth annotations/reference answers. They were collected from research literature and discussions with other LLM app builders. Implementations are provided in Python.


## ​


General Purpose Evaluation Metrics


These evaluation metrics can be applied to any LLM call and are a good starting point for determining output quality.


### ​


Rating LLMs Calls on a Scale from 1-10


The[Judging LLM-as-a-Judge with MT-Bench and Chatbot Arena](https://arxiv.org/abs/2306.05685) paper introduces a general-purpose zero-shot prompt to rate responses from an LLM to a given question on a scale from 1-10. They find that GPT-4’s ratings agree as much with a human rater as a human annotator agrees with another one (>80%). Further, they observe that the agreement with a human annotator increases as the response rating gets clearer. Additionally, they investigated how much the evaluating LLM overestimated its responses and found that GPT-4 and Claude-1 were the only models that didn’t overestimate themselves.


Code:[here](https://github.com/parea-ai/parea-sdk-py/blob/main/parea/evals/general/llm_grader.py) .


### ​


Relevance of Generated Response to Query


Another general-purpose way to evaluate any LLM call is to measure how relevant the generated response is to the given query. But instead of using an LLM to rate the relevancy on a scale, the[RAGAS: Automated Evaluation of Retrieval Augmented Generation](https://arxiv.org/abs/2309.15217) paper suggests using an LLM to generate multiple questions that fit the generated answer and measure the cosine similarity of the generated questions with the original one.


Code:[here](https://github.com/parea-ai/parea-sdk-py/blob/main/parea/evals/general/answer_relevancy.py) .


### ​


Assessing Uncertainty of LLM Predictions (w/o perplexity)


Given that many API-based LLMs, such as GPT-4, don’t give access to the log probabilities of the generated tokens, assessing the certainty of LLM predictions via perplexity isn’t possible. The[SelfCheckGPT: Zero-Resource Black-Box Hallucination Detection for Generative Large Language Models](https://arxiv.org/abs/2303.08896) paper suggests measuring the average factuality of every sentence in a generated response. They generate additional responses from the LLM at a high temperature and check how much every sentence in the original answer is supported by the other generations. The intuition behind this is that if the LLM knows a fact, it’s more likely to sample it. The authors find that this works well in detecting non-factual and factual sentences and ranking passages in terms of factuality. The authors noted that correlation with human judgment doesn’t increase after 4-6 additional generations when using` gpt-3.5-turbo` to evaluate biography generations.


Code:[here](https://github.com/parea-ai/parea-sdk-py/blob/main/parea/evals/general/self_check.py) .


### ​


Cross-Examination for Hallucination Detection


The[LM vs LM: Detecting Factual Errors via Cross Examination](https://arxiv.org/abs/2305.13281) paper proposes using another LLM to assess an LLM response’s factuality. To do this, the examining LLM generates follow-up questions to the original response until it can confidently determine the factuality of the response. This method outperforms prompting techniques such as asking the original model, “Are you sure?” or instructing the model to say, “I don’t know,” if it is uncertain.


Code:[here](https://github.com/parea-ai/parea-sdk-py/blob/main/parea/evals/general/lm_vs_lm.py) .


## ​


RAG Specific Evaluation Metrics


In its simplest form, a RAG application consists of retrieval and generation steps. The retrieval step fetches for context given a specific query. The generation step answers the initial query after being supplied with the fetched context.


The following is a collection of evaluation metrics to evaluate the retrieval and generation steps in an RAG application.


### ​


Relevance of Context to Query


For RAG to work well, the retrieved context should only consist of relevant information to the given query such that the model doesn’t need to “filter out” irrelevant information. The RAGAS paper suggests first using an LLM to extract any sentence from the retrieved context relevant to the query. Then, calculate the ratio of relevant sentences to the total number of sentences in the retrieved context.


Code:[here](https://github.com/parea-ai/parea-sdk-py/blob/main/parea/evals/rag/context_query_relevancy.py) .


### ​


Context Ranked by Relevancy to Query


Another way to assess the quality of the retrieved context is to measure if the retrieved contexts are ranked by relevancy to a given query. This is supported by the intuition from the[Lost in the Middle paper](https://arxiv.org/abs/2307.03172) , which finds that performance degrades if the relevant information is in the middle of the context window. And that performance is greatest if the relevant information is at the beginning of the context window.


The RAGAS paper also suggests using an LLM to check if every extracted context is relevant. Then, they measure how well the contexts are ranked by calculating the mean average precision. Note that this approach considers any two relevant contexts equally important/relevant to the query.


Code:[here](https://github.com/parea-ai/parea-sdk-py/blob/main/parea/evals/rag/context_ranking_pointwise.py) .


Instead of estimating the relevancy of every rank individually and measuring the rank based on that, one can also use an LLM to rerank a list of contexts and use that to evaluate how well the contexts are ranked by relevancy to the given query. The[Zero-Shot Listwise Document Reranking with a Large Language Model](https://arxiv.org/abs/2305.02156) paper finds that listwise reranking outperforms pointwise reranking with an LLM. The authors used a progressive listwise reordering if the retrieved contexts don’t fit into the context window of the LLM.


Aman Sanger (Co-Founder at[Cursor](https://cursor.sh/) ) mentioned ([tweet](https://twitter.com/amanrsanger/status/1732145826963828997) ) that they leveraged this listwise reranking with a variant of the Trueskill rating system to efficiently create a large dataset of queries with 100 well-ranked retrieved code blocks per query. He underlined the paper’s claim by mentioning that using GPT-4 to estimate the rank of every code block individually performed worse.


Code:[here](https://github.com/parea-ai/parea-sdk-py/blob/main/parea/evals/rag/context_ranking_listwise.py) .


### ​


Faithfulness of Generated Answer to Context


Once the relevance of the retrieved context is ensured, one should assess how much the LLM reuses the provided context to generate the answer, i.e., how faithful is the generated answer to the retrieved context?


One way to do this is to use an LLM to flag any information in the generated answer that cannot be deduced from the given context. This is the approach taken by the authors of[Evaluating Correctness and Faithfulness of Instruction-Following Models for Question Answering](https://arxiv.org/abs/2307.16877) . They find that GPT-4 is the best model for this analysis as measured by correlation with human judgment.


Code:[here](https://github.com/parea-ai/parea-sdk-py/blob/main/parea/evals/rag/answer_context_faithfulness_binary.py) .


A classical yet predictive way to assess the faithfulness of a generated answer to a given context is to measure how many tokens in the generated answer are also present in the retrieved context. This method only slightly lags behind GPT-4 and outperforms GPT-3.5-turbo (see Table 4 from the above paper).


Code:[here](https://github.com/parea-ai/parea-sdk-py/blob/main/parea/evals/rag/answer_context_faithfulness_precision.py) .


The RAGAS paper spins the idea of measuring the faithfulness of the generated answer via an LLM by measuring how many factual statements from the generated answer can be inferred from the given context. They suggest creating a list of all statements in the generated answer and assessing whether the given context supports each statement.


Code:[here](https://github.com/parea-ai/parea-sdk-py/blob/main/parea/evals/rag/answer_context_faithfulness_statement_level.py) .


## ​


AI Assistant/Chatbot-Specific Evaluation Metrics


Typically, a user interacts with a chatbot or AI assistant to achieve specific goals. This motivates to measure the quality of a chatbot by counting how many messages a user has to send before they reach their goal. One can further break this down by successful and unsuccessful goals to analyze user & LLM behavior.


Concretely:


1. Delineate the conversation into segments by splitting them by the goals the user wants to achieve.
2. Assess if every goal has been reached.
3. Calculate the average number of messages sent per segment.


Code:[here](https://github.com/parea-ai/parea-sdk-py/blob/main/parea/evals/chat/goal_success_ratio.py) .


## ​


Evaluation Metrics for Summarization Tasks


Text summaries can be assessed based on different dimensions, such as factuality and conciseness.


### ​


Evaluating Factual Consistency of Summaries w.r.t. Original Text


The[ChatGPT as a Factual Inconsistency Evaluator for Text Summarization](https://arxiv.org/abs/2303.15621) paper used` gpt-3.5-turbo-0301` to assess the factuality of a summary by measuring how consistent the summary is with the original text, posed as a binary classification and a grading task. They find that` gpt-3.5-turbo-0301` outperforms baseline methods such as SummaC and QuestEval when identifying factually inconsistent summaries. They also found that using` gpt-3.5-turbo-0301` leads to a higher correlation with human expert judgment when grading the factuality of summaries on a scale from 1 to 10.


Code:[binary classification](https://github.com/parea-ai/parea-sdk-py/blob/main/parea/evals/summary/factual_inconsistency_binary.py) and[1-10 grading](https://github.com/parea-ai/parea-sdk-py/blob/main/parea/evals/summary/factual_inconsistency_scale.py) .


### ​


Likert Scale for Grading Summaries


Among other methods, the[Human-like Summarization Evaluation with ChatGPT](https://arxiv.org/abs/2304.02554) paper used` gpt-3.5-0301` to evaluate summaries on a Likert scale from 1-5 along the dimensions of relevance, consistency, fluency, and coherence. They find that this method outperforms other methods in most cases in terms of correlation with human expert annotation. Noteworthy is that[BARTScore](https://arxiv.org/abs/2106.11520) was very competitive to` gpt-3.5-0301` .


Code:[Likert scale grading](https://github.com/parea-ai/parea-sdk-py/blob/main/parea/evals/summary/likert_scale.py) .


## ​


How To Get Started With These Evaluation Metrics


You can use these evaluation metrics on your own or[through Parea](https://docs.parea.ai/welcome/getting-started-evaluation) . Additionally, Parea provides dedicated solutions to evaluate, monitor, and improve the performance of LLM & RAG applications including custom evaluation models for production quality monitoring ([talk to founders](https://calendly.com/joschkabraun/chat) ).
