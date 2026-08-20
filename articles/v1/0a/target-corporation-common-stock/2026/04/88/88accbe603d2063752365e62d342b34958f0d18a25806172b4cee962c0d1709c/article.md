---
schema_version: "1.0.0"
document_id: "88accbe603d2063752365e62d342b34958f0d18a25806172b4cee962c0d1709c"
company_key: "target-corporation-common-stock"
company: "Target Corporation"
source_id: "target-corporation-common-stock-rss-2844690dfd24"
canonical_url: "https://tech.target.com/blog/scaling-marketing-campaign-forecasting-ai"
published_at: "2026-04-09T05:00:00+00:00"
first_seen_at: "2026-07-20T03:31:39.462984+00:00"
fetched_at: "2026-07-28T22:00:15.315546+00:00"
content_hash: "sha256:5815b69d3b9c2fd8ce9f6ab2e5442d8702c173c72bab70325e5fdd6258e97056"
---

# Scaling Marketing Campaign Forecasting with Generative AI

Marketing teams often must decide how much to invest in a campaign before it ever reaches a guest, and that decision can directly influence campaign performance and budget efficiency.


Target developed a solution to improve the accuracy of how we forecast campaign performance. This AI-powered system identifies and ranks similar past campaigns using semantic retrieval and large language models in a RAG architecture to predict how new campaigns are likely to perform.


As an AI/ML team within Target Tech focused on personalized offers, we recently enhanced our solution. In this post, we walk through why we evolved our approach, what changed, its architecture, data preparation, evaluation results, and what’s next on our road map.


Why We Rebuilt


The goal is simple: Use similar past campaigns to better forecast outcomes and build more accurate offer-redemption models so guests receive more relevant offers, and our marketing dollars work harder.


Our previous system relied on rule-based logic and basic embeddings that worked well at the time. However, as our marketing ecosystem matured and diversified, we observed a shift toward more sophisticated and niche campaign types that fell outside the system's original design parameters. This resulted in increased false positive rates and manual intervention overhead, limiting automation and scalability.


Beyond improving accuracy, partners needed to trust and tune the system. Our GenAI-based solution now returns ranked results with clear rationales, making matches auditable, explainable, and easier to refine with business stakeholders.


Architecture at a Glance


Figure 1. High-level design (HLD). We generate top N candidates from grounding Index(A) with a retriever and re-ranker(B), then use an LLM to filter and rank the final set via a prompt(c).


High level workflow steps:


1. Campaign as query:


Each incoming campaign request (markdown, guest intent such as engaged or lapsing, pyramid and categories, and other metadata) is treated as a query.


2. Embed the past:


Historical campaigns and offer data is aggregated, cleaned, and embedded using language models.


3. Grounding Service:


Embeddings are indexed and stored in our in-house AI grounding service.


4. Retrieve lookalikes:


We embed the new campaign’s metadata and retrieve the most similar prior historical campaigns from the index. For instance, if a new campaign aims to "drive trips" among lower engaged guests in apparel, the system automatically retrieves past clothing promotions with identical objectives.


5. Filter and rank with an LLM:


Retrieved candidates are passed to an LLM with a structured prompt that encodes our matching criteria hierarchy. The LLM filters and ranks for final selection.


A. Grounding Data


Accurate matching requires more than just advanced models — it depends on well-prepared, unified data. However, campaign and offer data often arrives in unstructured or semi-structured formats, so, we invested heavily in cleaning, connecting, and enriching our metadata.


Figure 2. Data pipeline. We unify, filter, and engineer features to produce a single, consistent “final offer pool” used for grounding.


We apply several filters to keep the dataset focused and relevant. For example, we limit to non-geo-segmented, omni-channel digital promotions that are standard and complete. We also focus on basket-based offers with dollar-off or percent-off rewards, while excluding manufacturer coupons and rebates.


On top of this, we engineer new features that add performance and context. These include metrics like distribution volume, redemption counts, and total discount spend. We also derive governance and rollout indicators, such as whether a campaign was sitewide or targeted, mass or niche, and whether it was “fully scaled” (a signal inferred from campaign text).


The result is a unified


final offer pool


that feeds directly into our retrieval index within Target’s AI platform, as illustrated in Figure 2.


With a unified and reliable dataset in place, we turned our focus to how campaigns could be retrieved, compared, and ranked effectively.


B. Retrieval Strategy


In the candidate generation stage, we evaluated multiple retrieval configurations, testing different prompt variations and index strategies to identify the most effective setup.


Offline evaluation showed that a multi-index approach gave the best raw retrieval quality, more explainable results, improved consistency across niche campaign types, and provided a governance ready framework for future policy and safety controls.


C. Prompting & LLM Filtering


The LLM prompt is designed to provide the model with both context and constraints, enabling consistent, explainable retrieval.


Each prompt carries three key components:


1. Input campaign facts: structured metadata describing the campaign being analyzed.


2. Retrieved candidates (top N): similar past campaigns returned by the retrieval engine.


3. Matching criteria hierarchy: an ordered set of attributes used to guide comparison and ranking (e.g., product domain → audience segment→ timing).


Below is a simplified illustration of the prompt structure. The example uses generic placeholders and anonymized fields for demonstration purposes only:


Rather than restricting the model’s latent understanding, these components help it anchor its reasoning in relevant past data while maintaining awareness of domain specific context.


Precision helps optimize the relevance of retrieved campaigns, while recall reduces the risk of overlooking valuable historical campaigns. To balance these metrics, we tested retrieving multiple candidate pool sizes (e.g., 10, 30) and evaluated re-ranker cutoffs ranging from K=1 to 10. We observed the best performance by retrieving a baseline pool of 10 candidates and having the LLM select the top 3 (K=3). This configuration achieved an optimal balance, delivering high precision with no need for manual intervention.


The key performance measure was coverage rate, which is how often the system provided at least one acceptable recommendation without requiring human override.


Beyond improved coverage, the new approach grounds the offer propensity model with more contextually relevant training data, resulting in improved forecast accuracy and campaign performance.


The result is a ranked set of comparable campaigns, each accompanied by a concise rationale. Because large language models tend to hallucinate, we introduced an additional validation layer to mitigate any risk.


Evaluation and Results


We evaluated the new system on a diverse set of recent marketing campaigns, using a time-separated, train-test setup.


Coverage by Recommendation Depth (K


)


With only the top recommendation (K=1), the system achieved 75% coverage, successfully providing suitable matches for most campaigns. With the top three matches (K=3), every campaign contained at least one strong and contextually aligned recommendation.


This increased the coverage rate to 100%, effectively ensuring that acceptable suggestions were always present and eliminating the need for manual search and correction.


This translates to guest value because offers are now more relevant, better timed and, ultimately, create a personalized and rewarding shopping experience. The benefit for Target is greater confidence in how we invest marketing dollars.


What’s Next


Our journey in building this new system is just the beginning. We are confident that the new LLM-driven strategy will continue to perform at par and evolve with the ever-evolving marketing landscape. The aim of offer personalization team is to make shopping experiences at Target joyful and exciting.


Looking ahead, we aim to close the loop by feeding back real-world performance into retrieval tuning. This will create an adaptive system, one that not only recalls the past but also learns dynamically from the present, thus delivering more personalized offers for our guests.


Acknowledgments


This work is made possible through collaboration across Target data science, product, engineering, and ML platform teams, with special thanks to partners who contributed to evaluation and reviews.
