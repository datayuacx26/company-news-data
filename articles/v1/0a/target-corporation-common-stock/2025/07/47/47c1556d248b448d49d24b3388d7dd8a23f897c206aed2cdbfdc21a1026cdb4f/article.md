---
schema_version: "1.0.0"
document_id: "47c1556d248b448d49d24b3388d7dd8a23f897c206aed2cdbfdc21a1026cdb4f"
company_key: "target-corporation-common-stock"
company: "Target Corporation"
source_id: "target-corporation-common-stock-rss-2844690dfd24"
canonical_url: "https://tech.target.com/blog/accessory-recommendations-with-llms"
published_at: "2025-07-01T05:00:00+00:00"
first_seen_at: "2026-07-20T03:31:39.462984+00:00"
fetched_at: "2026-07-28T20:57:22.944976+00:00"
content_hash: "sha256:cd3a65d36c338c1930e9c0b4669973976379ef0fcd79664fb7adf6e78ae6b732"
---

# Improving Accessory Recommendations with LLMs at Target

Accessories!


Whether it’s batteries for a toy, a case for a phone, or the perfect end table to complement a living room set, guests want to know which items go well together. However, providing high-quality accessory recommendations is a complex challenge, especially because of the sheer volume of our catalog and the many item attributes that might matter. Depending on the product, guests might prioritize color, kid-friendliness, flavor, material, brand—or any of a hundred other features.


To address this challenge, the Product Recommendations Team was recently asked to create a “related accessory” algorithm for both our Electronics and Home categories. Thanks to the power of Large Language Models (LLMs), we developed GRAM, a GenAI-based Related Accessory Model, for Home. It’s a scalable and performant algorithm that makes it easier than ever to pair the right accessory with the right product.


Technical Challenges


We faced three key technical challenges that significantly impacted the effectiveness of our system as we set out to design it.


1. Determining Attribute Importance:


Our first challenge was identifying the importance of all attributes across each category. Given our vast catalog, it would take a human expert an enormous amount of time to manually evaluate and prioritize the most relevant attributes for each accessory pairing. We solved this by using LLMs to automatically analyze the product data, identify the most important attributes, and assign them importance weights for various pairs of core (or seed) and accessory item types.


(Figure 1)


For instance, when recommending pillowcases as accessories for sheet sets (see Figure 1), color and material are treated as the most significant factors. In contrast, when suggesting an appropriate book to accompany a kids' craft activity kit, the intended audience attribute (infant, kids, adults, etc.) becomes the most important consideration.


The model formulates scoring rules using LLMs and uses them to compute scores for all accessory items. When a core item’s attribute value matches an accessory item’s attribute value, the attribute weight is added to the accessory item relevance score. Ultimately, the accessory items with the highest scores are selected as final recommendations (sales rank serves as a tie-breaking mechanism in cases where scores are equal).


2. Capturing Aesthetic Matches:


The second challenge involves situations where the matching requires an aesthetic sense. For these pairings, it is insufficient to recommend items whose attributes match—we must also consider how they coordinate together as part of a complete set. We serendipitously discovered that the LLM was quite good at using concepts like color harmony and stylistic coherence.


(Figure 2)


(Figure 3)


For instance, in Figures 2 and 3, the LLM uses attributes such as color, material, and style to create harmonious sets of attribute values that enable more diverse and creative recommendations. This holistic approach significantly enhances our accessory suggestions, resulting in visually appealing and cohesive arrangements.


3. Scaling Across Large Catalogs:


With hundreds of thousands of items in the Home category, it was crucial that our algorithm scale effectively. To do so, we constrained the algorithm to only consider attributes for pairs of core and accessory item


types


, rather than pairs of the items themselves. This vastly reduced time and cost complexity. Also, once scoring rules were established for each pair of core and accessory item types, the model needed only to score and sort all pairs of items in those types (an operation we also parallelized). The resulting algorithm is scalable enough that we foresee it will be easy to adapt it to categories beyond Home.


Integration with Human Insight


Although the model was effective as described, we made sure to include human-in-the-loop (HITL) processes to solidify the recommendations. Specifically, we collaborated with site merchants to create a list of the most commonly co-purchased accessory items, enabling support for cross-category recommendations.


As can be seen in Figures 4-6, the mode without HITL frequently suggests similar items, whereas employing HITL led to accessory recommendations from a far more diverse set of items. This is quite useful, because the model without HITL can be used effectively by guests who are still in the exploration stage (comparing similar accessories), while the model with HITL ends up facilitating basket expansion by exposing guests to a far wider array of accessory types based on what is already in their carts.


(Figure 4)


(Figure 5)


(Figure 6)


Results from A/B Testing


In February 2025, we conducted an A/B test where we added the Home Accessory model to the add-to-cart flyout. This resulted in the following boost to our business metrics:


- Around an 11% increase


in interaction rate indicating heightened engagement


- A roughly 12% increase


in display-to-conversion rates reflecting stronger content relevance and downstream impact


- More than 9% growth


in attributable demand


In summary, by combining both LLMs and human insights into a single capability, we have transformed the way accessories can be recommended at Target across diverse categories. This comprehensive system not only enhances user engagement but also facilitates seamless shopping experiences. We are excited to announce that this model was fully rolled out to production in April 2025, marking a significant milestone in our commitment to delivering exceptional value and convenience for our customers.


## RELATED POSTS


### Elevating Guest Repurchasing Behavior Using Buy It Again Recommendations – Part 2


By Amit Pande, David Relyea, Prathyusha Kanmanth Reddy, Pushkar Chennu, and Rankyung Park, March 4, 2025


This article introduces a new model for BIA recommendations using item- and category-level Hawkes processes.


### Elevating Guest Repurchasing Behavior Using Buy It Again Recommendations


By Amit Pande and Rankyung Park, November 2, 2023


Target’s Data Science team shares an inside look at our Buy It Again model.
