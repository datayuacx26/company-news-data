---
schema_version: "1.0.0"
document_id: "30e504c6bb6e865e892c291e556d697597dd7c934edc6ad979dbc1165f067157"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/knowledge-driven-cot-exploring-faithful-reasoning-in-llms-for-knowledge-intensive-question-answering"
published_at: "2023-10-28T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:22.552077+00:00"
content_hash: "sha256:4080bfc82849ec8d549cc27c882a5ad9cb7aeb424322851556ee35f3edf003e2"
---

# Knowledge-Driven CoT: Exploring Faithful Reasoning in LLMs for Knowledge-intensive Question Answering

Do not index


Original Paper


[https://arxiv.org/abs/2308.13259](https://arxiv.org/abs/2308.13259)


Blog URL


[blog.athina.ai /knowled...nswering](https://blog.athina.ai/knowledge-driven-cot-exploring-faithful-reasoning-in-llms-for-knowledge-intensive-question-answering)


**Original Paper:**[https://arxiv.org/abs/2308.13259](https://arxiv.org/abs/2308.13259)


**By:**[Keheng Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang%2C%20K) ,[Feiyu Duan](https://arxiv.org/search/cs?searchtype=author&query=Duan%2C%20F) ,[Sirui Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang%2C%20S) ,[Peiguang Li](https://arxiv.org/search/cs?searchtype=author&query=Li%2C%20P) ,[Yunsen Xian](https://arxiv.org/search/cs?searchtype=author&query=Xian%2C%20Y) ,[Chuantao Yin](https://arxiv.org/search/cs?searchtype=author&query=Yin%2C%20C) ,[Wenge Rong](https://arxiv.org/search/cs?searchtype=author&query=Rong%2C%20W) ,[Zhang Xiong](https://arxiv.org/search/cs?searchtype=author&query=Xiong%2C%20Z)


**Abstract:**


> Equipped with Chain-of-Thought (CoT), Large language models (LLMs) have shown impressive reasoning ability in various downstream tasks. Even so, suffering from hallucinations and the inability to access external knowledge, LLMs often come with incorrect or unfaithful intermediate reasoning steps, especially in the context of answering knowledge-intensive tasks such as KBQA. To alleviate this issue, we propose a framework called Knowledge-Driven Chain-of-Thought (KD-CoT) to verify and modify reasoning traces in CoT via interaction with external knowledge, and thus overcome the hallucinations and error propagation. Concretely, we formulate the CoT rationale process of LLMs into a structured multi-round QA format. In each round, LLMs interact with a QA system that retrieves external knowledge and produce faithful reasoning traces based on retrieved precise answers. The structured CoT reasoning of LLMs is facilitated by our developed KBQA CoT collection, which serves as in-context learning demonstrations and can also be utilized as feedback augmentation to train a robust retriever. Extensive experiments on WebQSP and ComplexWebQuestion datasets demonstrate the effectiveness of proposed KD-CoT in task-solving reasoning generation, which outperforms the vanilla CoT ICL with an absolute success rate of 8.0% and 5.1%. Furthermore, our proposed feedback-augmented retriever outperforms the state-of-the-art baselines for retrieving knowledge, achieving significant improvement in Hit and recall performance. Our code and data are released on
>
>
> [this https URL](https://github.com/AdelWang/KD-CoT/tree/main)


---


###


Summary Notes


##


Boosting AI Reasoning with Knowledge-Driven CoT for Enterprise AI Engineers


The field of artificial intelligence (AI) is rapidly advancing, with Large Language Models (LLMs) leading the way by creating text that closely mimics human writing.


However, these models often face challenges with reasoning tasks that require deep, detailed processing, such as generating incorrect information or relying on incomplete knowledge.


This is a big problem for AI Engineers in large companies who use LLMs for complex decision-making tasks. The Knowledge-Driven Chain-of-Thought (KD-CoT) approach is a new solution designed to improve the reliability and scalability of AI reasoning models.


####


The Challenge with AI Reasoning


LLMs are powerful but not perfect. They commonly face issues like:


- **Hallucination** : Creating incorrect information not found in the input data.


- **Incomplete Knowledge** : Struggling with questions that need information beyond what they were trained on.


These problems are especially severe in tasks requiring detailed, multi-step reasoning.


####


What is Knowledge-Driven CoT (KD-CoT)?


KD-CoT is a new framework that enhances the traditional Chain-of-Thought (CoT) reasoning by integrating external knowledge checks. This method aims to improve the accuracy of LLMs and reduce errors due to hallucination and incomplete knowledge.


####


Key Features of KD-CoT:


- **Structured Multi-Round QA Format** : KD-CoT uses a format where LLMs interact with an external QA system in several rounds, allowing for the verification and refinement of each reasoning step.


- **External Knowledge Verification** : Integrating an external QA system ensures each reasoning step is based on verified information.


- **KBQA CoT Collection** : A collection designed to provide the context and training needed to improve CoT reasoning in LLMs.


####


Methodology for Enhancing AI Reasoning


KD-CoT refines LLM reasoning by:


- **Structured CoT Reasoning** : Breaking the reasoning into a series of QA interactions, verified with external knowledge.


- **Feedback-Augmented Retriever** : A knowledge retriever improved with feedback from the CoT collection, enhancing the accuracy of information retrieval.


####


Proving KD-CoT's Effectiveness


KD-CoT was tested on standard datasets like WebQSP and ComplexWebQuestions, showing:


- **Higher Success Rates** : KD-CoT significantly outperformed traditional CoT models in reasoning accuracy.


- **Better Retriever Performance** : The feedback-augmented retriever provided unmatched precision and recall.


####


The Future of AI Reasoning


KD-CoT marks a significant advancement, but there is still room for improvement by:


- **Developing More Efficient Retrieval Systems** : Making the knowledge retrieval process quicker and more accurate.


- **Reducing Hallucination** : Further refining the model to decrease errors and ensure more reliable outputs.


####


Conclusion: Advancing AI Reasoning


KD-CoT represents a major step forward in integrating external knowledge verification with LLM reasoning processes.


For enterprise AI Engineers, it offers a solution to the current challenges and a path toward more reliable and scalable AI reasoning models. As AI continues to evolve, KD-CoT will play a crucial role in the future of AI reasoning.


**Additional Resources** : For a deeper look into KD-CoT, access the code and data at[KD-CoT GitHub Repository](https://github.com/AdelWang/KD-CoT/tree/main) .


**Visual Insights** : Check out the supplementary materials for visuals on LLMs' failure cases without KD-CoT, examples of KD-CoT in action, and detailed comparisons with other models.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
