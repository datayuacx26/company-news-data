---
schema_version: "1.0.0"
document_id: "c03e2ccb18df7c4cbc2792dfc4ecf7957b4554586ebbc3bc3a440ad7f791d0c7"
company_key: "yc-evidently-ai"
company: "Evidently AI"
source_id: "yc-evidently-ai-news-import-f9abf95c6ead"
canonical_url: "https://www.evidentlyai.com/blog/open-source-rag-evaluation-tool"
published_at: "2025-02-13T00:00:00+00:00"
first_seen_at: "2026-07-23T09:04:26.767431+00:00"
fetched_at: "2026-07-28T21:40:00.658555+00:00"
content_hash: "sha256:a2a4eb5dc5fed191c8a283b9813dfa74ed4f7869f67fc082d33ad831b8cbcbe3"
---

# Evidently 0.6.3: Open-source RAG evaluation and testing

## **\[fs-toc-omit\]** TL;DR


Evidently[open-source Python library](https://github.com/evidentlyai/evidently) now includes more tools for evaluating RAG systems.


With the latest update, you can:


- Score context relevance, even at the chunk level.
- Run ranking metrics like Hit Rate.
- Evaluate generation quality, with or without ground truth.
- Use different LLMs as evaluators.


These evaluations integrate with the Evidently framework, so you can generate Reports, set up Test Suites, and build live monitoring Dashboards.


> Want to try it now? Check out the[code example](https://docs.evidentlyai.com/examples/LLM_rag_evals) .


For details, keep reading.


## What is RAG evaluation?


RAG (Retrieval-Augmented Generation) combines search with generative models. It’s used in chatbots, Q&A systems, and other AI products to fetch relevant data before generating a response instead of relying only on what the LLM learned from its training data.


Like any AI system, RAG needs[quality evaluation](https://www.evidentlyai.com/llm-guide/rag-evaluation) , both during development and in production.


This RAG evaluation has two parts:


- **Retrieval** is step one. The system must find relevant information to answer the question. The key test here is: can RAG find answers we know it should?
- **Generation** is where the system formulates a response. Here, you check: Does the final answer fully address the user question? Is it accurate and true to the source?


Evaluating these separately helps diagnose specific problems. Sometimes, RAG retrieves the right data but doesn’t use it effectively. Other times, it retrieves the wrong data, leading the LLM to “hallucinate” to fill in the gaps. By running retrieval and generation checks separately, you can pinpoint where things go wrong.


Of course, you can also run them at once — but first, let’s break down how each of them works.


## Evaluating retrieval


**Retrieval** is a well-known problem in search and recommendation systems. Many established[ranking metrics](https://www.evidentlyai.com/ranking-metrics/evaluating-recommender-systems) exist, so it makes sense to reuse them for RAG.


Traditional retrieval evaluation relies on **ground truth** . You first prepare:


- a set of plausible queries (what you expect users to ask), and
- relevant documents for each query (that help answer it).


Then, you check if the system can f **ind and rank** these known documents correctly. Having the labeled dataset lets you run evaluations repeatedly as you refine your system. This approach works for RAG, too. If you can, you should totally use this: label a high-quality dataset with queries and relevant items for each and use ranking metrics for evaluation.


But in modern RAG applications, ground truth is often the bottleneck.


You are not always working with a document index. Instead, you deal with chunks — small text segments pulled from different sources. Deciding how to create these chunks is actually one of the problems you solve during experimentation:


- Which chunking strategy to use?
- Which embedding model works best?
- Should you combine keyword and vector search?


Each decision may impact retrieval quality. That’s why you need evaluations: to quantify if changes make things better or worse.


Since chunking strategies may change, what counts as a "correct source to find” also shifts. That makes it hard to fix a ground truth dataset once and for all. And it's generally quite tedious to label small text chunks as relevant or not. One way to solve this problem is **using LLMs to evaluate relevance** . This approach has already been tested in search, where LLMs help predict search preferences ([Thomas et al., 2023](https://arxiv.org/abs/2309.10621) *)* .


We implemented this same idea in Evidently.


You start with a **test set of valid questions** — queries that should be answerable using your data. You can create these manually or generate them synthetically.


For example, if we were to test a chatbot that answers queries about Evidently using our documentation, we could include different questions about the tool:


*You can generate and review synthetic datasets in*[Evidently Cloud](https://www.evidentlyai.com/register) *.*


Next, you need to:


- Run these questions through your RAG system.
- Capture the retrieved contexts — the chunks the system pulls in to answer.


Once you have this data, you can use Evidently Python library to assess retrieval quality.


**Overall context quality.** If you retrieve a single chunk or merge all chunks into one context, you can directly check its validity: “Does this context contain enough information to answer the question?” We provide a prebuilt[LLM judge](https://www.evidentlyai.com/llm-guide/llm-as-a-judge) evaluator that gives a label and explanation.


Here is a simple toy example:


**Per-chunk relevance.** But often RAG retrieves multiple chunks — either from different documents or different parts of the same document. In this case, Evidently lets you score each retrieved text individually to confirm that it contains useful information for answering the question. You can use a built-in **relevance judge** or check semantic similarity to the query.


Once all chunks are scored, you can aggregate the result for a specific query. The built-in approach we recommend is to check if at least one retrieved chunk is relevant — similar to how **Hit Rate** is calculated.


For example, here we retrieve three chunks for each query. (All are listed inside the “context” column). We score them independently. If at least one of them helps answer the question, we count it as a Hit.


If you prefer, you can aggregate relevance scores across all chunks:


You can also export results to compute dataset ranking metrics like NDCG or MRR.


With this setup, you can continuously evaluate retrieval quality and re-run tests whenever you update your RAG system — like trying different vector databases.


Since you don’t need ground truth, you can also use it for **production monitoring** . For example, you can track average relevance scores over time. If certain groups of queries score lower, it probably means your RAG database doesn’t have enough useful data in that area.


## Evaluating generation


**Generation** evaluation answers a simple question: are the responses good? There are two ways to assess this — one requires ground truth, one doesn’t.


### With ground truth


During testing, you can collect ground truth data and compare new outputs against it. In this case, you need:


- Input questions.
- Pre-approved answers.


You can create this dataset using LLMs, too. In this case, you’d first pick individual parts of texts and then ask questions answerable from them.


A good **evaluation dataset** makes a big difference. If you generate it synthetically, it is useful to let domain experts review it and edit some questions and answers as needed. This helps make your evals trustworthy and representative.


*Example RAG ground truth dataset with questions and answers.*


Then, you need to:


- Run the input questions through your RAG system.
- Capture the generated responses.
- Compare them against the ground truth.


Evidently provides multiple ways to run this comparison, including LLM-based matching, semantic similarity, and BERTScore.


Here is how this matching can look in a toy example:


In this case, we don’t look at the context: we evaluate only the final response as it is.


### Without ground truth


Once your system is live, you won’t have ground truth for every answer. Instead, you need to rely on reference-free[LLM evaluation methods](https://www.evidentlyai.com/llm-guide/llm-evaluation-metrics) .


You can also use this approach in testing, for example, after generating a diverse set of plausible questions without predefined answers.


*Example prompt to generate multiple test questions.*


This approach helps augment your test cases by creating questions that the users can ask but that might not be directly answerable from your knowledge source or have answers scattered across the texts. This makes the tests more realistic! And in production, users will generate these questions for you.


To evaluate the results, you can use reference-free LLM judges. Two of the most important criteria we suggest to look at are:


- **Answer Quality.** Is the response complete and relevant to the question? *(Considers question and response)*
- **Faithfulness.** Does the response stay true to the retrieved context without contradictions? *(Uses context and answer).*


For example, here is how the faithfulness evaluation looks for our toy example:


You can add other useful checks over your final response like:


- **Length constraints** : ensuring responses are within expected limits.
- **Refusal rate** : monitoring how often the system declines valid questions.
- **String matching** : checking for required wording (e.g., disclaimers or specific phrasing).
- **Response tone** : ensuring responses match the intended style.


You have all these options in Evidently.


## Putting it all together


You can combine all the evaluations you picked and get a summary report with score distribution.


This gives you a well-rounded evaluation of both generation and retrieval quality.


For example, here’s what you might see:


- Context is valid in 3 out of 4 cases.
- But most responses are unfaithful to the context.


In this scenario, the system retrieves the right data but doesn’t use it effectively. The next step would be improving your generation prompt to ensure responses stay true to the provided information.


You can also set up explicit pass/fail tests based on expected score distributions.


In this case, we expect all retrieved contexts to be valid and all responses to be faithful. But you can adjust these conditions — for example, allowing a certain percentage of responses to fail.


## Why Evidently?


There are a few other tools for RAG evaluation, but here’s why you might want to try **Evidently** .


It’s **open-source** — all metrics, the Report interface, and the testing API are free to use.


We take a **practical approach** to RAG evaluation, keeping things as simple as possible while providing a useful signal. Our goal is to create metrics that you can actually interpret and compare against your labels. We also added per-chunk relevance assessments, which can be fed into ranking metrics like Hit Rate — haven’t seen this implemented elsewhere.


We try to **minimize API calls** to keep evaluation costs low when you use LLM judges. Our zero-shot prompts work well with affordable models like GPT-4.0-mini. You can also swap them for other models and non-LLM alternatives like semantic similarity checks.


Evidently includes **100+ evaluation metrics** — ranking, classification, embedding-based similarity, regex, LLM-based scoring, and more. Having everything in one place makes it easier to manage evaluations without switching tools.


If you need **custom metrics** , we’ve got templates for both LLM and deterministic checks. You can easily tweak things like definitions of "correctness" to match your specific needs.


Finally, it’s not just about metrics. You get a **test interface** for structured checks, visual **reports** for easy debugging, and a self-hostable **dashboard** to track results over time.


**What’s next?** We’re working on further improvements, including testing prompts across different models and adding more parameters.


What do you think? We’d love your feedback — join the conversation[in our Discord](https://github.com/evidentlyai/evidently) !


> **Support Evidently:** if you like this release,[give us a star](https://github.com/evidentlyai/evidently/stargazers) on GitHub! ⭐


______________


*If you're running complex RAG or AI agent evaluations, check out* ***Evidently Cloud*** *. It helps you generate synthetic test data, set up and run LLM judges with no code, track evaluation results, and collaborate with your team — all in a single platform.*


[Sign up for free](https://www.evidentlyai.com/register) *or*[schedule a demo](https://www.evidentlyai.com/get-demo) *to see Evidently Cloud in action.*
