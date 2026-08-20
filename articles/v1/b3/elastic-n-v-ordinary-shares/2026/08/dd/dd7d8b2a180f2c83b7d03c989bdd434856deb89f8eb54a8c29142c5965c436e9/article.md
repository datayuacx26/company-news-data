---
schema_version: "1.0.0"
document_id: "dd7d8b2a180f2c83b7d03c989bdd434856deb89f8eb54a8c29142c5965c436e9"
company_key: "elastic-n-v-ordinary-shares"
company: "Elastic N.V."
source_id: "elastic-n-v-ordinary-shares-news-import-ebf5ac4e0909"
canonical_url: "https://www.elastic.co/search-labs/blog/time-series-database-compression-elasticsearch"
published_at: "2026-08-19T00:00:00+00:00"
first_seen_at: "2026-08-20T02:23:13.375428+00:00"
fetched_at: "2026-08-20T02:23:16.458251+00:00"
content_hash: "sha256:3fe715af5e6723e0b84b188eaca7d6042a34b407d7b1d5bc3008a1580fc386b5"
---

# ES95: Adaptive Compression for Elasticsearch Time-Series Metrics

*The best compression strategy is the one that understands your data.*


Observability workloads are storage-intensive by nature, and the composition of that storage determines both cost and query performance.` ES95` introduces adaptive compression: rather than applying the same encoding to every numeric field, it automatically selects the encoding that best matches each field's structure. The result is a 33.6% reduction in total doc-values storage, 19% to 74% reduction on floating-point gauge metrics and a 92% reduction in` @timestamp` . No configuration or migration required.


### Observability data is storage-intensive


Data processing systems are rarely limited by how fast they can compute. They’re limited by how fast they can move bytes: off disk, across the network, and through the memory hierarchy. Compression is how a storage engine trades CPU time for memory bandwidth, spending comparatively cheap CPU cycles so fewer bytes have to travel through the parts of the system that are usually constrained. In a read-heavy system like Elasticsearch, that trade-off pays back every time data is queried, often long after it was written.


Storage size and query performance move together; fewer bytes on disk means fewer bytes to read on every range query, every aggregation and every dashboard load. Compression is not just about saving storage. Every byte that is never written is also a byte that never has to be read.


The right encoding depends on the structure of the values themselves, and the largest wins come from exploiting the structure already present in the data rather than squeezing an opaque stream of bytes. Few workloads expose that structure more clearly than observability metrics.


A single host reports hundreds of metrics every few seconds, including CPU utilization, memory ratios, request latencies, and network throughput. Multiply that by thousands of hosts across weeks of retention, and the bytes accumulate fast. Most of that volume is structured but not uniform: timestamps arrive at near-constant intervals from thousands of concurrent series, counters increase monotonically, while gauges like` 23.47` or` 1.15` are short decimal measurements.


A fixed compression approach cannot adapt to that variety. A timestamp column and a floating-point gauge column compress through fundamentally different techniques, but a codec that applies the same approach to both will necessarily handle one of them poorly. For most of Elasticsearch's time-series codec history, gauges were on the losing end of that trade-off.


### The structure the old codec was not built to exploit


Elasticsearch stores time-series numeric values in *doc values* : a column-oriented structure where all values for the same field sit adjacent on disk. That adjacency makes compression possible: the codec compares consecutive values of the same field, finds patterns, and exploits them.


Figure 1: Column-oriented storage puts all values for the same field adjacent on disk.


