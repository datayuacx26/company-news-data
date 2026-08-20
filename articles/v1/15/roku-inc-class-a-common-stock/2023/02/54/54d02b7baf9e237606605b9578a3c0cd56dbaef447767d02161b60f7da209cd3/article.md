---
schema_version: "1.0.0"
document_id: "54d02b7baf9e237606605b9578a3c0cd56dbaef447767d02161b60f7da209cd3"
company_key: "roku-inc-class-a-common-stock"
company: "Roku Inc."
source_id: "roku-inc-class-a-common-stock-rss-7fd84d003d99"
canonical_url: "https://engineering.roku.com/multimodal-semantic-search"
published_at: "2023-02-14T21:09:28+00:00"
first_seen_at: "2026-07-20T23:21:49.245762+00:00"
fetched_at: "2026-07-23T14:12:06.863690+00:00"
content_hash: "sha256:ae0b757f9ffd4a8c5cee49c9f4353712c6d38c281d344d56b7389cb5b480023b"
---

# Improving the Roku Experience with Multimodal Semantic Search

Roku’s mission is to be the streaming platform that connects and benefits the global TV community. An important contributor to delivering on this mission is helping users to discover content through lexical and semantic search.


In this article, we


1. Dive into the retrieval part of the Roku search system, focusing on ambiguous queries such as categorical search queries, not title search queries.
2. Identify the limitations of our exact-match retrieval system.
3. Explain the results of a recent project where we adopt content images and textual description and build a new, hybrid retrieval system, blending both lexical exact-match and semantic search capabilities.
4. Conclude with a summary of the successful validation experiment we ran on the new hybrid retrieval system.


The hybrid semantic search + lexical retrieval system presented in this blog has been live for all English categorical voice search queries in production since the end of 2022.


# Context and motivation


The goal of this project is to improve the “Retrieve” step of search by enabling users to express free natural language queries


and


browse through the Roku catalogue, bypassing the need for


specifying


categories and instead going directly from the query to semantically relevant items.


# Background on the exact-match lexical retrieval system


The Roku search engine


handles


user queries in 2 stages:


- **Retrieve** : r educe the search space from millions of items to a few hundred candidates


- **Rank** : sort the above candidates to optimize for relevant business metrics, and only show the top few dozen


result


s


