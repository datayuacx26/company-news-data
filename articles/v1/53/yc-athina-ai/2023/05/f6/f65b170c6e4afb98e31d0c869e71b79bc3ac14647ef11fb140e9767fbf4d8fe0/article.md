---
schema_version: "1.0.0"
document_id: "f65b170c6e4afb98e31d0c869e71b79bc3ac14647ef11fb140e9767fbf4d8fe0"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/chain-of-knowledge-grounding-large-language-models-via-dynamic-knowledge-adapting-over-heterogeneous-sources"
published_at: "2023-05-22T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:26.378360+00:00"
content_hash: "sha256:fe3bd391d58261f813eec9004196802b557e4d5ad9e991d7cae0bdd1260b565d"
---

# Chain-of-Knowledge: Grounding Large Language Models via Dynamic Knowledge Adapting over Heterogeneous Sources

Do not index


Original Paper


[https://arxiv.org/abs/2305.13269](https://arxiv.org/abs/2305.13269)


Blog URL


[blog.athina.ai /chain-o...-sources](https://blog.athina.ai/chain-of-knowledge-grounding-large-language-models-via-dynamic-knowledge-adapting-over-heterogeneous-sources)


**Original Paper:**[https://arxiv.org/abs/2305.13269](https://arxiv.org/abs/2305.13269)


**By:**[Xingxuan Li](https://arxiv.org/search/cs?searchtype=author&query=Li%2C%20X) ,[Ruochen Zhao](https://arxiv.org/search/cs?searchtype=author&query=Zhao%2C%20R) ,[Yew Ken Chia](https://arxiv.org/search/cs?searchtype=author&query=Chia%2C%20Y%20K) ,[Bosheng Ding](https://arxiv.org/search/cs?searchtype=author&query=Ding%2C%20B) ,[Shafiq Joty](https://arxiv.org/search/cs?searchtype=author&query=Joty%2C%20S) ,[Soujanya Poria](https://arxiv.org/search/cs?searchtype=author&query=Poria%2C%20S) ,[Lidong Bing](https://arxiv.org/search/cs?searchtype=author&query=Bing%2C%20L)


**Abstract:**


> We present chain-of-knowledge (CoK), a novel framework that augments large language models (LLMs) by dynamically incorporating grounding information from heterogeneous sources. It results in more factual rationales and reduced hallucination in generation. Specifically, CoK consists of three stages: reasoning preparation, dynamic knowledge adapting, and answer consolidation. Given a knowledge-intensive question, CoK first prepares several preliminary rationales and answers while identifying the relevant knowledge domains. If there is no majority consensus among the answers from samples, CoK corrects the rationales step by step by adapting knowledge from the identified domains. These corrected rationales can plausibly serve as a better foundation for the final answer consolidation. Unlike prior studies that primarily use unstructured data, CoK also leverages structured knowledge sources such as Wikidata and tables that provide more reliable factual information. To access both unstructured and structured knowledge sources in the dynamic knowledge adapting stage, we propose an adaptive query generator that allows the generation of queries for various types of query languages, including SPARQL, SQL, and natural sentences. Moreover, to minimize error propagation between rationales, CoK corrects the rationales progressively using preceding corrected rationales to generate and correct subsequent rationales. Extensive experiments show that CoK consistently improves the performance of LLMs on knowledge-intensive tasks across different domains.


---


###


Summary Notes


####


Enhancing Accuracy in Large Language Models with the Chain-of-Knowledge Framework


####


Introduction


The reliability of Large Language Models (LLMs) is crucial for their application in various sectors. However, a common issue is their tendency to generate relevant but not always factually correct responses.


A new approach, the "Chain-of-Knowledge" (CoK) framework, addresses this by improving the factual accuracy of LLMs, reducing the occurrence of generating incorrect or unverifiable information, known as hallucination.


####


Framework Overview


The CoK framework enhances LLM outputs through a three-step process:


- **Reasoning Preparation** : Identifies relevant knowledge areas based on the query.


- **Dynamic Knowledge Adapting** : Updates rationales by incorporating knowledge from various sources.


- **Answer Consolidation** : Finalizes the answer with improved accuracy and facts.


####


Key Features


- **Adaptive Query Generator (AQG)** : A versatile component that enables smooth interaction with different LLMs and handles both structured and unstructured queries. It can access a broad range of knowledge sources, from Wikidata to specialized databases, ensuring high factual accuracy.


####


Experimental Results


The CoK framework shows a marked improvement in reducing factual inaccuracies in LLM outputs across various knowledge-demanding tasks, showcasing its effectiveness.


####


Comparison with Existing Methods


CoK outperforms other approaches like Verify-and-Edit and ReAct, especially in complex reasoning tasks, due to its unique progressive correction mechanism and ability to dynamically adapt knowledge.


####


Implementation and Accessibility


The CoK framework's code is publicly available, reflecting the developers' commitment to the AI and machine learning community. Its modular nature allows for easy adaptation to different LLMs and knowledge sources, addressing the challenges of information updates and privacy concerns.


####


Acknowledgments and Support


The development of the CoK framework was supported by prominent institutions such as DAMO Academy, National Research Foundation Singapore, and AI Singapore Programme, highlighting its significance and potential impact in the field.


####


Limitations and Future Directions


The framework's reliance on external knowledge sources means its accuracy depends on the reliability of these sources.


Additionally, despite efforts to minimize errors, the effectiveness of the system is still influenced by the initial rationale generation and knowledge retrieval accuracy.


####


Conclusion


The CoK framework marks a significant advancement in improving the factual accuracy of LLM outputs. By dynamically utilizing knowledge from various sources, it significantly boosts the reliability of LLMs in providing factually correct information.


This development is crucial for AI engineers at enterprise companies, offering a powerful tool to ensure the factual integrity of AI-generated content. As the framework continues to evolve, it paves the way for higher standards of factual accuracy in the AI field, catering to the increasing demand for accurate information.
