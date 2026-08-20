---
schema_version: "1.0.0"
document_id: "e6e6359de54ce05d90b7befcce9436ae93f73e7a2ac7ba0ec5d22f1cd23061ac"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/on-the-empirical-complexity-of-reasoning-and-planning-in-llms"
published_at: "2024-04-17T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:25:56.819128+00:00"
content_hash: "sha256:4b4ca0eda4757f9c67ca67bd0938a902a00a861ebb06b4ed470aa2903744e3eb"
---

# On the Empirical Complexity of Reasoning and Planning in LLMs

Do not index


Original Paper


[https://arxiv.org/abs/2404.11041](https://arxiv.org/abs/2404.11041)


Blog URL


[blog.athina.ai /on-the-...-in-llms](https://blog.athina.ai/on-the-empirical-complexity-of-reasoning-and-planning-in-llms)


**Original Paper:**[https://arxiv.org/abs/2404.11041](https://arxiv.org/abs/2404.11041)


**By:**[Liwei Kang](https://arxiv.org/search/cs?searchtype=author&query=Kang%2C%20L) ,[Zirui Zhao](https://arxiv.org/search/cs?searchtype=author&query=Zhao%2C%20Z) ,[David Hsu](https://arxiv.org/search/cs?searchtype=author&query=Hsu%2C%20D) ,[Wee Sun Lee](https://arxiv.org/search/cs?searchtype=author&query=Lee%2C%20W%20S)


**Abstract:**


> Large Language Models (LLMs) work surprisingly well for some complex reasoning problems via chain-of-thought (CoT) or tree-of-thought (ToT), but the underlying reasons remain unclear. We seek to understand the performance of these methods by conducting experimental case studies and linking the outcomes to sample and computational complexity in machine learning. We found that if problems can be decomposed into a sequence of reasoning steps and learning to predict the next step has a low sample and computational complexity, explicitly outlining the reasoning chain with all necessary information for predicting the next step may improve performance. Conversely, for problems where predicting the next step is computationally hard, adopting ToT may yield better reasoning outcomes than attempting to formulate a short reasoning chain.


---


###


Summary Notes


####


Simplifying the Complexity of LLM Reasoning and Planning


The advancement of Large Language Models (LLMs) has revolutionized artificial intelligence, especially in tackling complex reasoning and planning tasks.


These tasks, which require detailed multi-step processes, present challenges in both computational resources and problem-solving methodologies.


Recent strategies like Chain of Thought (CoT) and Tree of Thought (ToT) have shown promise, but understanding their full effectiveness remains a work in progress.


####


Breaking Down LLM Reasoning


LLMs are now capable of solving complex reasoning problems by outlining their thought processes, thanks to methods like CoT and ToT.


These techniques have broadened the scope of AI, especially in decision-making and analysis. However, the impact of these methods under varying conditions is still partly unknown.


####


Key Methodologies Explained


- **Chain of Thought (CoT):** Breaks reasoning into a sequential process, similar to human thinking.


- **Tree of Thought (ToT):** Builds on CoT by introducing branches, allowing multiple thought paths or solutions.


####


Exploring Complexities in LLMs


Research by Liwei Kang, Zirui Zhao, David Hsu, and Wee Sun Lee at the National University of Singapore investigates the complexities in reasoning and planning with LLMs, focusing on CoT and ToT. They look into:


- **Computational Complexity:** Complexity in learning (adjusting parameters) and reasoning (finding solutions after learning).


- **Sample Complexity:** The data amount needed for effective learning and generalization.


####


Research Insights


This study provides key findings through case studies on arithmetic, dynamic programming, air travel planning, and the Game of 24:


- **Problem Breakdown:** Splitting problems into smaller parts using CoT or ToT can decrease sample complexity and improve outcomes.


- **Annotations in CoT:** Including necessary information in CoT format can enhance performance by reducing learning complexity.


- **ToT Advantages:** In tasks where finding a simple solution chain is challenging, ToT outperforms CoT.


####


Practical Advice for AI Engineers


For AI engineers in businesses, this research offers valuable guidance:


- **Break Down Problems:** For complex tasks, divide the problem into smaller segments.


- **Choose the Right Method:** Depending on the task's complexity, select between CoT and ToT for better results.


- **Utilize Data Efficiently:** Remember the importance of sample complexity. Efficient data use can greatly influence LLM performance in complex tasks.


####


Conclusion


The research "On the Empirical Complexity of Reasoning and Planning in LLMs" sheds light on using CoT and ToT in LLMs for complex tasks.


It highlights the need for AI engineers to consider both computational and sample complexities in their work.


As AI continues to advance, further studies on these relationships will be key to unlocking LLMs' full problem-solving capabilities.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
