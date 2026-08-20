---
schema_version: "1.0.0"
document_id: "ed868132c2931dbf27f2a8248848dc3cdc49d3bc1bdd04f073395f9dceb6c0ba"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/can-we-edit-factual-knowledge-by-in-context-learning"
published_at: "2023-05-22T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:26.378360+00:00"
content_hash: "sha256:a8beadcbb891a84d53627005e1df35d0d894eb51144c93cee08953defceb1e59"
---

# Can We Edit Factual Knowledge by In-Context Learning?

Do not index


Original Paper


[https://arxiv.org/abs/2305.12740](https://arxiv.org/abs/2305.12740)


Blog URL


[blog.athina.ai /can-we-...learning](https://blog.athina.ai/can-we-edit-factual-knowledge-by-in-context-learning)


**Original Paper:**[https://arxiv.org/abs/2305.12740](https://arxiv.org/abs/2305.12740)


**By:**[Ce Zheng](https://arxiv.org/search/cs?searchtype=author&query=Zheng%2C%20C) ,[Lei Li](https://arxiv.org/search/cs?searchtype=author&query=Li%2C%20L) ,[Qingxiu Dong](https://arxiv.org/search/cs?searchtype=author&query=Dong%2C%20Q) ,[Yuxuan Fan](https://arxiv.org/search/cs?searchtype=author&query=Fan%2C%20Y) ,[Zhiyong Wu](https://arxiv.org/search/cs?searchtype=author&query=Wu%2C%20Z) ,[Jingjing Xu](https://arxiv.org/search/cs?searchtype=author&query=Xu%2C%20J) ,[Baobao Chang](https://arxiv.org/search/cs?searchtype=author&query=Chang%2C%20B)


**Abstract:**


> Previous studies have shown that large language models (LLMs) like GPTs store massive factual knowledge in their parameters. However, the stored knowledge could be false or out-dated. Traditional knowledge editing methods refine LLMs via fine-tuning on texts containing specific knowledge. However, with the increasing scales of LLMs, these gradient-based approaches bring large computation costs. The trend of model-as-a-service also makes it impossible to modify knowledge in black-box LMs. Inspired by in-context learning (ICL), a new paradigm based on demonstration contexts without parameter updating, we explore whether ICL can edit factual knowledge. To answer this question, we give a comprehensive empirical study of ICL strategies. Experiments show that in-context knowledge editing (IKE), without any gradient and parameter updating, achieves a competitive success rate compared to gradient-based methods on GPT-J (6B) but with much fewer side effects, including less over-editing on similar but unrelated facts and less knowledge forgetting on previously stored knowledge. We also apply the method to larger LMs with tens or hundreds of parameters like OPT-175B, which shows the scalability of our method. The code is available at
>
>
> [this https URL](https://github.com/Zce1112zslx/IKE)


---


###


Summary Notes


####


Simplified Blog Post: Updating Knowledge in AI with In-Context Learning


The world of artificial intelligence (AI) is always moving forward, with Large Language Models (LLMs) like GPT-3 leading the charge in innovations from chatbots to content generation.


Yet, these models face a big challenge: they often work with outdated or incorrect information.


This is a big problem for AI developers in businesses that need up-to-date and accurate information for their AI tools to be useful.


###


The Issue with Outdated Info in AI Models


LLMs learn from huge sets of data from the internet, capturing knowledge from a specific moment in time.


After their initial training, they don't automatically update or correct their knowledge, leading to potentially outdated or incorrect outputs.


For industries like finance, healthcare, or legal where accuracy is critical, this can be a big issue.


Traditionally, updating an LLM's knowledge means retraining or fine-tuning it, which can be costly and might cause the model to forget some of what it previously learned.


So, the question is, how can we keep LLMs up-to-date without these downsides?


###


A New Approach: In-Context Knowledge Editing


In-Context Learning (ICL) offers a solution. It allows LLMs to learn from new information in their input prompts, making it possible to edit their knowledge base on the fly.


####


How It Works


- **Copy** : The model is fed new information as part of its input.


- **Update** : It's prompted to focus on this new information when generating responses.


- **Retain** : Relevant, unchanged info is also included in the prompts to help the model keep important facts.


This method, known as In-Context Knowledge Editing (IKE), has been shown to be more efficient and scalable than traditional methods, with fewer unintended consequences.


###


Benefits and Implementation


For AI developers, IKE means crafting prompts that include the new or corrected information. Its advantages include:


- **Efficiency** : It avoids the high costs of retraining or fine-tuning.


- **Flexibility** : It works with a wide range of models and scenarios.


- **Scalability** : It's effective for models of all sizes, including giants like GPT-3.


####


IKE in Action


Experiments have shown that IKE can more accurately update knowledge in LLMs compared to older methods, making it a promising option for businesses that need their AI to stay accurate and up-to-date.


###


Real-World Use and Ongoing Challenges


IKE is practical for keeping LLMs current in fields like science, technology, and finance. However, there are still challenges, especially in managing multiple updates and dealing with different types of information. More research is needed to make IKE even more versatile and easier to use.


###


Conclusion


In-Context Knowledge Editing is a big step towards keeping LLMs relevant and accurate. It offers an efficient, flexible, and scalable way to update knowledge, addressing a vital need for AI applications in business.


As this technology evolves, it will likely open up new possibilities for using LLMs across various industries, ensuring they stay at the forefront of knowledge.


For those interested in exploring IKE further, resources and examples are available on GitHub, showcasing this innovative approach to knowledge editing in AI and its potential to advance the field of natural language processing.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
