---
schema_version: "1.0.0"
document_id: "b031bc3f19964b1367ecfbb1862d9d793b764759820bfa6443cd89b2a1b893e5"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/can-chatgpt-understand-too-a-comparative-study-on-chatgpt-and-fine-tuned-bert"
published_at: "2023-03-02T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:28.379811+00:00"
content_hash: "sha256:5f311ed7bad4cb4640c401722a74cd30c3c233483dfd5785782cdf98f7fb020c"
---

# Can ChatGPT Understand Too? A Comparative Study on ChatGPT and Fine-tuned BERT

Do not index


Original Paper


[https://arxiv.org/pdf/2302.10198](https://arxiv.org/pdf/2302.10198)


Blog URL


[blog.athina.ai /can-cha...ned-bert](https://blog.athina.ai/can-chatgpt-understand-too-a-comparative-study-on-chatgpt-and-fine-tuned-bert)


**Original Paper:**[https://arxiv.org/pdf/2302.10198](https://arxiv.org/pdf/2302.10198)


**By:** Qihuang Zhong, Liang Ding<,\[, Juhua Liu, Bo Du, Dacheng Tao


**Abstract:**


> ChatGPT has attracted great attention, as it can generate fluent and high-quality responses to human inquiries. Several prior studies have shown that ChatGPT attains remarkable generation ability compared with existing models. However, the quantitative analysis of ChatGPT’s understanding ability has been given little attention. In this report, we explore the understanding ability of ChatGPT by evaluating it on the most popular GLUE benchmark, and comparing it with 4 representative fine-tuned BERT-style models. We find that:
>
>
> 1) ChatGPT falls short in handling paraphrase and similarity tasks; 2) ChatGPT outperforms all BERT models on inference tasks by a large margin; 3) ChatGPT achieves comparable performance compared with BERT on sentiment analysis and question-answering tasks. Additionally, by combining some advanced prompting strategies, we show that the understanding ability of ChatGPT can be further improved.


---


###


Summary Notes


####


Can ChatGPT Understand as Well as It Generates Text? A Comparative Analysis with Fine-tuned BERT


The intrigue surrounding ChatGPT's natural language understanding (NLU) abilities, compared to other AI models like BERT, is significant in the field of artificial intelligence.


This blog post examines how ChatGPT stacks up against fine-tuned BERT models in NLU tasks, using the GLUE benchmark for evaluation.


####


Introduction


ChatGPT, based on OpenAI's InstructGPT, has made waves with its text generation skills. Yet, questions about its understanding capabilities linger, especially when compared to models like BERT.


Unraveling these differences is essential for AI Engineers seeking the best AI tools for their projects. This study aims to compare ChatGPT's performance in NLU tasks with that of fine-tuned BERT models.


####


Methodology


- **The GLUE Benchmark** : A set of evaluation tasks to measure a model's NLU performance, including sentiment analysis and question-answering.


- **Models Compared** : ChatGPT versus four BERT-style models.


- **Performance Metrics** : Accuracy, F1 score, and correlation coefficients were used to evaluate the models.


####


Results


- **ChatGPT's Strengths** : It excelled in inference tasks, showing superior reasoning, and matched BERT-base in sentiment analysis and question-answering.


- **Weaknesses** : Struggled with identifying negative paraphrases and assessing neutral similarities.


####


Analysis


- **Understanding vs. Generation** : ChatGPT's text generation is top-notch, but it falters in some understanding tasks, producing contradictory or nonsensical outputs.


- **The Role of Prompting** : Different prompting strategies, especially manual few-shot chain-of-thought (CoT) prompting, significantly improved ChatGPT's NLU performance.


- **Sensitivity to Training-Test Data Similarity** : The model's performance varied with the similarity between training examples and test data, suggesting an area for optimization.


####


Advanced Prompting Strategies


Experiments indicated that tailored prompts could notably enhance ChatGPT's understanding abilities, with manual few-shot CoT prompting standing out, especially in inference tasks.


####


Discussion


The study underscores the importance of sophisticated prompting strategies in optimizing ChatGPT's NLU tasks performance.


Although ChatGPT shows promise, it doesn't consistently surpass top-tier models like RoBERTa-large in all areas.


For AI Engineers, this means that while ChatGPT is a valuable asset, its deployment needs to be strategic, complementing its strengths and addressing its weaknesses with careful model selection and prompt design.


####


Conclusion


While ChatGPT demonstrates impressive text generation, it encounters challenges in some NLU tasks. However, with advanced prompting strategies, there’s a pathway to bolstering its understanding abilities.


AI Engineers should strategically deploy ChatGPT, leveraging its capabilities and mitigating its limitations for optimal use in enterprise applications. Future research should explore more advanced prompting techniques and hybrid models to further improve ChatGPT's NLU performance.


####


References


For more in-depth information and a thorough investigation into the study's methodologies and results, readers are encouraged to consult the original research and its comprehensive references on large language models and natural language processing tasks.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
