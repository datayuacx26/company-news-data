---
schema_version: "1.0.0"
document_id: "499dd649c86aed96bad9f303679719a2ed3beaa470bfa74e57abbc29e5073f3a"
company_key: "yc-zeroentropy"
company: "ZeroEntropy"
source_id: "yc-zeroentropy-news-import-2ae9b0afcf30"
canonical_url: "https://zeroentropy.dev/articles/equall-improves-legal-document-structuring-and-retrieval-accuracy-with-zeroentropy/"
published_at: "2025-10-09T00:00:00+00:00"
first_seen_at: "2026-07-22T21:03:20.034441+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:537d66970be8746296a680717bc10856a25ab505f4e99cb8fc5a998eb97f9a0e"
---

# Equall Improves Legal Document Structuring and Retrieval Accuracy with ZeroEntropy

TL;DR


With[ZeroEntropy](https://www.zeroentropy.dev/articles/equall-improves-legal-document-structuring-and-retrieval-accuracy-with-zeroentropy/zeroentropy.dev) ’s **zerank-1[reranker](https://www.zeroentropy.dev/concepts/reranker/)** , Equall achieved significantly higher[recall](https://www.zeroentropy.dev/concepts/recall-at-k/) and[precision](https://www.zeroentropy.dev/concepts/precision/) in their structured extraction workflows, leading to **fewer false negatives and faster verification cycles** across thousands of VC and corporate legal documents.


### **Company and Highlights**


[Equall](https://www.zeroentropy.dev/articles/equall-improves-legal-document-structuring-and-retrieval-accuracy-with-zeroentropy/equall.ai) is a frontier AI company building end-to-end systems to solve complex, real-world legal problems. Their products help lawyers and corporations **evaluate and manage legal risk** through automation and live data analysis.


Equall’s flagship platform processes thousands of **VC and corporate documents** — from certificates of incorporation to board consents — extracting structured information to help EC/VC lawyers verify compliance and detect inconsistencies in real time.


With[ZeroEntropy](https://www.zeroentropy.dev/articles/equall-improves-legal-document-structuring-and-retrieval-accuracy-with-zeroentropy/zeroentropy.dev) ’s **zerank-1 reranker** , Equall achieved significantly higher recall and precision in their structured extraction workflows, leading to **fewer false negatives and faster verification cycles** .


- **Domain** : Legal document processing
- **Use case** : Structured extraction and deep document search
- **Stack** : Internal extraction logic + ZeroEntropy reranker
- **Integration** : Simple API swap
- **Key metrics** : Recall@5 ↑, Recall@10 ↑, latency <100 ms


### **Problem**


Equall’s system parses hundreds of documents per deal. Those corporate legal documents introduce a uniquely challenging retrieval problem. They tend to be long, multi-part, and interdependent — often combining amendments, exhibits, annexes, and cross-references that evolve across corporate lifecycles. Understanding these relationships and retrieving the right context at the right time is critical: missed or mis-ranked references can lead to incomplete extractions or inconsistent lineage tracking in a domain where **accuracy is non-negotiable** .


Their extraction pipeline needs to:


- Retrieve the **right document segments** for each target field (e.g. dates, vesting terms, investors rights etc.. board resolutions, agreement dates).
- Maintain **high recall** across complex, domain-specific phrasing.
- Deliver **consistent latency** suitable for real-time lawyer workflows.


Off-the-shelf[vector search](https://www.zeroentropy.dev/concepts/dense-retrieval/) or large-[embedding](https://www.zeroentropy.dev/concepts/embedding/) -only systems struggled in this context — noisy retrievals, weak[calibration](https://www.zeroentropy.dev/concepts/score-calibration/) , and insufficient context handling.


### **Approach**


With ZeroEntropy’s **zerank-1** reranker, Equall integrated a stronger retrieval layer directly into its existing extraction pipeline. The swap was simple at the API level but delivered immediate gains in both accuracy and efficiency — higher recall and precision, coupled with latency below 100 ms. Because better recall allows for tighter and more relevant context windows, downstream models now operate faster and with greater consistency across complex document sets.


They benchmarked the reranker internally using annotated fields from real-world datarooms, evaluating **recall@k** across multiple extraction types — from simple/generic dates to complex and domain-specific clauses including accelerations, valuation caps, discount rate etc.


**Implementation was frictionless:**


#### Feed existing candidates


Existing embedding candidates fed into ZeroEntropy’s API.


#### Rerank top candidates


ZeroEntropy reranked the top candidates per field.


#### Minimal code changes


Minimal code changes, immediate lift in quality.


#### Sufficient rate limits


Rate limits were quite subsequent for our use case.


### **Results**


#### **Accuracy**


- **Consistent recall@5 and recall@10 gains** across nearly all document types.
- **Improved contextual understanding** in ambiguous cases (e.g., multiple annexes or cross-referenced terms).
- Outperformed internal baselines and commercial rerankers tested.
- No tuning needed — the API works out of the box with consistent results.


##### **Latency and Stability**


Metric Value


p50 latency 75 ms


p90 latency 130 ms


p99 latency 240 ms


Fully stable in production workloads with gradual rollout across extraction schemas.


##### **Product Impact**


- Reduced manual review time for lawyers.
- Increased trust in automated extraction.
- Enabled downstream “deep research” features powered by accurate retrieval.


### **Decision**


Equall is now expanding ZeroEntropy’s use beyond reranking, testing **ZeroEntropy newest models** and **end-to-end search pipelines** .
