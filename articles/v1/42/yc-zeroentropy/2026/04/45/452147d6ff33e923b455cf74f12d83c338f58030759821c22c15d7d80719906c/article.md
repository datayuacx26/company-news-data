---
schema_version: "1.0.0"
document_id: "452147d6ff33e923b455cf74f12d83c338f58030759821c22c15d7d80719906c"
company_key: "yc-zeroentropy"
company: "ZeroEntropy"
source_id: "yc-zeroentropy-news-import-2ae9b0afcf30"
canonical_url: "https://zeroentropy.dev/articles/zembed-1-vs-harrier/"
published_at: "2026-04-08T00:00:00+00:00"
first_seen_at: "2026-07-22T21:03:20.034441+00:00"
fetched_at: "2026-07-28T22:15:58.980681+00:00"
content_hash: "sha256:9a42a312b274961ed67ed1e1177d58c75538ef2cb05f24b1bf2f66fc463e1ca7"
---

# harrier-27b: Can 27B Parameters Beat zembed-1?

TL;DR


- zembed-1 retains the **#1 overall[embedding model](https://www.zeroentropy.dev/concepts/embedding/)** position, outperforming harrier-27b on average[NDCG@10](https://www.zeroentropy.dev/concepts/ndcg-at-k/) (0.715 vs 0.706) and[Recall@100](https://www.zeroentropy.dev/concepts/recall-at-k/) (0.771 vs 0.749)
- On a per-dataset basis, zembed-1 wins **17 out of 28 datasets** against harrier-27b on NDCG@10
- voyage-4 and harrier-27b are neck-and-neck for the #2 spot — tied 14–14 on dataset wins, with voyage-4 ahead by a sliver on overall averages
- The Harrier family scales well internally (270M → 0.6B → 27B), but even the largest variant doesn’t close the gap to zembed-1
- **[Explore the full interactive dashboard →](https://www.zeroentropy.dev/evals/)**


# zembed-1 vs Harrier


## A New Challenger, Evaluated Properly


Harrier is a recently released family of open-weight embedding models from Microsoft ([finetuned](https://www.zeroentropy.dev/concepts/fine-tuning/) Gemma and Qwen models), spanning three sizes: 270M, 0.6B, and 27B parameters. The largest variant — harrier-27b — has generated well-deserved attention. On[binary](https://huggingface.co/spaces/mteb/leaderboard)[MTEB](https://www.zeroentropy.dev/concepts/mteb/) , it ranked first among embedding models at time of release.


But as we explored in[Beyond Binary](https://www.zeroentropy.dev/articles/mteb-evals) , MTEB has a discrimination problem: given its (overwhelmingly) binary annotations, it can’t tell the difference between a document which perfectly answers a query and one which may only tangentially address it. So we ran all three Harrier models through the same[graded evaluation pipeline](https://www.zeroentropy.dev/concepts/graded-relevance-judge/) we use for our[evals dashboard](https://www.zeroentropy.dev/evals/) — 28 diverse datasets, three independent[LLM judges](https://www.zeroentropy.dev/concepts/llm-as-judge/) , continuous relevance scores from 0 to 10.


It is not a question of harrier-27b is a good model. As a matter of fact, it is. (And at 27 billion parameters and a whopping 5,376 output vector dimensionality, we would certainly hope it would be). But is it *the best?*


## The Three Model Problem: MTEB Evals


Embed-only Recall@K, averaged across 28 datasets and 3 judges. Toggle between models to compare.


On the global average across all 28 evaluation datasets, there are three embedding models which markedly outperform the rest:


Model NDCG@10 Recall@10 Recall@100


**zembed-1** **0.715** 0.471 **0.771**


voyage-4 0.712 **0.473** 0.751


harrier-27b 0.706 0.468 0.749


Below those,` qwen3-4b` ,` jina-v5-text-small` ,` cohere-embed-v4` , and` openai-v3-large` (in that order) form a cluster of second tier performance. But if you need top-tier accuracy, your choice lies in that trio.


So what separates them? On NDCG@10, not much — roughly one point across the trio (though it’s worth noting that harrier still comes out worst). But NDCG@10 is not the whole story.


**On Recall@100 — the metric that determines whether a relevant document even makes it to your reranker — zembed-1 leads by +2.0 points over voyage-4, and +2.2 over harrier-27b.**


That is where the separation becomes real. A[reranker](https://www.zeroentropy.dev/concepts/reranker/) or other downstream system can reorder or rework whatever the embedding model surfaces, but it cannot conjure up a document the embedder failed to retrieve. zembed-1’s recall advantage compounds downstream: fewer relevant documents lost at the[first stage](https://www.zeroentropy.dev/concepts/first-pass-retrieval/) means a strictly better candidate set for everything that follows.


## Head-to-Head: zembed-1 vs harrier-27b


Averages, of course, can obscure as much as they reveal. So let us go dataset by dataset. Our[evals dashboard](https://www.zeroentropy.dev/evals/) covers 28 datasets drawn from three MTEB task categories — retrieval, reranking, and instruction retrieval — spanning legal (AILAStatutes, LegalBench), medical (CovidRetrieval, TRECCOVID), financial (FiQA, FinQA),[multilingual](https://www.zeroentropy.dev/concepts/cross-lingual-retrieval/) (MIRACL, MLQA, Belebele, WikipediaRetrieval), and technical domains (StackOverflowQA, SCIDOCS), among others.


**Across 28 evaluation datasets, zembed-1 outperforms harrier-27b on NDCG@10 on 17 of them.**


The pattern of *where* each model wins is telling. zembed-1 dominates on **[instruction retrieval](https://www.zeroentropy.dev/concepts/instruction-following-reranker/)** (Core17, News21, Robust04 — tasks which require parsing nuanced query intent, not merely matching keywords), **medical and legal domains** (CovidRetrieval, LegalBench, TRECCOVID), **finance** (FiQA2018, FinQARetrieval), and **technology** (StackOverflowQA). harrier-27b, for its part, shows strength on **multilingual reranking** and a handful of niche benchmarks (RuBQReranking, Russian paragraph reranking; and TwitterHjerne, Danish Twitter retrieval).


Dataset zembed-1 harrier-27b Delta


**FinQARetrieval** **0.717** 0.619 +9.8


**FiQA2018** **0.823** 0.748 +7.5


**Robust04InstructionRetrieval** **0.857** 0.788 +6.9


**LEMBPasskeyRetrieval** **0.891** 0.825 +6.6


**Core17InstructionRetrieval** **0.899** 0.837 +6.2


**TRECCOVID** **0.922** 0.871 +5.1


**StackOverflowQA** **0.695** 0.651 +4.4


**CovidRetrieval** **0.820** 0.796 +2.3


**NQ** **0.767** 0.746 +2.0


**AlloprofReranking** **0.851** 0.832 +1.9


**LegalBenchCorporateLobbying** **0.875** 0.860 +1.5


**MLQARetrieval** **0.042** 0.029 +1.2


**MIRACLRetrievalHardNegatives** **0.568** 0.556 +1.2


**T2Reranking** **0.804** 0.794 +1.0


**News21InstructionRetrieval** **0.919** 0.910 +0.8


**BelebeleRetrieval** **0.179** 0.172 +0.7


**WikipediaRetrievalMultilingual** **0.778** 0.774 +0.5


HagridRetrieval 0.897 **0.899** -0.2


SciFact 0.789 **0.792** -0.3


ArguAna 0.564 **0.566** -0.3


QuoraRetrieval 0.685 **0.689** -0.4


VoyageMMarcoReranking 0.732 **0.739** -0.7


StatcanDialogueDatasetRetrieval 0.723 **0.742** -1.9


WikipediaRerankingMultilingual 0.567 **0.605** -3.8


AILAStatutes 0.700 **0.740** -4.0


RuBQReranking 0.736 **0.801** -6.5


TwitterHjerneRetrieval 0.694 **0.775** -8.1


SCIDOCS 0.540 **0.623** -8.3


## The Race for Second Place


As we established in our[previous head-to-head](https://www.zeroentropy.dev/articles/zembed-1-overperforms-voyage-4) , voyage-4 has been the reigning #2 embedding model. With harrier-27b now in the picture, that position is genuinely contested:


voyage-4 harrier-27b


Average NDCG@10 **0.712** 0.706


Average Recall@100 **0.751** 0.749


Dataset wins (head-to-head) 14 14


It is… remarkably close. The two models split dataset wins exactly (14–14), and depending on *your* vertical, either could be the better runner-up. If forced to pick, voyage-4 still holds thin but consistent edges on the overall averages — enough to keep the #2 title, but barely. (Neither, however, threatens first place.)


## Harrier’s Scaling Story


One genuinely interesting aspect of the Harrier family is its range of sizes. The scaling is clean — and instructive:


Model Params Avg NDCG@10 Avg Recall@100


harrier-270m 270M 0.625 0.674


harrier-0.6b 600M 0.658 0.704


harrier-27b 27B 0.706 0.749


A +3.3 point NDCG jump from 270M to 0.6B, then +4.9 points from 0.6B to 27B. Returns to scale are not completely diminishing — the largest absolute improvement comes from the largest model. Credit where credit is due: this is a decently-executed scaling curve, particularly for the 0.6b model which demonstrates equal or better performance to Cohere’s flagship` embed-v4` .


What Does Each Size Buy You?


- harrier-270m (0.625) outperforms bge-m3 (0.593) and openai-v3-small (0.601) — entirely respectable for a 270M-parameter model
- harrier-0.6b (0.658) is competitive with cohere-embed-v4 (0.667)
- harrier-27b (0.706) enters the top three — but requires 27 billion parameters and 5,376-dimensional output vectors to get there, compared to zembed-1’s 4 billion parameters and 2,560 dimensions


The contrast in size between harrier-27b and all other models bears emphasis: **27 billion parameters is absolutely massive for an embedding model, and that’s not a compliment.**


zembed-1 achieves its #1 ranking with **4 billion parameters** and a 2,560-dimensional output. harrier-27b needs nearly **7x the parameter count** and **2x the[vector](https://www.zeroentropy.dev/concepts/vector/) dimensionality** to land 0.9 points behind on NDCG@10. In a production setting — where embedding compute, storage costs, and index size are real constraints — the efficiency gap is hardly academic. Would *you* pay for a model with 7x higher inference cost, probably 7x as much latency, which outputs an embedding that’s twice as costly to store, just to get worse results?


We wouldn’t.


## What This Means


harrier-27b is a legitimate top-three embedding model — quite possibly the strongest new entrant we have seen since voyage-4. It is genuinely competitive, especially on multilingual reranking tasks, and we expect Microsoft will continue to iterate on the family.


But the leaderboard has not changed:


**zembed-1 leads on average NDCG@10, wins 17 of 28 datasets head-to-head against harrier-27b, and holds the highest Recall@100 of any embedding model — at 1/7th the parameter count and half the vector dimensionality.**


For the full interactive breakdown across all models, datasets, metrics, and reranker combinations, explore the[evaluation dashboard](https://www.zeroentropy.dev/evals/) .


### Get Started


zembed-1 is available today through multiple deployment options:


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


**Documentation** :[docs.zeroentropy.dev](https://docs.zeroentropy.dev/)


**HuggingFace** :[huggingface.co/zeroentropy](https://huggingface.co/zeroentropy)


**Get in touch** :[Discord community](https://discord.gg/5mkQCTnmY9) orcontact@zeroentropy.dev


Talk to us if you need a custom deployment, volume pricing, or want to see how zembed-1 + zerank-2 performs on your data.
