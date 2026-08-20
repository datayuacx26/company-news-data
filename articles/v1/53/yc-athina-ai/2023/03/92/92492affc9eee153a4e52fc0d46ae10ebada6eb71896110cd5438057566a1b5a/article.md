---
schema_version: "1.0.0"
document_id: "92492affc9eee153a4e52fc0d46ae10ebada6eb71896110cd5438057566a1b5a"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/larger-language-models-do-in-context-learning-differently"
published_at: "2023-03-07T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:29:11.084734+00:00"
content_hash: "sha256:a3d0fb88a1e6ca5c4ae18a157b9b744d3f2f404c10b50d28ebc4f819b92e9d5d"
---

# Larger language models do in-context learning differently

Do not index


Original Paper


[https://arxiv.org/abs/2303.03846](https://arxiv.org/abs/2303.03846)


Blog URL


[blog.athina.ai /larger-...ferently](https://blog.athina.ai/larger-language-models-do-in-context-learning-differently)


**Original Paper:**[https://arxiv.org/abs/2303.03846](https://arxiv.org/abs/2303.03846)


**By:**[Jerry Wei](https://arxiv.org/search/cs?searchtype=author&query=Wei%2C%20J) ,[Jason Wei](https://arxiv.org/search/cs?searchtype=author&query=Wei%2C%20J) ,[Yi Tay](https://arxiv.org/search/cs?searchtype=author&query=Tay%2C%20Y) ,[Dustin Tran](https://arxiv.org/search/cs?searchtype=author&query=Tran%2C%20D) ,[Albert Webson](https://arxiv.org/search/cs?searchtype=author&query=Webson%2C%20A) ,[Yifeng Lu](https://arxiv.org/search/cs?searchtype=author&query=Lu%2C%20Y) ,[Xinyun Chen](https://arxiv.org/search/cs?searchtype=author&query=Chen%2C%20X) ,[Hanxiao Liu](https://arxiv.org/search/cs?searchtype=author&query=Liu%2C%20H) ,[Da Huang](https://arxiv.org/search/cs?searchtype=author&query=Huang%2C%20D) ,[Denny Zhou](https://arxiv.org/search/cs?searchtype=author&query=Zhou%2C%20D) ,[Tengyu Ma](https://arxiv.org/search/cs?searchtype=author&query=Ma%2C%20T)


**Abstract:**


> We study how in-context learning (ICL) in language models is affected by semantic priors versus input-label mappings. We investigate two setups-ICL with flipped labels and ICL with semantically-unrelated labels-across various model families (GPT-3, InstructGPT, Codex, PaLM, and Flan-PaLM). First, experiments on ICL with flipped labels show that overriding semantic priors is an emergent ability of model scale. While small language models ignore flipped labels presented in-context and thus rely primarily on semantic priors from pretraining, large models can override semantic priors when presented with in-context exemplars that contradict priors, despite the stronger semantic priors that larger models may hold. We next study semantically-unrelated label ICL (SUL-ICL), in which labels are semantically unrelated to their inputs (e.g., foo/bar instead of negative/positive), thereby forcing language models to learn the input-label mappings shown in in-context exemplars in order to perform the task. The ability to do SUL-ICL also emerges primarily with scale, and large-enough language models can even perform linear classification in a SUL-ICL setting. Finally, we evaluate instruction-tuned models and find that instruction tuning strengthens both the use of semantic priors and the capacity to learn input-label mappings, but more of the former.


---


###


Summary Notes


####


Simplified Blog Post: Exploring In-Context Learning in AI Language Models


The field of artificial intelligence (AI) is constantly evolving, with language models at the heart of many advancements.


This post looks into in-context learning (ICL) in language models, a key technique that helps AI understand and use new information effectively.


We'll discuss how large language models manage to learn from the context of a given input, balancing their existing knowledge with new data.


####


Study Overview


Researchers explored how different AI models, including those from the GPT-3, InstructGPT, Codex, and PaLM families, adapt to tasks like Sentiment Analysis and Question Classification.


They tested these models using few-shot exemplars in two setups: Flipped-Label ICL and Semantically Unrelated Label ICL (SUL-ICL).


The focus was on how model size affects their ability to use semantic knowledge and input-label mappings.


####


Key Findings


####


Flipped-Label ICL


- **Small vs. Large Models** : Smaller models had trouble with flipped labels, depending on their pre-learned knowledge. Larger models, however, adapted well, focusing more on the new context.


- **Adaptability of Larger Models** : This shows that larger models are better at prioritizing new input over their existing knowledge.


####


Semantically Unrelated Label ICL (SUL-ICL)


- **Impact on Performance** : Smaller models performed poorly without semantic priors, while larger models did better, relying on the new context.


- **Larger Models' Approach** : This indicates that larger models are good at using context clues, which is key to their success in various tasks.


####


Instruction Tuning Effects


- **SUL-ICL Performance** : Instruction-tuned models were generally better in SUL-ICL but struggled with flipped labels. This means instruction tuning helps models use both semantic knowledge and context more effectively.


- **Balancing Act** : Instruction-tuned models are better at using both their knowledge and the new context, improving their overall performance.


####


Discussion


The study shows that larger language models excel at shifting their focus from their built-in knowledge to the context of the input.


This flexibility is crucial for performing well across different tasks. Moreover, instruction tuning further enhances this ability, pointing to a promising direction for future developments.


####


Conclusion


Larger language models have a unique ability to adjust their learning based on the context, relying less on their existing knowledge and more on the specifics of the input.


This research underscores the importance of model size and training methods in enhancing the adaptability and effectiveness of language models in ICL scenarios.


As AI continues to evolve, optimizing in-context learning will be key for tackling complex natural language processing tasks.


####


References


- Studies on the foundational concepts of in-context learning and the learning capabilities of transformer models across various scales and training setups.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
