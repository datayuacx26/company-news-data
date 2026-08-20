---
schema_version: "1.0.0"
document_id: "4aa5545fce98e858b7617ac19261c449c961a9ea5eb9e9f57e58a881e81f8b0c"
company_key: "yc-tensorfuse"
company: "Tensorfuse"
source_id: "yc-tensorfuse-news-import-97ab2b67de47"
canonical_url: "https://tensorfuse.io/blog/from-naive-rags-to-advanced-improving-your-retrieval"
published_at: "2024-05-22T00:00:00+00:00"
first_seen_at: "2026-07-24T04:04:56.878556+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:20162204934964e12e48ae03f4fef474f86e1f5af3cb78d62c52a0bd65303e1a"
---

# From Naive RAGs to Advanced: Improving your Retrieval

Tutorial


# From Naive RAGs to Advanced: Improving your Retrieval


Samagra Shamra


May 22, 2024


6 minutes read


RAG pipelines are everywhere and a lot of people are deploying these pipelines in production. However, after speaking with numerous companies, I have come to realize that building a naive RAG system is easy but improving it and making it production grade is super hard. During my time at Adobe Research, I have deployed numerous RAG systems (back then RAGs were called Natural Language Search)and I would like to share my insights here. This document aims to provide an understanding of the design space for improving RAG pipelines.


All RAG pipelines consist of three subsystems:


- Retrieval: This subsystem is responsible for indexing your data and retrieving it later to augment your LLM.
- Augmenting: This system presents the retrieved information to the LLM.
- Generation: This subsystem converts the retrieved information into a coherent response for your use.


This is the first post in a series of three posts focusing on RAGs. This post specifically addresses the identification and improvement of the retrieval subsystem in your RAG pipelines.


## **How do I identify if my retrieval subsystem is at fault?**


Let's first enumerate what can go wrong with your pipeline at the retrieval stage:


- Your vector database doesn't have the required information.
- Your vector database has the information, but it is not being correctly retrieved.
- Your customers don't know how to query your RAG, and therefore, your RAG performs poorly.


Each of these three problems has different solutions. To identify what exactly is going wrong, you need to have evaluation metrics that you are tracking. There is no other way. We tried manual eyeballing a lot, but it never works, and you are always guessing as to what can go wrong. So, I am listing down the metrics that you need to track:


- **Your retrieval metric** : Most naive RAG systems use embedding cosine similarity search. Other systems use sparse-dense matrix products. And many others might use any more complicated metric. You need to track this for every pair of (query, retrieved_data) pair. We will use this metric to zero down on the specific problem that our system has.
- **Does the retrieved context contain an answer to the query?:** This metric is commonly referred to as Context Relevance. You can measure this using Accuracy, Cosine Similarity, or other methods if you have a ground truth dataset. Alternatively, if you don't have a ground truth dataset, you can use an LLM as a judge. In simpler systems, metrics such as Hit-Rate or Mean Reciprocal Rate can be used as alternatives.


There are many YC companies that can assist you in setting up these metrics. At **@Tensorfuse** , we support all the mentioned metrics. If you are looking for LLM-as-a-judge metrics, **@Ragas** is a highly recommended open-source tool. I also came across a post by **@Parea** discussing various evaluation metrics.


## **Solutions**


Now that we have metrics set up let’s talk about actually improving our system. We will go from metric observation to problem to solution.


- **Low mean and median for the retrieval metric** : There could be two main reasons for this.


- First: Your database may have low coverage.


- This is easy to identify. You can automate this process by performing a word cluster analysis on failing queries to determine the domains where your database coverage is lacking.
- To address this issue, add relevant information to your database. Include metadata to identify the types of information that are missing. Use word clustering to identify the cluster of queries that need to be supplemented in your vector database.


- Second: Your embedding model is not suitable for your task or domain.


- For your domain: Perform PCA analysis (or any other clustering mechanism) on your data embeddings. If there are no recognizable or meaningful clusters in your data, then your embeddings are not suitable for your domain.
- For your task: If the domain PCA analysis forms meaningful clusters, then run the same PCA analysis on the query embeddings using the domain PCA base vectors. If the queries do not form clusters using the same vectors, then the embeddings are unsuitable for your task. You can find a helpful tutorial for PCA analysis here: **Visualizing Word Embedding with PCA and t-SNE**
- In either case, you will need to choose a different embedding model or fine-tune a model for your task/domain. You can refer to this leaderboard for the best embedding models available: **Embedding Models Leaderboard**
- Fine-tuning embeddings is a complex topic that deserves its own blog post. Let me know if you would like me to cover that.


- **Your users don’t know how to properly query your RAG** . While ideal users would query the RAG correctly, it is common to encounter users who struggle with this. Query rewriting can help address this issue. The following methods of query rewriting have shown promise:


- Use an LLM to identify the information that would answer the query. Use this information to reverse-construct a query that aligns with the functionality of your RAG.
- Create a pseudo-answer document that contains hypothetical answers to the user's query. This document should be in a language similar to your indexed database. Calculate the embedding similarity between the pseudo-answer and your database to retrieve the relevant information.
- Follow-up questions: Perplexity has nailed this flow. The gist is that you ask intelligent follow-up questions and then construct a query based on the answers to the follow-up questions.


- **High mean for the retrieval metric but low context relevance** : This indicates that there is an issue with your chunking strategy. While the retriever is able to retrieve related documents, these documents do not contain the complete context of the information. To address this, you need to change your chunking strategies. Below are two strategies that have shown promise in most applications:


- Abstract chunking: This approach involves indexing summaries at different hierarchical levels. For example, in an enterprise search application, you could index paragraph summaries, paragraphs, document summaries, folder summaries, team summaries, and organization summaries. The retrieval process would then involve retrieving the summaries first, followed by retrieving the components that have high values for the retrieval metric.
- Graphical chunking: This method of chunking is useful in scenarios where the queries are about real-world entities and their relationships. For instance, if you have questions about events or patents. In addition to storing paragraphs in your vector database, you also need to extract named entities from your data and store information about these entities and their relationships. Here is a great article on the concept: **Unifying LLM Knowledge Graph** .


Let me know if there are gaps in this article that you would like clarity on or if you want me to deep dive into any of the specific methods present here. You can always reach out to me **here** .
