---
schema_version: "1.0.0"
document_id: "d96386f591512d0a0513ec0b493c98bf2b09a6315fb511ee38c227ac521226aa"
company_key: "pinterest-inc-class-a-common-stock"
company: "Pinterest Inc."
source_id: "pinterest-inc-class-a-common-stock-rss-45b43224fdd9"
canonical_url: "https://medium.com/pinterest-engineering/enhancing-ad-relevance-integrating-real-time-context-into-sequential-recommender-models-bc3a2f9b682e"
published_at: "2026-05-08T19:01:00+00:00"
first_seen_at: "2026-07-20T04:35:24.920473+00:00"
fetched_at: "2026-07-28T22:15:07.431097+00:00"
content_hash: "sha256:b760c2c77f2b79dd6b77adb6a808fa38f8224a928b478e9b6adb7aa9503e7a8f"
---

# Enhancing Ad Relevance: Integrating Real-Time Context into Sequential Recommender Models

Pinterest


Engineering


Monetization


Ads Retrieval


Transformers


# **Enhancing Ad Relevance: Integrating Real-Time Context into Sequential Recommender Models**


[Pinterest Engineering](https://medium.com/@Pinterest_Engineering?source=post_page---byline--bc3a2f9b682e---------------------------------------)


5 min read


·


May 8, 2026


--


Huiqin Xin | Machine Learning Engineer II, Ads Vertical Modeling; Lakshmi Manoharan | Senior Machine Learning Engineer, Ads Vertical Modeling; Karthik Jayasurya | Staff Machine Learning Engineer, Ads Signals; Ziwei Guo | Senior Machine Learning Engineer, Ads Vertical Modeling; Alina Liviniuk | Machine Learning Engineer II, Ads Vertical Modeling


## Motivation: The Need for Real-Time Context


In a previous[post](https://medium.com/pinterest-engineering/ads-candidate-generation-using-behavioral-sequence-modeling-f9077ee1325d) , **Ads Candidate Generation using Behavioral Sequence Modeling** , we introduced a candidate generator (CG) that uses a Transformer-based two-tower model to leverage a user’s *offsite* conversion history — a powerful signal — to predict future interactions with advertisers and specific products. This was a significant step forward, moving beyond static interest categories to model the evolving user shopping journey.


However, a key limitation of the initial sequential model was its lack of online context information. The user embeddings were inferred offline purely from historical offsite behavior, meaning that at the moment an ad was served, the model had no knowledge of what the user was currently browsing on Pinterest. This is a crucial drawback, particularly for highly contextual surfaces like *Related Pins* and *Search* , where the user’s current Pin or search query represents a strong, immediate signal of intent. For example, on the Related Pins surface, if a user is viewing a Pin of a “vintage leather armchair,” the recommended ads should be highly relevant to that specific item, not just their general, long-term interests.


This lack of context severely limited the model’s effectiveness on these surfaces; in the previous production system, less than 1% of impressions on Related Pins were attributed to this CG, indicating its candidates struggled to survive the downstream ranking and auction stages.


## The Contextual Sequential Modeling Solution


To overcome this challenge, we developed the **Contextual Sequential Two Tower Model** , an evolution of the sequential recommender model specifically designed to incorporate real-time, online context. This approach focuses on three major areas: a new model architecture, a novel training approach, and a hybrid serving flow.


## Model Architecture: Integrating the Context Layer


The core architectural change was integrating a **context layer** directly into the query tower of the two-tower model.


Press enter or click to view image in full size


Figure 1. Contextual Sequential two-tower model architecture


As shown in the diagram above, the model now concatenates the output of the original Transformer encoder (which represents historical sequence information) with the output of the new context layer. This combined representation is then fed into the final Multi-Layer Perceptron (MLP) to derive the final user embedding.


For the Related Pins surface, the context layer’s input features are derived from the *subject Pin* (the Pin the user is currently viewing), specifically using features like the aggregated embedding representations of the top *interest categories* of the subject Pin, weighted by their confidence scores.


To further personalize the model, the user representation layer was augmented with embeddings of user demographic features, such as age, country, and gender.


## Model Training with Synthetic Context


Since real-time context is only available at serving time, we had to make the model capable of learning from this signal during offline training. The solution was to use **synthetic augmented data** .


Press enter or click to view image in full size


Figure 2. Model training with synthetic augmented data


During model training, we artificially inject pseudo-context information derived from the *positive label* (the conversion event) into the input sequence. For example, by projecting the *interest category* features from the positive item, we encourage the model to retrieve items that are semantically related to the *context* associated with that user session. A high dropout rate is used in the context layer during training to ensure the model still relies on the user’s historical event sequence (the Transformer output).


We opted to use synthetic augmented data over real context data due to two main challenges:


1. Merging onsite data with offsite data presents significant technical difficulties.
2. We cannot guarantee that a user has viewed ad impressions on Related Pins between two sequential offsite events.


## Hybrid User Embedding Inference


Given that the context features (e.g., subject Pin features) are only known at the ad request time (online), we adopted a **hybrid model inference** approach.


1. **Offline Inference:** The majority of the user tower (the Transformer encoder) is inferred offline, and the last hidden state of the transformer (the encoded representations of the event sequence) is stored in the feature store. This is refreshed on a daily basis for users with new offsite activity.
2. **Online Inference:** The remaining part of the user tower — the context layer and the final MLP head — is computed online at serving time, taking the real-time context features and the pre-computed offline user signal as inputs.


This architecture and serving flow enables the user embedding to be dynamically influenced by the real-time context, ensuring the recommendations are both personalized (from sequence) and contextually relevant.


## Results and Business Impact


### Offline evaluation


To assess the impact of integrating context features on the survival rate of model-retrieved ad candidates, we conducted an offline evaluation. Using logged features from real traffic ad data on Related Pins, we generated the model output embedding and calculated Recall@K, which measures the proportion of positive items found in the top-K retrieved items. Here the candidates that survived the ranking funnel and delivered to the users were considered positive items. This new model demonstrated a significant improvement, achieving a 3x to 10x increase in Recall@K compared to the production model.


Press enter or click to view image in full size


Table 1. Recall@K for production model and contextual model


## Survival Rate & Relevance


We were able to successfully drive up the survival rate of the candidates from this CG on the Related Pins surface. The median relevance of the candidates went up by **~275–300%** . On the Related Pins surface overall, the ads relevance metric improved by **1.08%** . Furthermore, we observed a significant increase in candidate delivery, with **2x** more ads candidates retrieved being delivered to impression.


## Topline Business Metrics


The improvement in candidate relevance translated into ~0.7% measurable lift in conversion-related business metrics ROAS (Return on Ad Spend). In particular, the model benefits more for top countries which account for a majority of total revenue and leads to ~1.4% ROAS lift.


## Future work


We plan to explore several key enhancements:


1. **Context Surface Expansion:** A key next step is to extend the context-enhanced candidate generator to other high-stakes contextual surfaces, notably Search. This is particularly crucial for Search because maintaining high relevance between the presented ad candidates and the user’s search queries is paramount.
2. **Advanced Fusion Techniques:** Move beyond simple concatenation of context layers with the sequential encoder output. We propose using **cross-attention-based fusion** , where the context layer embedding acts as the query and the sequence of encoded transformer outputs serves as the key/value. This approach will allow the final user-tower embedding to dynamically capture the importance of each history event based on the real-time context.


## Acknowledgements


We would like to thank Supeng Ge, Yang Liu, Richard Huang, Yu Liu, Zhuqing Zhang, Kevin Liao, Yu Gu, Wanyu Zhang, for their dedicated help; thank to Alice Wu, Leo Lu, Siping Ji, Ling Leng for their incredible support and leadership; thank to Joachim Groeger for the valuable discussion and support.
