---
schema_version: "1.0.0"
document_id: "f601b8a90530f0be441af18d23b79724b2c2f8483c97b48d4e6b6f2e2a708fbf"
company_key: "roku-inc-class-a-common-stock"
company: "Roku Inc."
source_id: "roku-inc-class-a-common-stock-rss-7fd84d003d99"
canonical_url: "https://engineering.roku.com/from-search-to-watch-the-power-of-rokus-search-ranking-system"
published_at: "2023-05-11T14:50:45+00:00"
first_seen_at: "2026-07-20T23:21:49.245762+00:00"
fetched_at: "2026-08-20T03:06:22.032514+00:00"
content_hash: "sha256:e77eaaf65452ae75412637461c7dc4d7acb6374dcbce0bbc71b3fcb5bd697e73"
---

# From Search to Watch: The Power of Roku’s Search Ranking System

**Authors** : Kapil Kumar, Rahul Agarwal, Ratul Ray, Thanh Dang


Figure 1 – Search Within the Roku Channel


Searching for information is a crucial aspect of our daily routines. At Roku, we understand the importance of using search to quickly and efficiently discover and access content on our streaming platform. We developed a **Search Ranking System** that makes it easier for users to find the content they want to watch.


In this blog, we introduce search ranking at Roku and discuss two use cases for the Search Ranking System: **content search ranking** **on The Roku Channel (TRC)** and **sports search ranking** **on the Roku Platform** (Roku Players, TVs and Streambars).


## Background


The Roku Channel is a free, ad-supported streaming service that offers thousands of movies and TV shows. Using the Search Ranking System, we have enhanced the user experience on TRC by showcasing pertinent results that are relevant to the user’s search query and context.


For instance, if a user searches for “swi” with the intent of finding “swimming with sharks,” the top results will display “swimming with sharks” itself, followed by query-related content like “Swingers” and “Switch.”


In another scenario, if a user has a Showtime subscription and types “bill,” they will see “Billions” as the top result, whereas “Billy the Kid” would be displayed if they don’t have a subscription.


Sports search ranking is another use case where we’ve implemented our Search Ranking System. (see Figure 2 below). With the rise of numerous sports streaming services, it has become increasingly important to help users **quickly find the games and events they’re looking for** . When a user searches for sports, our Search Ranking System uses factors such as the sports type, user’s interest, and fan engagement metrics to return relevant results


Figure 2 – Sports search on the Roku Platform


Our Search Ranking System is powered by a **Deep Learning architecture** that considers diverse features such as content metadata, user behavior, and search query words and phrases. However, during its development, we encountered **practical challenges** , such as managing queries with typos or misspellings and resolving queries with multiple possible interpretations.


We conducted both offline and online experiments to evaluate the effectiveness of our Search Ranking System. Offline experiments involved measuring its performance on a **held-out dataset** , whereas online experiments focused on assessing the model’s performance in a **live production environment** . Additionally, we employed techniques such as **Integrated Gradients** and **Interaction Importance** to measure the significance of various features in our model.


In summary, our Search Ranking System has significantly **enhanced the user experience** on Roku by delivering relevant results rapidly and efficiently. Leveraging our Deep Learning architecture and practical solutions to challenges, we have developed a robust system that helps users quickly locate the content they desire.


## Search Query Lifecycle


To comprehend how our new Search Ranking System operates, it’s essential to **understand how the search process works** . When a user initiates a query, the search process involves two significant steps, which are illustrated in Figure 3:


1. **Candidate Retrieval:** In this step, the system identifies a set of potential candidates, such as movies or sports events, using both lexical and semantic matches that may be relevant to the query.
2. **Reranker:** The system then reorders these candidates based on relevance before returning the results to the user.


The following sections will focus on the **Search Neural Ranker** architecture, its **Layers** and **Multitask** components, applications for Roku, and features of the resultant model. We will also analyze the significance of various layers using **Integrated** **Gradients** and **Layer** **Conductance,** and assess offline and online metrics.


## Search Neural Ranker


