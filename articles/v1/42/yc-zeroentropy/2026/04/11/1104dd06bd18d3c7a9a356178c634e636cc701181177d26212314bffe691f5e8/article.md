---
schema_version: "1.0.0"
document_id: "1104dd06bd18d3c7a9a356178c634e636cc701181177d26212314bffe691f5e8"
company_key: "yc-zeroentropy"
company: "ZeroEntropy"
source_id: "yc-zeroentropy-news-import-2ae9b0afcf30"
canonical_url: "https://zeroentropy.dev/articles/smarter-context-compression-for-llm-pipelines-zerank-2-as-a-calibrated-classifier/"
published_at: "2026-04-02T00:00:00+00:00"
first_seen_at: "2026-07-22T21:03:20.034441+00:00"
fetched_at: "2026-07-28T22:16:03.530620+00:00"
content_hash: "sha256:e1c68f8e90a615855b76d274a2cd15a574dc6bc5bfb7c6a29a28a35790ce880f"
---

# Smarter Context Compression for LLM Pipelines: zerank-2 as a Calibrated Classifier

TL;DR


- LLM pipelines are bottlenecked by context size — most of the text you feed an LLM is irrelevant to the task at hand.
- **zerank-2’s calibrated scores** turn the[reranker](https://www.zeroentropy.dev/concepts/reranker/) into a binary classifier: score each chunk against a query, threshold at a chosen cutoff, and keep only what’s relevant.
- In a real healthcare pipeline, this compressed 150-page clinical documents down to 3–10 relevant pages per criterion — **85%+ compression with 90%+ recall** .
- The pattern generalizes beyond[RAG](https://www.zeroentropy.dev/concepts/rag/) : document routing, duplicate detection, content moderation, and multi-label classification — all at **50–100x less cost** than LLM classification.


[Large language models](https://www.zeroentropy.dev/concepts/large-language-model/) are expensive to run — and the cost scales directly with how much text you put in the[context window](https://www.zeroentropy.dev/concepts/context-window/) . In many real-world pipelines, the bottleneck isn’t the LLM’s reasoning ability; it’s the sheer volume of context you have to provide before the LLM can reason at all.


This is where[ZeroEntropy](https://zeroentropy.dev/) ’s **zerank-2** reranker opens up a pattern that goes well beyond traditional search: using a reranker as a **calibrated binary classifier** to decide, page by page or[chunk](https://www.zeroentropy.dev/concepts/chunking/) by chunk, what actually belongs in your LLM’s context.


## What Is zerank-2?


zerank-2 is ZeroEntropy’s state-of-the-art multilingual[cross-encoder](https://www.zeroentropy.dev/concepts/cross-encoder/) reranker. Cross-encoders differ from[embedding models](https://www.zeroentropy.dev/concepts/bi-encoder/) in a fundamental way: rather than independently encoding a query and a document into vectors and comparing them, a cross-encoder reads the query and the document *together* and outputs a single relevance score. This joint attention makes cross-encoders substantially more accurate — at the cost of being slower for large-scale retrieval, which is why they are typically applied as a second-stage reranker on a shortlist of candidates.


zerank-2 pushes the state of the art on several axes:


Key Capabilities


- **[Instruction-following](https://www.zeroentropy.dev/concepts/instruction-following-reranker/)** : The model accepts a natural-language instruction alongside the query, letting you inject domain context, terminology, or custom ranking criteria. A healthcare query for “acute kidney injury” can be told to treat “AKI” as a synonym, or to prioritize lab values over clinical notes.
- **[Calibrated scores](https://www.zeroentropy.dev/concepts/score-calibration/)** : This is the key property for the use case in this post. The model is trained so that a score of 0.8 *means* approximately 80% relevance — consistently, across query types and domains. The score is not just a relative ranking signal; it carries absolute probabilistic meaning.
- **[Multilingual](https://www.zeroentropy.dev/concepts/cross-lingual-retrieval/)** : Trained across 100+ languages with near-English performance, including challenging scripts and code-switching queries.
- **Fast and cheap** : At $0.025 per 1M tokens — half the price of Cohere Rerank 3.5 — and with p50 latency around 130ms, it fits comfortably into production pipelines.


## The Core Insight: A Reranker Score Is a Relevance Probability


Most people use rerankers to sort a list. But zerank-2’s calibrated scores unlock a different usage pattern: **thresholding** .


For any query-document pair, zerank-2 produces a score in \[0, 1\]. Because of the calibration guarantee, you can interpret this score as a probability: “how likely is it that this chunk is relevant to this query?”


That turns the reranker into a binary classifier:


```text
score >= threshold  →  relevant  ✓  (include in context)
score <  threshold  →  not relevant  ✗  (discard)
```


## Use Case: Context Compression for Long Clinical Documents


To make this concrete, consider the problem ZeroEntropy tackled with a leading healthcare company that automates clinical review of prior authorization requests.


**The setup:** A prior authorization request is a lengthy document — often 100–200 pages of clinical notes, lab results, imaging reports, and physician letters. A clinical reviewer must assess whether the patient meets a set of coverage criteria, each expressed as a structured question:


Example Coverage Criteria


- *“Is there documentation of a failed trial of first-line therapy?”*
- *“Does the patient have a confirmed diagnosis of moderate-to-severe disease?”*
- *“Are there contraindications documented for alternative treatments?”*


There may be 50–100 such criteria per review, and the relevant evidence for each criterion is scattered across just a few pages of the full document.


**The naive approach:** Send the entire document to an LLM for each criterion. A 150-page document at ~2,000 characters per page is ~300,000 characters of context — multiplied across 80 criteria, that is 24 million characters fed to the LLM per case. At scale this is both slow and prohibitively expensive.


**The zerank-2 approach:** Score every page of the document against each criterion question. Keep only the pages that score above a threshold (or the top-K pages). Send only those to the LLM.


```text
from   zeroentropy   import   AsyncZeroEntropy


zclient   =   AsyncZeroEntropy()


async   def   score_pages_for_criterion  (
pages: list[  str  ],
criterion_question:   str  ,
batch_size:   int   =   10  ,
) -> list[  float  ]:
"""Score each page's relevance to a clinical criterion."""
all_scores: list[  float  ]   =   [  0.0  ]   *   len  (pages)


for   i   in   range  (  0  ,   len  (pages), batch_size):
batch   =   pages[i : i   +   batch_size]
response   =   await   zclient.models.rerank(
model  =  "zerank-2"  ,
query  =  criterion_question,
documents  =  batch,
)
for   result   in   response.results:
all_scores[i   +   result.index]   =   result.relevance_score


return   all_scores


def   select_relevant_pages  (
pages: list[  str  ],
scores: list[  float  ],
threshold:   float   =   0.4  ,
) -> list[  str  ]:
"""Return only pages that exceed the relevance threshold."""
return   [page   for   page, score   in   zip  (pages, scores)   if   score   >=   threshold]
```


The result is a compressed context containing only the pages with evidence relevant to that specific criterion — typically 3–10 pages instead of 150. The LLM then reasons over a manageable, high-signal context.


## The Recall vs. Context Size Tradeoff


The key question is: how much context do you need to preserve before you’ve captured essentially all the relevant evidence?


The curve has a characteristic shape: recall rises steeply at first, then flattens as you include more pages. In practice, the top 10–20 pages by zerank score capture the vast majority of ground-truth relevant pages across all criteria — often 90%+ recall at less than 15% of the total document characters.


You can discard 85%+ of document characters while preserving 90%+ of the relevant content.


## Setting the Threshold


Choosing a threshold is a one-time calibration step, and zerank-2’s calibrated scores make it interpretable rather than arbitrary.


### Option 1: Fixed threshold based on semantics


Because zerank-2’s scores are calibrated probabilities, you can pick a threshold with direct semantic meaning:


Threshold Meaning


0.2 Include anything with 20%+ chance of relevance (high recall, lower precision)


0.4 Balanced — good default for most RAG pipelines


0.6 Include only clearly relevant content (high precision, some recall loss)


0.8 Very conservative — near-certain relevance only


### Option 2: Calibrate on a labeled validation set


If you have ground-truth labels, you can directly optimize the threshold against[recall](https://www.zeroentropy.dev/concepts/recall-at-k/) :


```text
def   find_threshold_for_recall_target  (
scores_per_criterion: list[list[  float  ]],
ground_truth_pages: list[list[  int  ]],
target_recall:   float   =   0.95  ,
) ->   float  :
"""
Find the lowest threshold that achieves target_recall
on a labeled validation set.
"""
best_threshold   =   0.0


for   threshold   in   [t   /   100   for   t   in   range  (  0  ,   100  )]:
recalls   =   []
for   scores, gt_pages   in   zip  (scores_per_criterion, ground_truth_pages):
selected   =   {i   +   1   for   i, s   in   enumerate  (scores)   if   s   >=   threshold}
gt   =   set  (gt_pages)
recall   =   len  (selected   &   gt)   /   len  (gt)   if   gt   else   1.0
recalls.append(recall)


avg_recall   =   sum  (recalls)   /   len  (recalls)
if   avg_recall   >=   target_recall:
best_threshold   =   threshold
break


return   best_threshold
```


This gives you a principled threshold tied to a specific recall guarantee — for example, “include all content needed to answer 95% of criteria correctly.”


### Option 3: Top-K instead of threshold


If your downstream pipeline has a hard context budget (e.g. a 32K token limit), use top-K selection rather than a threshold:


```text
def   select_top_k_pages  (
pages: list[  str  ],
scores: list[  float  ],
k:   int   =   10  ,
) -> list[  str  ]:
ranked   =   sorted  (  enumerate  (scores),   key  =lambda   x:   -  x[  1  ])
top_indices   =   {i   for   i, _   in   ranked[:k]}
# Return in original order to preserve document flow
return   [page   for   i, page   in   enumerate  (pages)   if   i   in   top_indices]
```


## Beyond Context Compression: General Binary Classification


The context compression use case is specific to RAG, but the underlying pattern — **zerank-2 as a binary classifier** — generalizes broadly.


01


#### Document Routing


In a multi-stage pipeline, use zerank to decide which documents warrant expensive downstream processing. Score each document against a query or policy description; route only those above 0.5 to an LLM for detailed analysis.


02


#### Duplicate / Near-Duplicate Detection


Frame it as a relevance query: “Is this document essentially the same as the reference?” A high score flags near-duplicates.


03


#### Content Moderation / Policy Compliance


Score content against a policy description query. “Does this text contain instructions that could cause harm?” with a low threshold catches borderline cases for human review.


04


#### Multi-Label Classification


Run one zerank call per label/class. Each call is cheap; the calibrated scores give you a probability per class that you can threshold independently.


Here’s a multi-label classification example in practice:


```text
async   def   classify_document  (
document:   str  ,
class_descriptions: dict[  str  ,   str  ],
threshold:   float   =   0.5  ,
) -> dict[  str  ,   bool  ]:
"""
Classify a document against multiple categories using zerank-2.
Each category is described as a natural language query.
"""
results   =   {}
for   class_name, description   in   class_descriptions.items():
response   =   await   zclient.models.rerank(
model  =  "zerank-2"  ,
query  =  description,
documents  =  [document],
)
score   =   response.results[  0  ].relevance_score
results[class_name]   =   score   >=   threshold


return   results


# Example: clinical document triage
categories   =   {
"contains_lab_results"  :   "laboratory test results, blood work, or diagnostic measurements"  ,
"contains_imaging"  :   "radiology report, MRI, CT scan, X-ray, or imaging findings"  ,
"contains_diagnosis"  :   "diagnosis, clinical assessment, or documented medical condition"  ,
"contains_medication"  :   "medication, prescription, dosage, or drug therapy"  ,
}


labels   =   await   classify_document(clinical_note, categories,   threshold  =  0.4  )
```


## Putting It Together


The pattern zerank-2 enables in these pipelines is simple but powerful:


#### Express your classification task as a relevance query


What would a relevant chunk look like? Describe it in natural language.


#### Score your candidates


Run zerank-2 over all chunks, pages, or documents. This is fast and cheap.


#### Apply a threshold


Use the calibrated score to make a binary keep/discard decision. The score’s probabilistic interpretation makes threshold selection principled.


#### Send only what matters to the LLM


The LLM sees a high-signal, compressed context and reasons more accurately — at a fraction of the cost.


zerank-2 is not a replacement for LLMs. It is a fast, cheap filter that makes LLMs more effective by ensuring they spend their compute on content that actually matters.


### Get Started


ZeroEntropy offers a free 2-week trial with 1,000 queries.


[→ ZeroEntropy API Access the dashboard](https://dashboard.zeroentropy.dev/)[→ Documentation zerank-2 docs](https://docs.zeroentropy.dev/)[→ HuggingFace Open-weight models for self-hosted deployment](https://huggingface.co/zeroentropy)→ Contact Uscontact@zeroentropy.dev


```text
from   zeroentropy   import   AsyncZeroEntropy


client   =   AsyncZeroEntropy()    # uses ZEROENTROPY_API_KEY env var


response   =   await   client.models.rerank(
model  =  "zerank-2"  ,
query  =  "your query or criterion here"  ,
documents  =  [  "chunk 1 text"  ,   "chunk 2 text"  ,   "..."  ],
)


for   result   in   response.results:
print  (  f  "Document   {  result.index  }  : score=  {  result.relevance_score  :.3f  }  "  )
```


SOC 2 Type II and HIPAA-ready cloud options available for regulated industries.
