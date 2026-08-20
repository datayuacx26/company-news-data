---
schema_version: "1.0.0"
document_id: "66f13674004098c19ce5fbec4c29b37a3c76fd7d9d76df320a674bc39434f8a7"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-news-import-ebf5ac4e0909"
canonical_url: "https://www.elastic.co/search-labs/blog/bbq-quantization-jina-embeddings-v5"
published_at: "2026-07-10T02:50:43+00:00"
first_seen_at: "2026-07-22T05:42:30.133836+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:9d301a6d8a6cd2a46f67fb80f25a7fb10a35a4fd439862707afca84792aaca33"
---

# How BBQ shrinks Jina v5 embeddings by 29x without losing recall in Elasticsearch

Try out vector search for yourself using this[self-paced hands-on learning](https://www.elastic.co/demo-gallery/vector-search) for Search AI. You can start a[free cloud trial](https://cloud.elastic.co/registration?onboarding_token=search&cta=cloudregistration&tech=trial&plcmt=cross%20module&pg=search-labs) or try Elastic on your[local machine](https://github.com/elastic/start-local?cta=local-machine&tech=github&plcmt=cross%20module&pg=search-labs) now.


[BBQ quantization](https://www.elastic.co/search-labs/blog/better-binary-quantization-lucene-elasticsearch) cuts the memory footprint of[Jina embeddings v5](https://www.elastic.co/search-labs/blog/jina-embeddings-v5-text) vectors by 29x in[Elasticsearch](https://elastic.co/elasticsearch) . Recall@10 holds at 0.994 against a full-precision` float32` baseline. We tested this on a multilingual newscorpus across five languages, using` jina-embeddings-v5-text-small` to build a raw` float32` index and a` bbq_hnsw` index from the exact same[vectors](https://www.elastic.co/what-is/vector-embedding) . Then we measured memory, disk usage andretrieval quality on both. Disk usage came out nearly identical between the two indices. In-memory footprint is the number that actually decides whether your cluster fits the corpus, and it dropped from 12.71 MB to 0.44 MB for this test set. Jina v5's[quantization](https://www.elastic.co/search-labs/blog/better-binary-quantization-lucene-elasticsearch) -aware training is why the recall held.


## Prerequisites


- Elasticsearch 9.x with` jina-embeddings-v5-text-small`[inference](https://www.elastic.co/docs/explore-analyze/elastic-inference)endpoint available.
- Python 3.10+,
- Elasticsearch API key,


## What is quantization?


An *embedding* is a list of numbers. By default, each number is a` float32` , which uses 4 bytes. *Quantization* stores each number with fewer bits, trading precision for space.


Like a JPEG, a *quantizedvector* is a smaller, lower-fidelity copy of the original that still gets the job done.


*Gradual decrease of JPEG quality (image by Michael Gäbler,[CC BY 3.0](https://creativecommons.org/licenses/by/3.0/) via Wikimedia Commons).*


Name Bytes / dim 1024-d vector Compression


\`Float\` (Baseline) 4 4096 B 1x


\`int8\` 1 1024 B 4x


\`int4\` 0.5 512 B 8x


\`bbq\` ~0.14 142 B ~29x


## What is BBQ?


BetterBinary Quantization (BBQ) is Elasticsearch's 1-bit quantization mode for dense vectors. Eachdimension of the vector is stored as a single bit, plus a few corrective bytes per vector. Then, a rescoring step is applied atquery time. This keeps the final retrieval quality close to a full precision search.


For the math behind each level, see[Scalar quantization 101](https://www.elastic.co/search-labs/blog/scalar-quantization-101) ,[Optimized Scalar Quantization](https://www.elastic.co/search-labs/blog/optimized-scalar-quantization-elasticsearch) , and the[BBQ deep dive](https://www.elastic.co/search-labs/blog/better-binary-quantization-lucene-elasticsearch) .


### How does BBQ preserve search accuracy?


Plain 1-bit quantization leads to too high a search quality degradation on its own. BBQ maintains high retrieval quality through three mechanisms:


1. **Asymmetric precision:** Stored vectors use 1 bit per dimension.
2. **Corrective factors:** A few floats per vector record the rounding error and correct distances at scoring time.
3. **Oversample and rescore:** BBQ scans candidates with the bits and then reranks the top ones with higher precision. Fetching the top 10 means scanning about 30 candidates.


The result is the vectors that are roughly 32x smaller, with retrieval quality close to full precision. In the next section of the article, we’ll measure the memory savings and the recall on a real corpus.


## How Jina embeddings v5 works


Jina[embeddings](https://www.elastic.co/what-is/vector-embedding) v5 is amultilingual embedding model with quantization-aware training, which makes it a natural fit for BBQ in Elasticsearch: The 1024-dimensional vectors from` jina-embeddings-v5-text-small` sit above the dimensional floor where binary quantization stays accurate, and the model is trained so that 1-bit quantization loses little quality. Its main features are:


- **One model for many tasks:** v5 uses small[Low-Rank Adaptation (LoRA) adapters](https://arxiv.org/abs/2106.09685) on top of a single base model, one for each task: *retrieval* , *text-matching* ,clustering , and *classification* . Elasticsearch[picks the right adapter automatically](https://www.elastic.co/search-labs/blog/jina-embeddings-v5-text#getting-started) at index and query time.
- [Matryoshka dimensions:](https://arxiv.org/abs/2205.13147) v5 is trained so you can truncate the vector (1024, 512 to 256) and minimize search quality reduction. This is another way to shrink vectors, independent of quantization.
- **Quantization-aware training:** v5 is trained to work with BBQ, so its 1-bit vectors lose little accuracy.


We use` jina-embeddings-v5-text-small` . This model is available through[Elastic Inference Service](https://www.elastic.co/docs/explore-analyze/elastic-inference/eis) ([EIS](https://www.elastic.co/docs/explore-analyze/elastic-inference/eis) ) and outputs 1024 dimensions with 32ktoken context and is multilingual across 93 languages. That puts it above the[384-dimension threshold](https://www.elastic.co/docs/reference/elasticsearch/mapping-reference/dense-vector#dense-vector-quantization) , below which Elasticsearch no longer defaults to` bbq_hnsw` .


Full model details are in the[Jina v5 article on Search Labs](https://www.elastic.co/search-labs/blog/jina-embeddings-v5-text) .


## Setting up the BBQ vs. float32 comparison


We’ll create two indices: Both share mappings, and what changes is the` index_options.type` parameter, which tells Elasticsearch how to store thedense vector field (as raw` float32` HNSW or as 1-bit BBQ):


Index \`index_options\` Loaded into memory


\`vectors-float32\` \`hnsw\` Raw \`float32\` with no quantization (baseline)


\`vectors-bbq\` \`bbq_hnsw\` 1-bit BBQ quantization + corrective factors


We then embed the corpus once with Jina v5, index those same vectors into both, and compare them on disk usage, memory footprint, and recall. You can follow along with the full[supporting blog content notebook](https://github.com/elastic/elasticsearch-labs/blob/main/supporting-blog-content/quantizing-jina-embeddings-v5-bbq/quantization-jina-embeddings.ipynb) .


### Connect to Elasticsearch


### Create the two indices


*Note: In production, you can use[semantic_text](https://www.elastic.co/docs/reference/elasticsearch/mapping-reference/semantic-text) to let Elasticsearch manage the mapping and inference endpoint automatically.*


### Point at the Jina v5 inference endpoint


We call the model` jina-embeddings-v5-text-small` directly (no need to create an[inference endpoint](https://www.elastic.co/docs/api/doc/elasticsearch/operation/operation-inference-put) ) to turn text into vectors.


As result of the test, we got:


### Load a multilingual news dataset


We stream real news articles from[hotchpotch/multilingual_cc_news](https://huggingface.co/datasets/hotchpotch/multilingual_cc_news) , a parquet mirror of CC-News. We take about 1,000 articles from five languages (around 3,000 docs total), plus a small held-out set of headlines to use as search queries. Using multiple languages also lets Jina v5 show its multilingual strength.


### Generate the embeddings and bulk index


We embed the corpus a single time and feed those exact vectors into both indices.


We[force-merge](https://www.elastic.co/docs/api/doc/elasticsearch/operation/operation-indices-forcemerge) to a single segment so the storage numbers are stable and comparable.


## Results: Disk versus memory


The[disk usage API](https://www.elastic.co/docs/api/doc/elasticsearch/operation/operation-indices-disk-usage) reports how many bytes each index spends on vectors (` knn_vectors` ).


Result:


On disk, the two indices are about the same size. A quantized index still keeps the raw` float32` vectors (needed for rescoring and requantization during merges) and adds the 1-bit vectors on top, so BBQ ends up slightly larger on disk.


The real savings is in memory. The HNSW scan only needs the 1-bit vectors in RAM, while the raw floats are read from disk to rescore the top candidates. We size that footprint using the documented[kNN memory formulas](https://www.elastic.co/docs/deploy-manage/production-guidance/optimize-performance/approximate-knn-search) :` float` uses` num_vectors × dims × 4` and` bbq` uses` num_vectors × (dims/8 + 14)` .


BBQ's extra bytes on disk should match the 1-bit payload we computed for memory. Here, that’s` 13.25 - 12.80 = 0.45 MB` versus the computed` 0.44 MB` . They line up.


## Results: Recall


To check whether the quantized index returns results similar to the float baseline, we use recall:


` recall@k = | BBQ top-k ∩ float32 top-k | / k` , averaged over all queries.


We vary the oversampling factor (` num_candidates / k` ) that’s the number of candidates BBQ scans with 1-bit vectors before[reranking](https://www.elastic.co/search-labs/blog/elastic-semantic-reranker-part-1) the top ones against the original floats to find the lowest value that still matches` float32` .


As a result, we have:


BBQ starts at 0.994 recall@10 at 1x oversampling, holds there up to 3x, and then settles at 0.989 at higher factors, meaning it returns at least 98.9% of the same top-10 documents as float32 across all oversampling values. For more on how recall varies across datasets under quantization, see[Fast vs. accurate: Measuring the recall of quantized vector search](https://www.elastic.co/search-labs/blog/recall-vector-search-quantization) .


## BBQ quantization results summary


The same vectors, two storage formats, and one experiment:


- **Disk:** Roughly the same (` 12.80 MB` versus` 13.25 MB` ). BBQ keeps the raw floats around for rescoring and merging.
- **Memory:** 29x smaller (` 12.71 MB` versus` 0.44 MB` ). This is the number that decides whether your cluster fits the corpus.
- **Recall@10:**` 0.994` at 1x oversampling. Quantization-aware training pays off.


When to enable BBQ: If your dimension count is above the[384-dim floor](https://www.elastic.co/docs/reference/elasticsearch/mapping-reference/dense-vector#dense-vector-quantization) , if your vectors are the dominant memory cost, and if you can afford a few extra candidates to rescore. For Jina v5 specifically, the model is trained for it, so the recall hit on most corpora is small.


## Further reading on BBQ and vector quantization


- Run the full notebook from this article in the[supporting blog content repo](https://github.com/elastic/elasticsearch-labs/blob/main/supporting-blog-content/quantizing-jina-embeddings-v5-bbq/quantization-jina-embeddings.ipynb) .
- For the math behind BBQ, see[Better Binary Quantization in Lucene and Elasticsearch](https://www.elastic.co/search-labs/blog/better-binary-quantization-lucene-elasticsearch) .
- For more on Jina v5's architecture, see[Jina embeddings v5 on Search Labs](https://www.elastic.co/search-labs/blog/jina-embeddings-v5-text) .
- For a broader walkthrough on adopting BBQ, see[How to implement BBQ into your use case](https://www.elastic.co/search-labs/blog/bbq-implementation-into-use-case) .
- For the original research behind BBQ, see the paper[RaBitQ: Quantizing High-Dimensional Vectors with a Theoretical Error Bound for Approximate Nearest Neighbor Search](https://arxiv.org/abs/2405.12497) .


#### How helpful was this content?


Not helpful


Somewhat helpful


Very helpful


[Report an issue](https://discuss.elastic.co/c/elastic-community-ecosystem/elasticsearch-labs/101)


## Related Content


[Jina AI](https://www.elastic.co/search-labs/blog/category/jina-ai)[Relevance](https://www.elastic.co/search-labs/blog/category/relevance) +1


July 27, 2026


#### [56% faster, up to 50% better retrieval performance: What's inside Jina's new 600 million parameter listwise reranker](https://www.elastic.co/search-labs/blog/jina-reranker-35-legal-medical-structured-data)


Jina Reranker 3.5 beats v3 by 50%+ on case law, closes the gap with models 7x its size on legal, medical, and financial benchmarks, and beats them outright on structured data. It's a drop-in replacement for v3, with no API changes.


FWSM


By:[Felix Wang](https://www.elastic.co/search-labs/author/felix-wang)


and[Scott Martens](https://www.elastic.co/search-labs/author/scott-martens)


[Vector Database](https://www.elastic.co/search-labs/blog/category/vector-database)[ML Research](https://www.elastic.co/search-labs/blog/category/ml-research) +1


July 28, 2026


#### [17% faster search, zero config: auto-calibrating vector quantization in Elasticsearch](https://www.elastic.co/search-labs/blog/vector-quantization-auto-calibration-diskbbq)


Automatic calibration at merge time picks vector quantization parameters for each segment by predicting recall from a small sample. Here's how we built it into Elasticsearch's merge path.


TTTV


By:[Tommaso Teofili](https://www.elastic.co/search-labs/author/tommaso-teofili)


and[Thomas Veasey](https://www.elastic.co/search-labs/author/thomas-veasey)


[Jina AI](https://www.elastic.co/search-labs/blog/category/jina-ai)[Integrations](https://www.elastic.co/search-labs/blog/category/integrations)


July 23, 2026


#### [On-prem in under 5 minutes: Jina embedding models now available for on-prem deployment](https://www.elastic.co/search-labs/blog/on-prem-ai-jina-embedding-models)


All 28 Jina AI models, including rerankers, as ready-to-deploy Docker containers, with zero telemetry and no license server. Drop-in compatible with OpenAI, Cohere, Voyage AI and Elastic Inference Service APIs.


SM


By:[Scott Martens](https://www.elastic.co/search-labs/author/scott-martens)


[Vector Database](https://www.elastic.co/search-labs/blog/category/vector-database)[Relevance](https://www.elastic.co/search-labs/blog/category/relevance) +1


July 16, 2026


#### [A picture is worth 1.5x the words: What we learned benchmarking product search embeddings](https://www.elastic.co/search-labs/blog/multimodal-embeddings-ecommerce-product-search)


We benchmarked two embedding models on 5,000 real products and found that combining image and text beats either alone by up to 50%. Here's the data and the model that won.


SV


By:[Sofia Vasileva](https://www.elastic.co/search-labs/author/sofia-vasileva)


[Vector Database](https://www.elastic.co/search-labs/blog/category/vector-database)


July 13, 2026


#### [The disk that never woke up: what actually decided our Qdrant vector search benchmark rematch](https://www.elastic.co/search-labs/blog/vector-search-benchmark-elasticsearch-qdrant)


On the same hardware, Elasticsearch and Qdrant land in the same range at 56 QPS. The io_uring disk scorer and memory claims turned out to be the two things that mattered least.


JF


By:[Jim Ferenczi](https://www.elastic.co/search-labs/author/jim-ferenczi)
