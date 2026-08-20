---
schema_version: "1.0.0"
document_id: "31d79095b84ac033c44dd62e15c1c22668a48b36d0128b4c30e27b119d24a101"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/divide-and-prompt-chain-of-thought-prompting-for-text-to-sql"
published_at: "2023-04-23T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:27.423621+00:00"
content_hash: "sha256:4abbf7842d481d8927be93d305ea60ee6ea6d0c62edeb09970f2f152f94f7a55"
---

# Divide and Prompt: Chain of Thought Prompting for Text-to-SQL

Do not index


Original Paper


[https://arxiv.org/abs/2304.11556](https://arxiv.org/abs/2304.11556)


Blog URL


**Original Paper:**[https://arxiv.org/abs/2304.11556](https://arxiv.org/abs/2304.11556)


**By:**[Xiping Liu](https://arxiv.org/search/cs?searchtype=author&query=Liu%2C%20X) ,[Zhao Tan](https://arxiv.org/search/cs?searchtype=author&query=Tan%2C%20Z)


**Abstract:**


> Chain-of-thought (CoT) prompting combined with large language models (LLMs) have achieved encouraging results on complex reasoning tasks. Text-to-SQL is a critical semantic parsing task that converts natural language questions into SQL statements, involving a complex reasoning process. However, there is little work about using CoT prompting to activate LLM's reasoning capabilities on Text-to-SQL tasks. In this work, we propose a new paradigm for prompting Text-to-SQL tasks, called Divide-and-Prompt, which first divides the task into subtasks, and then approach each subtask through CoT. We present 3 prompting-based methods to enhance the Text-to-SQL ability of LLMs. Experiments show that these prompts guide LLMs to generate Text-to-SQL with higher execution accuracy.


---


###


Summary Notes


####


Simplifying Complex Queries with Divide and Prompt in Text-to-SQL Conversion


The advancement of Large Language Models (LLMs) like GPT-3 and BERT has significantly changed how machines process and generate human-like text, marking notable progress in the field of Artificial Intelligence and Natural Language Processing (NLP).


A particularly challenging task in this field is Text-to-SQL conversion, which involves translating natural language queries into Structured Query Language (SQL) commands. Despite progress, the complexity of Text-to-SQL often surpasses traditional models' capabilities, paving the way for innovative approaches like Chain of Thought (CoT) prompting, with a novel method known as "Divide and Prompt" (DnP).


###


**Understanding Divide-and-Prompt (DnP) Strategy**


####


**Simplifying Complex Problems**


The essence of the DnP strategy is to make complex problems simpler. It does this by:


- Breaking down a complex Text-to-SQL task into smaller, manageable subtasks.


- Addressing each subtask with specific prompts to guide the model towards the final SQL query.


This method mirrors the human approach to problem-solving by tackling parts of the problem one at a time.


####


**Different Flavors of DnP**


DnP comes in various forms, each serving a unique purpose:


- **Clause by Clause DnP (CC-DnP):** Constructs SQL queries step by step, focusing on one clause at a time for better syntactic structure management.


- **Schema Linking DnP (SL-DnP):** Prioritizes identifying relevant database schema elements before generating SQL commands, ensuring clarity in database structure understanding.


- **Generate and Refine DnP (GR-DnP):** Generates an initial SQL query and refines it through further prompts, allowing for corrections and adjustments to achieve more accurate queries.


###


**Evaluating DnP's Performance**


####


**Models and Metrics**


The performance of the DnP method was evaluated using top-tier models like GPT-3.5-Turbo, with the Spider dataset acting as the benchmark.


Assessment metrics included Execution Accuracy (EX), Valid SQL (VA), and Test-suite Accuracy (TS), offering a holistic view of the models' capabilities.


####


**Notable Outcomes**


DnP prompting showed considerable improvements, especially GR-DnP, in crafting complex queries. This indicates the effectiveness of iterative refinement in tackling Text-to-SQL challenges.


###


**Few-shot vs. Zero-shot Learning**


The DnP method highlighted the significance of few-shot learning, where feeding the model a few specific examples significantly boosted SQL query generation.


However, zero-shot learning, which involves generating queries without prior examples, proved challenging. This underscores the need for more structured training approaches for complex reasoning tasks.


###


**Looking Ahead**


The Divide and Prompt method marks a significant advancement in using CoT prompting for Text-to-SQL conversion. By breaking down complex issues into smaller parts, it enables LLMs to generate more accurate SQL queries.


Future efforts will aim at improving prompting techniques and diversifying datasets to enhance the models' robustness and applicability in various Text-to-SQL scenarios.


In summary, as we push the limits of LLM capabilities, innovative strategies like DnP open up new possibilities for how machines comprehend and interact with data. For AI Engineers in enterprise settings, leveraging these advancements is key to addressing real-world challenges more efficiently and accurately.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