[Retrieval systems typically use exact match similarity measures, one of the most popular being BM25](https://en.wikipedia.org/wiki/Okapi_BM25) . This method works very well when the search query is narrow and the user already knows what they are looking for,


e.g.,


searching for a specific movie title. However,


this


method under-


performs


when searching for categories


(


especially when such categories don


’


t exist in our catalogue


)


or


for browsing


the content of a large catalogue


, in general


.


In


a


previous solution, we implemented a category-to-category semantic search system, which allowed queries to be semantically matched with categories that exist in our catalogue. This approach reached some limitations when user queries


could not


accurately be matched with existing categories, or some categories were missing from relevant items, or some movies were tagged with generic categories that were not salient enough.


# Searching through images and descriptions


We would like users to be able


to search through


textual or image data that contains information


relevant


to


search result content


items.


- **Text** : a good description (including the title) should be a good summary of the item


- **Image** : the item image banner very often contains salient cues on the movie content


In the example below, the keyword “dinosaur” is not among the list of tagged categories, but the notion of dinosaur is clearly mentioned in the image, title and description.


A


traditional retrieval system would likely not surface the movie “Jurassic Park” from a user asking for “Dinosaur movies”, whereas a semantic search solution using


content


image and description


attribu


t


es


would


succeed.


Figure 1 – Hybrid Lexical + Semantic Search Results


# Semantic search 101


Instead of exactly matching keywords, we would like to represent user queries and items into lower dimensional dense vectors, so that a query and an item are semantically relevant if they lie close to each other in the representation space.


Once we can embed a query and an item in the same space, we can compare them using Euclidean, cosine, angular, or other relevant metrics.


*Figure 2 Comparing Queries to Items*


# Encoder for descriptions


To match queries and item descriptions in the same space, we make use of the pre-trained[Universal Sentence Encoder](https://arxiv.org/abs/1803.11175) (USE), open-sourced by[Google](https://www.tensorflow.org/hub/tutorials/semantic_similarity_with_tf_hub_universal_encoder) .


The USE model was pre-trained on a large corpus of textual data using:


- **Unsupervised**


objective: Wikipedia, web news,


etc.


- **Supervised**


objective: Stanford Natural Language Inference dataset


and was then fine-tuned on a variety of transfer tasks, including semantic text similarity, which is of


high


interest in our case.


Once we encode our items, we can store their embeddings and use them for different applications. This includes the semantic search use-case we are presenting in this article.


Taking the example of sharks movies, we can visualize in Figure 3 below our movie embeddings using a Principal Components Analysis (PCA) projection. We observe that related movies similar to *Jaws* are indeed close to each other in the USE representation space, meaning that their descriptions are similar.


*Figure 3 Visualizing movies similar to Jaws using a 2D projection on[tensorboard](https://www.tensorflow.org/tensorboard)*


# Encoder for images


To be able to represent textual queries and images in the same space, we make use of the open-sourced[CLIP](https://openai.com/blog/clip/) model


from[OpenAI](https://github.com/openai/CLIP) .


*Figure 4* *Two-towers architecture representing the CLIP Mode* l


(the image is from the OpenAI blog post:[CLIP](https://openai.com/blog/clip/) )


The model is a two-


towers


model with one textual head and one visual head. Pre-trained at matching images with their content descriptions, we can reuse it


in


this way:


- **At indexing time** : get the embedding of the item image using CLIP’s image head, store them and build a search index


- **At query inference time** : pass the user query through the CLIP’s text head and match the embedding with the indexed embeddings


As in the previous section, we can visualize the items’ 2D projections, this case in the CLIP image space.


In the example of dinosaur movies, we see in Figure 5 below that the items similar to *Jurassic Park* are situated in a close neighborhood in the CLIP embedding space.


*Figure 5 Visualizing movies similar to Jurassic Park using a 2D projection on[tensorboard](https://www.tensorflow.org/tensorboard)*


# Efficient vector search


A vector-based retrieval system needs to find the top K vectors matching the query. Exact


k-nearest neighbors (


KNN or exact KNN


)


techniques unfortunately


do not


scale to a


high volume


of


items;


hence it is common in the industry to make use of approximate nearest neighbor algorithms (ANN or approximate KNN). The most popular ANN algorithm is called


[HNSW](https://arxiv.org/abs/1603.09320) and is implemented in widely used libraries like


[Annoy](https://github.com/spotify/annoy) or[Faiss](https://github.com/facebookresearch/faiss) .


In the below figure from the[ANN benchmark](http://ann-benchmarks.com/) webpage, we can see the performance of different approximate KNN search algorithms in both throughput (queries per second, on the y-axis here) and recall (meaning the approximate results are close to the exact KNN algorithm results, on the x-axis here). A better approximate KNN search algorithm should improve both the throughput and the recall.


In practice, ML engineers can tune the algorithm hyper-parameters according to the performance / recall tradeoff chosen based on business needs.


*Figure 6 ANN Algorithm Performance* (The graph is from the[ANN benchmark](http://ann-benchmarks.com/) webpage)


According to the benchmark


, the current


state


–


of


–


the –


art


ANN algorithm is


[ScANN](https://arxiv.org/abs/1908.10396) , outperforming other methods in both latency and recall. Thus, we decide to use this ANN algorithm for our semantic search application.


The[ScANN model](https://www.tensorflow.org/recommenders/examples/efficient_serving) TensorFlow ops are available through a[Docker image](https://github.com/google-research/google-research/tree/master/scann/tf_serving) that we have slightly adapted to be usable in our system.


Developers can implement ScANN models as part of the[tensorflow_recommenders](https://www.tensorflow.org/recommenders) library through the[ScANN class](https://github.com/tensorflow/recommenders/blob/v0.7.2/tensorflow_recommenders/layers/factorized_top_k.py#L596-L766) . A notable feature of this class is that it enables the embedding model and the ANN model to be wrapped in the same TensorFlow graph. This enables us to serve the models on single real-time endpoints, thus saving even more latency through overhead latency optimization.


The image below summarizes the system optimization provided by using ScANN models:


*Figure 7 Traditional and ScANN -based Semantic Search*


# Designing a semantic search retrieval service


We would like to search through both images and descriptions. To achieve this, as


pictured


below, we run the two vector search processes in parallel, and then we combine the results after normalizing the similarity scores.


*Figure 8 Semantic Search Flow*


# Integrating semantic search into the search system


During the exploration part of this project, we found out that while the semantic search was very good at surfacing great content, it was sometimes missing popular items we really want to show to users. With traditional ElasticSearch-based retrieval being good by design at surfacing popular results, we are implementing a hybrid search experience where items coming from the traditional ElasticSearch service and the semantic search service are obtained in parallel, then combined to form a list of items to be shown to the user.


The image below summarizes the hybrid solution logic:


*Figure 9 Hybrid Search Flow Logic*


### Note: Semantic Search with Filters


***What happens if a user asks for “Free action movies”?***


The results must not only be semantically relevant to “action”; they also need to be free and of type “movie”. The challenge is that our ANN index only serves content vectors and is not aware of these two fields. Hence, in the results, there might be items that are not free, or of other types, e.g., “series” versus “movie.”


In order not to duplicate the ANN indexes (we could have an individual ANN index per possible combination of filters), which would not be a scalable solution, we choose to perform a post-filtering step: after receiving the candidates from the semantic search service, we use our main search index to filter the candidates that do not match with the filters.


A potential limitation of this approach is that we might end up with no candidates at all satisfying the conditions. We are addressing this issue in two ways:


- **Today** : we increase the


number


of items queried from the semantic search service


,


to


increase the probability


of


get


ting


candidates after the post-filtering step.


We monitor the ratio of semantic search items that survive the post-filtering step to ensure the feature is impacting categorical queries as expected.


- **In exploration** : we are investigating methods that enable the ANN search algorithm itself to include the filters, so that the search keeps the efficiency advantages of ANN search while enabling filtering.


# Testing the approach with real users’ feedback


To test our hypothesis that semantic search items will improve user engagement on categorical search queries, we ran an AB test affecting voice categorical queries in English-speaking markets for about a month.


We experimented with two treatment buckets:


- **Semantic Search only**
- **Semantic Search + Lexical** (hybrid solution, showing both semantic search and traditional search items)


We show in the table below


the relative changes observed in the experiment between the control bucket (no semantic search used, only exact match) and the two treatment buckets for some of the metrics we care about.


We were interested to see if users will be more likely to engage with search results (through clicks, but most of all, launches), and whether this helps increase the number of streaming hours.


The results in the table below show that the hybrid Semantic Search + Lexical approach achieved positive, statistically significant results across all metrics.


- **S** : statistically significant
- **NS** : not statistically significant


*Table 1 Hybrid Search AB Test Results*


Test Metric


Semantic Search + Lexical


Semantic Search Only


Click-Through Rate +6.25% / **S** +0.27% / **NS**


Launch Rate +7.46% / **S** +3.87% / **S**


Streaming Hours +4.73% / **S** +1.37% / **NS**


We found that the hybrid version outperformed both the control and the semantic search only groups.


The hybrid semantic search + lexical retrieval system is now being used for all English categorical search queries in production.


# Closing note


The goal of this project was to show the value of using image and text content in an embedding-based retrieval system to complement an existing exact-match search system. We used open-source pre-trained models and built a general framework that proved to be a successful feature in an online experiment. Now that the framework is built and the value of these new content features has been proven in a data-driven way, it is more straightforward for us to iterate and improve our semantic search capabilities to overcome some of the simplifications we have taken.


We are actively working on supporting semantic search features in international markets (rather than English only), fine-tuning the encoders on our business-specific labels with contrastive learning objectives, and considering additional unstructured content features besides main image and description.


The post[Improving the Roku Experience with Multimodal Semantic Search](https://engineering.roku.com/multimodal-semantic-search) appeared first on[Engineering Blog](https://engineering.roku.com/) .
