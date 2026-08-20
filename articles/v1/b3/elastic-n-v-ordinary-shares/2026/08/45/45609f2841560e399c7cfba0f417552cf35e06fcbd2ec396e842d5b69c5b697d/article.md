---
schema_version: "1.0.0"
document_id: "45609f2841560e399c7cfba0f417552cf35e06fcbd2ec396e842d5b69c5b697d"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-news-import-ebf5ac4e0909"
canonical_url: "https://www.elastic.co/search-labs/blog/elasticsearch-batched-query-phase"
published_at: "2026-08-10T00:00:00+00:00"
first_seen_at: "2026-08-10T18:52:38.864115+00:00"
fetched_at: "2026-08-10T18:52:40.748432+00:00"
content_hash: "sha256:2d6818b36e0af243e528b37470eaa1ba3831897778d5367ec16a9640ad1ebec6"
---

# How Elasticsearch's batched query phase improves search performance at scale

The batched query phase, which is live in Elasticsearch Serverless and released in Elasticsearch 9.5.0, changes how search is coordinated across the cluster. The coordinating node now batches all queries for each data node into one, instead of a separate transport request per shard. Data nodes can then do partial result reduction themselves, rather than shipping everything back to the coordinating node. In coordination-bound workloads, this can cut search execution time in half.


## How a search is executed in Elasticsearch


Let’s start with some Elasticsearch basics. When a search request lands on an Elasticsearch node, that node is called the *coordinating node* **** for the search. By default, any Elasticsearch node can act as a coordinating node. The coordinating node determines which shards need to be searched based on the index or indices specified by the search. The nodes that those shards live on are called *data nodes* . A coordinating node can also be a data node, as it may host shards that are relevant to the search.


Elasticsearch performs the search in two primary phases: the *query phase* (also known as the *scatter phase* ) and the *fetch phase* **** (also known as the *gather phase* ). The query phase is responsible for going to the data nodes and executing the query on each shard. Each shard responds with a set of document IDs (just the IDs, no data) and an associated score for each. These results are reduced. Next, the fetch phase goes back out to the data nodes to fetch the document` _source` (the data).


What exactly is a *reduction* **** in Elasticsearch? A reduction turns per-shard results from the query phase into a single merged result for the client. Suppose a search asks for the top five hits in a three-shard index, according to some relevance score. Elasticsearch must then get the top five hits from each targeted shard. Why? Because it’s possible that one shard contains the global top five, or, more likely, that the top five docs are spread across shards.


If three shards are being searched, the coordinating node will have 15` (docID, score)` pairs after the query phase. These results are reduced: The documents with the top five scores are kept and the rest thrown away. Then the fetch phase reaches back out to the data nodes to get the documents’` _source` data, which Elasticsearch then responds with.


shard1_results = \[(id: 231, score: 0.871), (id: 445, score: 0.812), (id: 88, score: 0.754), (id: 312, score: 0.701), (id: 567, score: 0.643)\]


shard2_results = \[(id: 847, score: 0.921), (id: 76, score: 0.843), (id: 125, score: 0.783), (id: 438, score: 0.729), (id: 590, score: 0.668)\]


shard3_results = \[(id: 512, score: 0.887), (id: 289, score: 0.798), (id: 74, score: 0.741), (id: 631, score: 0.682), (id: 405, score: 0.619)\]


// Take the top five scores from above (that’s the “reduction”) reduced_result = \[(id: 847, score: 0.921), (id: 512, score: 0.887), (id: 231, score: 0.871), (id: 76, score: 0.843), (id: 445, score: 0.812)\]


## What is the batched query phase?


### Without batching (shard fan-out)


To understand the batched query phase, we must first understand how the query phase worked without batching. The following diagram represents a three-node Elasticsearch cluster with 12 index shards. Suppose a client search request lands on Node 1. That makes Node 1 the coordinating node for the search.


The coordinating node first *fans out* to all shards, performing the client’s query on each shard. Each shard query is facilitated by a *transport request* , which is a protocol used for communication between Elasticsearch nodes. Notice that in this diagram, each shard gets its own transport request. Node 1 holds shards itself, but no network request is needed to query those shards.


After querying the shards, the coordinating node must reduce them, as described in the previous section. At this point, the coordinating node is holding 12 shard results. Once it reduces them all, it can proceed with the fetch phase and then respond back to the client.


### With batching


So what does the batched query phase change? The batched query phase first takes effect before dispatching the shard query transport requests. Now the coordinating node batches the shards it needs to query for each data node and requests them all at once.


