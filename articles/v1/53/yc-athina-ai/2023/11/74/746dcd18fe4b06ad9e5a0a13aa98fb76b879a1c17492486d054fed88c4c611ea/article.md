---
schema_version: "1.0.0"
document_id: "746dcd18fe4b06ad9e5a0a13aa98fb76b879a1c17492486d054fed88c4c611ea"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/how-to-prompt-llms-for-text-to-sql-a-study-in-zero-shot-single-domain-and-cross-domain-settings"
published_at: "2023-11-27T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:21.272352+00:00"
content_hash: "sha256:e616b603f5ce0ebc1e3c3fe2a58af055d330fae3b5fd5da8c2de92955f2d90d5"
---

# How to Prompt LLMs for Text-to-SQL: A Study in Zero-shot, Single-domain, and Cross-domain Settings

Do not index


Original Paper


[https://arxiv.org/abs/2305.11853](https://arxiv.org/abs/2305.11853)


Blog URL


[blog.athina.ai /how-to-...settings](https://blog.athina.ai/how-to-prompt-llms-for-text-to-sql-a-study-in-zero-shot-single-domain-and-cross-domain-settings)


**Original Paper:**[https://arxiv.org/abs/2305.11853](https://arxiv.org/abs/2305.11853)


**By:**[Shuaichen Chang](https://arxiv.org/search/cs?searchtype=author&query=Chang%2C%20S) ,[Eric Fosler-Lussier](https://arxiv.org/search/cs?searchtype=author&query=Fosler-Lussier%2C%20E)


**Abstract:**


> Large language models (LLMs) with in-context learning have demonstrated remarkable capability in the text-to-SQL task. Previous research has prompted LLMs with various demonstration-retrieval strategies and intermediate reasoning steps to enhance the performance of LLMs. However, those works often employ varied strategies when constructing the prompt text for text-to-SQL inputs, such as databases and demonstration examples. This leads to a lack of comparability in both the prompt constructions and their primary contributions. Furthermore, selecting an effective prompt construction has emerged as a persistent problem for future research. To address this limitation, we comprehensively investigate the impact of prompt constructions across various settings and provide insights into prompt constructions for future text-to-SQL studies.


---


###


Summary Notes


####


Simplifying Text-to-SQL Translation with Large Language Models: Insights and Techniques


The field of artificial intelligence (AI) has seen significant advancements, with one notable breakthrough being the translation of natural language queries into SQL (Structured Query Language) commands, known as text-to-SQL.


This development enables AI engineers and data analysts to query databases without needing to know complex SQL syntax. The emergence of Large Language Models (LLMs) like GPT-3 Codex and ChatGPT has further advanced text-to-SQL capabilities through in-context learning.


###


Understanding In-Context Learning for Text-to-SQL


In-context learning is a cutting-edge approach allowing LLMs to generate SQL queries from natural language prompts without requiring extensive training datasets. Key settings for in-context learning include:


- **Zero-shot:** The model attempts to generate SQL queries with no prior examples.


- **Single-domain Few-shot:** The model is given a few examples from the same domain to help it understand the context.


- **Cross-domain Few-shot:** This setting tests the model's ability to apply its knowledge across different domains using examples from various databases.


###


Building Effective Prompts


Creating effective prompts is essential for optimizing LLM performance in text-to-SQL tasks. A well-constructed prompt should have:


- **Task Instruction:** Clearly tells the model what to do.


- **Test Database:** Provides details on the database schema and content. This can include:


- **Test Question:** The natural language query to be translated into SQL.


- **Demonstrations:** (Optional) SQL queries and their natural language descriptions as examples.


###


Study Findings and Insights


Our research, utilizing the Spider dataset for cross-domain text-to-SQL performance, compared GPT-3 Codex and ChatGPT under various prompt settings. Key insights include:


- Accurate representation of table relationships and database contents in prompts significantly improves accuracy.


- Prompts that are normalized—concise and well-structured—enhance performance by ensuring clarity and reducing token count.


- Including in-domain demonstrations boosts performance in single-domain settings by providing relevant examples.


- In cross-domain settings, the length of the prompt greatly influences accuracy, with optimal lengths yielding better results.


###


Moving Forward with Text-to-SQL and LLMs


Our study offers strategies to enhance LLM performance in text-to-SQL translation, focusing on prompt construction and the strategic inclusion of demonstrations and schema representations.


For AI engineers in enterprise environments, these findings offer actionable insights for employing LLMs in text-to-SQL tasks effectively.


As we continue to refine these models, developing standardized prompt construction and evaluation methods will be crucial for unlocking their full potential.


###


Expanding on Previous Work


This study builds on previous research aimed at improving LLM capabilities in text-to-SQL translation, exploring techniques like demonstration retrieval, intermediate reasoning, and various prompt engineering strategies.


By providing a comprehensive evaluation across different settings, our work seeks to standardize approaches and enhance the effectiveness of LLMs in enterprise applications.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