Previous search rankers on the Roku platform relied on logistic regression that utilized click-through rate (CTR) of content and query level CTR signals. However, we chose to move to a Deep Learning-based architecture because it can learn complex, non-linear relationships between the query and potential results. In doing so, the system can identify and rank relevant results for users, even when the relationships between the query and results are not easily captured by simple algorithms.


We experimented with **Deep** **Learning** **Recommendation** **Model** (DLRM) and **Deep** **Cross** **Network** (DCN) architecture for the ranking task and chose DLRM because it showed slightly better performance in offline analysis. The Search Neural Ranker architecture based on DLRM shown in Figure 4 is used in the Search Ranking System for ranking movies and sports.


Figure 4 – Search Neural Ranker Architecture


### Multi-Task Ranker Optimizes a Key Metric


The evaluation of a search algorithm benefits from considering click as a metric, but for our scenario, streaming hours are the key metric. To increase streaming hours, we extended the DLRM architecture with two separate **Multi-Layer Perceptrons** (MLP) at the top, creating fully connected layers to enable the learning of multiple tasks. These tasks include the probability of a click and the probability of playing or streaming. Figure 5 below shows the architecture.


Initially, we used the average of click-and-play probabilities to rank items. However, we plan to improve this in future work by using online tuning to adjust the weight of click-and-play probabilities.


Figure 5 – Multi-Layer Perceptrons for Learning Click Probability and Play Probability


## Details of Each Layer in the Search Neural Ranker


The **Feature Representation Layer** in our model is designed to handle both categorical (or sparse) and continuous (or dense) features.


1. **Categorical** (or sparse) features: We employ an embedding layer that is learned for every each individual feature. The size of each embedding layer is determined by the cardinality of the feature multiplied by dimension size.
2. In contrast, **Continuous** (or dense) features are passed through an MLP. The output of the MLP has the same size as the embedding size used for sparse features.


### **Feature Interaction Layer**


