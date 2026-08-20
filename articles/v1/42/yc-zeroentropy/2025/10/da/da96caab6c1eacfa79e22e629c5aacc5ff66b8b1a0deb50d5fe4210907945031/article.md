---
schema_version: "1.0.0"
document_id: "da96caab6c1eacfa79e22e629c5aacc5ff66b8b1a0deb50d5fe4210907945031"
company_key: "yc-zeroentropy"
company: "ZeroEntropy"
source_id: "yc-zeroentropy-news-import-2ae9b0afcf30"
canonical_url: "https://zeroentropy.dev/articles/implementing-zeroentropy-reranking-with-turbopuffer-retrieval/"
published_at: "2025-10-05T00:00:00+00:00"
first_seen_at: "2026-07-22T21:03:20.034441+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:1bdf4f2b4cd2742d86835d6c074924cd32475f03e56a969b2c18a4180cd06acc"
---

# Implementing ZeroEntropy Reranking with turbopuffer Retrieval

TL;DR


A two-step search flow delivers both speed and accuracy: **turbopuffer** handles fast hybrid retrieval ([BM25](https://www.zeroentropy.dev/concepts/bm25/) + vector[ANN](https://www.zeroentropy.dev/concepts/ann-nearest-neighbor/) with[Reciprocal Rank Fusion](https://www.zeroentropy.dev/concepts/reciprocal-rank-fusion/) ), and **ZeroEntropy** reranks the results with a[cross-encoder](https://www.zeroentropy.dev/concepts/cross-encoder/) for high-quality, context-aware ordering. turbopuffer finds the hay; ZeroEntropy finds the needle.


## Hybrid Search as first-stage retrieval


If you’re building AI systems like[RAG](https://www.zeroentropy.dev/concepts/rag/) or AI[Agents](https://www.zeroentropy.dev/concepts/agent/) , you’re probably familiar with[semantic search](https://www.zeroentropy.dev/concepts/semantic-search/) and keyword search concepts:


- **Keyword Search (BM25):** lightning-fast inverted-index lookups, perfect for exact matches when you know what you’re looking for (“try except syntax Python”), but recall drops when phrasing shifts (“how to catch errors in Python”).
- **Semantic Similarity:** Nearest-neighbor vector search on precomputed vector[embeddings](https://www.zeroentropy.dev/concepts/embedding/) . Much better at conceptual queries, as vectors are based on the semantic meaning of the content, rather than any particular keywords.


With turbopuffer, you can combine both retrieval methods in a[hybrid setup](https://www.zeroentropy.dev/concepts/hybrid-search/) and use Reciprocal Rank Fusion to boost recall.


But recall alone isn’t enough. You might retrieve the correct answer out of millions of documents, but if it sits at position 67, your[LLM](https://www.zeroentropy.dev/concepts/large-language-model/) , or your users, will never see it.


## Why add a reranker?


A[reranker](https://www.zeroentropy.dev/concepts/reranker/) is a cross-encoder neural network that refines search results by reading both the query and each document together to evaluate how relevant they truly are.


By adding a reranker after fast retrieval (e.g., keyword, vector, or hybrid search), you let a **context-aware model** re-score and reorder the candidate results—surfacing the most relevant documents, reducing noise, and helping downstream models or users avoid missing the **“needle in the haystack.”**


## Putting it all together


We can put all these components together to create a two-step search flow that delivers both speed and accuracy: turbopuffer handles fast retrieval, and ZeroEntropy refines the results to produce high-quality, context-aware answers.


In other words, turbopuffer finds the hay; ZeroEntropy finds the needle.


#### Retriever (turbopuffer)


The first-stage engine that stores documents and fetches a candidate set quickly:


- **Keyword search (BM25):** Searches for exact words and phrases, which is great for precise terms and filters.
- **Semantic (Vector ANN):** Searches by meaning using embeddings, which is helpful for synonyms and paraphrases. This is the heart of[dense retrieval](https://www.zeroentropy.dev/concepts/dense-retrieval/) .
- **Hybrid (Reciprocal Rank Fusion)** : This merges the BM25 and ANN ranked lists into a single, stronger candidate list.


#### Reranker (ZeroEntropy)


A cross-encoder that reads the query and each candidate together and assigns a relevance score, returning a better-ordered list.


## Tutorial


Let’s jump into the implementation details!


[See the full colab link here.](https://colab.research.google.com/drive/1KW4vy3h2Gy0CYAsNy2t3cDA2N6udsU8c?usp=sharing)


### 0. Environment setup


Install the core libraries needed for this tutorial:


- **turbopuffer** — for fast document retrieval
- **zeroentropy** — for reranking and relevance scoring
- **sentence-transformers** — for generating text embeddings
- **python-dotenv** — for securely loading API keys and environment variables


```text
!  pip   -q   install   turbopuffer   zeroentropy   sentence-transformers   python-dotenv
```


### 1. Configure API Keys


If you don’t have keys yet:


- [turbopuffer](https://turbopuffer.com/) : create an API key in your dashboard and set` TURBOPUFFER_API_KEY` .
- [ZeroEntropy](https://www.zeroentropy.dev/) : create an API key and set` ZEROENTROPY_API_KEY` .


You can paste them into the cell below while testing in Colab; for production, use environment variables.


```text
import   os
from   dotenv   import   load_dotenv
load_dotenv()


# ↓ Optionally set directly for quick testing in Colab (replace the placeholders)
# os.environ['TURBOPUFFER_API_KEY'] = 'tpuf_...'
# os.environ['ZEROENTROPY_API_KEY'] = 'ze_...'


assert   os.getenv(  'TURBOPUFFER_API_KEY'  )   is   not   None  ,   'Please set TURBOPUFFER_API_KEY'
assert   os.getenv(  'ZEROENTROPY_API_KEY'  )   is   not   None  ,   'Please set ZEROENTROPY_API_KEY'
print  (  'Keys detected ✅'  )
```


### 2. Create a turbopuffer namespace and write documents


We generate embeddings with` sentence-transformers/all-MiniLM-L6-v2` (384 dims), then upsert` text` (BM25-enabled) +` vector` into turbopuffer.


```text
from   turbopuffer   import   Turbopuffer
from   sentence_transformers   import   SentenceTransformer
import   uuid, time


# Init SDK
tpuf   =   Turbopuffer(  api_key  =  os.environ[  'TURBOPUFFER_API_KEY'  ],   region  =  "gcp-us-central1"  ,)
ns_name   =   f  'ze_tpuf_demo_  {int  (time.time())  }  '
ns   =   tpuf.namespace(ns_name)


# Tiny corpus
docs   =   [
{
"id"  :   str  (uuid.uuid4()),
"title"  :   "Henry Cavendish"  ,
"text"  :   "In 1798, Cavendish used a torsion balance to measure the Earth's density and effectively determine the gravitational constant G. His experiment measured the tiny gravitational attraction between lead spheres."
},
{
"id"  :   str  (uuid.uuid4()),
"title"  :   "Isaac Newton"  ,
"text"  :   "Newton formulated the law of universal gravitation, F = G m1 m2 / r^2, explaining that gravitational force obeys an inverse-square dependence on distance."
},
{
"id"  :   str  (uuid.uuid4()),
"title"  :   "Charles-Augustin de Coulomb"  ,
"text"  :   "Coulomb used a torsion balance to study electrostatic forces, establishing Coulomb's law for charges, which also follows an inverse-square relationship."
},
{
"id"  :   str  (uuid.uuid4()),
"title"  :   "Gravitational Potential Energy"  ,
"text"  :   "The potential energy between two masses is U = -G m1 m2 / r; near Earth's surface U ≈ m g h."
}
]


# Build vectors (local, no external API)
model   =   SentenceTransformer(  'sentence-transformers/all-MiniLM-L6-v2'  )
```


### 3. Hybrid search (BM25 + ANN) and Reciprocal Rank Fusion (RRF)


We run **two queries** against turbopuffer and fuse the rankings client-side using RRF.


```text
from   typing   import   List, Dict


query   =   "Who first measured the gravitational constant — not the person who formulated the law of universal gravitation?"


top_k   =   5


# ANN vector search
vec   =   model.encode([query],   normalize_embeddings  =  True  )[  0  ].tolist()
ann   =   ns.query(
rank_by  =  (  "vector"  ,   "ANN"  , vec),
top_k  =  top_k,
include_attributes  =  [  "title"  ,  "text"  ]
).rows


# BM25 search
bm25   =   ns.query(
rank_by  =  (  "text"  ,   "BM25"  , query),
top_k  =  top_k,
include_attributes  =  [  "title"  ,  "text"  ]
).rows


def   rrf_with_scores  (list_of_rankings: List[List[Dict]], k:   int   =   60  ):
"""Return (fused_list, rrf_scores_dict)."""
scores   =   {}
for   ranking   in   list_of_rankings:
for   rank, item   in   enumerate  (ranking,   start  =  1  ):
scores[item.id]   =   scores.get(item.id,   0.0  )   +   1.0   /   (k   +   rank)
id2item   =   {item.id: item   for   ranking   in   list_of_rankings   for   item   in   ranking}
ordered_ids   =   sorted  (scores,   key  =  scores.get,   reverse  =  True  )
fused   =   [id2item[i]   for   i   in   ordered_ids]
return   fused, scores


# Fuse and print only RRF results
fused_results, rrf_scores   =   rrf_with_scores([ann, bm25])
fused_results   =   fused_results[:top_k]
```


### 4. Rerank with ZeroEntropy


Then we pass the fused candidate list to ZeroEntropy for high-accuracy reranking.


```text
import   os
from   zeroentropy   import   ZeroEntropy


def   zeroentropy_rerank_or_unranked  (results, query, model  =  "zerank-1-small"  , k  =  None  , return_scores  =  False  ):
"""Return results reordered by ZeroEntropy; optionally include scores."""
if   not   os.getenv(  "ZEROENTROPY_API_KEY"  ):
print  (  "Warning: ZEROENTROPY_API_KEY not set, returning unranked results"  )
return   results


ze   =   ZeroEntropy(  api_key  =  os.environ[  "ZEROENTROPY_API_KEY"  ])
documents   =   [  getattr  (r,   "text"  ,   str  (r))   for   r   in   results]    # list[str]
out   =   ze.models.rerank(
query  =  query,
documents  =  documents,
model  =  model,                   # "zerank-1" or "zerank-1-small"
top_n  =  k   or   len  (documents),
)


# out.results is sorted by relevance (desc)
if   return_scores:
return   [(results[r.index], r.relevance_score)   for   r   in   out.results]
else  :
return   [results[r.index]   for   r   in   out.results]


# Use it
reranked_with_scores   =   zeroentropy_rerank_or_unranked(
fused_results, query,   model  =  "zerank-1-small"  ,   return_scores  =  True
)


for   rank, (doc, score)   in   enumerate  (reranked_with_scores,   1  ):
print  (  f  "Rank   {  rank  }  .   {  doc.title  }   —   {  score  :.4f  }  "  )
```


## Results


**Hybrid Search *without* ZeroEntropy reranker:**


Rewards overlap on “gravity”/“inverse square,” *mistakenly ranking Newton #1.*


` 1) Newton 0.0328, 2) Cavendish 0.0323, 3) Coulomb 0.0317, 4) GPE 0.0156`


**Hybrid Search *with* ZeroEntropy reranker:**


Reads query + candidate together, understands negation and measurement, *correctly ranking Cavendish #1.*


` 1) Cavendish 0.9531, 2) Newton 0.4428, 3) GPE 0.3901, 4) Coulomb 0.2304`


### Why ZeroEntropy wins


RRF blends keyword and semantic similarity, so anything about “gravity” and “inverse square” tends to float to the top—even Newton, who is explicitly excluded by the question.


ZeroEntropy evaluates the full meaning of the query with each candidate. It recognizes that the user wants the person who measured G (Cavendish) and not the person who formulated the law (Newton), so it reorders the list correctly.


## Takeaway
