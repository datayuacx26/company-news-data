---
schema_version: "1.0.0"
document_id: "21f85c6c95cf6177f96b1428ac2156d1bf353db1b3695bf272acdf17218ac017"
company_key: "target-corporation-common-stock"
company: "Target Corporation"
source_id: "target-corporation-common-stock-rss-2844690dfd24"
canonical_url: "https://tech.target.com/blog/synthetic-digital-orders"
published_at: "2025-07-22T05:00:00+00:00"
first_seen_at: "2026-07-20T03:31:39.462984+00:00"
fetched_at: "2026-07-28T20:56:43.889340+00:00"
content_hash: "sha256:1448e2fd8c29e4644eebf3de8c66dea37958763402f10a8c132197e41f8d9658"
---

# Unlocking the Power of Synthetic Digital Orders in Retail

Retail businesses like Target are turning to synthetic digital orders to optimize operations, streamline supply chains, and enhance guest experience. These AI-driven, simulated orders mimic real-world transactions to help retailers improve everything from inventory management to predictive analytics.


Target’s Data Sciences team developed a new product called


Demand Profiler (DP).


It predicts the set of items most likely to be ordered digitally at a given time in future. This post will explore what Demand Profiler does, how it can transform retail operations, and highlight the key business use cases powered by this technology.


What are DP Orders?


Demand Profiler generates synthetic digital orders, which are simulated transactions designed to closely replicate real-world guest behavior in Target’s digital fulfilment channels but without the actual exchange of goods or services. These synthetic orders help with profile generation, process testing, and predictive modelling.


Fig 1.1 The input, output and internal components of the demand profiler.


Fig 1.2 A sample output from the Digital Profiler.


DP


uses sophisticated algorithms to capture the complexities of real transactions. It simulates demand by guest region, inventory fluctuations, and shopping behavior to produce rich, dynamic order profiles.


Key Features:


- AI-Driven Generation:


Uses machine learning to create realistic and evolving order sequences.


- Data-Rich:


Captures detailed order information such as item quantity, number of orders, and guest region.


- Real-Time Adaptation:


Responds to changes in market trends, guest behavior, and external factors like seasons or promotions.


Business Use Cases of DP Orders in Retail


Let’s look at how DP is driving real impact across retail operations.


1. Labor planning through ship-from-store unit forecasting.


Matching store labor to digital demand is a major challenge in retail, especially during demand spikes or promotional periods. DP orders help store operations by simulating future demand patterns, enabling better staffing decisions.


How It Works:


- Store operators can consume units allocated to a store using an


Order allocation simulator.


The simulator uses DP orders as forward-looking input that has the in-built signal of high demand/regular demand.


- These synthetic orders are aligned to expected demand from the


Forecast Engine,


providing a clearer picture of future product sales.


Fig 3.1 The bottoms-up approach to generating a simulation-based approach.


Benefits:


- Improved labor planning at stores by anticipating demand fluctuations.


- Reduced under- or over-utilization of resources.


Performance impact:


Achieved a ~40% improvement in future unit allocation accuracy.


(Analysis is ongoing to quantify the value impact on labor planning improvements.)


Fig 4.1A pre/post comparison of the metric after developmental changes were implemented


2. Inventory optimization at fulfillment centers.


When an item appears frequently in multi-item orders, it becomes important to reserve their planned inventory at our fulfillment centers to promote consolidation. The more consolidation, the lower the ship expense.


How It Works:


- DP can provide future single- to multi-line order ratios using specific order-level features such as items per order, orders per item, etc.


- Inventory planners can use this data to identify items having a high chance of appearing in a multi-item order, thus enabling analysts to reserve their inventory at fulfillment centers.


Fig 3.3 An example of how consolidation can be achieved at a FC.


Benefits:


- Improvement in order consolidation


- Reduction in excess packages


Performance impact:


Units consolidation has improved by 2x at fulfillment centers.


Fig 4.3 Day-wise Zone: lower distance covered to fulfil guest orders due to consolidation.


Key Techniques/Algorithms


It’s important to understand the underlying techniques/algorithms that power DP because they enable the creation, management, and analysis of synthetic orders at scale.


Conditional Probabilistic Models


: Using historical transaction data, we estimate the probability distribution for every feature of a synthetic order:


Fig 2.1 Different features of a synthetic order for which we estimate the probability distribution


The conditional probability of an order occurring as a single item order (A) given a guest destination region (B) can be formulated as:


P(A|B) = P (A ∩ B) / P(B)


Where,


- P (A ∩ B)


represents the probability of both events, i.e. number of single-item orders from a given guest region.


- P(B)


represents the probability of event B, i.e. number of orders from a given guest region.


2. Restricted Random Sampling:


DP generates orders using random sampling technique and conditional probability distribution models for every feature mentioned above. However, the numbers of orders and units generated are restricted by an item units alignment algorithm to meet forecasted demand for every item. This ensures we are not generating another demand forecast for the items appearing in these orders.


Random sampling


: The chance of getting a sample selected only once is:


P = 1 –((N-1/N)*(N-2/N-1)…..(N-n/N-(n-1)))


where,


P


is the probability,


n


is the sample size,


and


N


is the population.


Fig 2.2 Step-by-Step process to generate forecast aligned item sets using restricted sampling


3. Tools/Technologies:


The code is written in PySpark with Python at its core enabling code re-usability, scalability and modularity.


Product Accuracy


Fig 2.3 Accuracy of order features/components.


Conclusion and Future


Synthetic digital orders are revolutionizing the way retailers manage their operations, predict demand, and optimize guest experiences. From inventory management and supply chain testing to personalized marketing and workflow optimization, the potential applications of these synthetic orders are vast. As technology evolves, retailers who embrace this innovative approach will be better positioned to meet guest expectations, minimize operational risks, and drive growth.


For tech professionals, mastering the techniques to capture guest behavior is critical, and getting highly accurate item sets through DP is one of the major next steps. We are exploring new areas like Markov processes and leveraging existing products from data sciences, such as item affinity


models


,


to improve similar item sets in DP orders.


## RELATED POSTS


### Elevating Guest Repurchasing Behavior Using Buy It Again Recommendations – Part 2


By Pushkar Chennu, Amit Pande, Rankyung Park, David Relyea, and Prathyusha Kanmanth Reddy, March 4, 2025


This article introduces a new model for BIA recommendations using item- and category-level Hawkes processes.


### Improving Accessory Recommendations with LLMs at Target


By Amit Pande, David Relyea, AJ Dabruzzi, Longjiang Yang, Rankyung Park, and Alina Nesen, July 1, 2025


This GenAI-based algorithm makes it easier than ever to pair the right accessory with the right product.