In the Feature Interaction Layer, we explicitly compute second-order interactions of different features using the intuition provided in[Factorization Machines](https://www.csie.ntu.edu.tw/~b97053/paper/Rendle2010FM.pdf) (FM) for handling sparse data. This is accomplished by taking the dot product between all pairs of embedding vectors and processed dense features, as expressed by the formula shown in Figure 6 below.


Figure 6 – Second Order Interaction Calculation


### **Top Layers**


The dot products from the interaction layer are then concatenated with the original processed dense features and post-processed with two different MLP layers. Finally, the outputs are fed into a sigmoid function to produce the click probability and play probability as outputs in the top layers.


## Two Use Cases – Sports Ranker and TRC Ranker


We have implemented two use cases, the Sports Ranker and the TRC Ranker, to address the need for different approaches when recommending content to users. Since content types can differ significantly, a one-size-fits-all approach is not optimal.


When it comes to sports programs, they are often ephemeral, meaning that a particular content ID may not have any past clicks or interactions. Unlike movies, sports programs have a short lifespan and are not watched repeatedly. Therefore, for sports programs, we need to attribute clicks and events at a team level, rather than at a content level. This is because a user’s interest in a team is likely to be more enduring than their interest in a particular game or match.


## Features


In the first version of the models, we started with various user/content/query/context features some of the example features are


shown in Table 1 (TRC Content Ranker) and Table 2 (Sports Ranker) below.


TRC Content Ranker


Feature Name Type Description


profile_id Categorical Profile Id of the user that identifies the profile uniquely


search_query Categorical Query used by the user to find the content


ctr Dense Query level ctr


Table 1 – Example Features for Content Ranking


Output Labels: Click Probability, Play/launch Probability


Sports Ranker


Feature Name Type Description


league title Categorical League Title


search query Categorical Query used by the user to find the content


time of event Dense Time of event


Table 2 – Example Features for Sports Ranking


Output Labels: Click Probability, Play/launch Probability


Offline Evaluation


We trained the model on one month of data for offline evaluation and used it to predict click-and-play probabilities for the next two days of data, using the same ranker. The results were ranked based on the average of click and play probabilities, as shown in Table 3 below.


To evaluate the effectiveness of our Search Neural Ranker, we utilized Mean Reciprocal Rank (MRR) as the evaluation. MRR is an information retrieval metric that measures the effectiveness of a ranking system by calculating the average of the reciprocal ranks of the first relevant search results. In other words, it considers the position of the first correct answer in the ranked search results list. The formula for MRR is:


MRR = (1 / Q) * Σ (1 / rank_i)


where Q is the total number of queries, and rank_i is the position of the first relevant result for the ith query. The higher the MRR, the better the ranking system performs, with an ideal MRR of 1 indicating that the relevant search results always appear at the top of the list.


Using MRR as a metric, we evaluated the effectiveness of our Search Neural Ranker in providing users with relevant search results. Our system produced a MRR gain of 9.6% in Movies and Series MRR and 2.2% for Sports MRR, indicating significant improvement in the ranking system’s performance.


Metric Previous Ranker Change from Previous Ranker


Ranker with No Interaction Layer Ranker with Dot Interaction Layer (DLRM)


Movies and Series[MRR](https://en.wikipedia.org/wiki/Mean_reciprocal_rank) (Mean Reciprocal Rank) Baseline +7% +8%


Sports[MRR](https://en.wikipedia.org/wiki/Mean_reciprocal_rank) (Mean Reciprocal Rank) Baseline +1% +2%


Table 3 – MRR for Content Search and Sport Search across 3 Ranking Approaches


## Online Metrics


The results of the A/B test (shown in Table 4 below) were positive, indicating that our treatment model, the new Search Neural Ranker, outperformed the control model (the production model at the time). The statistical significance of the tests and the positive outcomes allowed us to replace the production model with the Search Neural Ranker.


Metric


Metric Description


SportsRanker


Movies and Series Ranker


Search Sessions CTR The Click Through Rate for sessions involving search. +1% +1 %


Search MRR Average Mean Reciprocal Rank for search sessions. +1% +5 %


Search Query Length Average query length for search sessions -3% -2 %


Search Streaming Hours Streaming hours from search sessions. – +1%


Search Sessions Abandonment rate Sessions where the user didn’t focus/click on search results. – -4%


Bounce Sessions which were abandoned without any search result focus/click – -0.5%


Table 4 – A/B Test Results


## Practical Challenges


1. **Hash** **Collisions:** **** The hashing trick is a useful method for dealing with high-cardinality features, i.e., features with many categories. It applies a hash function to the values in the feature to map features to a lower-dimensional space, enabling their use as input to machine learning models. This is particularly helpful since many machine learning algorithms cannot handle high-cardinality features directly. However, one potential drawback of the hashing trick is that it can result in collisions, where different values are mapped to the same hash value, thereby degrading the model’s performance. We have observed a hash collision rate of 18-20 % for our features. To address this issue, we use two hash tables or embedding layers for each feature.
2. **Inference Type AWS:** C hoosing the correct computing infrastructure is essential for the model to meet the inference SLA. For our model here, in our testing, we see the model is memory intensive, and based on experiments, a memory-optimized instance type in AWS Sagemaker turned out to be most


cost-effective while meeting latency constraints.


## Summary


We have implemented a sophisticated ranking algorithm on the Roku streaming platform to enhance the user experience and increase user interaction. Our main goal was to improve the content discovery process for users by optimizing search results. Doing so would boost user engagement, satisfaction, and loyalty, increasing retention rates and revenue growth for Roku. Integrating the new ranking algorithm has resulted in several positive outcomes, including improved content discovery and increased user engagement on the Roku platform.


## Appendix


- TF recommenders[https://www.tensorflow.org/recommenders/](https://www.tensorflow.org/recommenders/) .
- DLRM Papers:[https://arxiv.org/pdf/1906.00091.pdf](https://arxiv.org/pdf/1906.00091.pdf) ,[https://arxiv.org/pdf/1906.03109.pdf](https://arxiv.org/pdf/1906.03109.pdf)
- DLRM Tutorial:[https://captum.ai/tutorials/DLRM_Tutorial](https://captum.ai/tutorials/DLRM_Tutorial)


The post[From Search to Watch: The Power of Roku’s Search Ranking System](https://engineering.roku.com/from-search-to-watch-the-power-of-rokus-search-ranking-system) appeared first on[Engineering Blog](https://engineering.roku.com/) .
