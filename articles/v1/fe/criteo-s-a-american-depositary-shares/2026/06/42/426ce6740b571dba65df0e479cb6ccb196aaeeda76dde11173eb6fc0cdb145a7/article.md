---
schema_version: "1.0.0"
document_id: "426ce6740b571dba65df0e479cb6ccb196aaeeda76dde11173eb6fc0cdb145a7"
company_key: "criteo-s-a-american-depositary-shares"
company: "Criteo S.A."
source_id: "criteo-s-a-american-depositary-shares-rss-02db2411825d"
canonical_url: "https://medium.com/criteo-engineering/introducing-clepr-our-model-for-semantic-understanding-d3984eed84c8"
published_at: "2026-06-02T06:36:00+00:00"
first_seen_at: "2026-07-20T23:17:33.645392+00:00"
fetched_at: "2026-08-20T03:05:58.983824+00:00"
content_hash: "sha256:21dd8ecb037f33bf36e9d097aedae1f70bbd33f99bb558007cd5d7033bf71273"
---

# Introducing CLEPR, our model for semantic understanding

Author:[Paul Coursaux](https://www.linkedin.com/in/paulcoursaux/)


At Criteo, retail media is about helping brands reach shoppers directly on retailers’ property, right at the digital shelf where purchase decisions are made. Through[CMAX](https://www.criteo.com/fr/platform/commerce-max/) , our unified retail media platform, we connect advertisers to retailers’ audiences with[Sponsored Products](https://www.criteo.com/blog/boost-your-brand-with-sponsored-products-ads/) that appear alongside native results in onsite search and browsing experiences.


Sponsored products: Boost your brand with sponsored ads


In this context, the core problem we address is balancing two imperatives: delivering strong performance for advertisers while preserving a high‑quality, trustworthy shopping experience for retailers and their customers, as we define more precisely in our notions of *semantic relevance* and *outcome‑based relevance* below. We solve this by using our[proprietary deep learning models](https://medium.com/criteo-engineering/criteo-boosts-ad-performance-with-latest-ai-advancements-2fc3d5f65f56) to ensure that Sponsored Products not only perform, but also accurately reflect each user’s intent and each retailer’s catalog.


[Criteo Boosts Ad Performance with Latest AI Advancements](https://medium.com/criteo-engineering/criteo-boosts-ad-performance-with-latest-ai-advancements-2fc3d5f65f56)


In this article, we align with the accuracy and outcome‑based relevance framework introduced in our blog post on agentic recommendation systems,[“Leveraging Commerce Data for Outcome-Based Relevancy in Agentic Recommendation Systems”](https://medium.com/criteo-engineering/leveraging-commerce-data-for-outcome-based-relevancy-in-agentic-recommendation-systems-717d62589ec3) :


- accuracy or “semantic relevance” captures how well a product’s content matches the user’s query
- “outcome‑based relevance” measures whether recommendations actually satisfy user needs as evidenced by real commerce signals such as clicks and purchases


[Leveraging Commerce Data for Outcome-Based Relevancy in Agentic Recommendation Systems](https://medium.com/criteo-engineering/leveraging-commerce-data-for-outcome-based-relevancy-in-agentic-recommendation-systems-717d62589ec3)


Building on Criteo’s ongoing investment in AI‑driven retail media, we have therefore focused on strengthening the accuracy engine behind Sponsored Products.


Advertising on retailer’s property comes with unique constraints. Unlike off-site advertising, where ads are displayed beyond the retailer’s own digital properties, for example, on external websites, search platforms, or social media, and optimization primarily targets performance metrics (e.g., click-through rate), onsite Sponsored Products must satisfy strict accuracy requirements to preserve the quality of the shopping experience. For instance, if a shopper searches for “red dress,” the ads shown alongside search results must be tightly aligned with that intent: different brands, styles, lengths, or sizes of red dresses are acceptable, but different-colored dresses, such as a yellow dress, as well as unrelated products like jeans, shoes, or handbags, are not.


On-site Sponsored Products must satisfy strict accuracy requirements to preserve the quality of the shopping experience


Ads are displayed alongside organic results in response to explicit user queries, making relevance to search intent non-negotiable. An inaccurate ad is not just inefficient: it creates annoyance and cognitive friction for shoppers, undermines the perceived quality of the retailer’s search results, and ultimately harms Criteo’s reputation with retailers by degrading the overall shopping experience.


To address this challenge, we have leveraged the latest advances in Language Modeling to deepen our understanding of search intent and product semantics. In our Agentic Recommendation Service blog post, we showed that outcome-based relevancy improves by 60% when fine‑tuned on commerce data. This builds on top of years of experience gained by optimizing search models to deliver optimal Retail Media experience to retailers, brands, and consumers. By combining our unique commerce data with these latest advances in language models, we ensure that Sponsored Products deliver strong performance while meeting the high bar of accuracy required in retail media.


### Introducing our Contrastive Language Embedding for Product Retrieval model


Criteo’s retail media ecosystem operates at a truly global scale. We power Sponsored Products across multiple continents, languages, and retail verticals — from home improvement to fashion and consumer electronics. Our systems must understand billions of products across diverse catalogs, each shaped by local consumer behavior, regulatory environments, and cultural nuance.


Semantic understanding in this context is far from trivial. A simple query can carry very different meanings depending on geography or vertical. “Chips,” for example, refers to crisps in the US, but to fries in the UK; “biscuits” are soft, savory rolls in the US but sweet, crunchy cookies in Europe; and “Apple” may denote a smartphone brand in consumer electronics or a piece of fruit in grocery. These nuances are not edge cases; they are daily realities at a global scale.


As a result, we need a system capable of handling multiple languages while capturing cultural subtleties, domain-specific vocabulary, and local shopping intent. At the same time, it must operate reliably across billions of product listings and millions of daily queries without compromising latency.


To address these challenges, we developed CLEPR (Contrastive Language Embedding for Product Retrieval), a deep learning model designed to measure and optimize the semantic relevance between search keywords and product offerings.


While Large Language Models (LLMs) are powerful, they are not yet practical as the primary retrieval engine in our setting. On-site Sponsored Products must respond in a few tens of milliseconds and score billions of keyword–product pairs per day. Running an LLM at that frequency would be prohibitively expensive and too slow for real‑time retail search. Our CLEPR two-tower architecture addresses both problems at once. Similar to the DeepKNN system described in[“Optimizing Advertising Performance with Advanced Machine Learning and Vector Database Technology”](https://www.criteo.com/blog/optimizing-advertising-performance-with-advanced-machine-learning-and-vector-database-technology/),) these vector representations place related items close together in a shared space, enabling fast similarity scoring with low latency and predictable costs at Criteo scale.


[Optimizing Advertising Performance with Advanced Machine Learning and Vector Database Technology | Criteo](https://www.criteo.com/blog/powering-ai-assisted-and-agentic-commerce-criteos-vision-for-the-next-ai-era-2/)


We first experimented with[CLIP](https://en.wikipedia.org/wiki/Contrastive_Language-Image_Pre-training) , which was the most popular text–image encoder at the time. While CLIP’s text encoder could theoretically represent keywords to retrieve products, this method is less ideal: keywords, typically broad and concise, differ significantly from the detailed and descriptive product titles and descriptions CLIP was trained on. Concretely, CLEPR is a two-tower neural network architecture featuring two specialized encoders: one for search keywords and another for products. Using contrastive learning, CLEPR learns to map semantically similar keywords and products into nearby points in a shared embedding space, enabling us to compute relevance scores that capture the true intent behind user searches.


CLEPR Two-Tower architecture


#### Training CLEPR: overcoming data sparsity and quality challenges


Ideally, we would train CLEPR directly on organic search clicks: when a user searches for “running shoes” and then clicks a specific product page, this search‑to‑product interaction provides noisy but powerful evidence that the query and product are relevant to each other. By mining millions of such organic search‑to‑click events on retailers’ e‑commerce platforms, one could automatically build positive (keyword, product) training pairs without any manual labeling.


In practice, however, we do not collect these search‑to‑click events explicitly. We only see search events and product page visits as separate log events. To approximate the ideal signal, we reconstruct pseudo “organic clicks” by stitching together consecutive search events and subsequent product page views within the same session, using simple heuristics based on temporal proximity and session information. This reconstruction lets us train at scale on a natural behavioral signal, but it also makes the data inherently imperfect.


Once these heuristic search‑to‑product pairs are built, CLEPR learns to pull matched keyword–product pairs closer in a shared embedding space through contrastive learning.


CLEPR training data


However, even after reconstruction, this click-based training signal remains imperfect. In particular, it can produce false positives where a product is clicked after a query, but the click does not actually reflect the user’s search intent. To limit this noisy traffic, we apply strict data‑quality filters. We retain only keyword–product pairs with high exclusivity scores (where a keyword consistently leads to clicks on the same product across sessions) and enforce hard thresholds on minimum volume and recency of interactions. These safeguards do not fix issues such as relevant products that are never clicked, but they substantially reduce spurious positive pairs and focus training on the most reliable keyword–product relationships.


Even with these filtering strategies, an important concern remains: a model trained on click data may learn to reproduce behavioral biases rather than true semantic relevance. Clicks are biased by search result positioning, product popularity, and user behavior patterns that don’t necessarily reflect semantic relevance. For this reason, we adopt a contrastive learning objective: instead of predicting clicks directly, the model learns to bring embeddings of matched keyword–product pairs closer together while pushing apart mismatched pairs within the same batch. This relative training signal encourages the model to capture discriminative semantic structure rather than absolute click propensity. For example, even if a keyword–product pair appears with 10,000 clicks in the training data, we only count it once, preventing popularity bias and keeping the focus on semantic meaning. To obtain an accurate measure of model quality, we evaluate CLEPR on human-labeled keyword-product pairs, where annotators provide explicit relevance judgments. These annotations serve as ground truth, allowing us to measure whether CLEPR genuinely understands relevance beyond the patterns in click data. CLEPR’s contrastive learning approach, trained purely on filtered click data, must learn rich semantic representations that generalize to these human-judged examples — a true test of whether the model has captured meaningful relevance signals rather than simply overfitting to click biases.


#### CLEPR in production: bringing semantic guardrails to ranking at scale


Deploying CLEPR in our ranking pipeline brought semantic relevance directly into production. Acting as a guardrail, CLEPR ensures that candidate products meet a minimum relevance threshold before performance optimization, reducing mismatches between search intent and ad placement.


We followed a deliberately careful, experimentation‑driven path to get there. CLEPR was first introduced behind strict safeguards, with successive A/B tests and phased rollouts to validate every change before expanding its impact. At each step, we closely monitored both semantic and outcome-based relevance, only scaling up when experiments demonstrated clear, consistent gains.


Operating CLEPR at the Criteo scale also required solving non‑trivial ML engineering challenges.


On the inference side, we must continuously re‑embed a catalog of about 2B products whenever product content changes, and still score over 10 billion keyword–product pairs per day across languages, verticals, and retailers. This calls for highly optimized offline inference pipelines and infrastructure that can run massive batches of CLEPR forward passes in a reasonable time, while keeping latency low and costs predictable.


Training at this scale introduces its own constraints. To keep up with the volume and diversity of training data, we rely on Distributed Data Parallel (DDP) training with Ray over a multi‑GPU cluster, allowing us to efficiently ingest billions of keyword–product pairs, stabilize optimization, and iterate quickly on new CLEPR versions.


After deployment, we saw a +6% increase in Click-Through Rate (CTR) and a sharp drop in relevance issues. CLEPR proves that embedding semantic understanding into production not only improves shopper trust but also drives measurable engagement gains.


### What’s next? Powering AI-Native Commerce


By embedding CLEPR scores as features in our ad ranking and filtering pipelines, we create strong relevancy guardrails that improve user experience, increase ad engagement, and help advertisers reach the right audiences with the right products.


Building on this foundation, we are now extending CLEPR’s semantic intelligence into AI-native shopping experiences. In this context, we are exploring multimodality, where CLEPR can jointly leverage text, image, and other signals, and how it can best interoperate with Criteo’s Foundation Models to inject richer commerce knowledge into AI-native journeys.


Next, we’re extending this semantic intelligence into AI‑native shopping experiences. Building on Criteo’s agentic commerce vision described in[Sherry Smith](https://www.linkedin.com/in/sherryd-smith) ’s blogpost, “Why retail media rises in an agentic commerce era” ([Why retail media rises in an agentic commerce era | Criteo](https://www.criteo.com/blog/why-retail-media-rises-in-an-agentic-commerce-era/),) we are designing conversational shopping assistants on retailers’ properties, where commerce shifts from keywords to rich, natural‑language interactions. CLEPR ensures products surfaced in these experiences are tightly aligned with what shoppers actually mean.


[Why retail media rises in an agentic commerce era | Criteo](https://www.criteo.com/blog/why-retail-media-rises-in-an-agentic-commerce-era/)


At the same time, our Agentic Commerce Recommendation Service connects AI shopping assistants directly to merchant inventory through Criteo’s commerce intelligence. CLEPR translates open-ended queries into transaction-ready recommendations grounded in real shopping signals — not just catalog metadata.


As commerce shifts from keywords to conversations, CLEPR is evolving from a ranking feature into a foundational semantic layer for AI-driven discovery.


---


[Introducing CLEPR, our model for semantic understanding](https://medium.com/criteo-engineering/introducing-clepr-our-model-for-semantic-understanding-d3984eed84c8) was originally published in[Criteo Tech Blog](https://medium.com/criteo-engineering) on Medium, where people are continuing the conversation by highlighting and responding to this story.
