---
schema_version: "1.0.0"
document_id: "c1e62e910b1e96944b49023d87a2d3ccebe92ee98989bc9d3f245a2f51d4d356"
company_key: "roku-inc-class-a-common-stock"
company: "Roku Inc."
source_id: "roku-inc-class-a-common-stock-rss-7fd84d003d99"
canonical_url: "https://engineering.roku.com/personalised-retrieval-for-typed-text-queries"
published_at: "2024-09-13T12:42:25+00:00"
first_seen_at: "2026-07-20T23:21:49.245762+00:00"
fetched_at: "2026-08-20T03:06:22.032514+00:00"
content_hash: "sha256:f9947f4a6e60b9a3c7c62f010a6d2d475c811c127a4852e4f277aec1900223b5"
---

# Personalised Retrieval For Typed Text Queries

**Authors** : Kapil Kumar, Abhishek Majumdar,


Nitish Aggarwal,


Rahul Agarwal, Karim Lulu


## Introduction


Roku Search has transformed the way users discover content on their televisions, making it easy to find their favorite movies, TV shows, cast & crew, live sports, and news across various streaming platforms. By acknowledging the significant popularity of search among Roku users, we will highlight its role in driving engagement and keeping users connected to the content they love.


While web search is a primary feature on desktop and mobile platforms for user discovery, TV search presents its own unique challenges. Namely, since not all of the Roku’s TV remotes support voice search, typing with the remote requires significant effort as users must select each character individually on the screen. This sets an expectation for users to find their desired content with just a few characters. Such brevity demands a robust search architecture capable of effectively interpreting short user queries and delivering relevant content. Various factors, such as user preference, seasonality, trendiness, and popularity, influence the relevance of candidates, making the task of generating a relevant candidate pool critically important and extremely challenging.


In this blog post, we will explore how Roku generates personalized candidates for text queries by understanding user preferences and the implicit intent expressed in partially typed short queries, ultimately increasing the effectiveness of our multi-stage ranker for better optimization. We will share our journey in developing a personalized deep retrieval model, which includes a query embedding generator with multi-query projections, a dual encoder, and user-item embeddings using a graph neural network. Additionally, we will discuss how we addressed issues related to popularity and presentation bias in both training samples and user behavior.


## Search Architecture


Roku’s Platform hosts millions of items, necessitating an efficient and scalable search system. To address this challenge, Roku employs a two-layer


search approach to find relevant results as shown in Figure 1. The first layer, also called the Retrieval Layer, identifies a set of potential results that match the user’s query from multiple retrieval sources. This is


followed by a multistage Ranking Layer that optimizes item placement/position for each user.


The Retrieval Layer brings results from a lexical search engine i.e elastic search and a few dual-encoder models. The lexical search engine


indexes all the content that can be searched on Roku across various verticals like sports, movies, series and so on. The Personalized dual encoder model is trained on past user/item interactions that takes the query and other user features and generates a set of relevant and personalized candidate results. There are other dual encoder models that focus on specific aspects like boosting non popular items, language diversity and so on.


The second layer is the Ranking Layer. Here, the candidates from retrieval go through a series of reordering steps. These rank the items considering a variety of factors, including the similarity between the query and the candidate results, the popularity of the candidate results, the user’s viewing history, revenue


and more.


In this post we will focus on one of the main retrieval sources, the Personalized Dual Encoder.


## How We Retrieve / Personalized Dual Encoder Model


We will start with the dual encoder model that given a user query retrieves the most relevant movies and TV shows on Roku. The dual encoder model is a neural network that can be used to generate candidate results for search engines. As shown in Figure 2, the model consists of two towers: the User + Query tower and the Content tower. The User + Query tower, as the name suggests, takes user and query signals and encodes it into a dense vector representation. This tower is responsible for understanding a user’s intent via their past behavior and query using the user + query interaction data with content. Similarly, the content tower takes the content signals which are being searched. The content tower also encodes these signals into a dense vector representation. The two towers are trained to produce embeddings with a high similarity score for positive combination and lower similarity scores for negative combination. Our retrieval layer incorporates a diverse set of features:


- User-Centric Features:


- Graph Embeddings: Derived from user interaction history
- Demographic Information: Utilizing user-specific data


- Query-Based Features:


- Multi-Head Query Projections: Capturing various aspects of the query
- Custom FastText Model: Trained specifically on Roku search data


- Item-Specific Features:


- Graph Embeddings: Based on historical interaction data
- Performance Metrics: Including search popularity, click-through rates, and launch statistics
- Metadata Attributes: Such as release year and child-friendliness
- Temporal Context: Incorporating time-of-day information


By leveraging these diverse feature sets, our retrieval layer can provide more accurate and personalized search results. Let’s get into the detail of how we trained this model and the training data used for this model.


### **Training Data Augmentation**


