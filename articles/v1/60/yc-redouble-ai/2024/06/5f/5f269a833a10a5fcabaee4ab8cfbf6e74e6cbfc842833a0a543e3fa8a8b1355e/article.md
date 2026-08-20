---
schema_version: "1.0.0"
document_id: "5f269a833a10a5fcabaee4ab8cfbf6e74e6cbfc842833a0a543e3fa8a8b1355e"
company_key: "yc-redouble-ai"
company: "Redouble AI"
source_id: "yc-redouble-ai-rss-94bf26a72f2a"
canonical_url: "https://deconvoluteai.com/blog/rag/metrics-retrieval"
published_at: "2024-06-19T00:00:00+00:00"
first_seen_at: "2026-07-27T11:30:56.530208+00:00"
fetched_at: "2026-07-28T21:00:09.778529+00:00"
content_hash: "sha256:63c4d61e1e10b128a233aeedaa80698620bb67fd17e9761b9d52277c583c847f"
---

# Metrics for Evaluation of Retrieval in Retrieval-Augmented Generation (RAG) Systems

## Abstract


*This post details the evaluation metrics essential for assessing the retrieval component of Retrieval-Augmented Generation (RAG) systems. It categorizes metrics into binary and graded relevance types, distinguishing between order-unaware methods like Precision@k and Recall@k, and order-aware techniques such as Mean Reciprocal Rank (MRR) and Normalized Discounted Cumulative Gain (NDCG). The analysis explains how these metrics measure relevance and ranking quality to ensure the generation model receives the most useful context. The discussion concludes with practical guidance on implementing these metrics using an open-source experiments repository.*


---


## Introduction


When developing a Retrieval-Augmented Generation (RAG) system, thoroughly evaluating it before releasing it to users and continuously monitoring it while users interact with it is crucial to ensure it functions effectively and delivers value. In this post, we will focus on the evaluation of the retrieval stage of a RAG pipeline and explore specific metrics tailored for this purpose. Effective evaluation of the retrieval stage is essential for fine-tuning the entire RAG system. While tuning the generation stage alone can yield good results with modern frameworks (more on this in the next post), evaluating and optimizing the retrieval stage can significantly enhance overall performance. We will discuss various metrics that assess different aspects of retrieval performance and scenarios where each metric is particularly useful.


