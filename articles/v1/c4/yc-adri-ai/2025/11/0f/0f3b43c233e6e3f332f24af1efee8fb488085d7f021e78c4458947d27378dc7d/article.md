---
schema_version: "1.0.0"
document_id: "0f3b43c233e6e3f332f24af1efee8fb488085d7f021e78c4458947d27378dc7d"
company_key: "yc-adri-ai"
company: "Adri AI"
source_id: "yc-adri-ai-news-import-4d4ea4e66e6f"
canonical_url: "https://docs.getadri.ai/blog/chromasql/chromasql-preview"
published_at: "2025-11-10T00:00:00+00:00"
first_seen_at: "2026-07-21T04:52:15.508780+00:00"
fetched_at: "2026-07-28T21:27:35.329570+00:00"
content_hash: "sha256:60597291643bb3a9c7f005246b36ef7825d7041cb79232144b51c69bd17c85c2"
---

# ChromaSQL Early Preview

Excited to share an early preview of ChromaSQL, a new SQL-like language that helps ABAP developers access SAP’s tribal knowledge in a few seconds instead of wading through SE11/SE84 and stacks of notes. 🎉


Say a business user asks you to tighten credit limit checks at order entry. Instead of manually digging through your SAP system chasing BAdIs, tables, and all the related artifacts one by one, you just type what you’re after: “credit limit check enhancement.”


```text
SELECT id, distance, metadata.object_type    FROM sap_artifacts    USING EMBEDDING (TEXT 'credit limit check enhancement')    TOPK 5;
```


That one query runs a vector search across your SAP knowledge base and hands back the top five hits with similarity scores. You learn about:


- BAdI implementations like ZSD_CREDIT_CHECK,
- Tables (VBAK, KNKK) you need to regression test,
- CDS views with related logic,
- Even past transports you might have forgotten.


Now you can decide:


- which enhancements to review,
- what downstream integrations (credit exposure reports, for example) could break, and
- exactly what to drop into your change spec or automated checks.


Result: Less time wasted digging through SE11/SE84, fewer missed dependencies, and a clearer rollout plan for every change request.


We’re putting the final polish on the release right now.
