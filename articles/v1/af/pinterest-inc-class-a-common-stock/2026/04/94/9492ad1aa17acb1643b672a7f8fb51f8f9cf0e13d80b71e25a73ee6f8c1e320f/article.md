---
schema_version: "1.0.0"
document_id: "9492ad1aa17acb1643b672a7f8fb51f8f9cf0e13d80b71e25a73ee6f8c1e320f"
company_key: "pinterest-inc-class-a-common-stock"
company: "Pinterest Inc."
source_id: "pinterest-inc-class-a-common-stock-rss-45b43224fdd9"
canonical_url: "https://medium.com/pinterest-engineering/from-clicks-to-conversions-architecting-shopping-conversion-candidate-generation-at-pinterest-04cae5e1455b"
published_at: "2026-04-27T16:01:05+00:00"
first_seen_at: "2026-07-20T04:35:24.920473+00:00"
fetched_at: "2026-07-28T22:15:36.111958+00:00"
content_hash: "sha256:57cbde1901540cfe309092022239369c03788844dbf9e81040f4c41224b677be"
---

# From Clicks to Conversions: Architecting Shopping Conversion Candidate Generation at Pinterest

Pinterest


Engineering


Monetization


Machine Learning


Recommendation Systems


# From Clicks to Conversions: Architecting Shopping Conversion Candidate Generation at Pinterest


