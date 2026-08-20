---
schema_version: "1.0.0"
document_id: "ae01713165f0c154f3a563318f47680b4e433256dc63374d87e2f2daf9bbdf37"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/a-step-closer-to-comprehensive-answers-constrained-multi-stage-question-decomposition-with-large-language-models"
published_at: "2023-11-13T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:22.552077+00:00"
content_hash: "sha256:6b4ab3a63e133e29fe5bf596c337dcd159fc2928e4d8080eb542dbb8a5e77a8c"
---

# A Step Closer to Comprehensive Answers: Constrained Multi-Stage Question Decomposition with Large Language Models

Do not index


Original Paper


[https://arxiv.org/abs/2311.07491](https://arxiv.org/abs/2311.07491)


Blog URL


[blog.athina.ai /a-step-...e-models](https://blog.athina.ai/a-step-closer-to-comprehensive-answers-constrained-multi-stage-question-decomposition-with-large-language-models)


**Original Paper:**[https://arxiv.org/abs/2311.07491](https://arxiv.org/abs/2311.07491)


**By:**[Hejing Cao](https://arxiv.org/search/cs?searchtype=author&query=Cao%2C%20H) ,[Zhenwei An](https://arxiv.org/search/cs?searchtype=author&query=An%2C%20Z) ,[Jiazhan Feng](https://arxiv.org/search/cs?searchtype=author&query=Feng%2C%20J) ,[Kun Xu](https://arxiv.org/search/cs?searchtype=author&query=Xu%2C%20K) ,[Liwei Chen](https://arxiv.org/search/cs?searchtype=author&query=Chen%2C%20L) ,[Dongyan Zhao](https://arxiv.org/search/cs?searchtype=author&query=Zhao%2C%20D)


**Abstract:**


> While large language models exhibit remarkable performance in the Question Answering task, they are susceptible to hallucinations. Challenges arise when these models grapple with understanding multi-hop relations in complex questions or lack the necessary knowledge for a comprehensive response. To address this issue, we introduce the "Decompose-and-Query" framework (D&Q). This framework guides the model to think and utilize external knowledge similar to ReAct, while also restricting its thinking to reliable information, effectively mitigating the risk of hallucinations. Experiments confirm the effectiveness of D&Q: On our ChitChatQA dataset, D&Q does not lose to ChatGPT in 67% of cases; on the HotPotQA question-only setting, D&Q achieved an F1 score of 59.6%. Our code is available at
>
>
> [this https URL](https://github.com/alkaidpku/DQ-ToolQA)


---


###


Summary Notes


####


Simplifying Complex Queries: Enhancing Language Models with the D&Q Framework


The advent of large language models (LLMs) such as ChatGPT, GPT-4, and Bard has significantly transformed how enterprise AI teams operate, boosting efficiency in areas ranging from customer support to content generation. Despite their advancements, these models face challenges, particularly in generating reliable, up-to-date responses and avoiding inaccuracies or "hallucinations."


As demands for precision increase, finding solutions to these issues becomes crucial.


###


Identifying the Problem


LLMs struggle with multi-hop reasoning and accessing the latest information, which becomes more evident when answering complex questions that require synthesizing data from various sources. Although some improvements have been made through iterative refinement and better prompting, a fully autonomous, consistently accurate solution remains elusive.


###


The Decompose-and-Query (D&Q) Approach


Developed by Hejing Cao and a team from Peking University and Kuaishou Technology, the D&Q framework aims to enhance LLMs' reliability by:


- Breaking down complex queries into simpler sub-questions.


- Using a verified question-answer database for accurate responses.


- Adapting the decomposition process based on the context of the query.


####


Key Features of the D&Q Framework


- **ChitChatQA Dataset** : A dataset designed for real-world conversation scenarios to test LLMs on varied, intricate questions.


- **Trusted QA Database** : A repository of validated question-answer pairs that anchor the model's responses in factual information.


- **Trajectory Annotation and Adaptive Decomposition** : Techniques that enable LLMs to refine their problem-solving and information retrieval methods, leading to more precise answers.


###


Benefits for AI Engineers


The D&Q framework presents several actionable strategies for AI professionals looking to upgrade their LLM applications:


- **Integrate External Knowledge Sources** : Linking LLMs with external databases can drastically enhance response accuracy.


- **Utilize Trajectory Annotation for Fine-Tuning** : This method teaches LLMs to navigate problems and access information more effectively.


- **Apply Adaptive Decomposition** : By breaking down complex queries into simpler ones, LLMs can address a wider array of questions more efficiently.


###


Future Directions


The D&Q framework's success in improving LLM performance on datasets like ChitChatQA and HotPotQA underscores the potential of marrying LLMs with structured knowledge bases and sophisticated decomposition strategies for complex query handling.


For enterprise AI teams, adopting these methods means building stronger, more accurate, and dependable AI systems.


####


Final Thoughts


The D&Q framework by Hejing Cao and his team marks a pivotal evolution in enhancing LLMs' response quality by tackling challenges in multi-hop reasoning and current knowledge retrieval. This breakthrough offers enterprise AI engineers a valuable set of tools for augmenting AI system capabilities.


As the AI field progresses, embracing these innovations is crucial for leveraging LLMs to address real-world challenges effectively, setting the stage for future advancements that will continue to revolutionize the enterprise sector.
