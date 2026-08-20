---
schema_version: "1.0.0"
document_id: "4660d2331962aa0b54b0522ff7b4ff96e13ff83bb8d6483124916dae04ee3bd8"
company_key: "yc-zeroentropy"
company: "ZeroEntropy"
source_id: "yc-zeroentropy-news-import-2ae9b0afcf30"
canonical_url: "https://zeroentropy.dev/articles/zerank-2-advanced-instruction-following-multilingual-reranker/"
published_at: "2025-11-18T00:00:00+00:00"
first_seen_at: "2026-07-22T21:03:20.034441+00:00"
fetched_at: "2026-07-28T21:27:35.329570+00:00"
content_hash: "sha256:59e18aa612a7eb32da19045f6b87a6d4937af64751129c713795d14c2c1fd6bc"
---

# Introducing zerank-2: The Most Accurate Multilingual Instruction-Following Reranker

TL;DR


**zerank-2** is the world’s best[reranker](https://www.zeroentropy.dev/concepts/reranker/) , purpose-built to address the most important problems in information retrieval. It outperforms every other reranker on both accuracy and latency — at half the price — with native[instruction-following](https://www.zeroentropy.dev/concepts/instruction-following-reranker/) , true[multilingual parity](https://www.zeroentropy.dev/concepts/cross-lingual-retrieval/) across 100+ languages,[calibrated relevance scores](https://www.zeroentropy.dev/concepts/score-calibration/) with a new confidence statistic, and robust handling of SQL-style aggregation queries.


Today, we’re releasing **zerank-2** : the world’s best reranker, purpose-built to address some of the most important problems in information retrieval.


Rerankers are a crucial part of what makes search and[RAG](https://www.zeroentropy.dev/concepts/rag/) pipelines actually work in production — yet, even industry-standard rerankers (including Cohere’s rerank-3.5) fail at capturing nuanced relevance for real-world queries.


zerank-2 outperforms every other reranker on both accuracy and latency — at half the price.


It excels at multilingual and cross-lingual data, follows user (and agent) instructions precisely, and is robust to those complex aggregation queries common in enterprise AI workflows.


Additionally, we’ve normalized our relevance scores and added a confidence statistic, allowing for more consistent interpretation of reranker output across all kinds of queries and domains.


## Real Stories of How Rerankers Break in Production


From early stage startups to Fortune 50 AI teams, we kept hearing similar production failure modes:


Common Failure Modes


- “It works okay in English, but performance tanks for multilingual queries”
- “We need to set a relevance threshold, but don’t know how to interpret scores consistently.”
- “It can’t understand our domain terminology or specific use case without slow LLM query rewrites”
- “Prompting it with instructions breaks it entirely”
- “Documents that would provide *helpful* context to our agent get scored too low, just because they don’t directly answer the question.”


Many rerankers overfit on public benchmarks, yet don’t generalize to these real production issues.


## Introducing zerank-2


Today, we’re releasing **zerank-2** , a state-of-the-art[cross-encoder](https://www.zeroentropy.dev/concepts/cross-encoder/) reranker built specifically to solve some of the most common production failures.


zerank-2 was trained with our new[zELO](https://www.zeroentropy.dev/concepts/zelo/) training pipeline, which converts[pairwise preferences](https://www.zeroentropy.dev/concepts/pairwise-preference/) into absolute[Elo scores](https://www.zeroentropy.dev/concepts/elo-score/) .[You can read more about our methodology here](https://arxiv.org/abs/2509.12541) .


The model is already available[behind our API](https://docs.zeroentropy.dev/models) , and on[HuggingFace](https://huggingface.co/zeroentropy) .


## What zerank-2 solves


### 1. Native Instruction-Following


Providing instructions to a reranker model can significantly boost[accuracy](https://www.zeroentropy.dev/concepts/ndcg-at-k/) results in most situations.


With zerank-2, you can now append specific instructions, lists of abbreviations, business context, or user-specific memories to influence how results get reranked.


```text
<  query  > "Candidates with IMO experience" </  query  >
<  instruction  > We're looking for engineering talent for a marine logistics company. </  instructions  >


Document  :   "Candidate experience: Worked at the International Marine Organization"
ZeroEntropy zerank  -  2  :   0.3304
ZeroEntropy zerank  -  2   with   instructions  :   0.6421
```


You can also see how depending on the business context passed into zerank-2, it correctly disambiguates *polysemic queries* — it discerns that achievement in the IMO (International Math Olympiad) is relevant for hiring at an AI startup, but that working at the IMO (International Maritime Organization) is more so for working at a maritime logistics company.


**Example IF Query** Context: Fast-growing AI startup Context: Maritime logistics company


Candidate: IMO Gold Medalist 0.54 0.46


Candidate: Worked with the IMO on compliance. 0.46 0.60


zerank-2 also knows to give appropriate scores to documents not directly answering the user’s query, but providing useful context which ensures diversity and quality of the response.


```text
Query  :   "What should I cook for dinner tonight?"


Document   1  :   "I'm allergic to nuts so I'd rather not eat that"
Document   2  :   "I'm Alex"


❌ Cohere rerank  -  3.5  :
1.   0.0419   -   I  'm Ale  x
2.   0.0434   -   I  'm allergic to nuts so I'  d rather not eat that


❌ Voyage rerank  -  2  :
1.   0.5859   -   I  'm Ale  x
2.   0.5312   -   I  'm allergic to nuts so I'  d rather not eat that


❌ OpenAI text  -  embedding  -  3  -  large  :
1.   0.0840   -   I  'm Ale  x
2.   0.1671   -   I  'm allergic to nuts so I'  d rather not eat that


✅ ZeroEntropy zerank  -  2  :
1.   0.2994   -   I  'm allergic to nuts so I'  d rather not eat that
2.   0.1645   -   I  'm Ale  x


Nice to meet you, Alex. Sorry about your anaphylaxis. 🥜⚰️
```


### 2. True Multilingual Parity


Most rerankers exhibit a strong **modality gap** between languages, ranking the very same document, translated into another language, lower than its English equivalent. That gap widens even more on non-English to non-English tasks.


We trained zerank-2 to be robust to multilinguality across 100+ languages with near-English performance across major languages, even on challenging scripts (Chinese, Arabic), and code-switching queries (Spanglish, Hinglish).


Identical queries and documents translated across languages provide a controlled ablation of multi-lingual and cross-lingual performance.


Click the dropdown menu to compare against other models (Cohere, OpenAI)!


### 3. Score Bias Adjustment and New Confidence Score


Most rerankers’ scores are “relatively” correct yet nonetheless “absolutely” meaningless: a score of 0.7 might indicate 90%[relevance](https://www.zeroentropy.dev/concepts/calibration-discrimination/) in one case, while 0.7 from another, might mean 30%.


Worse, some rerankers, like Voyage rerank-3.5, always output scores around 0.5, regardless of the true relevancy of the document. This makes setting a threshold your agent or workflow can trust to filter low-quality results almost impossible.


**We fixed it.**


Through careful calibration across query types and domains, every time zerank-2 scores 0.8, it *actually* means ~80% relevance, consistently, and predictably.


Also, it now even outputs a new **Confidence Score** , to give a measure of its own confidence.


The graphs above show this in action. Voyage and Cohere scores scattered across the space, making any single threshold fail. zerank-2 correlates linearly with ground truth scores much more robustly.


### 4. SQL-Style Queries and Aggregation Queries


It was surprising to discover just how many unstructured queries from our clients actually resemble structured SQL. Yet rerankers are not only not robust to these at all, they often degrade performance against just doing nothing.


Even a mere “ORDER BY” on quantitative values confuses every reranker, returning “ordered” results often worse than first-pass retrieval embedding models:


```text
Query  :   "Which reranker is the fastest?"
Doc   1  : Jina  's reranker: rerank-m0 • 300 ms latenc  y
Doc   2  : Cohere  's reranker: rerank-3.5 • 120 ms latenc  y
Doc   3  : ZeroEntropy  's reranker: zerank-2 • 60 ms latenc  y


❌ Cohere rerank   3.5   prefers to put itself first
✅ ZeroEntropy ranks these perfectly
```


### Get Started


Available now via the ZeroEntropy API.


[→ ZeroEntropy API Access the dashboard](https://dashboard.zeroentropy.dev/)[→ Documentation zerank-2 docs](http://docs.zeroentropy.dev/)[→ Discord Community Join the conversation](https://discord.gg/5mkQCTnmY9)→ Contact Uscontact@zeroentropy.dev


```text
from zeroentropy   import   ZeroEntropy


ze = ZeroEntropy(api_key=  "your_api_key"  )


# Pointwise reranking
results = ze.models.rerank(
model=  "zerank-2"
query=  "your query"  ,
documents=[  "document a"  ,   "document b"  ],
)
```


**Drop-in replacement** for zerank-1 or any existing reranker you run in prod, with 1 line of code change.


**Pricing** : $0.025/1M tokens, which is 50% cheaper than all other commercial rerankers.
