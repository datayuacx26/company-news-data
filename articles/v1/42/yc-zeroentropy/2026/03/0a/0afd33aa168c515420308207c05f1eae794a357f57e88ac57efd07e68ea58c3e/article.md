---
schema_version: "1.0.0"
document_id: "0afd33aa168c515420308207c05f1eae794a357f57e88ac57efd07e68ea58c3e"
company_key: "yc-zeroentropy"
company: "ZeroEntropy"
source_id: "yc-zeroentropy-news-import-2ae9b0afcf30"
canonical_url: "https://zeroentropy.dev/articles/mteb-evals/"
published_at: "2026-03-28T00:00:00+00:00"
first_seen_at: "2026-07-22T21:03:20.034441+00:00"
fetched_at: "2026-07-28T22:17:07.377837+00:00"
content_hash: "sha256:a9c59e979a1be3e2dfe3388f896b34bde8e691f3554c0181c73cfded76c2cca9"
---

# Beyond Binary: A New Version of the MTEB

TL;DR


- [MTEB](https://www.zeroentropy.dev/concepts/mteb/) largely utilizes binary relevance scores, making metrics like[NDCG](https://www.zeroentropy.dev/concepts/ndcg-at-k/) unable to distinguish a perfect answer from a loosely related document.
- Graded (i.e., continuous) relevance restores discriminative power to metrics like NDCG that degenerate under binary labels.
- **We re-annotated 28 MTEB retrieval datasets** with graded[LLM pointwise scoring](https://www.zeroentropy.dev/concepts/llm-as-judge/) using three judges (GPT-5-nano, Grok-4-fast, Gemini-3-flash).
- **We evaluate 16 embedding models, 7 rerankers, and all 128 combinations** .
- **zembed-1 rises from to 1st place** on MTEB under graded relevance. **zerank-2 leads all rerankers.**
- **[Explore the dashboard →](https://www.zeroentropy.dev/evals/)**


# Beyond Binary Relevance


## Improving on MTEB’s Evaluation Methodology


MTEB was a significant contribution to the retrieval community: a shared evaluation suite that allowed, for the first time, apples-to-apples comparison across embedding and reranking models. But as the field has matured, the binary nature of its relevance judgments (each document is either fully “relevant” or fully “not relevant”) has proven limiting.


When frontier models are separated by fractions of a percent on[Recall@100](https://www.zeroentropy.dev/concepts/recall-at-k/) , the discriminative power of the evaluation methodology matters as much as the evaluation data itself.


Where Binary Breaks Down


- Query: “how does mRNA vaccination work?” — a paper detailing the lipid nanoparticle delivery mechanism and a paper which only mentions vaccines in passing will both score 1. The model that ranks the explanation first gets no credit.
- A crowdsourced annotator mislabels a document as relevant. Under binary labels, that error is absolute. Under graded scoring from three independent judges (7.0, 0.0, 0.0, averaging to 2.3), the damage is contained.
- Two embedding models retrieve the same 100 documents for a query but order them differently — one puts the best answer at rank 1, the other buries it at rank 40. Binary Recall@100 is identical for both; only graded NDCG can tell them apart.


## Graded Re-Annotation with LLM Judges


We took the queries and corpora from 28 MTEB datasets — spanning retrieval, reranking, and instruction-retrieval tasks — and produced new relevance judgments using[LLM pointwise annotation](https://www.zeroentropy.dev/concepts/pointwise-scoring/) .


**Graded relevance restores to NDCG the property that makes it useful: the ability to reward systems that rank better documents higher.**


Each query-document pair received a score on a continuous 0-10 scale from three independent frontier judges:


Judge Provider


GPT-5-nano OpenAI


Grok-4-fast xAI


Gemini-3-flash Google


Three judges from three providers. Inter-annotator agreement is high ([Pearson](https://www.zeroentropy.dev/concepts/pearson-correlation/) *r* = 0.7-0.8), lending confidence that the scores reflect genuine relevance rather than model-specific biases.


The annotation pipeline has five stages. Every step is idempotent and resumable — pair IDs are deterministic hashes of` (query_id, query_text, doc_id, doc_text)` , so re-runs skip completed work.


#### Dataset extraction


We pull queries, corpora, and original qrels from 28 MTEB datasets via HuggingFace, covering retrieval, reranking, and instruction-retrieval task types from the MTEB(Multilingual, v2) benchmark. Multi-subset datasets (e.g. MIRACL or WikipediaRetrievalMultilingual) are handled as separate subsets sharing a single evaluation.


#### Embed and rank


Each of the 16 embedding models encodes every corpus and query. Rankings are computed via brute-force[cosine similarity](https://www.zeroentropy.dev/concepts/cosine-similarity/) : L2-normalize both matrices, compute` queries @ corpus.T` , then` argpartition` for top-1000 per query. Models are served via their native APIs (OpenAI, Voyage, Cohere, Gemini) or self-hosted on Modal (zembed-1, qwen3, harrier, jina, bge).


#### Build the annotation pool


For each query, we union the top-K retrieved documents *across all 16 embedding models* . This ensures every judge scores every document that any model might surface. This is primarily a cost-saving measure — since we care about the relative performance of models on this list, we only need to annotate documents that at least one of them retrieved, rather than scoring the entire corpus. This will mean that absolute scores for a model may change as we add more embedding models to the leaderboard, but the pairwise ranking between any two models for any given dataset will remain invariant to such additions.


#### Pointwise LLM annotation


Each (query, document) pair is scored independently — one pair per LLM call, no listwise comparison between documents. The prompt structure forces reasoning before scoring: judges must write their analysis *first* and commit to a 0-10 float *last* , reducing anchoring bias. The rubric explicitly anti-inflates: “Most documents should score below 7. Scores of 9-10 should be rare.”


The[system prompt](https://www.zeroentropy.dev/concepts/system-prompt/) (rubric + query) is placed in the cacheable prefix, and work is sorted by query_id, so all documents for the same query share a cached prompt. At ~90%[cache hit rate](https://www.zeroentropy.dev/concepts/prompt-caching/) , this cuts input costs by roughly 5x. Each judge runs at up to 1024 concurrent API calls.


```text
You are a search relevance judge. You will be given a query and a
single document. Your task is to evaluate how relevant the document
is to the query.


Score the document using a floating point number between 0 and 10:


- 10: Near-perfect match. The document directly and comprehensively
answers the query.
- 9:  Extremely relevant. Substantial, directly useful information,
missing only minor details.
- 8:  Highly relevant. Significant related content that would
meaningfully help answer the query.
- 6-7: Moderately relevant. Clear connection with some useful
information, but much is tangential.
- 4-5: Borderline to weakly relevant. Shares a general subject
area but unclear if it helps.
- 2-3: Barely relevant. Only superficial or incidental connection
(e.g., shared keywords, different context).
- 0-1: Irrelevant. No meaningful connection to the query.


Important:
- Most documents should score below 7.
- Scores of 9-10 should be rare and reserved for truly exceptional
matches.
- Do not inflate scores.


Respond with JSON: {"reasoning": <string>, "score": <float>}


It is very important that you output your reasoning FIRST and the
score LAST. Think through why the document is or isn't relevant
before committing to a number.
```


#### Evaluate all 128 configurations


For each of the three judges independently, we compute NDCG@k and Recall@k curves from k=1 to k=100 across all 128 system configurations (16 embeddings × 7 rerankers + 16 embed-only). Reranked configurations take the top-100 documents from each embedding model and re-sort by[reranker](https://www.zeroentropy.dev/concepts/reranker/) score — meaning a reranker can reorder but never recover documents the embedder missed. Recall@100 is identical with or without a reranker; only the ranking within that window changes. Evaluation is parallelized across embedding models.


NDCG uses the full continuous scores. Recall uses per-judge thresholds calibrated to each judge’s score distribution (Gemini: 6.0, GPT-5-nano: 7.0, Grok-4-fast: 6.5) — the thresholds differ because the judges have different score distributions, but the resulting relevance sets are comparable in size (id est, they target the same percentiles). This normalization actually matters very little — we found nearly identical absolute performance metrics and relative rankings regardless of a flat 7.0 threshold, percentile based thresholding, or quantile normalization; the evaluation leaderboard was remarkably invariant to such considerations. (For example, the top three models on the average leaderboard are unchanged regardless of threshold from 0.4 to 0.9).


#### Aggregate into the dashboard


The per-dataset, per-judge evaluation curves are averaged in two stages: first across datasets for each judge (so “GPT-5-nano average” is the mean of that judge’s per-dataset NDCG/Recall curves), then across judges to produce the “Average” pseudo-judge shown by default on the dashboard. The dashboard lets you drill into any individual dataset or judge, switch between absolute and delta-vs-baseline views, and toggle between embed-only, reranked, or all configurations.


## Results Across 28 Datasets


**[Explore the full interactive dashboard →](https://www.zeroentropy.dev/evals/)**


The dashboard covers all 28 datasets, all judges, and all 128 system configurations. Results can be filtered by embed-only, reranked, or combined views; compared in delta mode against any baseline; and sorted by any metric at any K.


Embed-only Recall@K, averaged across 28 datasets and 3 judges. Toggle between absolute and delta-vs-baseline views.


The ranking shifts meaningfully from binary to graded. Of the embed-only models present on both leaderboards (averaged across datasets and judges on graded NDCG@10, compared against binary MTEB Retrieval):


- **harrier-27b** and **qwen3-embedding-4b** slip slightly, but stay near the top (1st→3rd and 3rd→4th)
- **harrier-0.6b** and **harrier-270m** drop sharply — 2nd→11th (70.8 → 0.658) and 5th→13th (66.4 → 0.625)
- **jina-v5-text-small** and **openai-v3-large** stay roughly in place (6th→8th and 9th→10th)
- **voyage-4** , absent from binary MTEB, lands second at 0.712
- **zembed-1** makes the largest jump, from 8th on binary (63.4) to 1st on graded (0.715)


One internal signal we use to sanity-check an evaluation: **the gap between models of different sizes in the same family** . When a small model scores nearly as well as its larger sibling on a benchmark — same architecture, same data, same training recipe — it’s usually either because the entire model family is quietly[overfitting](https://www.zeroentropy.dev/concepts/overfitting/) to that metric, or because the benchmark is so poor in discriminative power that it genuinely can’t grade the difference between a 0.6b model and 27b model. We encountered this with zerank-1 and zerank-1-small: near-identical scores on binary MTEB, but a clear gap on our internal graded evaluations. That experience is part of what motivated this work.


**Models trained on continuous relevance signals rise under graded evaluation. Models that may have been optimized for binary benchmarks lose ground. The metrics shift because the measurement sharpened.**


**28 datasets** across three MTEB task categories:


Category Datasets


**Retrieval** ArguAna, BelebeleRetrieval, CovidRetrieval, FiQA2018, FinQARetrieval, HagridRetrieval, LEMBPasskeyRetrieval, MIRACLRetrievalHardNegatives, MLQARetrieval, NQ, QuoraRetrieval, SCIDOCS, SciFact, StackOverflowQA, StatcanDialogueDatasetRetrieval, TRECCOVID, TwitterHjerneRetrieval, WikipediaRetrievalMultilingual


**Reranking** AILAStatutes, AlloprofReranking, LegalBenchCorporateLobbying, RuBQReranking, T2Reranking, VoyageMMarcoReranking, WikipediaRerankingMultilingual


**Instruction Retrieval** Core17InstructionRetrieval, News21InstructionRetrieval, Robust04InstructionRetrieval


**16 embedding models:** zembed-1, voyage-4, voyage-4-lite, voyage-4-nano, cohere-embed-v4, cohere-embed-multilingual-v3, openai-v3-large, openai-v3-small, qwen3-embedding-4b, gemini-embedding-001, gemini-embedding-2-preview, jina-v5-text-small, bge-m3, harrier-270m, harrier-0.6b, harrier-27b


**7 rerankers:** zerank-2, zerank-2-nano, voyage-rerank-2.5-lite, cohere-rerank-v4-pro, cohere-rerank-v4-fast, cohere-rerank-v3.5, jina-reranker-v3


Every embedding model was evaluated standalone and in combination with every reranker, yielding **128 system configurations** per dataset per judge.


## Explore the Dashboard


The full evaluation dashboard is available at[zeroentropy.dev/evals](https://www.zeroentropy.dev/evals/) . All results are interactive: switch datasets, compare judges, toggle between absolute and delta views, sort by any metric, and inspect the score distributions underlying each dataset’s annotations.


### Get Started


Build with the models that lead under graded relevance:


[→ ZeroEntropy API fully managed, lowest-friction path to production](https://docs.zeroentropy.dev/models)[→ HuggingFace open weights, run it on your own infrastructure](https://huggingface.co/zeroentropy/)[→ AWS Marketplace deploy within your existing AWS environment](https://aws.amazon.com/marketplace/seller-profile?id=seller-nurj4uavxb4z2)


```text
from   zeroentropy   import   ZeroEntropy
zclient   =   ZeroEntropy()
response   =   zclient.models.embed(
model  =  "zembed-1"  ,
input_type  =  "query"  ,   # "query" or "document"
input  =  "What is retrieval augmented generation?"  ,   # string or list[str]
dimensions  =  2560  ,   # optional: must be one of [2560, 1280, 640, 320, 160, 80, 40]
encoding_format  =  "float"  ,   # "float" or "base64"
latency  =  "fast"  ,   # "fast" or "slow"; omit for auto
)
```


**Evaluation Dashboard** :[zeroentropy.dev/evals](https://www.zeroentropy.dev/evals/)


**Documentation** :[docs.zeroentropy.dev](https://docs.zeroentropy.dev/)


**HuggingFace** :[huggingface.co/zeroentropy](https://huggingface.co/zeroentropy)


**Get in touch** :[Discord community](https://discord.gg/5mkQCTnmY9) orcontact@zeroentropy.dev
