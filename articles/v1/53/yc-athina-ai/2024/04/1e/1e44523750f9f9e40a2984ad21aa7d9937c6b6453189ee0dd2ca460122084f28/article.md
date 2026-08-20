---
schema_version: "1.0.0"
document_id: "1e44523750f9f9e40a2984ad21aa7d9937c6b6453189ee0dd2ca460122084f28"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/tree-of-reviews-a-tree-based-dynamic-iterative-retrieval-framework-for-multi-hop-question-answering"
published_at: "2024-04-22T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:d31cec42ee579bdee57be06498df2583e0c431cf97b4339237c69c227d880162"
---

# Tree of Reviews: A Tree-based Dynamic Iterative Retrieval Framework for Multi-hop Question Answering

Do not index


Original Paper


[https://arxiv.org/abs/2404.14464](https://arxiv.org/abs/2404.14464)


Blog URL


[blog.athina.ai /tree-of...nswering](https://blog.athina.ai/tree-of-reviews-a-tree-based-dynamic-iterative-retrieval-framework-for-multi-hop-question-answering)


**Original Paper:**[https://arxiv.org/abs/2404.14464](https://arxiv.org/abs/2404.14464)


**By:**[Li Jiapeng](https://arxiv.org/search/cs?searchtype=author&query=Jiapeng%2C%20L) ,[Liu Runze](https://arxiv.org/search/cs?searchtype=author&query=Runze%2C%20L) ,[Li Yabo](https://arxiv.org/search/cs?searchtype=author&query=Yabo%2C%20L) ,[Zhou Tong](https://arxiv.org/search/cs?searchtype=author&query=Tong%2C%20Z) ,[Li Mingling](https://arxiv.org/search/cs?searchtype=author&query=Mingling%2C%20L) ,[Chen Xiang](https://arxiv.org/search/cs?searchtype=author&query=Xiang%2C%20C)


**Abstract:**


> Multi-hop question answering is a knowledge-intensive complex problem. Large Language Models (LLMs) use their Chain of Thoughts (CoT) capability to reason complex problems step by step, and retrieval-augmentation can effectively alleviate factual errors caused by outdated and unknown knowledge in LLMs. Recent works have introduced retrieval-augmentation in the CoT reasoning to solve multi-hop question answering. However, these chain methods have the following problems: 1) Retrieved irrelevant paragraphs may mislead the reasoning; 2) An error in the chain structure may lead to a cascade of errors.
>
>
> In this paper, we propose a dynamic retrieval framework called Tree of Reviews (ToR), where the root node is the question, and the other nodes are paragraphs from retrieval, extending different reasoning paths from the root node to other nodes. Our framework dynamically decides to initiate a new search, reject, or accept based on the paragraphs on the reasoning paths. Compared to related work, we introduce a tree structure to handle each retrieved paragraph separately, alleviating the misleading effect of irrelevant paragraphs on the reasoning path; the diversity of reasoning path extension reduces the impact of a single reasoning error on the whole. We conducted experiments on three different multi-hop question answering datasets. The results show that compared to the baseline methods, ToR achieves state-of-the-art performance in both retrieval and response generation. In addition, we propose two tree-based search optimization strategies, pruning and effective expansion, to reduce time overhead and increase the diversity of path extension. We will release our code.


---


###


Summary Notes


####


Revolutionizing Multi-hop Question Answering with the Tree of Reviews (TOR) Framework


In the swiftly advancing field of artificial intelligence (AI), multi-hop question answering has become a key component for systems that perform complex reasoning across different information sources. The traditional methods used for this task often face challenges, such as error propagation, which can lead to incorrect answers.


The Tree of Reviews (TOR) framework emerges as a groundbreaking solution to these problems, utilizing a tree-based structure for dynamic iterative retrieval.


This blog post explores the workings of TOR, its advantages, and its implications for AI engineering in enterprise settings.


####


What is Multi-hop Question Answering?


Multi-hop question answering involves gathering information from multiple sources to answer a single question. Traditionally, this process follows a linear approach, which is susceptible to error propagation – one mistake in the retrieval or reasoning process can compromise the entire answer.


####


Introducing the TOR Framework


TOR presents an innovative approach to overcome the limitations of traditional multi-hop question answering.


Its tree structure starts with the initial question at the root, branching out to nodes that represent paragraphs from retrieved documents. The TOR approach is characterized by:


- **Paragraph Review Block** : This evaluates each paragraph, deciding whether to start a new search or to accept or reject paragraphs based on reasoning paths.


- **Dynamic Reasoning Paths** : It explores different paths of reasoning, dynamically integrating evidence from various sources.


- **Error Reduction** : TOR aims to reduce reasoning errors and the influence of irrelevant information.


####


Experiments and Results


Testing TOR against three datasets showed its enhanced performance in retrieval effectiveness and accuracy, highlighting its ability to reduce the impact of irrelevant information and reasoning errors.


####


Advantages of a Tree Structure


TOR's tree-based model offers numerous benefits over linear retrieval methods, such as:


- **Flexible and Error-Resilient Reasoning** : It simultaneously manages multiple inquiries, allowing a more detailed assessment of information relevance and sufficiency.


- **Dynamic Information Handling** : The ability to dynamically navigate and assess various reasoning paths leads to a more accurate question-answering process.


####


Future Prospects and Considerations


TOR sets a new standard in multi-hop question answering, offering a path towards developing smarter, more error-resilient AI systems.


For AI engineers in enterprise environments, TOR opens up possibilities for creating advanced AI solutions.


However, it's essential to keep in mind the ethical considerations, such as bias and privacy, that come with AI advancements.


####


Ethical Considerations


While TOR advances the field, it does not eliminate all ethical concerns related to AI, emphasizing the need for AI engineers to address these issues thoughtfully.


####


Future Work


Future research will focus on improving retrieval efficiency and accuracy, expanding TOR to other complex reasoning tasks, and tackling the ethical challenges of advanced AI models.


####


Conclusion


The Tree of Reviews (TOR) framework represents a significant leap forward for multi-hop question answering, addressing traditional method limitations and enabling the development of more sophisticated AI systems.


As AI continues to evolve, frameworks like TOR will be crucial in shaping the intelligent question-answering technologies of the future.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
