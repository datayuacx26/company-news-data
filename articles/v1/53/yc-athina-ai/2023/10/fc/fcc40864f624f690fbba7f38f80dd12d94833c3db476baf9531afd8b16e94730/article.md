---
schema_version: "1.0.0"
document_id: "fcc40864f624f690fbba7f38f80dd12d94833c3db476baf9531afd8b16e94730"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/large-language-models-as-analogical-reasoners"
published_at: "2023-10-03T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:23.018316+00:00"
content_hash: "sha256:dc46db6c1a51f9c38d9ebc539c108705995f14442e9b8bbfff04f60006c45976"
---

# Large Language Models as Analogical Reasoners

Do not index


Original Paper


[https://arxiv.org/abs/2310.01714](https://arxiv.org/abs/2310.01714)


Blog URL


[blog.athina.ai /large-l...easoners](https://blog.athina.ai/large-language-models-as-analogical-reasoners)


**Original Paper:**[https://arxiv.org/abs/2310.01714](https://arxiv.org/abs/2310.01714)


**By:**[Michihiro Yasunaga](https://arxiv.org/search/cs?searchtype=author&query=Yasunaga%2C%20M) ,[Xinyun Chen](https://arxiv.org/search/cs?searchtype=author&query=Chen%2C%20X) ,[Yujia Li](https://arxiv.org/search/cs?searchtype=author&query=Li%2C%20Y) ,[Panupong Pasupat](https://arxiv.org/search/cs?searchtype=author&query=Pasupat%2C%20P) ,[Jure Leskovec](https://arxiv.org/search/cs?searchtype=author&query=Leskovec%2C%20J) ,[Percy Liang](https://arxiv.org/search/cs?searchtype=author&query=Liang%2C%20P) ,[Ed H. Chi](https://arxiv.org/search/cs?searchtype=author&query=Chi%2C%20E%20H) ,[Denny Zhou](https://arxiv.org/search/cs?searchtype=author&query=Zhou%2C%20D)


**Abstract:**


> Chain-of-thought (CoT) prompting for language models demonstrates impressive performance across reasoning tasks, but typically needs labeled exemplars of the reasoning process. In this work, we introduce a new prompting approach, analogical prompting, designed to automatically guide the reasoning process of large language models. Inspired by analogical reasoning, a cognitive process in which humans draw from relevant past experiences to tackle new problems, our approach prompts language models to self-generate relevant exemplars or knowledge in the context, before proceeding to solve the given problem. This method presents several advantages: it obviates the need for labeling or retrieving exemplars, offering generality and convenience; it can also tailor the generated exemplars and knowledge to each problem, offering adaptability. Experimental results show that our approach outperforms 0-shot CoT and manual few-shot CoT in a variety of reasoning tasks, including math problem solving in GSM8K and MATH, code generation in Codeforces, and other reasoning tasks in BIG-Bench.


---


###


Summary Notes


####


Blog Post: Boosting AI's Problem-Solving Skills with Analogical Reasoning


In the dynamic world of artificial intelligence, Large Language Models (LLMs) like GPT-3.5, GPT-4, and PaLM2 are leading the charge. These models are revolutionizing how machines understand and tackle problems.


However, enabling these AI systems to reason like humans is a complex challenge. This is where analogical reasoning comes into play, offering a groundbreaking approach to enhance the reasoning capabilities of LLMs without relying on labeled examples.


####


Understanding Analogical Reasoning


Analogical reasoning is a cognitive process where humans solve new problems by drawing on the solutions to similar past problems. It's a fundamental human skill that AI researchers are striving to replicate in LLMs.


By integrating analogical reasoning, LLMs can better solve problems by relating them to previously encountered situations, thus enhancing their problem-solving skills.


####


The Method of Analogical Prompting


Analogical prompting is a technique where LLMs are prompted to generate relevant examples or knowledge in context before addressing a problem. This method has several key benefits:


- **Eliminates Manual Labeling:** It removes the need for manually labeled examples, making the problem-solving process more efficient.


- **Tailors to Specific Problems:** It generates examples and knowledge specific to each problem, providing a customizable approach to problem-solving.


####


Key Findings


Researchers conducted extensive tests on various reasoning tasks, such as mathematical problem-solving and code generation, using models like GPT-3.5, GPT-4, and PaLM2. They compared analogical prompting to other approaches like few-shot learning. The results were impressive:


- **Enhanced Performance:** Analogical prompting consistently improved performance across different tasks and models.


- **Effectiveness in Complex Tasks:** This method was particularly effective in complex tasks, such as code generation.


- **Superior to Retrieval-Based Methods:** It outperformed methods that rely on retrieving examples, especially in larger models.


- **Scales with LLM Size:** Its effectiveness increases with the size of the LLM.


####


Limitations and Next Steps


While promising, analogical prompting has its challenges, such as higher computational demands and the potential for failure if the LLM lacks relevant knowledge. Future research aims to refine this method to overcome these hurdles and further enhance AI problem-solving.


####


Broader Implications


The study also explored other enhancements in LLM reasoning, emphasizing the critical role of effective reasoning in AI. Approaches like incorporating structured knowledge and external reasoning modules were discussed. Analogical reasoning is seen as a key advancement in AI, with the potential to significantly improve machine learning algorithms.


####


Conclusion


Analogical prompting is a potent method for improving the reasoning abilities of LLMs, offering adaptability, enhanced performance, and efficiency.


By enabling LLMs to leverage self-generated examples and knowledge, it opens up new avenues for AI problem-solving. As AI research progresses, analogical reasoning is poised to be a major contributor to developing AI systems with human-like reasoning and problem-solving capabilities.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
