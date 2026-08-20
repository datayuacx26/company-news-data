---
schema_version: "1.0.0"
document_id: "4a5a4d19f6df288cb79bd31d412c80cbff45ad41d84a99b9a2fc2478eeb27542"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/demonstrate-search-predict-composing-retrieval-and-language-models-for-knowledge-intensive-nlp"
published_at: "2023-01-23T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:29.187426+00:00"
content_hash: "sha256:67022d00c95e89fb1e5ce5aba8673f074cdb2c305db50067c7a1cb8d9bf6cc9d"
---

# Demonstrate-Search-Predict: Composing retrieval and language models for knowledge-intensive NLP

Do not index


Original Paper


[https://arxiv.org/abs/2212.14024](https://arxiv.org/abs/2212.14024)


Blog URL


[blog.athina.ai /demonst...sive-nlp](https://blog.athina.ai/demonstrate-search-predict-composing-retrieval-and-language-models-for-knowledge-intensive-nlp)


**Original Paper:**[https://arxiv.org/abs/2212.14024](https://arxiv.org/abs/2212.14024)


**By:**[Omar Khattab](https://arxiv.org/search/cs?searchtype=author&query=Khattab%2C%20O) ,[Keshav Santhanam](https://arxiv.org/search/cs?searchtype=author&query=Santhanam%2C%20K) ,[Xiang Lisa Li](https://arxiv.org/search/cs?searchtype=author&query=Li%2C%20X%20L) ,[David Hall](https://arxiv.org/search/cs?searchtype=author&query=Hall%2C%20D) ,[Percy Liang](https://arxiv.org/search/cs?searchtype=author&query=Liang%2C%20P) ,[Christopher Potts](https://arxiv.org/search/cs?searchtype=author&query=Potts%2C%20C) ,[Matei Zaharia](https://arxiv.org/search/cs?searchtype=author&query=Zaharia%2C%20M)


**Abstract:**


> Retrieval-augmented in-context learning has emerged as a powerful approach for addressing knowledge-intensive tasks using frozen language models (LM) and retrieval models (RM). Existing work has combined these in simple "retrieve-then-read" pipelines in which the RM retrieves passages that are inserted into the LM prompt. To begin to fully realize the potential of frozen LMs and RMs, we propose Demonstrate-Search-Predict (DSP), a framework that relies on passing natural language texts in sophisticated pipelines between an LM and an RM. DSP can express high-level programs that bootstrap pipeline-aware demonstrations, search for relevant passages, and generate grounded predictions, systematically breaking down problems into small transformations that the LM and RM can handle more reliably. We have written novel DSP programs for answering questions in open-domain, multi-hop, and conversational settings, establishing in early evaluations new state-of-the-art in-context learning results and delivering 37-120%, 8-39%, and 80-290% relative gains against the vanilla LM (GPT-3.5), a standard retrieve-then-read pipeline, and a contemporaneous self-ask pipeline, respectively. We release DSP at
>
>
> [this https URL](https://github.com/stanfordnlp/dsp)


---


###


Summary Notes


####


Blog Post Simplified: Enhancing NLP with DSP Framework


####


Introduction


In the fast-evolving world of natural language processing (NLP), combining language models (LMs) with retrieval models (RMs) is changing how we tackle complex tasks.


These tasks, like answering multi-part questions or engaging in detailed conversations, require more than just understanding language; they need the ability to find and use information from large databases.


While traditionally, LMs were improved with text prompts, tougher tasks need an extra layer of precision that RMs provide.


####


What is the DSP Framework?


The DSP (Demonstrate-Search-Predict) framework is a cutting-edge approach that blends the strengths of LMs and RMs. Here’s a breakdown:


- **Demonstrate** : Show the system what the final outcome should look like through example annotations.


- **Search** : The system looks for necessary information within a specific knowledge base, making complex queries easier to manage.


- **Predict** : Then, it predicts answers based on the information it gathered earlier, providing a well-rounded response.


####


How It Works and Its Impact


DSP has been tested in various settings like open-domain questions and conversations, showing significant improvements over other models. For those interested, the framework and examples are shared on GitHub.


####


Methodology in Action


Using GPT-3.5 as a starting point, DSP's approach is tested with multi-hop question answering, demonstrating its ability to simplify and effectively address complex queries, outperforming traditional models.


####


Advantages of DSP


- **Flexibility and Power** : DSP offers a unique way to program complex information retrieval, handling intricate queries with ease.


- **Efficiency** : It uses pre-trained models, making sophisticated NLP systems more accessible and reducing costs and efforts in deployment.


- **High Abstraction Level** : Developers and researchers can build complex NLP systems without getting bogged down in the details of model training.


####


Looking Ahead


Plans are in place to test DSP further with more datasets and LMs, aiming to improve its adaptability and efficiency for a wider range of NLP tasks.


####


Conclusion


The DSP framework is a major step forward in combining language and retrieval models for NLP, offering a scalable and efficient method for developing systems that can handle complex queries accurately.


As DSP continues to evolve, it sets the stage for exciting advancements in NLP technology.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
