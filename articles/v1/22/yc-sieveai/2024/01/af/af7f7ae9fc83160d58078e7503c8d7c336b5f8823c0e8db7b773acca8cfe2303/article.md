---
schema_version: "1.0.0"
document_id: "af7f7ae9fc83160d58078e7503c8d7c336b5f8823c0e8db7b773acca8cfe2303"
company_key: "yc-sieveai"
company: "sieve"
source_id: "yc-sieveai-news-import-c83e13839616"
canonical_url: "https://usesieve.com/blog/building-global-traffic-datasets-fragmented-port-records"
published_at: "2024-01-01T00:00:00+00:00"
first_seen_at: "2026-07-26T00:04:09.595708+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:4c1a70ee17e4c8cccf3869c5c6b88045a5f09e93d5fad5969c11e0e85fea84f3"
---

# Building Global Traffic Datasets from Fragmented Port Records

## Context


Following global changes in tariff policy, a commodities-focused hedge fund wanted a clearer view of trade traffic using alternative data. The team aimed to build a time-series view of TEU volume globally, but the data was not available as a centralized dataset and had to be sourced and assembled from fragmented port-level records.


## Issues


Manual data collection presented significant challenges:


- **Fragmented sources:** No centralized database existed, so the team had to find TEU records port by port across countries
- **Messy formats:** Data showed up in PDFs, static web pages, and filtered web interfaces - nothing standardized
- **Language barriers:** Source materials varied by language, making extraction and validation harder to scale
- **Inconsistent reporting:** Some ports published total TEUs, while others split volume by load status, trade direction, or both
- **Normalization challenge:** Before anything could be analyzed, all of it had to be mapped into a consistent schema


## sieve solution


sieve built a reliable data pipeline that:


- **Handled any format:** sieve's AI extracted the required TEU data from PDFs, websites, and filtered pages without needing separate workflows
- **Worked across languages:** The same workflow processed source materials across different languages without extraction errors
- **Standardized the data:** We mapped inconsistent reporting styles into a clean schema ready for analysis
- **Preserved traceability:** Every data point was linked back to the exact source table and page it came from


Instead of taking weeks to stitch together fragmented port records by hand, the client received a clean, normalized, and fully traceable dataset ready for analysis in days. This gave the client a proprietary dataset that allowed them to make investment decisions with confidence as tariff conditions evolved.


### Need to build datasets from fragmented sources?


Stop spending weeks collecting and normalizing scattered data. Let sieve handle the extraction and standardization while you focus on analysis.


Contact us athello@usesieve.com to discuss your use case.