We possess a highly valuable dataset derived from past user interactions, i.e the User Query and Item which was launched by the user. These pairs are referred to as past user interactions. While this data is already impressive, we enhance its utility by applying innovative techniques. One such technique is known as query prefix expansion. While the term may sound complex, the concept is quite straightforward.


Consider a user typing the query “harry.” Instead of using just this single query, we generate an extensive set of training data from it by simulating the user’s typing process incrementally.


This process yields the following sequence:


1. “h”
2. “ha”
3. “har”
4. “harr”
5. “harry”


Each of these sequences becomes a distinct entry in our training data.


The rationale behind this approach is twofold. First, it significantly increases the volume of data available for training, which enhances the model’s performance. As is well-known, more data typically results in a better model. Secondly, this method is particularly beneficial for typeahead search functionality.


In a typeahead search, as users begin typing their query, results are shown immediately. By including these shorter versions of the query, we enable our model to start making predictions earlier in the typing process. This means the model does not need to wait until the full query, “harry,” is typed out before it begins generating results.


This approach is highly effective in improving the responsiveness of our search functionality. The model can start suggesting results more quickly, thereby creating a smoother and faster user experience.


### **Strategy for Negative Sampling**


In the previous section, we augmented the training data with positive samples but we also need to have negative samples. In the most basic scenario, the dual encoder model normally creates negative samples on the fly by using in-batch


data as negative samples. In-batch negatives leverage other samples within the same training batch as negative examples. For a given positive pair (anchor and positive sample), other samples in the batch are treated as negative examples. This approach creates a larger set of negative samples without additional computational overhead. For example, if our batch was as given below, we will use mad max as a negative sample for query “h” and harry potter as a negative example for query “m”.


**SNo.** **Query** **Item Id** **Item Name** **Popularity log(clicks of last 30 days)**


Q1 h I1 harry potter 3


Q2 ha I1 harry potter 3


Q3 m I2 Mad max 1.5


We also include some random global negatives, which are pulled from the global dataset to cast a wider net. However, one common issue we found with negative sampling was that we were getting accidental hits. It sounds like a happy accident, but it’s not what we desire. Let’s consider the three examples in a batch in the above table again. We will also end up taking harry potter as a negative example for h and that is not what we want. So we removed the negative samples if we see the same Item occur as both positive and negative as you can see in the table below.


**Positive example Pairs** **Negative Example pairs** **Negative example pairs after removing accidental hits**


\[Q1, I1\] \[Q1, I1\] ← Accidental hit


\[Q1, I2\]


\[Q1, I2\]


\[Q2, I1\] \[Q2, I1\]← Accidental hit


\[Q2, I2\]


\[Q2, I2\]


\[Q3, I2\] \[Q3,I1\] \[Q3,I1\]


**Negative Sampling Bias**


The next thing we noticed was that while training the dual encoders is that popular items get picked up much more often as a negative example, which doesn’t allow good learning of their embeddings. To deal with this we pass on an additional feature called candidate sampling probability that is used to modify the value of the logits. More details on this method