Finally, I will briefly discuss how the metrics are implemented in the[experiments repo](https://github.com/daved01/rag-experiments) which you can use to experiment with different settings.


## Defining System Goals


Before selecting metrics to evaluate your RAG system, it's essential to set priorities based on the application's context. For instance, a customer support application might prioritize quick response times and providing helpful answers, even if they are not completely exhaustive. On the other hand, applications dealing with medical or academic data need to prioritize accuracy and completeness to ensure the information provided is reliable and thorough.


Clearly defining these goals helps in choosing the right metrics for evaluation. By understanding the specific needs of your application, you can select metrics that provide meaningful insights and drive improvements in your RAG system's performance. This strategic approach ensures that the system not only meets technical benchmarks but also aligns with user expectations and business objectives.


## Retrieval Metrics


In the retrieval stage, the system fetches relevant documents from a large corpus based on the input query. The effectiveness of this stage is critical, as it directly influences the quality of the generated responses, as discussed in detail in my previous[post](https://deconvoluteai.com/blog/rag/failure-modes) . We categorize retrieval metrics into *order unaware* and *order aware* \[1\], \[2\], each providing unique insights into the retrieval performance.


We categorize retrieval metrics into two main types: binary relevance and graded relevance. Binary relevance metrics classify each retrieved document as either relevant (True) or not relevant (False). These are further divided into order unaware and order aware metrics. Order-unaware metrics evaluate the relevance of retrieved documents without considering their order, which is particularly suitable for RAG systems where having as many relevant documents as possible is the primary goal. Order-aware metrics take the ranking of retrieved documents into account, providing additional insights when the order of the results impacts the performance.


Graded relevance metrics, on the other hand, assess the degree of relevance of each document, giving more weight to more relevant items. This approach allows for a more nuanced evaluation of the RAG system, where varying levels of relevance are considered to optimize the retrieval process effectively.


### Binary Relevance Metrics


Binary relevance metrics classify each retrieved result as either relevant (True) or not relevant (False). This allows us to introduce metrics based solely on relevancy, and further distinguish between metrics that consider the order of retrieved documents and those that do not.


### Order Unaware Metrics


Order-unaware metrics evaluate the relevance of retrieved documents without considering their order. This approach is particularly suitable for RAG systems, where the primary goal is to maximize the number of relevant documents used as context for the language model. In this category, a result is either relevant (True) or not relevant (False), focusing solely on the presence of relevant information rather than its position. These metrics rely on classification concepts like true positives (relevant documents correctly retrieved) to measure effectiveness. See my[post](https://deconvoluteai.com/blog/evaluating-nlp-models) for more details on these concepts.


**Precision@k**


Precision@k measures the proportion of relevant documents among the top kk


k


retrieved documents. It is especially useful when you prioritize delivering relevant documents over completeness to the model, such as in customer support where quick answers matter \[2\].


Precision@k=Number of Relevant Items Retrieved in Top kk\\text{Precision@k} = \\frac{\\text{Number of Relevant Items Retrieved in Top k}}{k}


Precision@k


=


k


Number of Relevant Items Retrieved in Top k


​


Let's say we have a total of 77


7


documents in our dataset. We retrieve k=5k=5


k


=


5


documents, and two of them are relevant to a query.


- Dataset:` \[doc1, doc2, doc3, doc4, doc5, doc6, doc7\]`
- Retrieved:` \[doc6, doc2, doc3, doc4, doc5\]`
- Relevant:` \[doc2, doc3, doc7\]`


In this case, the score is:


Precision@5=25\\text{Precision@5} = \\frac{2}{5}


Precision@5


=


5


2


​


**Recall@k**


Recall@k assesses how many of the relevant documents are retrieved in the top kk


k


results. It is useful when completeness of information is critical, such as in medical or academic tools where missing relevant documents could lead to incomplete answers.


Recall@k=Number of Relevant Items Retrieved in Top kTotal Number of Relevant Items\\text{Recall@k} = \\frac{\\text{Number of Relevant Items Retrieved in Top k}}{\\text{Total Number of Relevant Items}}


Recall@k


=


Total Number of Relevant Items


Number of Relevant Items Retrieved in Top k


​


Using the same example, we have retrieved 55


5


documents, but only 22


2


are relevant, and overall there are 33


3


relevant documents in the dataset. This means:


Recall@5=23\\text{Recall@5} = \\frac{2}{3}


Recall@5


=


3


2


​


**F1@k**


F1@k provides a single metric that balances the trade-off between precision and recall, offering a more comprehensive evaluation of the retrieval performance. It is useful when you need a balance between precision and recall, ensuring neither metric is disproportionately high or low.


F1@K=2⋅Precision@k⋅Recall@kPrecision@k+Recall@k\\text{F1@K} = 2 \\cdot \\frac{\\text{Precision@k} \\cdot \\text{Recall@k}}{\\text{Precision@k} + \\text{Recall@k}}


F1@K


=


2


⋅


Precision@k


+


Recall@k


Precision@k


⋅


Recall@k


​


In the example above, the F1@5F1@5


F


1@5


score is:


F1@5=2⋅25⋅2325+23=0.5\\text{F1@5} = 2 \\cdot \\frac{\\frac{2}{5} \\cdot \\frac{2}{3}}{\\frac{2}{5} + \\frac{2}{3}} = 0.5


F1@5


=


2


⋅


5


2


​


+


3


2


​


5


2


​


⋅


3


2


​


​


=


0.5


More details on precision, recall, and F1 score can be found in my[post](https://deconvoluteai.com/blog/evaluating-nlp-models) . These three metrics are great tools to start the evaluation. However, as we can see, they treat all retrieved documents as equally important. Often that is fine in RAG, but if we want to consider the order of retrieved documents or put more weight on specific ones, we need other evaluation methods.


### Order Aware Binary Relevance Metrics


Order-aware metrics evaluate the relevance of retrieved documents by considering their order, providing additional insights especially in scenarios where the quality of the top-ranked results is particularly critical. These metrics build on binary relevance by incorporating concepts from ranking systems to assess how well the retrieval system prioritizes relevant documents.


For instance, you might typically return a large number of documents from a query due to similarity, but you can't fit all of them into your context. In such cases, you want the most relevant documents to appear at the top of the list, ensuring they are prioritized and used as context. Order-aware metrics help to evaluate and optimize this prioritization, ensuring that the most pertinent information is given higher importance.


**Mean Reciprocal Rank (MRR)**


MRR evaluates the rank of the first relevant document in the retrieved list for a list of queries. It's particularly useful when the relevance of the very first relevant result is crucial, such as in user-facing applications where the first few results significantly impact user satisfaction \[3\].


MRR=1N∑i=1N1ranki\\text{MRR} = \\frac{1}{N} \\sum_{i=1}^{N} \\frac{1}{\\text{rank}_i}


MRR


=


N


1


​


i


=


1


∑


N


​


rank


i


​


1


​


Here, NN


N


is the number of queries, and rankirank_i


r


an


k


i


​


is the position of the first relevant document for the ii


i


-th query.


Let's consider the following example from above. We have 55


5


retrieved and 22


2


relevant documents for a query:


- Dataset:` \[doc1, doc2, doc3, doc4, doc5, doc6, doc7\]`
- Retrieved:` \[doc6, doc2, doc3, doc4, doc5\]`
- Relevant:` \[doc2, doc3, doc7\]`


The first relevant document,` doc2` , is at rank 22


2


in the retrieved documents. This means, the reciprocal rank for the first query in this example is


RR1=1rank of first relevant document=12\\text{RR}_{1} = \\frac{1}{\\text{rank of first relevant document}} = \\frac{1}{2}


RR


1


​


=


rank of first relevant document


1


​


=


2


1


​


Now we have a second query with the results:


- Retrieved:` \['doc5', 'doc4', 'doc3', 'doc2', 'doc1'\]`
- Relevant:` \['doc3', 'doc2'\]`


For this query, the first relevant document is` doc3` , so the reciprocal rank is


RR2=13\\text{RR}_{2} = \\frac{1}{3}


RR


2


​


=


3


1


​


With this we can calculate the MRR for the two queries as


MRR=1N∑i=1N1ranki=12(12+13)≈0.4167\\text{MRR} = \\frac{1}{N} \\sum_{i=1}^{N} \\frac{1}{\\text{rank}_i} = \\frac{1}{2} \\left( \\frac{1}{2} + \\frac{1}{3} \\right) \\approx{0.4167}


MRR


=


N


1


​


i


=


1


∑


N


​


rank


i


​


1


​


=


2


1


​


(


2


1


​


+


3


1


​


)


≈


0.4167


**Mean Average Precision (MAP)**


To understand MAP, we must first understand Average Precision (AP). AP extends precision by incorporating a ranking, making it a more nuanced metric. MAP is then the mean over NN


N


APs. Both AP and MAP correlate directly with customer satisfaction and are best used when you need a balanced evaluation of precision across multiple queries.


Average Precision (AP) is calculated as follows:


AP=∑k=1n(Precision@k⋅rel(k))Total Number of Relevant Items\\text{AP} = \\frac{\\sum_{k=1}^{n} (\\text{Precision@k} \\cdot \\text{rel}(k))}{\\text{Total Number of Relevant Items}}


AP


=


Total Number of Relevant Items


∑


k


=


1


n


​


(


Precision@k


⋅


rel


(


k


))


​


Here, nn


n


is the number of retrieved documents, and rel(k)\\text{rel}(k)


rel


(


k


)


is a binary indicator function that is 11


1


if the item at rank kk


k


is relevant and 00


0


otherwise. The kk


k


in Precision@k\\text{Precision@k}


Precision@k


indicated how many of the retrieved documents, starting at index 00


0


, we consider for the calculation of the precision. Let's illustrate this with an example. Suppose we have the following retrieved and relevant documents for a query:


- Retrieved:` \[doc1, doc2, doc3, doc4, doc5\]`
- Relevant:` \[doc2, doc3, doc5\]`


To calculate AP, we first calculate the precision at each rank.


Document doc1 doc2 doc3 doc4 doc5


rank 11


1


22


2


33


3


44


4


55


5


P(k)⋅relkP(k) \\cdot \\text{rel}_k


P


(


k


)


⋅


rel


k


​


0/10/1


0/1


1/21/2


1/2


2/32/3


2/3


2/42/4


2/4


3/53/5


3/5


The first document` doc1` has a rank of 1 (it is the first), which means we are considering the first retrieved document` \[doc1\]` , consequently p@1=1/1p@1 = 1/1


p


@1


=


1/1


. But` doc1` not relevant, so gets assigned a rel1=0rel_1 = 0


re


l


1


​


=


0


. For the precision of the second retrieved document, Precision@2\\text{Precision@2}


Precision@2


, we are considering the first two retrieved documents (` \[doc1, doc2\]` ), and one of which is relevant, so p@2=1/2p@2 = 1/2


p


@2


=


1/2


. Because it is the second one that is relevant, rel2=1rel_2 = 1


re


l


2


​


=


1


. And so on for the remaining retrieved documents.


Then, sum all those precisions at the ranks up and divide by the total number of relevant documents and we get the Average Precision.


AP=(0.5+0.67+0.6)3≈0.59\\text{AP} = \\frac{(0.5 + 0.67 + 0.6)}{3} \\approx 0.59


AP


=


3


(


0.5


+


0.67


+


0.6


)


​


≈


0.59


We would do the same for more queries. Then, the Mean Average Precision (MAP) is computed as the mean of APs over all queries:


MAP=1N∑i=1NAP(i)\\text{MAP} = \\frac{1}{N} \\sum_{i=1}^{N} \\text{AP}(i)


MAP


=


N


1


​


i


=


1


∑


N


​


AP


(


i


)


Here, NN


N


is the total number of queries.


This approach ensures that the evaluation considers both the precision and the ranking of the relevant documents, providing a comprehensive assessment of the system's performance.


### Graded Relevance Metrics


These metrics measure not just if items are relevant but also how relevant they are, giving more weight to more relevant items \[4\]. This approach allows for a more nuanced evaluation of the RAG system, where the degree of relevance is considered, leading to better optimization of the retrieval process.


In a RAG system, suppose a user queries about "machine learning applications," and the system retrieves the following documents, with relevance scores assigned based on their usefulness in answering the query:


- Document 1: "Applications of Machine Learning in Healthcare" (relevance score: 3)
- Document 2: "Machine Learning for Autonomous Vehicles" (relevance score: 2)
- Document 3: "Using Machine Learning for Fraud Detection" (relevance score: 3)
- Document 4: "Machine Learning Techniques for Beginners" (relevance score: 1)
- Document 5: "History of Machine Learning" (relevance score: 0)


In this example, the documents are ranked by their relevance scores. Document 1 and Document 3 are highly relevant, Document 2 is somewhat relevant, Document 4 is minimally relevant, and Document 5 is not relevant at all. By using graded relevance metrics like DCG and NDCG, we can account for these varying levels of relevance in our evaluation.


**Discounted Cumulative Gain (DCG@k)**


DCG@k sums the relevance scores for the top-k items, introducing a penalty for lower-ranked documents to prioritize higher-ranked ones.


DCG@k=∑i=1krelilog⁡2(i+1)\\text{DCG@k} = \\sum_{i=1}^{k} \\frac{\\text{rel}_i}{\\log_2(i+1)}


DCG@k


=


i


=


1


∑


k


​


lo g


2


​


(


i


+


1


)


rel


i


​


​


Here, relirel_i


re


l


i


​


is the relevance score of the item at rank ii


i


.


With the example from above, we would have for the top 3 documents:


- Document 1: relevance score = 3
- Document 2: relevance score = 2
- Document 3: relevance score = 3


With this we can calculate DCG@3DCG@3


D


CG


@3


DCG@3=3log⁡2(2)+2log⁡2(3)+3log⁡2(4)≈3+1.26+1.5=5.76DCG@3 = \\frac{3}{\\log_2(2)} + \\frac{2}{\\log_2(3)} + \\frac{3}{\\log_2(4)} \\approx 3 + 1.26 + 1.5 = 5.76


D


CG


@3


=


lo g


2


​


(


2


)


3


​


+


lo g


2


​


(


3


)


2


​


+


lo g


2


​


(


4


)


3


​


≈


3


+


1.26


+


1.5


=


5.76


**Normalized DCG (NDCG@k)**


NDCG normalizes DCG values so that they can be effectively compared across queries by dividing the DCG by the ideal DCG (IDCG). It is best used when not just relevance, but also the position of items is important, providing a more nuanced evaluation than DCG alone.


NDCG@k=DCG@kIDCG@k\\text{NDCG@k} = \\frac{\\text{DCG@k}}{\\text{IDCG@k}}


NDCG@k


=


IDCG@k


DCG@k


​


Here, here IDCG@kIDCG@k


I


D


CG


@


k


is the maximum possible DCG@kDCG@k


D


CG


@


k


for the set of relevance scores.


For the example from above we can calculate the ideal DCG (IDCG@3). To do so, we sort the documents by their relevance scores in descending order:


- Document 1: relevance score = 3
- Document 3: relevance score = 3
- Document 2: relevance score = 2


Then, we can calculate


IDCG@3=3log⁡2(2)+3log⁡2(3)+2log⁡2(4)≈3+1.89+2=5.89IDCG@3 = \\frac{3}{\\log_2(2)} + \\frac{3}{\\log_2(3)} + \\frac{2}{\\log_2(4)} \\approx 3 + 1.89 + 2 = 5.89


I


D


CG


@3


=


lo g


2


​


(


2


)


3


​


+


lo g


2


​


(


3


)


3


​


+


lo g


2


​


(


4


)


2


​


≈


3


+


1.89


+


2


=


5.89


With this, we can calculate NDCG@3NDCG@3


N


D


CG


@3


as


NDCG@3=5.765.89=0.978NDCG@3 = \\frac{5.76}{5.89} = 0.978


N


D


CG


@3


=


5.89


5.76


​


=


0.978


## Experimenting with RAG Evaluation


The metrics discussed above are all implemented in the experiments[repo](https://github.com/daved01/rag-experiments) , allowing you to experiment with them directly. This section provides a demonstration of how to use these metrics and an overview of the design decisions behind the implementation.


### Using the Metrics


Let's assume you have some RAG pipelines defined in your code, as explained[here](https://deconvoluteai.com/blog/rag/baseline) . To evaluate the retriever, you first need labeled data. In the file` prompt_queries.json` , you can define the queries along with the expected retrieved documents. For order-aware metrics, the order of these documents matters. If you want to use graded relevance metrics, you must also provide a relevance score.


In the main application code, you can initialize a` RetrievalEvaluator` and use it as follows:


```text
# Initialize with config and experiment inputs.
retrieval_evaluators = RetrievalEvaluator(config, prompts_queries)


# Run evaluators
results_with_evals = retrieval_evaluators.run(results_with_or_without_evals)
```


If everything goes well, the results of the evaluations will be output in a JSON file, providing detailed insights into the performance of your retrieval stage based on the chosen metrics.


### Design Decisions


The implementation is designed to be modular and extensible, allowing you to easily add new metrics or modify existing ones. The` RetrievalEvaluator` class serves as a central hub for running evaluations, integrating seamlessly with your RAG pipelines. All evaluators consist of a` Metrics` class which implement the metrics, and an` Evaluator` service which uses these metrics. The use of JSON for input and output ensures compatibility with various data formats and ease of use in different environments.


By leveraging these metrics, you can fine-tune your RAG system to optimize the retrieval stage, ultimately enhancing the quality of the generated responses.


If you are interested in tuning the generation stage, and curious how you can do this even without providing labelled data, check out the next post!


## Conclusion


Choosing the right metrics for evaluating the retrieval stage is crucial for optimizing a RAG system's performance. Each metric offers unique insights, and the choice should align with your specific needs and priorities. While order-unaware metrics are often sufficient for RAG systems, order-aware metrics can provide valuable additional information in certain scenarios. By understanding and applying these metrics, you can ensure your RAG system retrieves the most relevant documents, thereby enhancing the quality of the generated responses.


## References


\[1\] Amit Chaudhary. (2024) *Evaluation Metrics For Information Retrieval.*[https://amitness.com/posts/information-retrieval-evaluation](https://amitness.com/posts/information-retrieval-evaluation)


\[2\] Pincone. *RAG Evaluation: Don't let customers tell you first.*[link](https://www.pinecone.io/learn/series/vector-databases-in-production-for-busy-engineers/rag-evaluation/)


\[3\] E.M. Voorhees. (1999) *TREC-8 Question Answering Track Report.* Proceedings of the 8th Text Retrieval Conference: pp. 77.[link](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication500-246.pdf)


\[4\] Järvelin, Kalervo, and Jaana Kekäläinen. (2002) *Cumulated Gain-Based Evaluation of IR Techniques.* ACM Transactions on Information Systems 20, no. 4: pp. 422–46.[https://doi.org/10.1145/582415.582418](https://doi.org/10.1145/582415.582418)
