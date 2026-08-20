---
schema_version: "1.0.0"
document_id: "1ecdd7dc9a37855ebf4160775569d1e4a4d33e2d1fff401508b13f805ff235e7"
company_key: "roku-inc-class-a-common-stock"
company: "Roku Inc."
source_id: "roku-inc-class-a-common-stock-rss-7fd84d003d99"
canonical_url: "https://engineering.roku.com/improving-search-relevance-using-crossencoders-and-llms"
published_at: "2025-03-11T14:55:31+00:00"
first_seen_at: "2026-07-20T23:21:49.245762+00:00"
fetched_at: "2026-08-20T03:06:22.032514+00:00"
content_hash: "sha256:83b2c9608854bb0f9d85c7078abf802ecc9f414cdaba05615beaee395b81939c"
---

# Improving Search Relevance Using CrossEncoders and LLMs

Authors: Kapil Kumar


Karim Lulu


Rahul Agarwal


Search relevance is a critical component of any content discovery system. While traditional semantic retrieval using bi-encoders offers efficient search capabilities, it often suffers from false positives due to its inability to capture complex query-document relationships. Alternatively, Large Language Models (LLMs) can provide superior relevance understanding, but their computational costs and high latency make them impractical for production search systems.


Left (Before) / Right (After) integrating cross encoders for search relevance for query legal dramas


