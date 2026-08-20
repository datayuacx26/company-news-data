---
schema_version: "1.0.0"
document_id: "b5e013944b0d895a4f6df828fab4d8f7f3727c72f56060eee68229b410680e2e"
company_key: "yc-sync-2"
company: "sync."
source_id: "yc-sync-2-news-import-712131a3f47a"
canonical_url: "https://sync.so/blog/lipsync-at-scale-introducing-the-batch-api/"
published_at: "2025-08-11T17:54:14+00:00"
first_seen_at: "2026-07-22T15:25:00.668099+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:d193be86022a8080559e4293ac9ae0cae7e762737dda5a3a8be04a16f0ae83ff"
---

# Lipsync at Scale: Introducing the Batch API

Today the sync. labs team is excited to announce the launch of our batch processing API endpoint. You can now run multiple AI lipsync generations in parallel, up to 500 lip sync jobs in a single request using a simple JSONL file.


Teams trying to lip sync at scale have long struggled with this exact problem: efficiently syncing and testing hundreds of videos at once without standing up custom orchestration. With the new batch lipsync API, personalized video campaigns,[content localization](https://sync.so/blog/video-localization/) , multilingual marketing, AI dubbing pipelines, and any other workflow that needs lip sync at high volume can now be processed in one call. A/B test hundreds of video variants at once, or push a single campaign across hundreds of languages with ease.


Batch processing works with all of our latest lipsync models, returns per-job status and webhook notifications, and is built for production-grade throughput, so you can move from prototype to scale without rewriting your pipeline.


Learn more about this new capability and how to call it in our[batch processing API docs](https://sync.so/docs/api-reference/guides/batch-processing) .