[Pinterest Engineering](https://medium.com/@Pinterest_Engineering?source=post_page---byline--04cae5e1455b---------------------------------------)


7 min read


·


Apr 27, 2026


--


Authors: Richard Huang | Machine Learning Engineer II; Yu Liu | Senior Machine Learning Engineer; Ziwei Guo | Senior Machine Learning Engineer; Andy Mao | Staff Machine Learning Engineer; Supeng Ge | Sr. Staff Machine Learning Engineer


## Introduction


At Pinterest, conversion ads are crucial for matching users with products they are likely to purchase, boosting value for both users and advertisers¹. While conversion actions like checkout or add-to-cart are highly valuable, they are also technically challenging to optimize for. Because they occur offsite, conversion events are significantly sparser and noisier than onsite engagement signals. Historically, Pinterest’s shopping ads retrieval relied on engagement-based models. While effective for driving interaction, this system was not designed to optimize for lower-funnel conversions. This gap motivated us to build a dedicated candidate generation model tailored for conversions, aiming to surface higher-intent products and improve advertiser performance.


We launched our first shopping conversion model in 2023, achieving meaningful wins across both conversion and engagement, including a higher clickthrough rate (CTR). Further iterations in 2025 unlocked even stronger conversion value and improved Return on Ad Spend (RoAS) for our advertisers. This blog post documents our journey building this conversion candidate generation model, from its technical design and challenges to the key learnings of deploying it to our 600+ million monthly active users at Pinterest.


## Training Data Design


Modeling conversion events is challenging. Unlike frequent, real-time onsite engagements (e.g., clicks), offsite conversions are reported by advertisers, making the data sparse, noisy, and delayed. Despite these difficulties, conversions remain one of the most valuable signals for a purchase intent model, offering a far stronger indication of advertiser value and true user intent than engagement alone. To address the inherent sparsity of conversions, we made several key design decisions:


- **Multi-Surface Model:** We train a single model across all shopping surfaces (Homefeed, Related Pins, Search) to avoid fragmenting sparse conversion labels. At the same time, we incorporated surface-specific features to learn contextual differences between these surfaces.
- **Dual Positive Signals:** We supplement primary conversion signals with onsite engagement data (clicks, repins). This broadens data coverage, improving model generalization and ad funnel survival rates. To mitigate click data noise and decrease false positive clicks, we apply a log-based re-weighting function *w* based on the click duration:


Press enter or click to view image in full size


where *t* is the non-negative click duration in seconds and *tₘₐₓ* is a tunable constant used to cap the re-weighting function.


- **Negative Sampling:** On top of the existing in-batch negatives, we use ad impressions with no engagement as “harder negatives.” These samples can reflect the real distribution of served ads, exposing the model to a more representative inventory and promoting robust contrastive learning.


In summary, our multi-task approach uses engagement prediction as an auxiliary task to stabilize training and boost performance. The crucial challenge is balancing the two tasks, ensuring the high-value conversion signal is not diluted by the more frequent engagement data.


## Feature Engineering


At the core of our model are features that capture critical signals about our users and shopping catalog, grouped into two categories: User-side and Pin-side.


**User-side features** are split into two types. First, context features capture a user’s real-time intent, which is vital for applications like Related Pins and Search. Examples include a subject Pin’s visual and GraphSAGE² embeddings. Second, preference & historical features capture long-term interests for personalization. These include demographics, aggregated historical actions, and sequential data processed by a Transformer to create a user history embedding.


**Pin-side features** take a multi-faceted approach, incorporating ID features, multi-modal/ content features for semantic understanding, and performance features tracking engagement.


This structured representation of users and Pins ensures an effective matching process, delivering both personalization and relevance in recommendations.


## Model Architecture and Loss function Design


We use a two-tower model for retrieval, where user and Pin features are encoded separately, as there are no explicit user-Pin interaction features at this retrieval stage. To capture richer relationships among features within each tower, we employ DCN v2 (Deep & Cross Network v2)³ as the foundation of our cross layers. This enhances the model’s capacity to model non-linear interactions and boosts retrieval quality. After the cross layers, the output embeddings are fed into the final MLP head(s).


**1. Parallel DCN v2 and MLP Cross Layers Architecture** Early in our iterations, our cross-layer design was simple: a stacked architecture where DCN v2 cross network processed the input first, feeding its output into an MLP for dimension reduction. While efficient, we hypothesized that this sequential arrangement imposed a fundamental limit on the model’s learning capacity. To move beyond the sequential design, we designed a new parallel architecture by adding an MLP in parallel (see Figure 1). Its success stems from eliminating the primary drawback of a sequential flow: the information bottleneck. In the old setup, the MLP could only learn from features already processed by DCN v2, potentially losing valuable signals from the original input.


Press enter or click to view image in full size


Figure 1: Sequential (left) and Parallel DCN v2 and MLP (right) Cross Layers Architecture


In contrast, our parallel design allows both the cross network and the deep network to learn directly and simultaneously from the same input features. This effectively decouples the learning tasks, the cross network captures richer and more expressive explicit feature interactions by applying cross operations that combine the original input with each successive layer’s output to construct higher-order feature crosses, while the 3-layer MLP learns implicit abstract patterns in parallel. Because the cross network always references the original input at every layer, it constructs higher-order feature crosses without any information being lost or distorted by a preceding MLP transformation. The combined output of both funnels yields a richer and more expressive representation, unlocking a higher level of performance.


We applied this design to both the Pin and query towers, validating it on the conversion task where it delivered a **+11% gain in offline recall@1000** ⁴. Given its success in boosting core learning ability, particularly in its ability to surface stronger feature interactions while keeping a low latency for the retrieval task, this parallel architecture was subsequently **adopted by all our production engagement retrieval models** , achieving similar recall improvements as well as significant gains in online metrics.


**2. From a Multi-Head to a Unified Multi-Task Architecture** In the first version of our model, we designed a multi-head structure to comprehensively make use of the conversion data and engagement data. To leverage the relative abundance of click data, we used a **multi-head architecture** with shared encoders followed by engagement and conversion heads. The engagement head helped stabilize shared parameters, while the conversion head preserved the unique purchase-intent signal. The two heads were trained simultaneously using a distinct sampled softmax loss (see Figure 2). To balance the influence of engagement data without diluting the conversion signal, different loss weights were applied. At serving time, only the conversion Pin and query embeddings were used.


Press enter or click to view image in full size


Figure 2: Multi-head architecture, 2023 (left) and Unified multi-task architecture, 2025 (right)


Through in-depth data analysis and several online experiments, we identified sparsity and noise in the conversion labels as one of the main bottlenecks of the previous model performance. To better stabilize query embeddings in regions of low conversion coverage, we moved from a multi-head architecture to a **unified single-head multi-task architecture** (cf. Figure 2). By merging the conversion and engagement heads, it allows the final embeddings to directly benefit from the multi-task optimization during serving.


Building on top of this, we also observed that conversion data at the Pin level exhibit high variance, making it challenging to reliably model purchase intent from Pin-level supervision alone. To address this, we introduce an **advertiser-level loss function** as an additional training objective, enabling the model to better capture conversion signals at a more stable and consistent granularity. With other model improvements and feature additions, we saw on average an **increase of +42% recall@100** ⁴ for conversion tasks compared to our previous 2023 model.


## Conclusion


In summary, our modeling journey in crafting the shopping conversion candidate generation was driven by the necessity of overcoming the inherent sparsity and noise of offsite conversion events. We addressed this through a sequence of loss design and architectural innovations. Key modeling decisions included the adoption of a unified model across all surfaces and the strategic use of conversion and click duration-weighted engagement data. Architecturally, we leveraged a highly effective Parallel DCN v2 and MLP Cross Layers architecture, and we progressed from an initial separate multi-head design to an unified multi-task architecture that introduced an advertiser-level matching objective to better align with the natural granularity of the conversion signal.


Introducing this new CG to production in 2023 delivered a **2.3% increase in shopping conversion volume** and a **2.7% lift for the shopping impression to conversion rate** . Beyond conversions, it also improved the Pinners’ shopping experience, with **CTR increasing by 1.5%** and **CTR over 30 seconds rising by 2.2%** . Building on this foundation, further iterations and refinements throughout 2025 continued to push the model’s performance forward, resulting in a **3.1% improvement in RoAS** for US shopping campaigns⁴, reinforcing that strong advertiser outcomes and a great Pinner experience are not at odds, but deeply intertwined.


## Acknowledgments


Ads Retrieval: Yang Liu, Jay Ma (former), Peifeng Yin (former), Qingmengting Wang, Richika Sharan, Jitong Qi, Yufeng Su, Huiqin Xin


Ads Ranking: Weiwei Ying (former), Yiwei Sun (former), Aayush Mudgal, Hongda Shen, Han Sun


Ads Signal: Jiayin Jin (former), Daniel Yang (former), Chongyuan Xiang, Lakshmi Manoharan, Litian Tao, Siping Ji


Leadership: Alice Wu, Leo Lu (former), Ling Leng (former), Hari Venkatesan (former), Behnam Rezaei (former), Jamieson Kerns


## References


¹ A. Mudgal, et al. 2024.[Evolution of Ads Conversion Optimization Models at Pinterest](https://medium.com/pinterest-engineering/evolution-of-ads-conversion-optimization-models-at-pinterest-84b244043d51) . Pinterest Engineering Blog.


² W. L. Hamilton, et al. 2017.[Inductive Representation Learning on Large Graphs](https://arxiv.org/pdf/1706.02216) . In NIPS.


³ R. Wang, et al. 2020.[DCN V2: Improved Deep & Cross Network and Practical Lessons for Web-scale Learning to Rank Systems](https://arxiv.org/pdf/2008.13535) . WWW ’21: Proceedings of the Web Conference 2021.


⁴ Pinterest Internal Data, US, 2023 to 2025.
