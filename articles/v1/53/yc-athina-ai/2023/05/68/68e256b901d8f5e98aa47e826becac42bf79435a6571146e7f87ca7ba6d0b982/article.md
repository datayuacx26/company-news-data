---
schema_version: "1.0.0"
document_id: "68e256b901d8f5e98aa47e826becac42bf79435a6571146e7f87ca7ba6d0b982"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/pearl-prompting-large-language-models-to-plan-and-execute-actions-over-long-documents"
published_at: "2023-05-23T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:26.378360+00:00"
content_hash: "sha256:3ac0c7913b7fcf52b5ad214cd58911390d6c3ebfbfc3c9ee8e7c24229c850b88"
---

# PEARL: Prompting Large Language Models to Plan and Execute Actions Over Long Documents

Do not index


Original Paper


[https://arxiv.org/abs/2305.14564v1](https://arxiv.org/abs/2305.14564v1)


Blog URL


[blog.athina.ai /pearl-p...ocuments](https://blog.athina.ai/pearl-prompting-large-language-models-to-plan-and-execute-actions-over-long-documents)


**Original Paper:**[https://arxiv.org/abs/2305.14564v1](https://arxiv.org/abs/2305.14564v1)


**By:**[Simeng Sun](https://arxiv.org/search/cs?searchtype=author&query=Sun%2C%20S) ,[Yang Liu](https://arxiv.org/search/cs?searchtype=author&query=Liu%2C%20Y) ,[Shuohang Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang%2C%20S) ,[Chenguang Zhu](https://arxiv.org/search/cs?searchtype=author&query=Zhu%2C%20C) ,[Mohit Iyyer](https://arxiv.org/search/cs?searchtype=author&query=Iyyer%2C%20M)


**Abstract:**


> Strategies such as chain-of-thought prompting improve the performance of large language models (LLMs) on complex reasoning tasks by decomposing input examples into intermediate steps. However, it remains unclear how to apply such methods to reason over long input documents, in which both the decomposition and the output of each intermediate step are non-trivial to obtain. In this work, we propose PEARL, a prompting framework to improve reasoning over long documents, which consists of three stages: action mining, plan formulation, and plan execution. More specifically, given a question about a long document, PEARL decomposes the question into a sequence of actions (e.g., SUMMARIZE, FIND_EVENT, FIND_RELATION) and then executes them over the document to obtain the answer. Each stage of PEARL is implemented via zero-shot or few-shot prompting of LLMs (in our work, GPT-4) with minimal human input. We evaluate PEARL on a challenging subset of the QuALITY dataset, which contains questions that require complex reasoning over long narrative texts. PEARL outperforms zero-shot and chain-of-thought prompting on this dataset, and ablation experiments show that each stage of PEARL is critical to its performance. Overall, PEARL is a first step towards leveraging LLMs to reason over long documents.


---


###


Summary Notes


##


PEARL: A New Frontier in AI's Understanding of Long Documents


In the world of AI and machine learning, processing and making sense of lengthy documents remains a significant challenge for engineers at big companies.


Traditional methods often struggle with the complexity of these texts. That's where PEARL comes in, a new solution that uses Large Language Models (LLMs) in innovative ways to address this issue.


###


The Challenge


Understanding long documents goes beyond just reading the text. It involves drawing complex conclusions and making connections throughout the entire document.


While existing methods like chain-of-thought prompting have made progress, they still don't fully grasp the intricacies of lengthy texts.


PEARL introduces a structured method to break down complex questions into manageable steps, leading to more insightful answers.


###


PEARL: What Makes It Different?


PEARL, which stands for Prompting Large Language Models to Plan and Execute Actions Over Long Documents, advances beyond previous methods by focusing on the specific challenges of handling long documents.


###


How PEARL Works


PEARL's approach is divided into three main phases:


- **Action Mining** : Identifying actionable steps from the dataset that can help answer a question, utilizing the capabilities of LLMs.


- **Plan Generation** : Creating an executable plan from these steps, tailored to address the question at hand.


- **Plan Execution** : Implementing the plan step-by-step, thoroughly analyzing the document.


###


Digging Deeper into PEARL's Process


- **Action Mining** : Starts with a few seed actions and expands them using the LLM to cover various specific actions derived from training questions.


- **Plan Generation** : Plans are sequences of actions arranged logically for effective execution.


- **Plan Execution** : Actions are carried out one after the other, with each step informed by the outcome of the previous one, ensuring a detailed examination of the document.


###


Performance and Results


When tested on the challenging parts of the QuALITY dataset, designed for generative question-answering, PEARL outperforms existing methods. It shows remarkable accuracy and the ability to reason deeply across longer texts.


###


Looking Ahead


Despite its advancements, PEARL has areas for improvement, such as the risk of generating incorrect information, its computational demands, and the possibility of complicating simple questions.


Future work could focus on optimizing action mining, improving the plan generation process, and making the model adaptable to different types of documents.


###


Conclusion


PEARL represents a significant step forward in using LLMs for complex reasoning over long documents.


It simplifies questions into actionable plans and executes them methodically, overcoming limitations of prior methods.


For AI engineers, PEARL not only provides a powerful tool for handling extensive texts but also paves the way for more nuanced and comprehensive text analysis.


As AI and machine learning continue to evolve, PEARL exemplifies the progress in understanding and processing vast amounts of information.


It suggests a future where our comprehension of data can match its depth, heralding new innovations in the use of large language models.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
