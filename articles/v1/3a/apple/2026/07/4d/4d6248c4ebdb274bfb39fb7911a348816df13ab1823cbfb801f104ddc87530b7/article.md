---
schema_version: "1.0.0"
document_id: "4d6248c4ebdb274bfb39fb7911a348816df13ab1823cbfb801f104ddc87530b7"
company_key: "apple"
company: "Apple"
source_id: "apple-news-import-9ba92da28538"
canonical_url: "https://machinelearning.apple.com/research/personalizing-incremental-video-search"
published_at: null
first_seen_at: "2026-07-21T07:16:20.305331+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:bac01363044fd892463c0184cdcdbebbd52c4a15f23f4424fb1aad3320c05060"
---

# Personalizing Incremental Video Search with Hybrid Text and ID Embeddings

[View publication](https://arxiv.org/abs/2607.13493)


Incremental video search requires high-quality ranking after each keystroke, where intent is often underspecified (e.g., 1–3 character prefixes). We present a personalization system for Apple TV search that combines complementary semantic and collaborative signals at ranking time. Our approach learns two item embedding spaces: (i) a text-based multilingual encoder (TextEmb) fine-tuned on co-engagement triplets via contrastive learning, and (ii) an ID-based collaborative embedding model (IdEmb) trained on interaction-derived positives. At serving time, we construct user representations from recent watch history and inject text- and ID-based user–item cosine similarities into a pairwise XGBoost ranker. We evaluate the system with temporally held-out offline datasets and a three-week online controlled experiment. Offline, for sessions with user history, the personalized ranker improves NDCG@10 by 2.99% and MRR by 3.30% over the non-personalized baseline. Crucially, slice analyses show that personalization is most needed in incremental search, where intent is still forming: on ambiguous prefix queries (1–3 characters), NDCG@10 lift is +8.63%, versus only +1.46% on longer, more fully specified queries. Users with longer watch histories benefit more from personalization than newer users: NDCG lift rises from +2.13% for users with 1–5 history items to +4.37% for users with 51–100. This larger lift occurs even though baseline relevance is lower for long-history cohorts (NDCG@10 drops from 0.733 to 0.680), indicating that personalization adds the most value where default ranking underperforms. Online, treatment yields statistically significant gains of +1.14% tap-through rate and +1.23% conversion rate, with a 2.91% improvement in converted-item rank position. We further analyze coverage–precision trade-offs between semantic and collaborative embeddings through ablations isolating each signal, and evaluate embedding quality on a held-out corpus with LLM-judged similarity labels to reduce click/exposure bias.
