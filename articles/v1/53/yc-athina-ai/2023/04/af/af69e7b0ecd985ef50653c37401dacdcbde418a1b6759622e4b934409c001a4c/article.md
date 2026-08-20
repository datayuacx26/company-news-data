---
schema_version: "1.0.0"
document_id: "af69e7b0ecd985ef50653c37401dacdcbde418a1b6759622e4b934409c001a4c"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/revisiting-automated-prompting-are-we-actually-doing-better"
published_at: "2023-04-07T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:27.423621+00:00"
content_hash: "sha256:70a911a8ed26300a6ac70063ec2ef4b68f6e4c9beefbff621fab418447af03d1"
---

# Revisiting Automated Prompting: Are We Actually Doing Better?

Do not index


Original Paper


[https://arxiv.org/abs/2304.03609](https://arxiv.org/abs/2304.03609)


Blog URL


[blog.athina.ai /revisit...g-better](https://blog.athina.ai/revisiting-automated-prompting-are-we-actually-doing-better)


**Original Paper:**[https://arxiv.org/abs/2304.03609](https://arxiv.org/abs/2304.03609)


**By:**[Yulin Zhou](https://arxiv.org/search/cs?searchtype=author&query=Zhou%2C%20Y) ,[Yiren Zhao](https://arxiv.org/search/cs?searchtype=author&query=Zhao%2C%20Y) ,[Ilia Shumailov](https://arxiv.org/search/cs?searchtype=author&query=Shumailov%2C%20I) ,[Robert Mullins](https://arxiv.org/search/cs?searchtype=author&query=Mullins%2C%20R) ,[Yarin Gal](https://arxiv.org/search/cs?searchtype=author&query=Gal%2C%20Y)


**Abstract:**


> Current literature demonstrates that Large Language Models (LLMs) are great few-shot learners, and prompting significantly increases their performance on a range of downstream tasks in a few-shot learning setting. An attempt to automate human-led prompting followed, with some progress achieved. In particular, subsequent work demonstrates automation can outperform fine-tuning in certain K-shot learning scenarios. In this paper, we revisit techniques for automated prompting on six different downstream tasks and a larger range of K-shot learning settings. We find that automated prompting does not consistently outperform simple manual prompts. Our work suggests that, in addition to fine-tuning, manual prompts should be used as a baseline in this line of research.


---


###


Summary Notes


##


**Automated Prompting in AI: A Critical Examination**


The development of Large Language Models (LLMs) like GPT-3 has significantly advanced Natural Language Processing (NLP).


Among these advancements, *prompt-based learning* stands out for its potential to efficiently use these models, especially when data is limited.


Automated prompting, which generates prompts to guide LLMs in task execution without needing extensive training, has been gaining attention.


However, does it live up to its expectations? Let's delve into the realities of automated prompting with insights from recent research, focusing on its application in enterprise settings.


####


**Understanding Automated Prompting**


Automated prompting creates queries or "prompts" automatically to help LLMs understand and complete tasks with little to no additional training.


It's particularly useful in a few-shot learning setting, where the aim is to achieve good results with few examples.


Automated methods like AutoPrompt and Differentiable Prompts have been developed to streamline the once manual and time-consuming task of prompt creation.


####


**Evaluating Automated Prompt Efficiency**


Contrary to what one might expect, automated prompts don't always outperform manual ones. Key takeaways from recent studies, including research from the University of Cambridge and Imperial College London, include:


- Automated prompts can perform inconsistently across different tasks and data sizes compared to manual prompts.


- With enough data (100 examples or more), traditional fine-tuning might be more effective.


- Automated prompts sometimes generate irrelevant or semantically meaningless queries, which could hinder performance.


####


**Advice for AI Engineers**


Given these insights, AI Engineers in enterprise contexts should consider the following strategies:


1. **Compare Approaches** : Test both manual and automated prompts, particularly in situations with limited data, to see which performs better.


1. **Don't Dismiss Fine-Tuning** : Consider fine-tuning with larger datasets as it may be more effective than automated prompting.


1. **Assess Prompt Quality** : When using automated prompts, examine their relevance and meaningfulness to ensure they're suitable for the task.


1. **Test Widely** : Automated prompting's effectiveness can vary, so it's essential to try it across different tasks and data sizes to find where it excels.


1. **Value Simplicity** : Don't underestimate simple, manually-crafted prompts. They can sometimes outdo more complex automated ones.


####


**Conclusion**


While automating AI tasks is appealing, automated prompting doesn't always beat manual methods or fine-tuning with LLMs.


For AI Engineers in the enterprise sector focusing on efficiency and results, a balanced approach that weighs manual prompting and fine-tuning as potential strategies is recommended. Continuously evaluating and critically analyzing new AI methodologies will ensure the optimal use of LLMs in tackling complex NLP challenges.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
