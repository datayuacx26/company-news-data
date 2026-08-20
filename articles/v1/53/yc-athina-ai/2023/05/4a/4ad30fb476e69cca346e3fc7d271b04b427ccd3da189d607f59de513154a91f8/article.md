---
schema_version: "1.0.0"
document_id: "4ad30fb476e69cca346e3fc7d271b04b427ccd3da189d607f59de513154a91f8"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/efficient-prompting-via-dynamic-in-context-learning"
published_at: "2023-05-18T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:26.378360+00:00"
content_hash: "sha256:3cc2932aa57e2d377c8b8a8b758b4647f7abb4d461cc4dc9f690bdf7135051ea"
---

# Efficient Prompting via Dynamic In-Context Learning

Do not index


Original Paper


[https://arxiv.org/abs/2305.11170](https://arxiv.org/abs/2305.11170)


Blog URL


[blog.athina.ai /efficie...learning](https://blog.athina.ai/efficient-prompting-via-dynamic-in-context-learning)


**Original Paper:**[https://arxiv.org/abs/2305.11170](https://arxiv.org/abs/2305.11170)


**By:**[Wangchunshu Zhou](https://arxiv.org/search/cs?searchtype=author&query=Zhou%2C%20W) ,[Yuchen Eleanor Jiang](https://arxiv.org/search/cs?searchtype=author&query=Jiang%2C%20Y%20E) ,[Ryan Cotterell](https://arxiv.org/search/cs?searchtype=author&query=Cotterell%2C%20R) ,[Mrinmaya Sachan](https://arxiv.org/search/cs?searchtype=author&query=Sachan%2C%20M)


**Abstract:**


> The primary way of building AI applications is shifting from training specialist models to prompting generalist models. A common practice for prompting generalist models, often referred to as in-context learning, is to append a few examples (demonstrations) to the prompt to help the model better understand the task. While effective, in-context learning can be inefficient because it makes the input prompt much longer, consuming valuable space in the context window and leading to larger computational costs. In this paper, we propose DynaICL, a recipe for efficient prompting with black-box generalist models that dynamically allocate in-context examples according to the input complexity and the computational budget. To achieve this, we train a meta controller that predicts the number of in-context examples suitable for the generalist model to make a good prediction based on the performance-efficiency trade-off for a specific input. We then dynamically allocate the number of demonstrations for an input according to predictions from the meta controller and the given computation budget. Experimental results show that dynamic example allocation helps achieve a better performance-efficiency trade-off in two practical settings where computational resources or the required performance is constrained. Specifically, DynaICL saves up to 46% token budget compared to the common practice that allocates the same number of in-context examples to each input. We also find that a meta controller trained on a certain backbone model and tasks can successfully generalize to unseen models and tasks.


---


###


Summary Notes


####


Efficient Prompting with Dynamic In-Context Learning (DYNA ICL)


The landscape of artificial intelligence (AI) is constantly evolving, moving towards models that can handle multiple tasks instead of just specializing in one. Generalist models like GPT and BERT are at the forefront of this shift, offering versatility in solving various problems.


However, a key challenge with these models is the inefficiency of traditional in-context learning, which often requires substantial computational resources.


This blog post introduces an innovative approach to tackle this issue: Efficient Prompting via Dynamic In-Context Learning (DYNA ICL), aimed at improving the efficiency of generalist models.


####


The Challenge with Traditional In-Context Learning


In-context learning is a technique where examples are added to prompts to help AI models understand and perform tasks without needing specific training for each task.


Although effective, this method can be resource-intensive, demanding a high amount of computational power.


####


Introducing Dynamic In-Context Learning (DYNA ICL)


DYNA ICL presents a solution to overcome the limitations of traditional in-context learning by optimizing the use of in-context examples. This approach involves:


- **Meta Controller** : A smaller model that determines the ideal number of in-context examples required for efficient prompting. It's trained through data synthesis and reinforcement learning techniques.


- **Dynamic Example Allocation** : Adjusting the number of in-context examples based on the meta controller's advice and the available computational resources.


DYNA ICL has been shown to save up to 46% in token usage, significantly lowering the computational demands of in-context learning.


####


Results and Insights


The effectiveness of DYNA ICL was validated using models like ChatGPT across various tasks. The findings indicated that not only does DYNA ICL conserve computational resources, but it also maintains or enhances the performance of tasks. Moreover, it showed promising generalization abilities, performing adeptly on tasks and models it hadn't encountered before.


####


Potential Beyond NLP


Initially tested on natural language processing (NLP) tasks, the principles of DYNA ICL hold promise for wider applications across different AI fields. Its dynamic example allocation algorithm, due to its simplicity and adaptability, could pave the way for more advanced efficiency-enhancing methods.


####


Conclusion


Dynamic In-Context Learning marks a significant advancement towards creating more efficient and versatile AI models.


By dynamically adjusting in-context examples according to the task complexity and computational limits, DYNA ICL provides a scalable and adaptive solution that can cater to diverse tasks and models.


This method not only solves the efficiency issues of traditional in-context learning but also broadens the horizons for applying generalist AI models across various domains.


As AI continues to progress, the efficiency and adaptability of models become increasingly crucial. The development of techniques like DYNA ICL highlights the need for innovative solutions to surpass the current technological constraints, setting the stage for the future evolution of AI.


The principles of efficient prompting and dynamic in-context learning are poised to significantly influence the development of artificial intelligence.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
