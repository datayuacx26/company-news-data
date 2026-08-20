---
schema_version: "1.0.0"
document_id: "d07a17e316e56cba12b24d295180b6ee8c3830b8e3e150f1d31d069c4bd0c462"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-news-import-ebf5ac4e0909"
canonical_url: "https://www.elastic.co/search-labs/blog/vector-search-benchmark-elasticsearch-qdrant"
published_at: "2026-07-13T13:56:02+00:00"
first_seen_at: "2026-07-21T17:41:41.822757+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:34f4d4d5de34f9bed5e033cf782296872ba56a501d19b7678a5e02e888ef6fc5"
---

# The disk that never woke up: what actually decided our Qdrant vector search benchmark rematch

Try out vector search for yourself using this[self-paced hands-on learning](https://www.elastic.co/demo-gallery/vector-search) for Search AI. You can start a[free cloud trial](https://cloud.elastic.co/registration?onboarding_token=search&cta=cloudregistration&tech=trial&plcmt=cross%20module&pg=search-labs) or try Elastic on your[local machine](https://github.com/elastic/start-local?cta=local-machine&tech=github&plcmt=cross%20module&pg=search-labs) now.


[Vector search](https://www.elastic.co/search-labs/blog/introduction-to-vector-search) vendors like a goodbenchmark , and lately[Elasticsearch](https://elastic.co/elasticsearch) and Qdrant have been trading them. Earlier this year we[published one](https://www.elastic.co/search-labs/blog/vector-search-benchmark-elasticsearch-vs-qdrant) comparing Elasticsearch's` bbq_disk` against Qdrant on a disk-rescore workload, where full-precision vectors live on disk and get read back to rescore the top candidates. Qdrant[replied](https://qdrant.tech/blog/benchmark-elastic-diskbbq/) with better numbers on their side and a set of explanations for why: an io_uring-based async disk scorer, and remarks about Elasticsearch needing more memory.


We didn't want to fire back with another round of numbers. Reproducing a benchmark is the easy part; understanding why it produces the numbers it does is the useful part, and it is the part both original posts skipped. So we stood up Qdrant's exact setup on our own cluster, loaded the same[21 million vectors](https://huggingface.co/datasets/kenhktsui/wiki_dpr_e5) , reproduced their result, and then traced every number back to its cause.


Here is what we found. The result comes down to a few setup choices: how many segments you build, whether the data is warm in memory, how you return the result ids, and which hardware you run on. Control those, and it cuts both ways: matched to Qdrant's setup the two engines are on par onquery speed, and with fast default ingestion Elasticsearch is faster at search and faster to index. The big multipliers in both posts, our 7x and their reply, are artifacts of those choices, not a verdict on either engine. The two reasons Qdrant leaned on hardest, an io_uring disk scorer and Elasticsearch's memory use, turn out to be the two that mattered least: the disk is never read during the run, and the memory gap is a difference in labels, not in bytes.


We hold our own original post to the same standard. Both sides changed several things at once and reported the result rather than the reason. One rule for the rest of this post: every claim gets a number and a mechanism, or it doesn't ship.


## Vector search benchmark setup: hardware, config and query set


Here is exactly what each side ran.


Elastic (original post) Qdrant (response) This post (apples-to-apples)


Nodes 3 × n4-standard-8 (7 vCPU / 26 GB), GCP 3 × m6g.large (2 vCPU / 8 GB), AWS 3 × m6g.large (2 vCPU / 8 GB)


Elasticsearch bbq_disk 2-bit, replicas: 1 (cited original) bbq_disk 2-bit, vectordb_document, bfloat16, replicas: 0


Qdrant 2-bit, async scorer off, RF=2 TurboQuant 4-bit, async scorer on, RF=1 TurboQuant 4-bit, async scorer on, RF=1


Copies per shard 2 = 2 (matched) 1 1 = 1 (matched)


Query set 10k fixed, recall@100 10k fixed, recall@100 10k fixed, recall@100


Three things are worth pinning down, because all three have been used as talking points.


1. Replica count was matched in both rounds: An Elasticsearch index with` replicas: 1` keeps two copies of each shard, which is exactly what Qdrant's RF=2 does. In this round, both sides ran a single copy. It was never a thumb on the scale in either direction.
2. We changed a few things on our side on purpose, and we will own them:` vectordb_document` index mode, float32 to bfloat16 for stored full-precision vectors (3,072 down to 1,536 bytes each), and a merge policy tuned for vector data. Those choices matter later, so we are flagging them up front rather than burying them.
3. Memory is compared like for like. Qdrant's post sets Elasticsearch's JVM heap allocation against Qdrant's total memory use, but those are different accounting categories, since heap size is not total memory consumption. On these 8 GB nodes we give the JVM heap 2 GB, 25% of node RAM, deliberately: that follows Elastic's Vector Search Optimized profile, which leaves the other 75% for the OS page cache where the vectors actually live, rather than the general 50%-heap upper bound. The honest comparison is resident index plus runtime plus active page cache, and on an 8 GB node both engines live inside the same envelope.


Look at the middle column, though. Between our original run and their response, Qdrant changed almost everything at once: new hardware, 4-bit[quantization](https://www.elastic.co/search-labs/blog/better-binary-quantization-lucene-elasticsearch) instead of 2-bit, the async scorer on, a different replication factor. Then it credited the win to one of those changes. Drawing meaningful conclusions from many simultaneous changes is hard, and pinning down which change actually moved the number is the whole job of the sections that follow.


## What does disk rescore cost in a vector search benchmark?


In this benchmark, the answer is almost nothing, because the disk is never read. Two back-of-the-envelope budgets show why, and the live measurements later in the post confirm it.


The design under test is built around one idea: keep a small quantized copy of each vector in RAM for approximate search, keep the full-precision originals on disk, and read a handful of those originals back to rescore the top candidates for accuracy.


Two budgets determine performance: disk I/O for rescore reads and RAM for what has to stay resident. Both are computable on the back of an envelope, so let us compute them before measuring anything.


### The disk I/O budget for vector search rescoring


Rescoring reads the top 100 candidates per query. A 768-dimensional float32 vector is 768 × 4 = 3,072 bytes. Because those reads are not page-aligned, the real cost per vector is closer to an 8 KB page read. Each query needs roughly 800 KB and 100 random reads. Scale that up:


Target QPS (cold) Rescored vectors/s Reads/s (IOPS) per node MB/s per node


10 1,000 333 3


100 10,000 3,333 27


1,000 100,000 33,333 273


10,000 1,000,000 333,333 2,731


Two things fall out immediately: First, cold rescore is an IOPS problem, never a bandwidth one. Even at 10,000 QPS, you need only about 2.7 GB/s per node, but a third of a million IOPS per node. Second, the total working set for the benchmark is tiny:` 100 candidates × 10,000 queries × 8 KB = 8 GB across the cluster, or 2.7 GB per node` . That fits in the free page cache on an 8 GB node with room to spare. The first time the benchmark cycles through its 10,000 queries, the originals it touches are pulled into page cache, and every read after that is a memory copy.


Put a ceiling on it too. On gp3 at baseline (3,000 IOPS/node), if the reads did go to disk, the workload would top out around 90 QPS from IOPS alone; provisioned gp3 or local NVMe would push that to roughly 480 or 3,000. Hold that number. It is the ceiling that would matter in a benchmark that actually touched the disk.


### The RAM budget for a disk-based vector index


What has to stay resident is the quantized copy plus the search structure. Everything else is page cache. At 4-bit quantization and an HNSW graph with m=16:


Component Bytes/vector Per node @ 21M


4-bit quantized vectors (always_ram) 384 2.69 GB


HNSW graph, m=16 (2m links × 4 B) 136 0.95 GB


Resident total 520 3.64 GB


Raw float32 originals (on disk) 3,072 21.5 GB (on disk)


We measured the resident footprint on the running nodes at 3.4 to 3.9 GB, sitting right on the 3.64 GB estimate. The point to hold is that at 21 million vectors, the entire searchable index (quantized vectors plus graph) is about 3.6 GB per node and it fits in RAM, leaving a couple of gigabytes to spare. Nothing in this benchmark forces the disk-backed design to actually use the disk. We come back to that at the end, because it is the real story.


## Does io_uring make vector search faster?


Qdrant's reply credits two levers it says we omitted: a two-stage prefetch-and-rescoreretrieval pattern and an async, io_uring-based disk scorer. They are two faces of the same rescore step, and the same evidence answers both. Take the async scorer first, since it is the one Qdrant makes the centerpiece: io_uring to parallelize disk reads during rescoring. It is a good feature, and in this benchmark it had exactly one job: parallelize the disk reads during rescoring. It got to do none of them. We verified that in three independent ways.


First, the arithmetic. From the RAM budget, the quantized index and graph are pinned in RAM. From the I/O budget, the rescore working set is 2.7 GB per node and lives in page cache after the first pass. That leaves no disk reads on the hot path to accelerate.


Second, their own methodology guarantees it. Each operating point in the harness runs a full 10,000-query recall pass, followed by a timedthroughput window that cycles through the same 10,000 queries again. By the time the stopwatch starts, every vector those queries will touch is already resident. The measurement is warm by construction.


Third, and this is the part we insisted on doing rather than arguing, we measured it. We were careful here because it is easy to accidentally test io_uring in the off state and not notice. Running Qdrant in Docker, we found that io_uring was not even initializing: the default seccomp profile blocks the io_uring syscalls, so it silently fell back to synchronous reads (` failed to initialize io_uring instance: Operation not permitted` ). We fixed that, ran the container with seccomp unconfined, confirmed zero io_uring errors in the logs, and re-ran. This honestly means we have both states:


- Async scorer effectively off (synchronous fallback): 31.6 QPS at ef=50.
- Async scorer on (io_uring confirmed working): 35.8 QPS at ef=50.


A 13% move, and even that is within the run-to-run noise for a measurement whose ceiling is set by CPU, not disk. Put plainly, switching on the feature that the entire result was credited to changed about as much as running the benchmark a second time. To close the loop, during the actual throughput window we watched the block device on all three nodes with` iostat` : read throughput held at 0 MB/s and 0 IOPS, while CPU sat at 60-70% and climbed to 100% as we added concurrency. The bottleneck is the CPU doing[quantized distance computations](https://www.elastic.co/search-labs/blog/bbq-vector-comparison-simd-instructions) and graph traversal. It is not the disk, because the disk is asleep.


The two-stage prefetch-and-rescore pattern, the lever Qdrant lists first, is answered by the same run. Every Qdrant number here comes from Qdrant's own reproduction script, which uses their two-stage query throughout, so two-stage was on for the whole benchmark, including the io_uring comparison, and the disk still held at 0 IOPS. It is also not something we omitted: prefetch is the approximate search over the quantized vectors, which lives in RAM, and rescore reads the originals, which live in page cache here. That is the disk-rescore workload, and` bbq_disk` runs the identical shape. The one part of two-stage that is not about disk, how many candidates you rescore, is CPU rather than I/O; it is already included in these numbers and available to both engines, so it is not a hidden lever either.


There is a smaller detail worth noting. Qdrant's post does not say which disk they ran on. That would normally matter in a benchmark whose premise is disk access, but here it does not, for the same reason the async scorer does not: you cannot be bottlenecked on a device you never read from. The disk is left unspecified, and as it happens it is also beside the point.


Here's what I've seen, as an engineer who has sat through a lot of these conversations: io_uring gets treated as a universal fix far more often than it is the bottleneck. It is excellent when the working set overflows RAM, and the cold I/O budget above shows exactly that case, where parallelizing reads buys you throughput. This benchmark never enters that regime. Async disk reads are a real answer to a real question, and this benchmark just never asked it. Of everything that changed between the two runs, io_uring is the one that got the headline and moved the number the least.


## Why does faster ingestion mean slower vector search queries?


This is where reproducing the number got interesting, and where the honest driver of the gap turned out to live.


Our first load produced 128 segments and roughly half the throughput we expected. Qdrant's published provenance, right there in[their results JSON](https://github.com/qdrant-labs/wiki-dpr-disk-rescore-benchmark/blob/main/results/published/qdrant_4vcpu16gb.json) , records 67 segments. That one difference explained almost everything. When we merged our collection down to match their segment count, all three metrics moved together toward their numbers:


Segments Recall@100 QPS Avg latency


128 (our first, parallel load) 0.9745 35.8 112 ms


66 (merged to match theirs) 0.9531 53.3 75 ms


67 (Qdrant published) 0.9596 67.2 59.5 ms


Look at what moves. Recall falls as segments drop, because with fewer segments each query examines fewer total candidates. That is the tell that segment count, not tuning, was inflating our recall. And throughput rises, because HNSW query cost is very sensitive to the number of segments: every query fans out across every segment's graph, so twice the segments is close to twice the per-query work, plus the contention of doing all that on two cores. The small remaining gap between our 53 and their 67 is warm-cache completeness in our shorter run, not the engine.


Here is the part worth stating carefully, because it is architectural and it cuts against us as much as for us. We use the same segmented design; this is not a Qdrant quirk. The question is how you get to few segments. You get there by ingesting single-threaded, so data concentrates into fewer, larger segments. Fewer segments means faster queries, but you pay for it with slow ingestion. Our parallel load was fast and produced many segments; their single-threaded load was slower and produced few. It is a real tradeoff, and it is the same one on both sides.


Where the two designs genuinely differ is how much that tradeoff hurts. Building an HNSW graph is expensive, and its query cost scales poorly with segment count. On these 2-vCPU nodes, constructing the graph at` ef_construct=256` took about 1.6 hours pinned at 100% CPU. The IVF layout behind[bbq_disk](https://www.elastic.co/search-labs/blog/better-binary-quantization-101) is cheap to build and far less sensitive to segment count, so we can ingest with many threads while keeping querylatency low. We do not have to choose. That is not a benchmark trick. It is the IVF vs. HNSW tradeoff, stated honestly.


## Why document retrieval, not vector search, explains the rest of the latency


Now turn the same lens on our own result, because the same accounting applies to our own numbers too.


At the search-light end of the sweep, a large share of the per-query time is not vector search at all. It is the fetch phase assembling the response.` vectordb_document` returns full source documents, which is exactly what you want for retrieval and[hybrid search](https://www.elastic.co/search-labs/blog/hybrid-search-elasticsearch) , where you actually need thedocument back. A pure vector benchmark only needs the top-N ids, and today` _id` lives in the same stored-fields column as` _source` , the text and the vector. So fetching an id pulls that entire compressed block through the decompressor for every hit. When we isolate the id from that column, Elasticsearch steps straight up into Qdrant's low-segment throughput range. That gap is document retrieval, not vector search.


The fix is structural, not a tuning flag: give` _id` its own doc-values field so returning an id never touches` _source` . That is what the[vectordb_columnar](https://www.elastic.co/search-labs/blog/elasticsearch-columnar-storage) mode we are building automatically does. We are calling it out here because good benchmarking means naming your own costs as clearly as anyone else's, and this one is a document-retrieval cost sitting inside a vector-search score.


## Elasticsearch vs Qdrant vector search benchmark: our numbers, on the same box


Elasticsearch on the same three m6g.large nodes,` vectordb_document` with bfloat16, warm, shown both as it runs today and with the id isolated:


Visit % Recall@100 QPS (default) QPS (id-isolated)


1 0.894 44 89


2 0.939 39 73


3 0.956 35 56


5 0.970 31 40


The id-isolated column is measured by dropping the stored-fields fetch entirely, which is a near-upper-bound proxy for a doc-values` _id` (the doc-values read is cheap but not literally free). At around 0.96 recall, id-isolated Elasticsearch does about 56 QPS, sitting right alongside Qdrant's 53 to 67 on identical hardware. Same ballpark. The gaps in both posts came from setup and document retrieval, not from the vector engine: segment counts, the id fetch, the hardware. Their reply we can account for in full here; the larger 7x from our own first post we cannot yet, and we take that up below. When you control for those, two well-built systems doing the same work on the same box land close together, which is what you would expect.


So here is the plain claim: performance depends on the setup. Match Qdrant's slow, single-threaded ingestion, the one that gives them their 67-segment configuration, and the two engines are on par on the search itself: id-isolated Elasticsearch does about 56 QPS against their 53 to 67 at the same recall. But that low segment count is bought with slow ingestion. Let both engines ingest fast, which is the natural default, and Qdrant lands back at 128 segments and about 35 QPS, while Elasticsearch degrades less as the segment count climbs, because the IVF layout behind` bbq_disk` is less sensitive to segment count than HNSW. So with default, fast ingestion, Elasticsearch is faster at search too, and it reached a queryable index faster to begin with. The one place we still trail is returning the ids, which is document retrieval rather than search, and it is exactly what` vectordb_columnar` removes.


We deliberately did not run the 4 vCPU / 16 GB tier. It lifts both engines together and shows the identical trend, so it would add cost without adding insight.


## Our original 7x Elasticsearch vs Qdrant benchmark number, explained


The rule in this post is that every claim gets a number and a mechanism. That rule was also applied to our[original benchmark](https://www.elastic.co/search-labs/blog/vector-search-benchmark-elasticsearch-vs-qdrant) .


That post measured a 7x throughput advantage for Elasticsearch, and the number is real for that configuration. The setup was disclosed in full, including that Qdrant does better on NVMe. But the post attached a mechanism to the number: Qdrant was bottlenecked by random disk reads of the original vectors during rescoring, a problem made worse by network-attached storage. The arithmetic in this post undercuts that explanation. Those round-1 nodes had 26 GB of RAM, more headroom than the 8 GB nodes here where we watched the disk sit at 0 IOPS, so if the disk was idle here, it was almost certainly idle there too.


So what actually held round-1 Qdrant to 4.5 QPS at 0.97 recall? We cannot claim to know yet, but the original post's own configuration points the way: it ran Qdrant on 2-bit quantization with oversampling pinned at 1. Two-bit codes are coarse, and with oversampling fixed at 1 the only lever left to recover recall is ef. Reaching 0.97 recall on ef alone means a very large ef, and a very large ef makes each query expensive on its own, before disk enters the picture at all. That is still a hypothesis, but the mechanism the original post named, random disk reads during rescore, does not survive the same arithmetic we just applied to Qdrant.


## Benchmarking, not benchmarketing


The numbers reproduce; the explanations don't. A benchmark that made "disk reads" its headline ran with the disk asleep, and we watched it sit at 0 IOPS for the whole window. The honest differences we could actually find were a segment count (an ingest-speed-versus-query-speed tradeoff where the IVF layout lets` bbq_disk` ingest fast and query fast) and a stored-field retrieval cost on our side (document retrieval, not vector search, and something` vectordb_columnar` removes). None of it is io_uring, and none of it is "Java is heavy."


The original and arguably bigger point got lost in the io_uring discussion: putting vectors on disk was never really about the rescore, it is about memory. The key was keeping the searchable index, the quantized vectors and the IVF or HNSW structure, compact and disk-resident enough to serve more vectors per gigabyte of RAM than a design that pins everything in memory. Qdrant's setup pins the 4-bit vectors with` always_ram` and keeps the HNSW graph in RAM, about 3.6 GB per node for 21 million vectors.` bbq_disk` keeps the quantized IVF on disk. At 21 million vectors on 8 GB nodes, everything fits in RAM either way, which is precisely why this benchmark cannot tell the two designs apart. It is measuring the case where the interesting variable has been held constant.


The interesting question is what happens when the searchable index stops fitting. Scale thecorpus until the quantized vectors and the structure exceed page cache, and the two designs diverge: one keeps serving from disk, the other needs more RAM per node. That is the regime` bbq_disk` was built for, and it is the one neither post has measured. The rescore-under-disk-pressure case, many more distinct queries than fit in cache, is worth measuring too, but it is the second question, not the first.


We tried to get there, and our first attempt still fit in cache, because 21 million vectors on these nodes do not spill. So we are not done. We are building a benchmark with a corpus large enough that the searchable index no longer fits in RAM, and we will publish those numbers, with the reasons attached and checked the same way.


The ask here is to the reader, not to Qdrant: when a benchmark hands you a clean multiplier, chase every number back to a cause before you believe the story around it. Half the time it is a warm cache or a segment count.


The Qdrant team built a good engine, and we're not questioning that. The honest verdict is not about who is faster: matched to their setup, the two are on par, and on the fast default path, Elasticsearch is quicker for bothindexing and searching. The numbers were fine. The reasons attached to them were the part worth checking, starting with our own. And the benchmark that would actually stress a disk-backed index, a working set that does not fit in RAM, is still to be written. We will bring the numbers.


#### How helpful was this content?


Not helpful


Somewhat helpful


Very helpful


[Report an issue](https://discuss.elastic.co/c/elastic-community-ecosystem/elasticsearch-labs/101)


## Related Content


[Vector Database](https://www.elastic.co/search-labs/blog/category/vector-database)[ML Research](https://www.elastic.co/search-labs/blog/category/ml-research) +1


July 28, 2026


#### [17% faster search, zero config: auto-calibrating vector quantization in Elasticsearch](https://www.elastic.co/search-labs/blog/vector-quantization-auto-calibration-diskbbq)


Automatic calibration at merge time picks vector quantization parameters for each segment by predicting recall from a small sample. Here's how we built it into Elasticsearch's merge path.


TTTV


By:[Tommaso Teofili](https://www.elastic.co/search-labs/author/tommaso-teofili)


and[Thomas Veasey](https://www.elastic.co/search-labs/author/thomas-veasey)


[Vector Database](https://www.elastic.co/search-labs/blog/category/vector-database)[Relevance](https://www.elastic.co/search-labs/blog/category/relevance) +1


July 16, 2026


#### [A picture is worth 1.5x the words: What we learned benchmarking product search embeddings](https://www.elastic.co/search-labs/blog/multimodal-embeddings-ecommerce-product-search)


We benchmarked two embedding models on 5,000 real products and found that combining image and text beats either alone by up to 50%. Here's the data and the model that won.


SV


By:[Sofia Vasileva](https://www.elastic.co/search-labs/author/sofia-vasileva)


[Integrations](https://www.elastic.co/search-labs/blog/category/integrations)[Vector Database](https://www.elastic.co/search-labs/blog/category/vector-database) +1


July 21, 2026


#### [4 NVIDIA AI tasks, 1 Elasticsearch API: Embeddings, chat, completion, and rerank](https://www.elastic.co/search-labs/blog/elasticsearch-nvidia-inference)


Set up NVIDIA hosted models in Elasticsearch with one API key and a model ID. No custom integration code needed.


K


By:[Jan Kazlouski](https://www.elastic.co/search-labs/author/jan-kazlouski)


[Vector Database](https://www.elastic.co/search-labs/blog/category/vector-database)[Jina AI](https://www.elastic.co/search-labs/blog/category/jina-ai) +1


July 10, 2026


#### [How BBQ shrinks Jina v5 embeddings by 29x without losing recall in Elasticsearch](https://www.elastic.co/search-labs/blog/bbq-quantization-jina-embeddings-v5)


A hands-on test comparing BBQ and float32 vector indices in Elasticsearch, measuring memory, disk and recall@10 across five languages.


JR


By:[Jeffrey Rengifo](https://www.elastic.co/search-labs/author/jeffrey-rengifo)


[AI](https://www.elastic.co/search-labs/blog/category/ai)[Vector Database](https://www.elastic.co/search-labs/blog/category/vector-database)


July 7, 2026


#### [Short queries, formal documents: how HyDE improved semantic search precision by 50% in Elasticsearch](https://www.elastic.co/search-labs/blog/hyde-semantic-search-elasticsearch)


HyDE boosts semantic search precision and recall by 50% on short queries. Here's how to implement it in Elasticsearch with the Inference API and semantic_text.


JR


By:[Jeffrey Rengifo](https://www.elastic.co/search-labs/author/jeffrey-rengifo)
