---
schema_version: "1.0.0"
document_id: "f42e908906515f962f24c421775ff828b310f0dcc72f382c5edea4033cf4e551"
company_key: "yc-zeroentropy"
company: "ZeroEntropy"
source_id: "yc-zeroentropy-news-import-2ae9b0afcf30"
canonical_url: "https://zeroentropy.dev/articles/how-assembled-powers-high-quality-ai-customer-support-with-zeroentropy/"
published_at: "2026-02-12T00:00:00+00:00"
first_seen_at: "2026-07-22T21:03:20.034441+00:00"
fetched_at: "2026-07-28T22:20:47.930048+00:00"
content_hash: "sha256:e9a84e4af625cc833cf00261d44d9ce7d87f946ceaa7633fa1e598acef54b5e6"
---

# How Assembled Powers High-Quality AI Customer Support with ZeroEntropy

TL;DR


After integrating ZeroEntropy’s[reranking](https://www.zeroentropy.dev/concepts/reranker/) into their retrieval pipeline and validating it on live production traffic, Assembled migrated to 100% of their reranking volume through ZeroEntropy.


Assembled builds the unified support operations platform trusted by Stripe, Canva, Robinhood, Notion, and hundreds of other companies to manage AI and human agents across chat, email, and voice. With support quality as their top priority, Assembled needed a retrieval layer that could match the stakes, where a wrong answer erodes customer trust, and a missed document means a frustrated end-user.


## The Challenge


Assembled’s[AI agents](https://www.zeroentropy.dev/concepts/agent/) handle customer support across three channels: chat, email, and phone, each with different retrieval demands. Behind each one sits a[hybrid search](https://www.zeroentropy.dev/concepts/hybrid-search/) system combining[dense vector search](https://www.zeroentropy.dev/concepts/dense-retrieval/) with[keyword-based retrieval](https://www.zeroentropy.dev/concepts/sparse-retrieval/) . After both retrieval paths return candidate chunks, the reranker decides which results actually make it to the[LLM](https://www.zeroentropy.dev/concepts/large-language-model/) .


The reranker is the last line of defense before generation. If it gets the ranking wrong, the model[hallucinates](https://www.zeroentropy.dev/concepts/hallucination/) or gives a bad answer, and Assembled’s customers see it immediately.


As Assembled scaled across verticals and use cases, they needed a reranker that was consistently more accurate, particularly in the long tail of domain-specific queries where retrieval failures are hardest to detect.


## Evaluation


Assembled runs one of the more rigorous retrieval evaluation setups in their category. Their golden dataset has been curated over eighteen months across ten to fifteen different customers, with each customer contributing ten to fifty expert-annotated examples. Domain experts review production queries and label the exact source documents the system should have retrieved. The core metric is straightforward: where does the correct source appear in the top-k results, if it appears at all.


On top of this, Assembled maintains regression sets and hallucination benchmarks to catch degradation across model updates.


When evaluating ZeroEntropy, Assembled didn’t rely on offline benchmarks alone. They ran long-term experiments across live production traffic, with an engineer monitoring performance over weeks to verify that ZeroEntropy’s accuracy held steady at the level they had originally evaluated.


## Results


After validating performance through production A/B experiments, Assembled migrated 100% of their reranking traffic to ZeroEntropy, moving from an initial partial rollout to full production deployment.


#### Consistent accuracy across channels


ZeroEntropy reranking performs reliably across Assembled’s three distinct retrieval paths: real-time chat, latency-sensitive phone, and the heavy-duty email pipeline that scores every article in a customer’s knowledge base.


#### Production-validated performance


Long-running experiments on live traffic confirmed that ZeroEntropy matched or exceeded the accuracy benchmarks set during initial evaluation, giving Assembled the confidence to consolidate their entire reranking volume onto a single provider.


#### Domain-specific improvements with zerank-2 prompting


Assembled leverages zerank-2’s[instruction-following capabilities](https://www.zeroentropy.dev/concepts/instruction-following-reranker/) to tailor reranking behavior for specific customer verticals. By prompting the reranker with domain context, the team improved retrieval relevance without any model changes, particularly on queries where the semantic intent was difficult to capture from keywords alone.


## What’s Next


Assembled and ZeroEntropy are exploring several areas for deeper collaboration. Assembled’s embedding layer can still be further optimized, and the team is evaluating ZeroEntropy’s upcoming models as a potential upgrade.


On the retrieval intelligence side, Assembled is building feedback loops that identify knowledge gaps, surfacing cases where a customer’s knowledge base simply doesn’t contain the right documents, and prompting them to create better content.


### About Assembled


Assembled is the unified support operations platform that combines workforce management with AI-powered issue resolution. Founded by three early Stripe employees, the company has raised $70.7M and serves customers including Stripe, Etsy, DoorDash, and Robinhood. Assembled’s AI agents automate over 50% of support tickets while improving customer satisfaction. Learn more at assembled.com.


### About ZeroEntropy


ZeroEntropy builds state-of-the-art retrieval models for AI applications. Its reranking, embedding, and search APIs power the most demanding retrieval pipelines, from clinical reasoning systems to enterprise customer support. ZeroEntropy’s open-weight models consistently outperform alternatives on both public benchmarks and real-world production workloads. Backed by Y Combinator and Initialized Capital.


Learn more at zeroentropy.dev.
