---
schema_version: "1.0.0"
document_id: "99eed69e743d8cdf424a759a9200715351239e007dc37b16fd31ea518d6b5b6e"
company_key: "yc-tiptap"
company: "Tiptap"
source_id: "yc-tiptap-news-import-30112aa6d3bf"
canonical_url: "https://tiptap.dev/blog/release-notes/tiptaps-new-pricing-model-is-live"
published_at: "2025-06-06T00:00:00+00:00"
first_seen_at: "2026-07-22T16:45:40.643427+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:e1134bb9f458067f0060dbf0fce8a74884249f00ac360e9b55b99fcfac7617f9"
---

# Tiptap’s new pricing model is live

## **What’s changed**


We’ve moved from a mix of pricing inputs, like concurrent + monthly connections, and feature-based tiers, to a simple, document-based model.


From now on, you will be billed based on the **paid features** you use and the **number of documents** you store in Tiptap Cloud.


### Document-based


A document is anything you save to Tiptap Cloud to use features like collaboration, version history, or comments. If you’re working locally and saving data in your own database, that doesn’t count.


Delete a document from Tiptap Cloud? It stops counting. Simple.


Here’s how this looks in practice:


- **Start** – $49/month → 500 documents
- **Team** – $149/month → 5,000 documents
- **Growth** – $999/month → 50,000 documents
- **Enterprise** – Custom or full on-prem


You can increase your document limit without changing plans.
Check out the new pricing, paid features, and services here:[tiptap.dev/pricing](https://tiptap.dev/archive/pricing-q2-25)


## **Why we made this change**


The old structure was… messy. Pro extensions were free for individual devs but licensed differently for teams. Some were tied to the cloud, some weren’t. The free plan included cloud features even if you never used them. It didn’t feel consistent or easy to explain.


We chose documents as the core metric because that’s where the value is.


### Tiptap Cloud is more than storage


Once a document lives in the system, you can:


- Search it semantically with vectors
- Run backend processing or enrichment
- Do RAG-style queries across all documents
- Enable real-time collaboration and comments
- Access version history or metadata


All of that value builds on the document. So it makes sense to tie pricing directly to them. It’s clear, fair, and scales with your use.


### To make everything simpler


- The **free plan is gone** . We now offer a time-limited trial for new users.
- We’ve[open-sourced 10 formerly paid Pro extensions](https://tiptap.dev/blog/release-notes/were-open-sourcing-more-of-tiptap) under the MIT license.
- Paid plans now focus on **high-value feature bundles** like Content AI, Conversion, Collaboration, and Documents.


You either use Tiptap Cloud and pay based on pro features and usage, or you use Tiptap OSS and everything is free. No more in-between.


## **What this means for you**


- If you’re already on Tiptap Cloud, nothing breaks. We’ll be in touch to help you review usage and adjust if needed.
- If you’ve been building with Tiptap OSS, things just got easier. You don’t have to worry about which extensions are free, which aren’t, and what’s included in what. It’s all clearer now.
- If you’re just getting started: try the platform, explore the docs, and use the trial to see what fits.


Thanks to everyone who helped us shape this new model. We think it makes Tiptap more consistent, more scalable, and a whole lot easier to understand.


As always, we’re listening. Let us know what you think:humans@tiptap.dev
