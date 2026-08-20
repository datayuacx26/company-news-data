---
schema_version: "1.0.0"
document_id: "46b78224d253dd4f75e6e53f796945a1ebf5813ab143ec9e423b20b3cb0e0470"
company_key: "yc-lancedb"
company: "LanceDB"
source_id: "yc-lancedb-news-import-bc01535eeacf"
canonical_url: "https://www.lancedb.com/blog/newsletter-march-2026"
published_at: null
first_seen_at: "2026-07-24T14:00:34.339937+00:00"
fetched_at: "2026-07-28T21:37:36.757263+00:00"
content_hash: "sha256:7b7d70542422ff478dd470ac2b75710bbd66986f4654fc3f2900f4ef3acaee87"
---

# 📄 Lance Blob V2, 🤗 Upload Lance Datasets to HF Hub, 🦞 LanceDB for OpenClaw's Memory

## **📄 Lance Blob V2: Making Multimodal Data a First-Class Citizen in the Lakehouse**


Lance Blob V2 introduces four storage semantics—inline, packed, dedicated, and external—allowing Lance to automatically optimize how data is stored based on size and access pattern. This removes the need to choose between efficient small reads and avoiding large rewrite costs.


By separating storage layout decisions at the format layer, datasets can efficiently handle everything from KB-scale thumbnails to multi-GB videos. The result is fewer unnecessary rewrites, better locality for small data, and scalable access patterns for large blobs.


