---
schema_version: "1.0.0"
document_id: "43a44cf808c95bf9af5a4d00a665b822e064cadee5c2a7cc89b26f03098a41d4"
company_key: "target-corporation-common-stock"
company: "Target Corporation"
source_id: "target-corporation-common-stock-rss-2844690dfd24"
canonical_url: "https://tech.target.com/blog/buy-it-again-part-2"
published_at: "2025-03-04T06:00:00+00:00"
first_seen_at: "2026-07-20T03:31:39.462984+00:00"
fetched_at: "2026-07-28T20:58:14.920102+00:00"
content_hash: "sha256:7d638830205a7037e74e5ae90a90b2b8e1fe5d9a84c7bbbadc35f4ec325db936"
---

# Elevating Guest Repurchasing Behavior Using Buy It Again Recommendations – Part 2

Buy It Again (BIA) recommendations play a key role for retailers like Target in enhancing customer experience and boosting site engagement. Guests appreciate the convenience of quickly adding their frequently purchased items to their cart. BIA recommendations predict items that they’re likely to repurchase based on their past buying patterns.


This article introduces a new model for BIA recommendations using item- and category-level Hawkes processes. We call it the


[Short- and Long-term Hawkes process model](https://dl.acm.org/doi/10.1145/3626772.3661374) , or


SLH-BIA


. It offers three key contributions:


- A novel Hawkes process-based approach to BIA recommendations.


- Improved performance compared to traditional models.


- Successful deployment in a commercial setting, with positive A/B test results showing over a 30% increase in click-through rate and approximately a 30% revenue increase.


Leveraging Hawkes Processes for Enhanced BIA Recommendations


The Hawkes process is a mathematical model used to forecast future events based on past occurrences. It captures the self-exciting nature of events, where past events increase the likelihood of future events within a specific time window. The model measures how past events influence the emergence of new events over time. These self-excitation values serve as repurchase scores, which are used to rank items.


Applying a Hawkes process with an Exponential distribution to the repurchase probability of a product—illustrated in Figure 1 with strawberries—provides insights into customer behavior. This model shows how the likelihood of purchasing an item again changes over time since the last purchase, effectively capturing repeat-purchase patterns.


Figure 1. Hawkes process modeling of repurchase probability for strawberries using the self-excitation function with Exponential distribution.


However, some products, such as shampoo, have longer repurchase cycles compared to others, such as strawberries. To capture both short-term and long-term trends, we combine Exponential and Normal distributions within the self-excitation function, as shown in Figure 2. This approach addresses various repurchase frequencies, offering valuable insights for customer repurchase strategies.


Figure 2.


Hawkes Process Modeling of short-term and long-term repurchase patterns by combining Exponential distribution and Normal distribution in the self-excitation function.


In the SLH-BIA model, the self-excitation function defines the intensity for repurchase at both item and category levels by considering previous purchases of an item and the same product category. It combines Exponential and Gaussian distributions to account for both short-term and long-term aspects. Additionally, the SLH-BIA model differs from previous approaches, such as short-term and life-time repeat consumption (SLRC) model, by avoiding collaborative filtering due to its high time complexity and its non-essentiality for BIA performance.


PCIC SLRC SLH-BIA


NDCG@10 0.062 0.143 0.131


Training Time (hrs) 2 1300 250


Table 1: Model performance and training time on 1% and 100% Target sales dataset, respectively. Normalized Discounted Cumulative Gain (NDCG) is a ranking quality metric, comparing rankings to ground truth where all relevant items are at the top of the list. @10 indicates the number of items in predictions used for NDCG evaluation is ten.


Table 1 compares the performance and training times of PCIC, SLRC, and SLH-BIA. Our existing deployed baseline, a


hierarchical


model consisting of a personalized category model and a personalized item model within categories (PCIC), as a category-level model, has a relatively small model size and dataset, resulting in shorter training times compared to other models. In contrast, SLRC are item-level models, and SLH-BIA is an item- and category-level model, which handles a much larger item dimension and subsequently have larger model sizes. While SLRC and SLH-BIA show similar performance in terms of NDCG@10, SLRC takes significantly longer to train due to its collaborative filtering component. This makes SLH-BIA our ideal candidate for production-scale training on billions of customer transactions.


Enhancing model scalability


To improve the SLH-BIA model, we've made several adjustments to decrease execution time and enhance model scalability, allowing for better management of an evolving product catalog and real-time data analysis. These improvements include:


- Data Parallel Execution


: Using Spark Scala to split data into manageable chunks, reducing processing time and memory usage.


- Training Data Sampling


: Subsampling the training data using a Samples Per Item (SPI) threshold. We found that an SPI between 5k and 10k yielded optimal NDCG values and completed training within 24 hours, as detailed in Table 2. This was achieved by selecting the longest repeat-purchase transactions for each item, rather than random sampling, which might inadvertently omit items.


Table 2: Performance of SLH-BIA for different SPI and comparison with PCIC (our existing deployed baseline). Pre-processing and training times (hrs) with different sizes of training data and model performance.


- Model Compression


: Instituting an N threshold to omit items unpurchased more than N times in a year, with N=1 using the full catalog. Increasing N excludes rarely bought items, trimming the filtered catalog. This method presents a trade-off: Training on the frequency-based filtered catalog may generate inaccurate recommendations, while shrinking the model by removing less-purchased items could help reduce latency, albeit with the risk of unintentionally omitting potential repeat purchases.


Table 3: The impact of using different item repurchase thresholds (N) in SLH-BIA vs our existing deployed baseline, PCIC. Listed are the CTR and AD lift as well as the training time and inference latency. SLH-BIA variations use SPI set to 5K


To test this hypothesis, we A/B tested several variants of SLH-BIA against our baseline, PCIC. The test was run on 20% randomized traffic for each variant over two weeks. As shown in Table 3, removing infrequently purchased items reduced computational complexity while enhancing model performance. Consequently, we significantly reduced model training time from roughly 250 hours to about 3 hours, while serving real-time inference with less than 70ms latency.


Evaluation


We compared our BIA model against state-of-the-art baselines using three publicly available datasets: ValuedShopper, Instacart, and Dunnhumby. Table 4 shows our model outperforms state-of-the-art baseline models in recall and NDCG metrics by around 85% and 10%, respectively. Additionally, Table 3 shows that from A/B tests with millions of live customers, our model exhibited more than a 30% increase in click-through rate and roughly a 30% revenue increase compared to our existing deployed baseline, PCIC.


Table 4: Performance comparison of SLH-BIA model with existing baselines. Top 2 models are bolded.


References


Links to datasets in table:


[Valued Shoppers Challenge](https://www.kaggle.com/c/acquire-valued-shoppers-challenge/overview) ,


[Instacart Market Basket Analysis](https://www.kaggle.com/c/instacart-market-basket-analysis) ,


[dunhumby](https://www.dunnhumby.com/source-files/)


Links to other approaches in table:


Repeatnet:


P Ren, Z Chen, J Li, Z Ren, J Ma and MD Rijke "Repeatnet: A repeat aware neural recommendation machine for session-based recommendation. Proc AAAI Vol. 33. 4806–4813.


Sets2Sets:


H Hu X He “Sets2sets: Learning from sequential sets with neural networks” KDD 2019 1491–1499.


PG:


R Bhagat, S Muralidharan, A Lobzhanidze and S Vishwanath "Buy-it-again: Modeling repeat purchase recommendations" KDD 2018 62–70.


MPG:


TIFUKNN:


Hu, Haoji, et al. "Modeling personalized item frequency information for next-basket recommendation." Proceedings of the 43rd International ACM SIGIR Conference on Research and Development in Information Retrieval. 2020.


UPCP


: Faggioli, Guglielmo, Mirko Polato, and Fabio Aiolli. "Recency aware collaborative filtering for next basket recommendation." Proceedings of the 28th ACM Conference on User Modeling, Adaptation and Personalization. 2020.


DNNTSP


: Yu, Le, et al. "Predicting temporal sets with deep neural networks." Proceedings of the 26th ACM SIGKDD International Conference on Knowledge Discovery & Data Mining. 2020.


ReCANet


: Ariannezhad, Mozhdeh, et al. "ReCANet: A repeat consumption-aware neural network for next basket recommendation in grocery shopping." Proceedings of the 45th International ACM SIGIR Conference on Research and Development in Information Retrieval. 2022.


PCIC


: Pande, Amit, Kunal Ghosh, and Rankyung Park. "Personalized Category Frequency prediction for Buy It Again recommendations." Proceedings of the 17th ACM Conference on Recommender Systems. 2023.


SLRC


: Wang, Chenyang, et al. "Modeling item-specific temporal dynamics of repeat consumption for recommender systems." The world wide web conference. 2019.


SLH-BIA


: Park, Rankyung, et al. "SLH-BIA: Short-Long Hawkes Process for Buy It Again Recommendations at Scale." Proceedings of the 47th International ACM SIGIR Conference on Research and Development in Information Retrieval. 2024.


## RELATED POSTS


### Elevating Guest Repurchasing Behavior Using Buy It Again Recommendations


By Amit Pande and Rankyung Park, November 2, 2023


Target’s Data Science team shares an inside look at our Buy It Again model.