Notice that only one transport request was made to Node 2 and one to Node 3. This is the first change in the batched query phase. Each transport request induces some overhead, so this change alone buys us our first performance improvement, reducing latency and CPU cycles spent on overhead.


The second change in the batched query phase has to do with reducing shard results. Now partial reductions occur on the data nodes themselves. For instance, after querying shards 2, 5, 8, and 11, Node 2 then reduces those results. The coordinating node, upon receiving partially reduced results from Node 2 and Node 3, must then perform a **** final reduction. This is the second major enhancement we get from the batched query phase: the spreading out of reduction work across the data nodes. This reduces memory pressure on the coordinating node, which we’ll see measured later in the benchmarking section.


## The benefits of batching


*Fan-out* (no batching) is how search worked for a very long time. It has advantages: Each shard is a separate request that returns and can be retried independently from all the others. With many shards involved though, each shard request will cause overhead, due to many round trips going between the coordinating node and the data nodes.


Also, the coordinating node doesn’t have enough info to be able to decide the pace at which to send requests to each data node. It sends a maximum of five concurrent requests per data node by default. Five is a bit of a magic number: It allows some parallelism, while at the same time preventing a single query from taking over an entire data node. In reality, if all shard requests involving a search request are presented in one batch to each data node, the data node can then look at its internal state and adapt its pace dynamically.


The batched query phase results in several benefits, including:


-


Increased efficiency, thanks to fewer round trips: Less CPU spent on transport overhead and fewer bytes going through the transport layer.


-


Spreading out the load of reductions: The coordinating node was previously the bottleneck for reductions, and now data nodes share the work.


-


Better resource usage: We may be able to better max out the data node’s CPUs.


The average search against many shards can now be served much quicker, with lower latency and higher throughput.


## Batched query phase benchmarks: Latency and memory usage


To measure the benefits of the batched query phase, we ran a couple of benchmarks. The first displays the benefit of reduced transport overhead. This benchmark was built off the “many-shards-quantitative”[nightly benchmark](https://elasticsearch-benchmarks.elastic.co/) . It runs on a three-node Elasticsearch cluster, running batches of searches targeting 1,000, 5,000, and 20,000 shards. The queries are` match_all` queries with` size: 0` . That means querying the shards themselves is effectively a no-op. This benchmark is meant to isolate the work of coordinating the search across the cluster.


We ran the same benchmark with and without the batched query phase (by toggling the cluster setting` search.batched_query_phase` ). At 5,000 shards, the batched query phase makes searches twice as fast. By 20,000 shards, it’s 2.2x faster. These results are a ceiling for what a realistic workload can expect to benefit; any real query work at the shard level will dilute the overall result. However, the gains are real, and the *coordination work* of your queries will benefit as shown.


Next, we benchmarked the benefits of the batched query phase on large reductions. In our benchmark, we ran a large terms aggregation over a data set called` http_logs` (which can be found in our[rally-tracks](https://github.com/elastic/rally-tracks) repo). This data set has 247 million documents, which we indexed in seven indices each with 100 shards, again in a three-node Elasticsearch cluster. We ran a single[terms aggregation](https://www.elastic.co/docs/reference/aggregations/search-aggregations-bucket-terms-aggregation) query for the` clientip` field with` size: 1000` and` shard_size: 50000` . That means we’re asking for the top 1,000 terms, although each shard will return 50,000 buckets to be reduced to that 1,000.


The *memory used* is measured by something called a *circuit breaker* , which accounts for memory usage in Elasticsearch. This field can be found in the` /_nodes/stats` response under` nodes.<node_id>.breakers.request.estimated_size_in_bytes` . It estimates the memory in use for processing in-flight search requests.


The memory benchmark shows how the work of reductions is now spread across the cluster. In blue is the memory used with` search.batched_query_phase: false` . We can see that the node elasticsearch-0 is the coordinating node, as it bears the entire reduction load itself. This effect is no longer the case with` search.batched_query_phase: true` in red. The node elasticsearch-1 is the coordinating node, but it uses far less memory. That’s because the data nodes elasticsearch-0 and elasticsearch-2 do reductions themselves, reducing the results before they’re returned to the coordinating node.


## Summary


Batching shard queries per data node reduces transport overhead and allows us to partially reduce results on the data nodes themselves. The result is lower latency for searches targeting many shards and better distribution of work across the cluster for reduction-heavy workloads. It also opens the door for future enhancements around shard concurrency, which we’re excited to pursue.
