---
schema_version: "1.0.0"
document_id: "97fc37e64e70473666b25aa92d165e96f61723ec098c2c8dd491efe1fd90c0e9"
company_key: "yc-parea"
company: "Parea"
source_id: "yc-parea-news-import-2baa56c7e8be"
canonical_url: "https://docs.parea.ai/blog/building-and-evaluating-evals-for-retrieval"
published_at: null
first_seen_at: "2026-07-22T08:15:23.356185+00:00"
fetched_at: "2026-07-28T21:20:14.720808+00:00"
content_hash: "sha256:e7c1a2b66034ebd23dea5d125e5beb4b88e6904fff5cc167544cab2b1087b605"
---

# Building and Evaluating Evals for Retrieval

[Joschka Braun](https://joschkabraun.com/) on Mar 5, 2024


We help companies build & improve their AI products with our hands-own services. Request a consultation[here](https://calendly.com/parea-ai/consulting) .


In the[previous post](https://lantern.dev/blog/evaluating) , we benchmarked embedding models on synthetic clinical notes with hit rate & mean-reciprocal rank. Given that one typically doesn’t have access to the “correct” answer in a production environment, it’s of interest to have a reference-free way to evaluate the correctness of a retrieval setup. In this post, we will use & improve upon Parea AI’s pre-built eval for reference-free measuring of the hit rate of our retrieval setup.


We’ll use[Lantern](https://lantern.dev/) , a Postgres vector database and toolkit, to set up and manage our vectors. It’s faster than pgvector, and cheaper than Pinecone. Here’s the[Github repo](https://github.com/parea-ai/Asclepius-synthetic-clincal-notes-retrieval-benchmark) with our code & analysis, and a link to all[experiments](https://app.parea.ai/public-experiments/parea/Asclepius-retrieval-benchmark/) . And if you read the previous blog post, you can skip ahead toReference-free evaluation of Hit Rate .


## ​


The Dataset


We’ll use the[Asclepius Clinical Notes](https://huggingface.co/datasets/starmpcc/Asclepius-Synthetic-Clinical-Notes?row=0) dataset of synthetic physician summaries from clinical settings. Paired with those settings are questions about the summary and answers to those questions.


Each data point has the following parts:


1. A note about the medical situation
2. A question about the medical situation. An example might be, “Based on the note, what treatment plan did the doctor prescribe for x.”
3. The answer to that question
4. A task categorization to indicate what the question is asking (e.g., provide a simplified paraphrase (Paraphrasing) vs answer a question about the note (Q&A)).


The Q&A task subset has 20,038 samples, and the Paraphrasing task subset has 19,255 samples. We will choose 400 random samples from each task subset to execute the experiments.


This dataset is helpful for two reasons:


1. Since the data is synthetic and clinical, it’s unlikely to appear in the training dataset for the embedding models.


2. Measuring the performance of the embedding model on the Paraphrasing subset means assessing how well the embedding model clusters texts that express the same content. Measuring the performance on the Q&A subset means assessing how well the embedding model clusters related texts/content together. The latter assessment is predictive for using the embedding model to retrieve FAQs to power a chatbot.


## ​


The Embedding Models Used for Retrieval


We’ll define our retrieval system using BAAI’s[bge-base-en-v1.5](https://huggingface.co/BAAI/bge-base-en-v1.5) and OpenAI’s embedding models. From OpenAI, we’ll use concretely:


- text-embedding-ada-002 - which is their previous generation
- text-embedding-3-small - with embedding dimensions 512 and 1536
- text-embedding-3-large - with embedding dimensions 256 and 3072


## ​


Reference-free evaluation of Hit Rate of a retrieval system


An LLM-based eval metric is an easy way to measure a retrieval system’s hit rate without access to the correct answer (i.e “reference-free”). We can use a zero-shot prompt which instructs the model to assess whether the answer to a given question is among a list of answers. To have parseable outputs, we can use JSON mode to instruct the model to return a field called` thoughts` (which gives the model the ability to think before deciding) and a field called` final_verdict` (which is used to parse the decision of the LLM). This is encapsulated in Parea’s pre-built LLM evaluation ([implementation](https://github.com/parea-ai/parea-sdk-py/blob/d4d833d79fad4f9c367d38d9a9368153c9471459/parea/evals/rag/context_has_answer.py) and[docs](https://docs.parea.ai/api-reference/sdk/python#context-has-answer-factory) ), which leverages gpt-3.5-turbo-0125 as the default LLM.


To improve the accuracy of the LLM-based eval metric, few-shot examples are used. We select these samples:


- example 1: an example where jina-v2-base-en didn’t retrieve the correct answer for a Q&A task
- example 2: an example where bge-base-en-v1.5 didn’t retrieve the correct answer for a paraphrasing task


We use these examples to create four additional evaluation metrics such that we have five evals in total: One 0-shot, two 1-shot, and two 2-shot evaluation metrics (to understand how the order of few-shot samples affects results); their names:


- ` 0_shot`
- ` 1_shot_false_sample_1`


- Enhances` 0_shot` with few-shot example 1


- ` 1_shot_false_sample_2`


- Enhances` 0_shot` with few-shot example 2


- ` 2_shot_false_1_false_2`


- Enhances` 0_shot` with first few-shot example 1, then 2


- ` 2_shot_false_2_false_1`


- Enhances` 0_shot` with first few-shot example 2, then 1


The eval metrics are implemented[here](https://github.com/parea-ai/Asclepius-synthetic-clincal-notes-retrieval-benchmark/blob/main/evals.py) .


## ​


Experiments


### ​


A bit meta: evaluating our evals


To assess how well our evals align with hit rate, we are assessing a binary classification task (“Did the evaluation agree with the measured hit rate?”). So, we can use accuracy to quantify the performance of the evals, i.e., the percentage of how often the new eval agrees with the hit rate.


### ​


Experiment setup


We will benchmark each eval metric for each task subset and embedding model to correctly assess hit rate on 400 randomly selected samples. For that, we will follow these steps:


1. Use Lantern to embed each data entry of the task subset with the respective embedding model in two parts


- Embed the question as a vector
- Embed the answer as a vector


2. Use Parea’s SDK to execute the experiment, i.e., perform the following steps:


1. Define a function that, given a question and task category, searches the answer column of the task subset for the top 20 approximate nearest neighbors (ANNs) using the vector representation of that question ([code](https://github.com/parea-ai/Asclepius-synthetic-clincal-notes-retrieval-benchmark/blob/main/experiment.py#L30) ).
2. Define evaluation metrics that are applied to every sample’s output to calculate


- Hit rate @ 20 - Measures if the correct answer appears in the top 20. This is a binary yes/no result. A higher average is better ([code](https://github.com/parea-ai/Asclepius-synthetic-clincal-notes-retrieval-benchmark/blob/main/evals.py#L155) ).
- The LLM-based evaluation metrics as described inabove section ([code](https://github.com/parea-ai/Asclepius-synthetic-clincal-notes-retrieval-benchmark/blob/main/evals.py) ).


3. Create and run the experiment, which applies the above function and applies it over 400 random samples dataset ([code](https://github.com/parea-ai/Asclepius-synthetic-clincal-notes-retrieval-benchmark/blob/main/experiment.py#L98) ).


## ​


The results


The graph below shows how well each evaluation metric aligns with the measured hit rate. While the 0-shot pre-built evaluation metric achieves 83% on the Q&A subset (blue color), it only reaches 53% on the Paraphrasing subset (orange color). Note that the average retrieval performance on Q&A is 83% and is 58% on Paraphrasing. This means that the pre-built evaluation metric is well-suited for systems which are ready for production, but not if you’re still trying to improve performance on your labeled data.


Adding few-shot examples doesn’t improve performance and can even hurt it on the Q&A subset. On the other hand, we can see how adding one few-shot example increases the accuracy on the Paraphrasing subset by 22% (2nd and 3rd bar). While combining them is synergistic and can improve the accuracy of the eval metric to 81% (4th bar), their order matters a lot (compare 4th & 5th bar).


Additionally, we did an ablation study on using chain-of-thought in JSON mode, i.e., on requiring the` thoughts` field in the response. In the bar plots below, you can see the effect on the accuracy of the eval metric when not using the` thoughts` field (blue) and when using the` thoughts` field (orange). While there is a positive effect on the Q&A subset (1st plot below), the effect is less pronounced than on the Paraphrasing subset (2nd plot below), where improvements are up to 17% in absolute accuracy (4th bar). In particular, it’s interesting how the effectiveness of chain-of-thought increases when adding few-shot examples (bars 2 to 4 in lower plot).


## ​


Summary


In this post, we saw that our pre-built evaluation metric approximates hit rate well while not requiring labeled data. Additionally, applying prompt engineering techniques such as few-shot examples & chain-of-thought in JSON mode proves very effective when the performance of the evaluation metric isn’t high on that particular subset.


Parea + Lantern is a fast and easy way to get started with AI applications. Lantern lets you easily build a vector database from your Postgres database with ease and generate embeddings even easier - just pick a column and a model. With Parea’s toolkit, you’ll spend less time debugging your LLM application. Our all-in-one platform lets you debug, test, evaluate, and monitor every stage.
