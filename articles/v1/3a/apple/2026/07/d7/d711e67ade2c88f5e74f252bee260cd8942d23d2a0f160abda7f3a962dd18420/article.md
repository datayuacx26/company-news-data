---
schema_version: "1.0.0"
document_id: "d711e67ade2c88f5e74f252bee260cd8942d23d2a0f160abda7f3a962dd18420"
company_key: "apple"
company: "Apple"
source_id: "apple-news-import-9ba92da28538"
canonical_url: "https://machinelearning.apple.com/research/multilingual-semantic-retrieval"
published_at: null
first_seen_at: "2026-07-21T07:16:20.305331+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:e8c9f6a22036bad0ee49501874932ae1ce09ecd0a75810000e4ca79d94a7563e"
---

# Multilingual Semantic Retrieval for Apple Music Search

[View publication](https://arxiv.org/abs/2607.10239)


Apple Music serves listeners across 150+ storefronts in dozens of languages, with a catalog that grows by hundreds of thousands of new tracks daily. At this scale, search recall on misspelled, transliterated, and cross-lingual queries becomes a dominant driver of session quality, particularly for tail queries that account for the majority of unique queries. We present a multilingual semantic retrieval system built on a 305M-parameter Siamese bi-encoder fine-tuned from GTE-multilingual-base with curriculum-scheduled multi-objective training. The model is integrated into the search stack via a hybrid retrieval architecture that blends dense nearest-neighbor results with the existing token-based index using quantile distribution matching, enabling deployment without retraining downstream rankers. Offline, the model achieves a 69% relative improvement in Hit@10 over GTE-multilingual-base. In a worldwide online A/B test, the system delivers a 2.28% relative conversion-rate (CR) lift overall, an 86% reduction in the no-result rate, and gains across every storefront with no observed regressions. The improvement is concentrated where it is needed most: tail queries see a 7.93% relative CR lift, compared with 0.89% for mid-frequency queries and 0.14% for head queries—evidence that semantic retrieval improves recall on hard queries without disturbing well-served popular ones. To our knowledge, this is one of the largest search-quality improvements deployed on the platform.


- * Equal contribution
