---
schema_version: "1.0.0"
document_id: "52c85c3fee6fb921318ce8bc413cd7e77dbd38519dbad88f7302270ea446f91f"
company_key: "yc-chonkie"
company: "Chonkie"
source_id: "yc-chonkie-atom-7829c83b5d35"
canonical_url: "https://github.com/feyninc/chonkie/releases/tag/v1.6.6"
published_at: "2026-05-13T16:34:07+00:00"
first_seen_at: "2026-07-24T22:18:14.501867+00:00"
fetched_at: "2026-08-20T03:16:58.267707+00:00"
content_hash: "sha256:f719a8ce16cfc895bcc931d9dba708bfc8d63b6fd20f593d33eb0c471e5db4c1"
---

# v1.6.6

## Highlight


🔥 CodeChunker update and huge performance speedup.
bechmarks below using[semble](https://github.com/MinishLab/semble) as a reference:


Metric Baseline Semble (uring the current version) Delta % Change


**NDCG@10** (search quality) 0.854 0.856 +0.002 **+0.2%**


**p50 latency** 7.19 ms 6.16 ms -1.03 ms **-14.3%**


**p90 latency** 22.15 ms 18.57 ms -3.57 ms **-16.1%**


**p95 latency** 25.12 ms 20.60 ms -4.51 ms **-18.0%**


**p99 latency** 28.69 ms 23.70 ms -4.99 ms **-17.4%**


**Index time** 5,138 ms 2,853 ms -2,285 ms **-44.5%**


## What's Changed


- update code chunker by[@chonk-lain](https://github.com/chonk-lain) in[#587](https://github.com/feyninc/chonkie/pull/587)


## Dependencies


- chore(deps): bump urllib3 from 2.6.3 to 2.7.0 by[@dependabot](https://github.com/dependabot) \[bot\] in[#584](https://github.com/feyninc/chonkie/pull/584)
- chore(deps): bump langchain-core from 1.2.31 to 1.3.3 by[@dependabot](https://github.com/dependabot) \[bot\] in[#581](https://github.com/feyninc/chonkie/pull/581)
- chore(deps-dev): update turbopuffer requirement from ~=1.0 to >=1,<3 by[@dependabot](https://github.com/dependabot) \[bot\] in[#585](https://github.com/feyninc/chonkie/pull/585)
- chore(deps): bump mako from 1.3.11 to 1.3.12 by[@dependabot](https://github.com/dependabot) \[bot\] in[#579](https://github.com/feyninc/chonkie/pull/579)
- chore(deps): bump authlib from 1.6.11 to 1.6.12 by[@dependabot](https://github.com/dependabot) \[bot\] in[#588](https://github.com/feyninc/chonkie/pull/588)


**Full Changelog** :[v1.6.5...v1.6.6](https://github.com/feyninc/chonkie/compare/v1.6.5...v1.6.6)