Consider this real example from our system: A search for “legal dramas” using bi-encoders returned comedies like “22 Jump Street” and “21 Jump Street” – films about undercover cops in school, not legal dramas at all. The results also include unrelated titles matching only partial keywords like “guardian” or “citizen”, showing that the system failed to understand the actual query intent. The intuition behind this is that when we train[bi-encoders](https://www.sbert.net/examples/applications/cross-encoder/README.html) , because the query and item towers are separate for latency requirements, bi-encoders may not pay enough attention to all the words in the query. Hence some of the results here are “dramas” but not “legal dramas”.


In contrast, the right side shows results from our improved system which correctly include legal dramas like “The Judge” and “Conviction”, correctly matching the user’s search intent.


This blog post explores how we achieved such results by fine-tuning a cross-encoder model, combining the best of both worlds: we demonstrate how our approach maintains the efficiency of semantic retrieval while significantly reducing false positives through cross-attention mechanisms. Through model distillation techniques, we achieved remarkable improvements in search quality, Our fine-tuned model resulted in a 4.5% reduction in search abandonment rates and a 2.4% increase in visit-to-stream rates, all while keeping computational costs manageable for production deployment. Our results show that cross-encoders can effectively bridge the gap between lightweight but imprecise bi-encoders and powerful LLMs.


## Using LLMs as a Judge


A proposed improvement to our baseline bi-encoder system would be to use LLMs as a judge. Here, a LLM will evaluate each query-item pair to determine relevance. If deemed relevant, the items are passed to a ranker for final ordering before being presented to users. This approach combines the efficiency of traditional retrieval with the deep semantic understanding of LLMs.


However, this architecture faces several significant challenges:


- High Latency: LLMs are computationally intensive, requiring several seconds to evaluate each query-document pair. For a typical search with 1000 candidates, this could mean minutes of processing time, making it impractical for real-time search applications.
- Computational Costs: Running LLM inference at scale is extremely expensive. Each search query requires multiple LLM calls (one for each candidate document), leading to high GPU/CPU utilization and significant cloud computing costs.
- Resource Scalability: As the number of search queries and document corpus grows, the computational requirements grow linearly with each query-document pair evaluation, making it challenging to scale efficiently.


Due to these limitations, particularly the high latency and costs, we need to look at more efficient alternatives like cross-encoders. Cross-encoders can provide similar semantic understanding capabilities while being orders of magnitude faster and more cost-effective than LLMs, making them more suitable for production search systems.


## The Solution: Cross-Encoders as a Judge


Cross-encoders have emerged as an excellent alternative for judging search relevance. Cross-encoders are neural networks that process paired inputs through transformer layers to produce relevance scores. Deep cross-encoders use multiple transformer layers, enabling better capture of complex relationships but requiring more compute, while shallow cross-encoders use fewer layers, offering faster inference with potentially reduced effectiveness. This creates a fundamental trade-off between accuracy and computational efficiency. Given our latency constraints, we implemented shallow cross-encoders, which delivered similar performance metrics while maintaining rapid response times.


The diagram below shows how we implemented it:


###### **Initial Retrieval Layer**


- Bi-encoder retrieves top-K candidates (K=1000)
- Optimized for recall rather than precision
- Sub-100ms latency requirement


**CrossEncoder Judgment Layer**


` scores = cross_encoder.predict(\[


(query, doc) for doc in candidates


\])


`


#### Benefits of Cross-Encoders Over LLM Judge (8B LLM vs Shallow Cross Encoder)


Below we list the benefits of cross-encoders over LLMs at the judgement step.


##### **Latency Improvements**


- LLM: 2-3 seconds per query
- CrossEncoder: 100-200ms for all candidates
- 15x speedup in judgment phase
- Caching the query and document pair score gives us an even better latency.


##### **Cost Efficiency**


- 95% reduction in compute costs
- Able to handle 10x more traffic with same infrastructure
- Better resource utilization


##### **Consistency**


- More stable predictions
- No hallucinations unlike LLMs
- Easier to debug and maintain


## Evaluation


In this section, we evaluate different cross-encoder architectures (both deep and shallow). We then propose a way to improve the best cross-encoder using LLMs and show offline and live results.


**Dataset:** We used majority voting Ensemble (ref Agents is all you need) over 50K sample from real user item interaction data to generate a dataset of whether a query/item pair is relevant or not


We experimented with various open source pre-trained cross-encoders on our evaluation dataset. We used two metrics to measure this performance MRR@K and NDCG@K.


##### **MRR@K (Mean Reciprocal Rank at K)** : Measures the average position of the first relevant item in the top K results.


Formula: MRR = 1/|Q| * Σ(1/rank_i) where:


- |Q| is the number of queries
- rank_i is the position of the first relevant result for query i
- Only considers positions up to K


##### **NDCG@K (Normalized Discounted Cumulative Gain at K)** : Measures ranking quality considering both relevance and position up to K results


For example, given ranking results where 1 = relevant, 0 = irrelevant:


\[1, 0, 1, 0, 1\] (actual ranking)


\[1, 1, 1, 0, 0\] (ideal ranking)


###### Calculate DCG:


- DCG = Σ(rel_i / log2(i + 1))
- DCG = 1/log2(2) + 0/log2(3) + 1/log2(4) + 0/log2(5) + 1/log2(6)
- DCG = 1 + 0 + 0.5 + 0 + 0.386 = 1.886


###### Calculate IDCG (using ideal ranking):


- IDCG = 1/log2(2) + 1/log2(3) + 1/log2(4) + 0/log2(5) + 0/log2(6)
- IDCG = 1 + 0.631 + 0.5 = 2.131


Final NDCG = DCG/IDCG = 1.886/2.131 = 0.885


` * (Only labelled for 1000 samples as it was too slow to run)`


Baseline refers to performance of the production model on real user item interaction dataset before introduction of the cross encoders. We can see that pre-trained cross encoders did well in pushing back false positives vs. the production model.


The Tiny BERT cross-encoder was the fastest, while ms-marco-MiniLM-L-6-v2 cross-encoder delivered the best performance. However, jina-reranker-v1-turbo-en was the slowest and showed no improvement over ms-marco-MiniLM-L-6-v2 cross encoder


## The Secret Sauce: Knowledge Distillation


In an effort to leverage the knowledge embedded in large language models (LLMs), we employed a technique known as model distillation ([reference](https://arxiv.org/pdf/2405.07920) ). Model distillation involves transferring knowledge from a complex model, often referred to as the teacher, to a simpler model, known as the student. This process allows the student model to learn from the teacher’s outputs, capturing not just the correct classifications but also the nuances and uncertainties that the teacher model has learned during its extensive training.


We applied this technique by using an LLM as the teacher i.e labels from the LLM as a judge to fine-tune a TinyBERT cross-encoder, which we selected due to its speed and good performance.The distillation process demonstrated significant gains in classification over our dataset.


Distribution of Scores before (left) and After(right) fine-tuning the TinyBert Cross encoder


Here, label 0 refers to cases where item/query pair is not relevant and 1 refers to cases where query and item pair are relevant, We can observe that before fine-tuning there was significant overlap between relevant and irrelevant score distributions, but after fine-tuning clear separation between classes with well-calibrated confidence scores. Furthermore, we also observe an increase of 10% in precision and 24% increase in recall on our offline evaluation dataset.


ROC-AUC curve for both original and fine-tuned model


When we tested this model in production, we saw a **4.5% reduction in search abandonment rates** and a **+2.4% lift in visit-to-stream rates** for search sessions.


## Key Takeaways


Implementing cross-encoders with LLM distillation significantly improved our search relevance while keeping the system production-ready. Some key takeaways:


- Cross-encoders offer a practical alternative to using LLMs as a judge.
- Knowledge distillation helps transfer LLM capabilities effectively.
- Proper monitoring and optimization are crucial for production success.


## References


- [Bi-Encoders and Cross-Encoders](https://www.sbert.net/examples/applications/cross-encoder/README.html)
- [TinyBERT and Knowledge Distillation](https://arxiv.org/abs/1909.10351)
- [LLM-as-a-Judge Paradigm](https://blog.vespa.ai/improving-retrieval-with-llm-as-a-judge/)
- [More Agents is all you need](https://arxiv.org/pdf/2402.05120)
- [The Illustrated Guide to Cross-Encoders: From Deep to Shallow](https://medium.com/@kakumar1611/the-illustrated-guide-to-cross-encoders-from-deep-to-shallow-2a23a8630016)


The post[Improving Search Relevance Using CrossEncoders and LLMs](https://engineering.roku.com/improving-search-relevance-using-crossencoders-and-llms) appeared first on[Engineering Blog](https://engineering.roku.com/) .
