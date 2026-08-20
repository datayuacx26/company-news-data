---
schema_version: "1.0.0"
document_id: "7999c36917f7c8d4fa9768f14bbff6e71906dfe1a492686e95f313c4a383e05f"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/enhancing-few-shot-text-to-sql-capabilities-of-large-language-models-a-study-on-prompt-design-strategies"
published_at: "2023-05-21T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:26.378360+00:00"
content_hash: "sha256:c84f8ce4d434dcbedd82def459db58ce976d0cee8e9cb2d1030cd6dfc38896e4"
---

# Enhancing Few-shot Text-to-SQL Capabilities of Large Language Models: A Study on Prompt Design Strategies

Do not index


Original Paper


[https://arxiv.org/abs/2305.12586](https://arxiv.org/abs/2305.12586)


Blog URL


[blog.athina.ai /enhanci...rategies](https://blog.athina.ai/enhancing-few-shot-text-to-sql-capabilities-of-large-language-models-a-study-on-prompt-design-strategies)


**Original Paper:**[https://arxiv.org/abs/2305.12586](https://arxiv.org/abs/2305.12586)


**By:**[Linyong Nan](https://arxiv.org/search/cs?searchtype=author&query=Nan%2C%20L) ,[Yilun Zhao](https://arxiv.org/search/cs?searchtype=author&query=Zhao%2C%20Y) ,[Weijin Zou](https://arxiv.org/search/cs?searchtype=author&query=Zou%2C%20W) ,[Narutatsu Ri](https://arxiv.org/search/cs?searchtype=author&query=Ri%2C%20N) ,[Jaesung Tae](https://arxiv.org/search/cs?searchtype=author&query=Tae%2C%20J) ,[Ellen Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang%2C%20E) ,[Arman Cohan](https://arxiv.org/search/cs?searchtype=author&query=Cohan%2C%20A) ,[Dragomir Radev](https://arxiv.org/search/cs?searchtype=author&query=Radev%2C%20D)


**Abstract:**


> In-context learning (ICL) has emerged as a new approach to various natural language processing tasks, utilizing large language models (LLMs) to make predictions based on context that has been supplemented with a few examples or task-specific instructions. In this paper, we aim to extend this method to question answering tasks that utilize structured knowledge sources, and improve Text-to-SQL systems by exploring various prompt design strategies for employing LLMs. We conduct a systematic investigation into different demonstration selection methods and optimal instruction formats for prompting LLMs in the Text-to-SQL task. Our approach involves leveraging the syntactic structure of an example's SQL query to retrieve demonstrations, and we demonstrate that pursuing both diversity and similarity in demonstration selection leads to enhanced performance. Furthermore, we show that LLMs benefit from database-related knowledge augmentations. Our most effective strategy outperforms the state-of-the-art system by 2.5 points (Execution Accuracy) and the best fine-tuned system by 5.1 points on the Spider dataset. These results highlight the effectiveness of our approach in adapting LLMs to the Text-to-SQL task, and we present an analysis of the factors contributing to the success of our strategy.


---


###


Summary Notes


####


Boosting Text-to-SQL Efficiency in Large Language Models with Innovative Prompt Design


In the dynamic field of artificial intelligence, businesses are increasingly relying on AI to sift through and make sense of extensive datasets.


AI engineers play a crucial role in fine-tuning these systems for enterprise use, with a significant challenge being the enhancement of semantic parsing in question answering systems.


This involves translating natural language questions into SQL commands efficiently. This blog post explores recent research that introduces new methods to improve the few-shot Text-to-SQL capabilities of Large Language Models (LLMs), providing valuable insights for AI engineers in the business sector.


###


The Text-to-SQL Challenge


Semantic parsing, specifically converting natural language to SQL queries, is essential for pulling relevant data from relational databases.


Despite progress, these systems struggle, especially in few-shot scenarios where only limited examples are available. The study by Nan et al. tackles these issues by proposing innovative prompt design strategies to enhance Text-to-SQL conversion tasks.


###


Innovative Strategies for Better Text-to-SQL Conversion


####


Choosing Demonstrations with SQL Syntax in Mind


A notable contribution from this study is a new method for selecting demonstrations based on the SQL syntactic structure.


This aims to balance diversity and similarity in demonstrations to boost model performance. By converting SQL queries into syntax vectors, the system can more effectively find relevant examples for any query.


####


Enhancing Schema Representation


The study also examines different ways to represent schema within LLM instructions. It suggests turning structured knowledge into a linear "code" sequence to improve the model's grasp of the database schema.


Additionally, the research looks into augmenting schema-related knowledge by including natural language definitions and summaries of entity-relationships in the prompts.


####


A Comprehensive Strategy for Text-to-SQL


Addressing performance variability due to the demonstration number, the research proposes a comprehensive strategy.


It involves using an initial model to create a draft SQL query, then selecting relevant demonstrations based on this draft.


A majority vote among various model predictions helps finalize the SQL query, marking a significant advancement in Text-to-SQL task optimization for LLMs.


###


Key Findings and Their Implications


The research tested these strategies across multiple datasets, like Spider and its variants, with LLMs such as Codex and gpt-3.5-turbo.


Execution Accuracy was the main metric. Results showed that a mix of diverse yet similar demonstration sampling, prompt schema augmentation, and the comprehensive strategy markedly enhanced performance.


####


Takeaways for AI Engineers


This research offers AI engineers practical methods to refine Text-to-SQL capabilities, leading to more precise and efficient data retrieval, which is crucial for informed decision-making and operational efficiency. Highlighted strategies include:


- **Balancing Example Selection** : Careful demonstration choice based on SQL structure can sharpen Text-to-SQL accuracy.


- **Schema Representation Optimization** : Trying different schema representations and augmentations within instructions can deepen the model's understanding of database structures.


- **Adopting Comprehensive Strategies** : Preliminary models and majority voting for demonstration and query finalization can effectively tackle performance variability.


###


Wrapping Up


The study by Nan et al. marks a leap forward in optimizing Text-to-SQL conversion for LLMs through inventive demonstration selection, schema representation, and comprehensive strategies.


For AI engineers in enterprises, applying these strategies could unlock higher efficiency and accuracy in data retrieval and analysis.


AI engineers should consider these approaches, tailoring and incorporating them to fit their organization's needs.


Staying updated with AI research and innovation is key to harnessing AI's full potential in enterprise applications.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