[Read more →](https://www.lancedb.com/blog/lance-blob-v2)


## **🤗 A Guide to Uploading Lance Datasets on the Hugging Face Hub**


You can now upload a Lance dataset—including data, indexes, and versions—directly to Hugging Face and query it via hf:// without downloading. Vector search, full-text search, SQL, and nested filtering are all supported out of the box.


Updates are incremental: new columns like embeddings or labels can be added without rewriting existing data. This makes it practical to evolve datasets over time while preserving existing blobs and indexes.


[Read more →](https://www.lancedb.com/blog/upload-lance-datasets-to-hf-hub)


## **🦞 Why LanceDB Is the Most Natural Memory Layer for OpenClaw**


OpenClaw agents persist memory across sessions, and LanceDB is emerging as the default storage layer for that memory. It runs embedded (no service required) and stores embeddings, metadata, and indexes together in a single table.


This enables unified querying—vector, full-text, and structured filtering—over agent memory. Combined with append-friendly storage, it matches how agents accumulate and retrieve knowledge over time.


[Read more →](https://www.lancedb.com/blog/openclaw-lancedb-memory-layer)


## **📖 Also Published This Month**


- [Lance File Format 2.2: Taming Complex Data](https://www.lancedb.com/blog/lance-file-format-2-2-taming-complex-data)
- [Memory for OpenClaw: From Zero to LanceDB Pro](https://www.lancedb.com/blog/openclaw-memory-from-zero-to-lancedb-pro)
- [OpenClaw + LanceDB + Seed 2.0: Turn Visual Ideas into Reality, Fast!](https://www.lancedb.com/blog/openclaw-lancedb-seed2)


## **📅 Upcoming Events**


**Data Engineering Open Forum** – April 16 in SF


Jack Ye and Pablo Delgado, ML Engineer at Netflix, will present on multimodal feature engineering at scale with Netflix, covering how LanceDB supports large-scale storage, retrieval, and dataset workflows.


🗓️ Session details:[https://www.dataengineeringopenforum.com/?session=powering-netflixs-multimodal-feature-engineering-at-scale#agenda](https://www.dataengineeringopenforum.com/?session=powering-netflixs-multimodal-feature-engineering-at-scale#agenda)


🔗 Register link:[https://luma.com/deof2026?utm_source=li-speaker](https://luma.com/deof2026?utm_source=li-speaker)


[TokioConf 2026](https://www.tokioconf.com/speakers) **** – April 20 in Portland


Weston Pace & Lu Qiu will share a deep dive into optimizing a Rust-native search database, focusing on I/O scheduling, async profiling, and achieving storage-level performance.


🗓️ Conference schedule:[https://www.tokioconf.com/schedule](https://www.tokioconf.com/schedule)


## **🏗️ LanceDB Enterprise Updates**


Feature Description


New CLI preflight command New preflight command in the LanceDB enterprise CLI validates deployments and gives build information.


Faster vector index prewarm Prewarming vector indexes is now faster and more efficient. Instead of using random queries, all partitions are loaded deterministically.


Parallel insert calls New multipart write APIs allow inserting data from multiple parallel streams. The SDK clients will automatically use parallel requests when they detect that the input is sufficiently large.


Feature Engineering: Azure support LanceDB Enterprise Feature Engineering now supports Azure-based deployments using Azure blob storage and AKS.


Feature Engineering: Auto-backfill New option to automatically trigger backfills on specified computed columns when new data arrives.


Feature Engineering: Added declarative error handling conditions Adds new declarative handling of OOMs, worker crashes and timeouts into the error handling framework.


## **🌟 Open Source Releases**


Feature Description


Lance v3.0.0 - 4.0.0
[Release notes](https://github.com/lance-format/lance/releases)


- Major indexing + query performance gains: faster FTS (~50% with WAND), SIMD-optimized vector search, and reduced indexing time and memory ([#6241](https://github.com/lance-format/lance/pull/6241) ,[#5923](https://github.com/lance-format/lance/pull/5923) ,[#6174](https://github.com/lance-format/lance/pull/6174) )
- Expanded transactional model with atomic multi-table transactions and improved conflict handling ([#6173](https://github.com/lance-format/lance/pull/6173) ,[#6003](https://github.com/lance-format/lance/pull/6003) )
- Blob V2 + storage evolution with external blob support and improved layout semantics ([#6066](https://github.com/lance-format/lance/pull/6066) ,[#6064](https://github.com/lance-format/lance/pull/6064) )
- Distributed + remote improvements including ANN prewarming and object store integrations ([#6269](https://github.com/lance-format/lance/pull/6269) ,[#6090](https://github.com/lance-format/lance/pull/6090) ,[#6192](https://github.com/lance-format/lance/pull/6192) )
- Format and scan performance improvements (up to 3× faster scans) ([#5982](https://github.com/lance-format/lance/pull/5982) ,[#6016](https://github.com/lance-format/lance/pull/6016) )


LanceDB v0.30
[Release notes](https://github.com/lancedb/lancedb/releases)


- Parallel inserts for local and remote tables (multipart writes) ([#3062](https://github.com/lancedb/lancedb/pull/3062) ,[#3071](https://github.com/lancedb/lancedb/pull/3071) )
- Type-safe expression builder APIs (Python + Rust) ([#3150](https://github.com/lancedb/lancedb/pull/3150) ,[#3032](https://github.com/lancedb/lancedb/pull/3032) )
- Expanded query support: Float16/64 + Uint8 vectors, hybrid search improvements, explain plans ([#3193](https://github.com/lancedb/lancedb/pull/3193) ,[#3006](https://github.com/lancedb/lancedb/pull/3006) )
- Remote table capabilities: index params, prewarming, schema caching, background updates ([#3087](https://github.com/lancedb/lancedb/pull/3087) ,[#3110](https://github.com/lancedb/lancedb/pull/3110) ,[#3015](https://github.com/lancedb/lancedb/pull/3015) ,[#3021](https://github.com/lancedb/lancedb/pull/3021) )
- Improved ingestion APIs: RecordBatch support, new writer path, dict→SQL struct updates ([#2948](https://github.com/lancedb/lancedb/pull/2948) ,[#3029](https://github.com/lancedb/lancedb/pull/3029) ,[#3089](https://github.com/lancedb/lancedb/pull/3089) )


lance-graph v0.5.3 - v0.5.4
[Release notes](https://github.com/lancedb/lance-graph/releases)


- Vector-first ANN integration into Cypher for hybrid graph + vector reranking ([#140](https://github.com/lancedb/lance-graph/pull/140) )
- Expanded query capabilities with parameterized queries and node-return support ([#125](https://github.com/lancedb/lance-graph/pull/125) ,[#142](https://github.com/lancedb/lance-graph/pull/142) )


lance-duckdb v0.5.2 - v0.5.3
[Release notes](https://github.com/lance-format/lance-duckdb/releases)


- Full SQL surface including MERGE INTO and dataset versioning ([#155](https://github.com/lance-format/lance-duckdb/pull/155) ,[#162](https://github.com/lance-format/lance-duckdb/pull/162) )
- Improved query execution with index-aware planning and deferred materialization ([#169](https://github.com/lance-format/lance-duckdb/pull/169) ,[#175](https://github.com/lance-format/lance-duckdb/pull/175) )


lance-spark v0.3.0
[Release notes](https://github.com/lance-format/lance-spark/releases)


- Deeper Spark integration: Spark 4.1 support, MERGE/replace workflows, and index visibility ([#299](https://github.com/lance-format/lance-spark/pull/299) ,[#251](https://github.com/lance-format/lance-spark/pull/251) ,[#282](https://github.com/lance-format/lance-spark/pull/282) )
- Improved performance with fragment pruning, caching, and filter pushdown ([#311](https://github.com/lance-format/lance-spark/pull/311) ,[#261](https://github.com/lance-format/lance-spark/pull/261) ,[#297](https://github.com/lance-format/lance-spark/pull/297) )


## **🫶 Community Contributions**


Thank you to contributors from Netflix, Uber, Bytedance, Huawei, Baidu, and Linkedin for improvements across storage, indexing, query execution, distributed processing, and ecosystem integrations in LanceDB, Lance, lance-spark, and other products


Notable contributions this month:


- [@beinan](https://github.com/beinan) — Enabled vector-first ANN integration in lance-graph, bringing hybrid graph + vector reranking into Cypher workflows
- [@Mesut-Doner](https://github.com/Mesut-Doner) — Introduced type-safe expression APIs in Rust, improving composability and safety of query construction
- [@pratik0316](https://github.com/pratik0316) — Added type-safe expression builder API in Python, aligning query ergonomics across SDKs
- [@nyl3532016](https://github.com/nyl3532016) — Extended vector search capabilities with prefiltering support across Spark and core query paths
- [@burlacio](https://github.com/burlacio) — Expanded cloud storage support with Azure ADLS Gen2 (abfss://) integration across the ecosystem
- [@XuQianJin-Stars](https://github.com/XuQianJin-Stars) — Added atomic multi-table transaction support, enabling more reliable multi-dataset workflows
- [@yingjianwu98](https://github.com/yingjianwu98) — Improved storage efficiency with encoding and compression enhancements for complex data layouts
- [@HemantSudarshan](https://github.com/HemantSudarshan) — Added Levenshtein-based schema suggestions, improving developer experience in query debugging
- [@LuciferYang](https://github.com/LuciferYang) — Improved Spark execution reliability and performance with fixes across scan planning and Arrow integration
- [@mrncstt](https://github.com/mrncstt) — Enabled structured updates via dict→SQL conversion, improving usability of update workflows


A heartfelt thank you to our community contributors of Lance and LanceDB this past month:


[@VedantMadane](https://github.com/VedantMadane) •[@pratik0316](https://github.com/pratik0316) •[@lennylxx](https://github.com/lennylxx) •[@majiayu000](https://github.com/majiayu000) •[@myandpr](https://github.com/myandpr) •[@marca116](https://github.com/marca116) •[@dask-58](https://github.com/dask-58) •[@Mesut-Doner](https://github.com/Mesut-Doner) •[@mrncstt](https://github.com/mrncstt) •[@omair445](https://github.com/omair445) •[@veeceey](https://github.com/veeceey) •[@Abhisheklearn12](https://github.com/Abhisheklearn12) •[@ChinmayGowda71](https://github.com/ChinmayGowda71) •[@Zelys-DFKH](https://github.com/Zelys-DFKH) •[@BillionClaw](https://github.com/BillionClaw) •[@octo-patch](https://github.com/octo-patch) •[@ddupg](https://github.com/ddupg) •[@yingjianwu98](https://github.com/yingjianwu98) •[@xloya](https://github.com/xloya) •[@zhangyue19921010](https://github.com/zhangyue19921010) •[@HemantSudarshan](https://github.com/HemantSudarshan) •[@nyl3532016](https://github.com/nyl3532016) •[@fangbo](https://github.com/fangbo) •[@ndpvt-web](https://github.com/ndpvt-web) •[@XuQianJin-Stars](https://github.com/XuQianJin-Stars) •[@burlacio](https://github.com/burlacio) •[@wojiaodoubao](https://github.com/wojiaodoubao) •[@fenfeng9](https://github.com/fenfeng9) •[@dardourimohamed](https://github.com/dardourimohamed) •[@cheungxi](https://github.com/cheungxi) •[@cijiugechu](https://github.com/cijiugechu) •[@erandagan](https://github.com/erandagan) •[@Gallardot](https://github.com/Gallardot) •[@wombatu-kun](https://github.com/wombatu-kun) •[@FarmerChillax](https://github.com/FarmerChillax) •[@shepmaster](https://github.com/shepmaster) •[@majin1102](https://github.com/majin1102) •[@ztorchan](https://github.com/ztorchan) •[@yanghua](https://github.com/yanghua) •[@touch-of-grey](https://github.com/touch-of-grey) •[@bryanck](https://github.com/bryanck) •[@fecet](https://github.com/fecet) •[@apoc](https://github.com/apoc) •[@rahil-c](https://github.com/rahil-c) •[@AndreaBozzo](https://github.com/AndreaBozzo) •[@durch](https://github.com/durch) •[@LuciferYang](https://github.com/LuciferYang) •[@dik654](https://github.com/dik654) •[@chyyran](https://github.com/chyyran) •[@beinan](https://github.com/beinan) •[@ChunxuTang](https://github.com/ChunxuTang) •[@aheev](https://github.com/aheev) •[@leiyuou](https://github.com/leiyuou) •[@jja725](https://github.com/jja725) •[@jiaoew1991](https://github.com/jiaoew1991) •[@a-sane](https://github.com/a-sane) •[@ivscheianu](https://github.com/ivscheianu) •[@jtuglu1](https://github.com/jtuglu1) •[@mikewhb](https://github.com/mikewhb)


## **🤝 Lance Community Sync Recap**


This month’s community syncs focused on the Lance 3.0 and upcoming 4.0 releases, including adoption of the 2.2 file format and ongoing improvements to indexing and query performance. Ecosystem momentum continues to build with Lance as a core DuckDB extension, a new PrestoDB connector, and early discussion of distributed vector indexing with significant build speed improvements.


The next Lance Community Sync will take place on **Thursday, April 9** .


- 📬[Subscribe to the Lance mailing list to receive the meeting invite →](https://groups.google.com/a/lance.org/g/dev)
- 📄[Add discussion topics to the meeting notes →](https://docs.google.com/document/d/1cP058pJLVjj39DGaFV5yHI6qyWBwp1B2YKyIwXzYlAI/edit?usp=sharing)
- 📺[Watch previous recordings →](https://www.youtube.com/playlist?list=PLQysAafL04jUgOj7j6mCE5fcZEBB9Amu9)