can be found[here](https://www.tensorflow.org/extras/candidate_sampling.pdf) .


In the chart below, V1 refers to version with just in-batch negatives and V2 refers to version with in-batch negatives plus accidental hit removed and negative popularity bias removed using candidate sampling probability. We observe that after removing accidental hits and negative popularity bias, recall@12 was 0 for fewer popular items.


## Feature Engineering


Now let’s move to the methods used to train the model, including feature engineering. Our model contains both numerical and categorical features. Numerical features can be easily used in our model but we had to work harder to use categorical features because of their sparsity. In our model, we employ embedding representations for all sparse features, including Query


, Item ID, and Item Description. Initially, we used a single hash function and embedding table for each feature to manage categorical variables efficiently without excessive memory usage.


Let’s denote the number of unique values


for a feature as N and the number of hash buckets as K. The expected number of a hash collisions for any two items can be approximated by:


```text
E(Number of Collisions) = N - K + K(1 - 1/K)^N
```


Let’s understand with an example. Lets say we have N ≈ 1,000,000 and K = 1,000, this yields:


```text
E(Number of Collisions) ≈ 1000000 - 1000 + 1000(1-1/1000)^1000000 = 999000
```


This collision meant that we are suffering a high rate of collisions and that is natural as we are putting 1000000 items in 1000 buckets. To address this, we developed composite embeddings. This approach uses two tables for each feature, each with a distinct hash seed. The expected number of collisions occurring in both tables is given by the same formula but the fact is that we effectively have K^2


buckets now:


```text
E(Number of Collisions) ≈ 1000000 - 1000000 + 1000000(1-1/1000000)^1000000 = 367880
```


This reduces the collision rate by 63%. The composite embedding for a feature value is created by concatenating or combining the embeddings from both tables:


```text
E_composite(x) = [E_1(h_1(x)); E_2(h_2(x))]
```


where E_1


and E_2


are the embedding lookups for the two tables, and h_1


and h_2


are the corresponding hash functions.


This approach


is a variation of composite embeddings and significantly improved our model’s accuracy and performance by preserving more of the unique information in our categorical variables.


### **Query Representation with Multiple Projections**


In our model, query representation poses a unique challenge due to the nature of TV search interfaces. Unlike traditional keyboard-based searches, TV users typically input only a short sequence of characters using remote controls, significantly constraining the search query’s informativeness.


Thus, for Query Representation, We have two types of representations:


- **FastText fine-tuned embeddings for short strings** : We utilize word2vec-style embeddings trained on title data, augmented to better handle short input strings. This method allows us to capture semantic meaning even from minimal character input, crucial for TV search scenarios.
- **Multiple query projections** : When we were fixing the collision problem, we stumbled onto something cool


. As we increased the number of embedding tables for query feature, Our model was getting smarter! We experimented and found that three projections worked best. After that, we didn’t see much improvement.


Sometimes the first projection, sometimes it’s the second or third projection which helps the most.


The power of this approach lies in its ability to capture different aspects of the query:


```text
E_query = [E_1(h_1(q)); E_2(h_2(q)); E_3(h_3(q))]
```


An example for the query c is shown below in Figure 4. Assuming that the correct result name starts with the letter c or any word in the title starts with the letter c, we can see that with One Projection we see 6 wrong results, for Two Projections we see 6 wrong results, while for Three Projections we see 3 wrong results. Together, they’re bringing in more relevant results than before.


The effectiveness of this method can be attributed to the complementary nature of the projections. Each projection may excel at capturing different aspects of the query, collectively providing a more comprehensive representation. This synergy allows our model to surface more relevant results than previously possible, effectively turning a technical solution for hash collisions into a powerful tool for improving search quality.


### **Personalization**


Personalization plays a crucial role in understanding user preferences and retrieving the most relevant matches for their queries. To address this challenge, we developed a specialized Heterogeneous GraphSage Neural Network. We also use embeddings from this model as input to the dual encoder model. This advanced system constructs comprehensive “Taste Profiles” for users by analyzing multiple data points:


1. User-item interactions
2. User demographics
3. Item characteristics, including visual and textual data


Recognizing that user interests are dynamic and evolve over time, we implemented novel training methodologies. These techniques enable our system to predict and adapt to changes in user preferences across various time periods, ensuring that our recommendations remain relevant and timely.


To further refine our recommendation capabilities and mitigate biases towards highly popular items, we introduced an innovative approach that enhances the traditional random deep walk algorithm. This strategy evaluates the connections between items and users based on several key factors:


1. Novelty: Introducing users to new, potentially interesting content
2. Popularity: Balancing between well-known and niche items
3. Information gain: Assessing how much a recommendation contributes to expanding a user’s interest profile


We can see an example below, After introducing personalized embeddings a user for query “the” sees:


While a different user who has more affinity towards horror will see


### Training Data and Evaluation


We employ a comprehensive set of offline and online metrics to evaluate and compare various state-of-the-art approaches in our search model. These metrics are designed to assess both retrieval accuracy and user experience.


**Offline Metrics:**


1. Mean Average Precision at K (MAP@K):


1. Definition: The percentage of sessions where the launched


item appears in the top K results from the dual encoder model.
2. Purpose: Evaluates the model’s ability to retrieve relevant items in top positions.


2. Query Length at K (QL@K):


1. Definition: The average query length when the dual encoder model successfully retrieves the clicked item within the top K results.
2. Purpose: Measures the model’s efficiency in retrieving relevant items with shorter queries.


**Online Metrics:**


For real-world performance evaluation, we utilize key engagement and satisfaction indicators:


1. Streaming Hours: Total time users spend streaming content.
2. Abandonment Rate: Percentage of search sessions where users exit without selecting a result.


These online metrics are carefully chosen to correlate with our offline metrics, ensuring consistency between controlled evaluations and live performance. This correlation validates that improvements observed in offline testing translate effectively to positive user outcomes in production environments. Additionally, we have guardrail metrics to ensure the model’s performance does not degrade key aspects of the user experience.


Our AB results showed an increase in streaming hours and a reduction in abandonment rate, thereby improving the overall user experience and discovery process


**Conclusion**


In this blog, we shared the evolution of Roku’s search architecture, addressing TV-specific challenges and implementing advanced personalization. Key improvements include query expansion techniques, bias mitigation, enhanced embedding methods, and graph-based recommendation systems. These efforts have yielded significant gains in both offline and online performance metrics, enhancing content discovery and user experience on the Roku platform. As we continue to evolve our search system, we remain committed to enhancing the user experience, making content discovery on Roku more intuitive, personalized, and engaging than ever before.


The post[Personalised Retrieval For Typed Text Queries](https://engineering.roku.com/personalised-retrieval-for-typed-text-queries) appeared first on[Engineering Blog](https://engineering.roku.com/) .
