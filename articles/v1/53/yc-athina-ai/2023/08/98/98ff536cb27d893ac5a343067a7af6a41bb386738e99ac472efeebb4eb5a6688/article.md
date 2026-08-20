---
schema_version: "1.0.0"
document_id: "98ff536cb27d893ac5a343067a7af6a41bb386738e99ac472efeebb4eb5a6688"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/an-automatically-discovered-chain-of-thought-prompt-generalizes-to-novel-models-and-datasets"
published_at: "2023-08-03T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:24.228240+00:00"
content_hash: "sha256:302af81dfebeba1af0c50035d28244fc4e5656f23f48b5ad9384a52a3d65dcaa"
---

# An automatically discovered chain-of-thought prompt generalizes to novel models and datasets

Do not index


Original Paper


[https://arxiv.org/abs/2305.02897](https://arxiv.org/abs/2305.02897)


Blog URL


**Original Paper:**[https://arxiv.org/abs/2305.02897](https://arxiv.org/abs/2305.02897)


**By:**[Konstantin Hebenstreit](https://arxiv.org/search/cs?searchtype=author&query=Hebenstreit%2C%20K) ,[Robert Praas](https://arxiv.org/search/cs?searchtype=author&query=Praas%2C%20R) ,[Louis P Kiesewetter](https://arxiv.org/search/cs?searchtype=author&query=Kiesewetter%2C%20L%20P) ,[Matthias Samwald](https://arxiv.org/search/cs?searchtype=author&query=Samwald%2C%20M)


**Abstract:**


> Emergent chain-of-thought (CoT) reasoning capabilities promise to improve performance and explainability of large language models (LLMs). However, uncertainties remain about how reasoning strategies formulated for previous model generations generalize to new model generations and different datasets. In this small-scale study, we compare different reasoning strategies induced by zero-shot prompting across six recently released LLMs (davinci-002, davinci-003, GPT-3.5-turbo, GPT-4, Flan-T5-xxl and Cohere command-xlarge) on a mixture of six question-answering datasets, including datasets from scientific and medical domains. Our findings demonstrate that while some variations in effectiveness occur, gains from CoT reasoning strategies remain robust across different models and datasets. GPT-4 has the most benefit from current state-of-the-art reasoning strategies and exhibits the best performance by applying a prompt previously discovered through automated discovery.


---


###


Summary Notes


##


Improving AI Reasoning with Chain-of-Thought Prompting in Large Language Models


The field of Artificial Intelligence (AI) has seen remarkable advancements with the introduction of Large Language Models (LLMs).


These models have transformed how we tackle intricate problems, analyze data, and even mimic human-like text generation.


A notable study led by Konstantin Hebenstreit and colleagues, titled "An automatically discovered chain-of-thought prompt generalizes to novel models and datasets," explores the potential of LLMs in performing sophisticated reasoning tasks across various datasets, including those from the scientific and medical fields.


This post aims to simplify and share their findings, highlighting what it means for AI Engineers in enterprise companies.


###


Enhancing AI's Reasoning Power


####


Introduction to Advanced Reasoning


The push towards Chain-of-Thought (CoT) reasoning is driven by the goal to improve LLMs' performance and explainability. CoT, particularly when paired with zero-shot prompting, shows promise in elevating LLMs' ability to tackle tasks without requiring specific training examples.


####


Goal: Testing CoT's Flexibility


The study's main aim was to assess how well different zero-shot CoT prompts, whether newly developed or previously discovered, work with new LLMs and a variety of question-answering datasets.


This is vital for AI Engineers who wish to use cutting-edge models for a range of tasks, from understanding natural language to solving complex issues in healthcare and science.


###


Study Approach


####


Framework and Datasets


- **Framework** : The research used the ThoughtSource framework to generate, assess, and annotate CoT reasoning across datasets.


- **Dataset Selection** : The datasets spanned a broad spectrum, from commonsense reasoning to scientific and medical queries, ensuring a thorough evaluation.


####


Prompts: Unlocking LLMs' Potential


- **Design and Examples** : The study explored various zero-shot reasoning strategies, from straightforward prompts like "Let’s think step by step" to more structured approaches. This variety helped gauge CoT reasoning's adaptability and efficiency in various situations.


####


Models: Core of AI Reasoning


- **Selection** : Six recent LLMs known for their CoT reasoning capabilities were chosen, ensuring uniform testing conditions. This is crucial for AI Engineers who want to replicate or expand on the findings.


####


Success Measurement


- **Metrics** : The researchers employed Krippendorff's alpha to assess reliability and conducted a detailed power analysis for sample size determination, providing a solid foundation for evaluating CoT effectiveness.


###


Key Takeaways and Considerations


####


Performance Insights


The findings indicate that certain CoT prompts vastly outperform the baseline across different models and datasets, with the "Zhou" prompt emerging as particularly effective. This is a valuable insight for AI Engineers in choosing and designing prompts for varied applications.


####


Identified Limitations


The study acknowledges some constraints, including dataset sampling, model variations, and potential data contamination. These points are crucial for AI Engineers, emphasizing the need for ongoing testing and adaptation in practical applications.


###


Looking Ahead: Implications for AI Engineers


####


Future Research Directions


The study positions its discoveries within the broader scope of zero-shot CoT prompting research, suggesting paths for future investigations.


For AI Engineers, this means opportunities for testing more LLMs and configurations lie ahead.


###


Conclusion


Hebenstreit and his team's research offers fresh perspectives on using LLMs for complex reasoning tasks. For AI Engineers in enterprise environments, the results highlight the critical role of selecting and evaluating CoT prompts, understanding the capabilities of different models, and keeping up with research advancements.


As AI continues to evolve, this study serves as a guide for leveraging LLMs to address real-world challenges, marking a step forward toward smarter, more capable, and transparent AI systems.


In the fast-paced world of AI, staying informed and adaptable is essential. This research lays a solid groundwork for future innovations and applications of AI in complex problem-solving, marking another advancement in our journey towards more sophisticated and explainable AI systems.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
