---
schema_version: "1.0.0"
document_id: "0de048d6a6b024ca7adbfde01f38934913bf356897ba2e1898c6e5f008e2ef33"
company_key: "airbnb-inc-class-a-common-stock"
company: "Airbnb Inc."
source_id: "airbnb-inc-class-a-common-stock-rss-af0970d10a00"
canonical_url: "https://airbnb.tech/ai-ml/personalizing-airbnb-search-by-learning-from-the-guest-journey/"
published_at: "2026-07-27T19:00:17+00:00"
first_seen_at: "2026-07-27T20:07:31.697617+00:00"
fetched_at: "2026-07-28T21:49:03.166047+00:00"
content_hash: "sha256:c57e132af1289dcad7c2a49b832e6cad088f6ee387b4f76516e31cee1438f358"
---

# Personalizing Airbnb search by learning from the guest journey

By:[Daochen Zha](https://www.linkedin.com/in/daochen-zha/) ,[Chun How Tan](https://www.linkedin.com/in/chunhowt/) ,[Xin Liu](https://www.linkedin.com/in/xin-liu-908b6b18/) ,[Bin Xu](https://www.linkedin.com/in/bin-xu-96253aa5/) ,[Han Zhao](https://www.linkedin.com/in/han-zhao-692944116/) ,[Xiaowei Liu](https://www.linkedin.com/in/xiaowei-liu-60415841/) ,[Jun Shi](https://www.linkedin.com/in/jun-shi-5a3207149/) ,[Tracy Yu](https://www.linkedin.com/in/tracy-xiaoxi-yu/) ,[Hui Gao](https://www.linkedin.com/in/hui-gao-275a924/) ,[Huiji Gao](https://www.linkedin.com/in/huiji-gao/) ,[Liwei He](https://www.linkedin.com/in/liweihe/) ,[Michael Kinoti](https://www.linkedin.com/in/michael-kinoti-7a309215/) ,[Stephanie Moyerman](https://www.linkedin.com/in/stephanie-moyerman/) , and[Sanjeev Katariya](https://www.linkedin.com/in/sanjeevkatariya/)


## Introduction


Planning a trip on Airbnb rarely happens in a single session. A guest searching for a place to stay in San Francisco might browse dozens of listings over several days, leaving behind a trail of views. Typically, over a period of years, that same guest will have accumulated many previous bookings, reviews, and the occasional cancellation. Taken together, these events reveal a great deal about what that guest values in a stay. For years, Airbnb’s search ranking captured this through hand-crafted features: aggregated statistics such as total past bookings or average listing price. These worked well, but as the feature count grew into the hundreds, the approach became harder to scale and increasingly limited in expressiveness. In this blog post, we describe how we built a sequence modeling system that encodes the full guest journey using a Transformer, learning richer representations of guest preferences to deliver more personalized search results.


An example of a guest journey, which is typically long, exploratory, and complex.


## Challenges


Event sequences per guest present three core challenges. First, they are dominated by listing views, which account for the vast majority of all events — some guests accumulate hundreds of thousands of them — making raw sequences computationally intractable to model directly.


The distribution of event types, with the majority being listing views.


Second, unlike social media platforms, which optimize for engagement, Airbnb optimizes for booking conversion. Bookings are rare, compared to events, and deliberate, whereas a listing view could reflect genuine intent or simply idle browsing. Building a model that generalizes from these sparse, noisy signals to surface the listings most relevant to a guest is fundamentally different from social media engagement modeling. Third, training a sequence model on hundreds of millions of search-label pairs is expensive; targeted optimizations are needed to reduce costs and improve the throughput of model training.


## Solution


### Designing the guest sequence


How to tackle this challenge, which is also a rare opportunity to use Transformer technology to improve guest experience?


After research into similar use cases, we came up with a plan of action. Rather than modeling all events uniformly, we split the guest sequence into two parts:


- The long-term sequence captures infrequent but informative events from the past seven years: bookings, reviews, cancellations, and other past interactions. We cap this sequence at 80 events.
- The short-term sequence captures listing views from the past 21 days. We cap the short-term sequence at 200 events.


The 80-event and 200-event capping thresholds are set so that only the longest 2% of sequences are truncated. Together, the two sequences give the model both depth from a guest’s booking history and the immediacy of their most recent browsing activity. Events are encoded using a shared feature pool with a[unified embedding table](https://arxiv.org/abs/2305.12102) \[1\] for high-cardinality IDs such as listing and host identifiers, along with hierarchical geographic IDs.


An overview of our solution.


### Making training & serving efficient


We’ve implemented three strategies that, together, deliver roughly a 4x improvement in training throughput. The most impactful is batching of searches. Consider a guest who performs three searches — at T=4, T=7, and T=10 — interspersed with events at other timestamps. Because our encoder uses a causal mask, it processes the sequence cumulatively. The embedding produced at position T=3 captures everything the guest has done up to that point, making it the correct sequence representation for the search at T=4. Similarly, no events are ignored between searches: the embedding at T=6 captures the entire sequence from T=1 through T=6 to serve the search at T=7, and the embedding at T=9 captures the full history up to T=9 to serve the search at T=10. Rather than running the encoder three separate times from scratch, we run it once over the full event sequence and route each search to its corresponding intermediate embedding, allowing three searches to share a single encoder forward pass.


An illustration of batching of searches.


The other two strategies reduced padding waste. (Data is processed in fixed chunks, and empty space in actual data is filled with zeros, referred to as “padding.”) To minimize padding within batches, we bucketize sequences by length. And to eliminate padded searches from the ranking model, we use sparse calculation of searches.


The serving design reflects the same efficiency principle. Running a multi-layer Transformer encoder over a full guest sequence — spanning up to seven years of events and 21 days of views — at query time would be expensive. Instead, we decouple the two stages of inference. The sequence encoder runs as a daily batch job, processing event history and storing the resulting embeddings for guests who have generated new events. When a guest performs a search, the ranking model retrieves their stored embedding in real time and combines it with the live search query, then uses the information to produce scores for the retrieved listing candidates. This split keeps latency low at serving time, while still incorporating the full depth of a guest’s history into every ranking decision.


An illustration of model serving.


### Three milestones


Before rolling out any changes, we rigorously evaluate them using A/B testing, where guests are randomly but consistently assigned to either our current model or a new model. These tests typically run for three weeks, during which we closely monitor key business metrics and “guardrail” metrics to ensure there are no unintended negative side effects. A new model is only launched if there is strong, statistically proven evidence that it improves the guest experience without failing any safety checks.


Following this standard, we rolled out the system in three stages.


- First, we tested the system with the long-term sequence alone, establishing that sequence-learned guest representations could meaningfully improve our existing ranking system.
- Second, we added the short-term view sequence, which introduced data scaling challenges that are addressed by the efficiency strategies above.
- Third, we introduced a setwise ranker \[[2](https://dl.acm.org/doi/abs/10.1145/3746252.3761567) ,[3](https://dl.acm.org/doi/abs/10.1145/3746252.3761521) \], co-trained with the sequence encoder. Unlike a pointwise ranker that scores each listing independently, the setwise ranker considers a set of candidates together, allowing it to reason about relative differences across listings. Combined with the rich guest representations from the sequence encoder, it can make more personalized ranking decisions by jointly understanding the guest’s travel history and how the retrieved listings compare to each other.


## Results


Offline, the long-term sequence alone improved Normalized Discounted Cumulative Gain (NDCG) of booking labels by +0.44% over our existing system. Adding the short-term view sequence brought the combined improvement to +1.48%. The setwise ranker added a further +2.3% improvement in offline ranking quality. In total, the improvement added up to 3.78%. At Airbnb, where the ranking model has been refined over more than a decade, a 0.3% improvement is considered significant. This substantial improvement means our model can rank the listings a guest likes higher, which may potentially help guests find what they want more easily.


Online A/B tests confirmed the gains. With the long-term sequence alone: +0.31% uncanceled bookers (guests who have booked), +0.38% views. Adding the short-term sequence: +0.55% uncanceled bookers, +0.82% uncanceled nights, +0.90% views — all statistically significant. The rise in views suggests that the model is effectively surfacing listings that capture guests’ interests, which directly translates into the higher booking conversions we observed. The setwise ranker delivered a further +0.28% uncanceled bookings and +0.32% booking requesters.


We also applied the system to promotional email listing ranking. Using the same sequence embeddings, without any architecture changes, produced +0.16% uncanceled bookers, +0.23% uncanceled nights, and +5.04% email clicks — demonstrating that the guest representations generalize beyond search.


## Conclusion


What we built learns directly from the full arc of a guest’s relationship with Airbnb; their past trips, recent browsing activity, and everything in between. Moving from hand-crafted feature aggregation to learned sequence representations gives our ranking system access to a kind of guest understanding that was previously out of reach: not just what a guest has done across their history at Airbnb, but what those actions suggest about what they are looking for right now. The results across search ranking and promotional emails, which are two independently iterated surfaces, validate that this signal is both powerful and general.


The work is far from finished. We are exploring near-real-time sequence embedding updates, richer event types such as wishlists and map interactions, target-aware ranking, generative recommender, and continued improvements to setwise ranking. Each of these directions builds on the same foundation: the more faithfully we can represent the guest journey, the better we can connect guests with homes they will love. For a deeper technical treatment, see our KDD ’26 paper[JourneyFormer: Encoding Airbnb guest journey with sequence modeling](https://arxiv.org/abs/2606.19108) \[4\].


Interested in learning more about our technical journey? Browse our[previous publications](https://sites.google.com/view/airbnb-relevance-publications/home) to see how our systems have evolved. If tackling these kinds of challenges excites you, explore our[open roles](https://careers.airbnb.com/) .


## Acknowledgments


We would like to especially thank the following people for their great collaboration and for continuing to advance sequence modeling at Airbnb (listed alphabetically): Ashish Jain, Gil Forsher, Hao Li, Haozhen Ding, Jiawei Yao, Kedar Bellare, Linyun He, Mingyang Xu, Pallavi Adusumilli, Pengyu Hou, Shashank Dabriwal, Sid Reddy, Sophie Wang, Tanya Piplani, Vijay Velagapudi, Yangbo Zhu, Yan Zhang, Yi Li, Yiwei Wang, Yuli Han, and Zhiwei Wang.


## References


\[1\] Coleman, Benjamin, et al. “Unified embedding: Battle-tested feature representations for web-scale ML systems.” Advances in Neural Information Processing Systems, 2023.


\[2\] Tang, Jie, et al. “Learning to Comparison-Shop.” ACM International Conference on Information and Knowledge Management. 2025.


\[3\] Haldar, Malay, et al. “Beyond Pairwise Learning-To-Rank At Airbnb.” ACM International Conference on Information and Knowledge Management. 2025.


\[4\] Zha, Daochen, et al. “JourneyFormer: Encoding Airbnb Guest Journey with Sequence Modeling.” ACM SIGKDD Conference on Knowledge Discovery and Data Mining. 2026.


*All product names, logos, and brands are property of their respective owners. All company, product, and service names used in this website are for identification purposes only. Use of these names, logos, and brands does not imply endorsement.*
