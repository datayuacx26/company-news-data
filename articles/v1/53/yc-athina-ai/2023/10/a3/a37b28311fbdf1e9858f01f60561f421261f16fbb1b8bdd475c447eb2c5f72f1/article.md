---
schema_version: "1.0.0"
document_id: "a37b28311fbdf1e9858f01f60561f421261f16fbb1b8bdd475c447eb2c5f72f1"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/longllmlingua-accelerating-and-enhancing-llms-in-long-context-scenarios-via-prompt-compression"
published_at: "2023-10-10T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:22.552077+00:00"
content_hash: "sha256:8cf09925a4657da0a5a196ee2f4f0f1e1ca48f7f4363997b3c3b753a319c6e4f"
---

# LongLLMLingua: Accelerating and Enhancing LLMs in Long Context Scenarios via Prompt Compression

Do not index


Original Paper


[https://arxiv.org/abs/2310.06839](https://arxiv.org/abs/2310.06839)


Blog URL


[blog.athina.ai /longllm...pression](https://blog.athina.ai/longllmlingua-accelerating-and-enhancing-llms-in-long-context-scenarios-via-prompt-compression)


**Original Paper:**[https://arxiv.org/abs/2310.06839](https://arxiv.org/abs/2310.06839)


**By:**[Huiqiang Jiang](https://arxiv.org/search/cs?searchtype=author&query=Jiang%2C%20H) ,[Qianhui Wu](https://arxiv.org/search/cs?searchtype=author&query=Wu%2C%20Q) ,[Xufang Luo](https://arxiv.org/search/cs?searchtype=author&query=Luo%2C%20X) ,[Dongsheng Li](https://arxiv.org/search/cs?searchtype=author&query=Li%2C%20D) ,[Chin-Yew Lin](https://arxiv.org/search/cs?searchtype=author&query=Lin%2C%20C) ,[Yuqing Yang](https://arxiv.org/search/cs?searchtype=author&query=Yang%2C%20Y) ,[Lili Qiu](https://arxiv.org/search/cs?searchtype=author&query=Qiu%2C%20L)


**Abstract:**


> In long context scenarios, large language models (LLMs) face three main challenges: higher computational/financial cost, longer latency, and inferior performance. Some studies reveal that the performance of LLMs depends on both the density and the position of the key information (question relevant) in the input prompt. Inspired by these findings, we propose LongLLMLingua for prompt compression towards improving LLMs' perception of the key information to simultaneously address the three challenges. We conduct evaluation on a wide range of long context scenarios including single-/multi-document QA, few-shot learning, summarization, synthetic tasks, and code completion. The experimental results show that LongLLMLingua compressed prompt can derive higher performance with much less cost. The latency of the end-to-end system is also reduced. For example, on NaturalQuestions benchmark, LongLLMLingua gains a performance boost of up to 17.1% over the original prompt with ~4x fewer tokens as input to GPT-3.5-Turbo. It can derive cost savings of $28.5 and $27.4 per 1,000 samples from the LongBench and ZeroScrolls benchmark, respectively. Additionally, when compressing prompts of ~10k tokens at a compression rate of 2x-10x, LongLLMLingua can speed up the end-to-end latency by 1.4x-3.8x. Our code is available at
>
>
> [this https URL](https://aka.ms/LLMLingua)


---


###


Summary Notes


####


Accelerating LLM Efficiency in Long Contexts with LongLLMLingua


####


Introduction


Large Language Models (LLMs) like ChatGPT have significantly advanced the field of natural language processing (NLP). Despite their success, they struggle with long contexts, which can lead to more computational needs, higher expenses, and slower responses.


This challenge is crucial for AI engineers in enterprises needing efficient and affordable solutions without sacrificing quality. LongLLMLingua emerges as a groundbreaking approach to enhance LLMs' efficiency in handling long contexts through prompt compression, offering a highly relevant solution for AI specialists.


####


Understanding the Problem


The main issue LongLLMLingua addresses is the prompt compression problem. The goal is to compress prompts without losing essential information, ensuring the LLM's output remains accurate while minimizing input size. This involves an optimization process that prioritizes the most relevant information to maintain or even enhance the LLM's performance.


####


Foundation: LLMLingua


LongLLMLingua builds on the LLMLingua framework, which compresses prompts by removing less informative tokens through a smaller model. This forms the groundwork for LongLLMLingua's advanced techniques tailored for long-context scenarios.


####


LongLLMLingua: The Advanced Solution


LongLLMLingua introduces key features for better handling long contexts:


- **Question-Aware Compression** : It identifies and retains crucial documents and tokens based on their relevance to the query.


- **Document Reordering Mechanism** : This rearranges documents to improve the model's understanding by leveraging its sensitivity to token positioning.


- **Dynamic Compression Ratios** : Applies varying compression ratios to optimize information preservation based on document importance.


- **Post-Compression Recovery** : Restores vital details lost during compression to enhance output accuracy.


####


Performance Evaluation


LongLLMLingua was tested against benchmarks like NaturalQuestions, LongBench, and ZeroSCROLLS, showing remarkable improvements over existing methods and uncompressed prompts in terms of cost, speed, and accuracy.


These results confirm LongLLMLingua's effectiveness and versatility for long-context scenarios.


####


Contextualizing LongLLMLingua


The development of LongLLMLingua is supported by extensive research on handling long contexts in LLMs, prompt information distribution, retrieval methods, and compression techniques.


Placing LongLLMLingua within this research landscape underscores its innovative contributions and potential to push the field forward.


####


Conclusion


LongLLMLingua marks a significant advancement in using LLMs for long-context scenarios, reducing costs, latency, and improving performance.


It enhances the practicality of LLMs and opens up new possibilities for complex applications.


For AI engineers in enterprise settings, incorporating LongLLMLingua could be a game-changer, positioning them at the forefront of AI technology innovation.


LongLLMLingua stands as a testament to ongoing AI innovation, highlighting the continuous drive for efficiency and effectiveness in NLP technologies.


As the boundaries of LLM capabilities expand, LongLLMLingua is poised to be a key player in the future of natural language processing.


**For more details** : The full research paper on LongLLMLingua offers an in-depth look at the methodology, experiments, and results, providing valuable insights for AI professionals interested in the latest LLM advancements.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