The[time-series codec before ES95](https://www.elastic.co/search-labs/blog/time-series-data-elasticsearch-storage-wins) applied the same fixed encoding to every numeric field: delta encoding followed by normalization, GCD (greatest common divisor) reduction, and bit-packing. Each encoding technique activated where it helped and skipped where it would not. For timestamps and integer counters, this approach was remarkably effective. For floating-point gauges, it could find almost nothing to work with.


The reason is how they’re stored. To support range queries, Elasticsearch stores floating-point values as integers that preserve numeric ordering. A change of 0.01 in a CPU percentage reading translates to a jump of trillions in that integer space. The codec sees those large jumps and has no strategy to further reduce their footprint. Storage stays near the original eight bytes per value.


The codec was doing the right thing with the representation it had, but the latter was chosen for querying, not compression, and the goals conflict at the bit level.


### The cost of a fixed format


A compression stage for floating-point values was already on the roadmap, so the interesting part wasn’t the algorithm. The obstacle was architectural.


The previous codec baked its compression approach into the storage format. Adding a new encoding meant changing the meaning of existing bytes on disk, which forced a format migration, a rollout that can last weeks or months in large production clusters. Over time, that migration burden constrains codec development itself. The question stops being *Is this a good compression idea?* and becomes *Is it worth another format migration?* That rigidity limits the cadence of codec evolution and leads to missed compression improvements.


### The right encoding without configuration


` ES95` solves this at the architecture level for time-series indices. Each field's encoding is no longer baked into the format. It is selected automatically at write time, based on what the field mapping already declares: the field's name, its data type, and its metric role. Timestamps are encoded differently than counters. Counters are encoded differently than gauges.` ES95` encodes all of them, and it chooses the right strategy for each.


Users already tell Elasticsearch everything the codec needs to know. The mapping describes the data; the codec chooses the compression strategy.


Compression strategy is a codec concern, not a user concern.


The alternative would have been to expose per-field encoding selection as a configuration parameter, letting users opt in to better compression for specific fields. That would shift the burden of knowing which encoding fits which data type onto those least equipped to make that call and would guarantee that most deployments never see the benefit.` ES95` keeps that decision inside the codec, where it belongs. This matters most in managed and serverless deployments, where users expect the system to automatically make optimal storage decisions.


### The timestamp result nobody planned for


With the adaptive architecture in place, the team set out to ship the planned float-compression algorithm. Before it arrived, the architecture proved itself by substantially improving compression for timestamps.


A time-series index is sorted first by its time-series identifier (` _tsid` constructed by the metric’s dimensions) and then by timestamp within each series. Timestamps on disk aren’t one smooth sequence; there are many smooth sequences laid end to end, one per series, with a large jump at every boundary where one series ends and the next begins.


The codec compresses data in fixed-size blocks without regard to those series boundaries. A block straddling a series boundary holds timestamps from two different series. The jump between them breaks monotonicity, reducing delta encoding effectiveness on blocks spanning different time series. A block that would otherwise compress to near-zero bits per value ended up needing nine or more, because bit-packing encodes every value in a block using the same fixed number of bits, so one large jump sets the cost for all of them.


*Figure 2: SplitDelta preserves monotonic sequences by splitting blocks at time-series boundaries.*


In the ideal case, every block belongs to a single series: timestamps increase at near-constant intervals, delta encoding captures the regularity, and bit-packing compresses the result to near-zero bits per value. With few series, boundary blocks are rare and the overhead barely registers. On an observability cluster ingesting millions of documents across thousands of series, that changes. The cost scales along two dimensions: series count and data density. More series means more boundary events; sparser series means multiple jumps packed into single blocks. In high-churn environments, both compound, and boundary blocks accumulate into a standing tax on the most-read field in any time-series workload. It’s why` @timestamp` storage grew faster than the data that produced it.


The fix was to detect series boundaries and treat each run as its own independent sequence. Instead of trying to encode across the jump between two series, which forces every value in the block to pay the storage cost of that one large jump, each run is compressed on its own terms. The boundary simply becomes a seam: the jump is never seen by the encoder on either side.


That encoding is called` SplitDelta` .` @timestamp` and monotonic long counters now use it by default. No format change. No migration. Existing segments retain legacy encoding.


On the high-cardinality[Elasticsearch Rally](https://github.com/elastic/rally) benchmark, that single unplanned encoding cut counters storage by 20%–30% and` @timestamp` storage by 92.4%, from 1.03 GB to 79 MB. Gigabytes to megabytes, and no, that isn’t a typo.


The pluggable pipeline had already paid for itself.` SplitDelta` , which wasn’t part of the original plan, slotted in without a format change or migration before ALP even shipped.


### ALP: recovering the decimal that was always there


Most floating-point metrics can be expressed as short decimals with no loss of accuracy: CPU utilization at` 23.47` , load average at` 1.15` .[ALP](https://dl.acm.org/doi/10.1145/3626717) (Adaptive Lossless floating-Point compression) recovers that decimal structure from the floating-point representation, converting values into integers that the existing pipeline already handles well.` ES95` feeds ALP's output into the same mature integer compression pipeline used for timestamps and counters, extracting additional savings rather than treating ALP as a standalone encoding. Values that don’t fit ALP's model (such as irregular high-precision floats or special values) fall back to direct bit-packing or the original representation without degrading the rest of the block.


*Figure 3: ALP recovers decimal structure from floating-point values so they can be compressed as integers.*


` ALP` lets Elasticsearch treat floating-point metrics according to the structure they actually contain rather than the binary representation they happen to use. It’s applied automatically to double-valued gauge fields, selected by field type and metric role, through exactly the door the architecture had built for it.


### What the numbers say


Here’s what the[Elasticsearch Rally](https://github.com/elastic/rally) benchmark looks like on a high-cardinality workload containing 2.26 billion data points. Results are from an internal` tsdb-metricsgen` benchmark.


**Field or metric**


**Storage reduction (%)**


` @timestamp`


**−92.4%**


` cpu.load_average.5m`


**−74.3%**


` system.cpu.utilization`


**−63%**


` memory.utilization`


**-19%**


Total doc values


**−33.6%**


That overall 33.6% reduction deserves context.


A time-series index contains more than metrics. Every data point also carries the labels that identify the series: host names, IP addresses, regions, container IDs. Those dimension fields are stored as keywords.` ES95` doesn’t target dimensions.


On this benchmark, two dimension fields,` host.ip` and` host.mac` , accounted for 44% of doc-values storage after` ES95` ran. The 33.6% total reflects that mix. The per-field breakdown is the honest picture. Compression for dimension fields is an active area of work.


The per-field variation is the most convincing result. Some gauges shrank by nearly three quarters, while others moved by less than a fifth. That spread is direct evidence that` ES95` matches compression to the structure actually present in each field. A fixed encoding treats every field identically and misses most of those wins.


### Better compression without extra configuration


The storage reductions from` SplitDelta` and` ALP` are the most visible results of` ES95` . The more consequential result is the architecture that produced them.


Before` ES95` , every new compression technique required a format evolution. That reality shaped which ideas were practical to pursue. Today, new encodings become implementation decisions inside the codec rather than migration projects. Existing data never needs to move, and users gain better compression on newly written data simply by upgrading Elasticsearch.` SplitDelta` and` ALP` are the first encodings to benefit from this architecture. They will not be the last.


Asking users to choose compression algorithms would only duplicate information Elasticsearch already has. There are no per-field compression parameters to tune, and no expert knowledge is required to get good storage efficiency. Different fields get different strategies because` ES95` understands what kind of data each field contains, not because a user configured it. As the codec evolves, those decisions evolve with it. The API does not.


In Elasticsearch Serverless, good defaults are part of the product. Users expect the system, not configuration, to make storage decisions.` ES95` is designed to honor that expectation: encoding that starts right and gets better over time.


### The compression was always there


` ES95` establishes a new standard for how time-series codec evolution works. New encodings become implementation decisions, not migration projects. Users get better compression on newly written data with every Elasticsearch upgrade.


The compression was already in the data.` ES95` just removed what was hiding it.
