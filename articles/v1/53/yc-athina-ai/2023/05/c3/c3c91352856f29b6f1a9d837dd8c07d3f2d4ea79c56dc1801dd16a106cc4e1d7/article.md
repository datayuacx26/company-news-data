---
schema_version: "1.0.0"
document_id: "c3c91352856f29b6f1a9d837dd8c07d3f2d4ea79c56dc1801dd16a106cc4e1d7"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/reprompting-automated-chain-of-thought-prompt-inference-through-gibbs-sampling"
published_at: "2023-05-17T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:26.378360+00:00"
content_hash: "sha256:4c13ef081f6a8e4483875704f342c05baaf9857e821036ec68f5bcf00af922e0"
---

# Reprompting: Automated Chain-of-Thought Prompt Inference Through Gibbs Sampling

Do not index


Original Paper


[https://arxiv.org/abs/2305.09993](https://arxiv.org/abs/2305.09993)


Blog URL


**Original Paper:**[https://arxiv.org/abs/2305.09993](https://arxiv.org/abs/2305.09993)


**By:**[Weijia Xu](https://arxiv.org/search/cs?searchtype=author&query=Xu%2C%20W) ,[Andrzej Banburski-Fahey](https://arxiv.org/search/cs?searchtype=author&query=Banburski-Fahey%2C%20A) ,[Nebojsa Jojic](https://arxiv.org/search/cs?searchtype=author&query=Jojic%2C%20N)


**Abstract:**


> We introduce Reprompting, an iterative sampling algorithm that automatically learns the Chain-of-Thought (CoT) recipes for a given task without human intervention. Through Gibbs sampling, Reprompting infers the CoT recipes that work consistently well for a set of training samples by iteratively sampling new recipes using previously sampled recipes as parent prompts to solve other training problems. We conduct extensive experiments on 20 challenging reasoning tasks. Results show that Reprompting outperforms human-written CoT prompts substantially by +9.4 points on average. It also achieves consistently better performance than the state-of-the-art prompt optimization and decoding algorithms.


---


###


Summary Notes


####


Streamlining AI Reasoning with Automated Reprompting


The landscape of artificial intelligence is continuously advancing, with Large Language Models (LLMs) such as ChatGPT and InstructGPT leading the way.


These models excel in various tasks but often struggle with complex, multi-step reasoning. Traditionally, overcoming this issue required manually crafting Chain-of-Thought (CoT) prompts, a process not scalable.


The innovative technique of "Reprompting" changes this by automatically generating and refining CoT prompts through Gibbs sampling, enhancing LLM performance significantly.


####


In-Context Learning Explained


At the core of LLM capabilities is in-context learning. This method involves presenting models with example tasks to guide their responses.


CoT prompts are crucial here, as they provide a detailed walkthrough of the reasoning needed to solve complex tasks, boosting LLM effectiveness in multi-step reasoning.


####


The Advantages of Reprompting


Reprompting uses Gibbs sampling to automate CoT prompt creation. It starts with a basic example and iteratively improves the prompts to develop effective CoT strategies.


This process not only makes LLM optimization for complex reasoning more efficient but also minimizes the need for manual prompt creation.


####


Experimental Validation


Reprompting was tested against traditional approaches like zero-shot, few-shot, and manual CoT prompts over 20 reasoning tasks from various benchmarks.


Using models like ChatGPT and InstructGPT, Reprompting's prompts showed superior accuracy and consistency.


####


**Reprompting Experiment Details:**


- **Iterations and Samples:** The study details the iterations and sample selection, ensuring reproducibility.


- **Benchmarks:** It compares Reprompting against manual CoT prompts and advanced algorithms, showcasing its effectiveness.


- **Model Range:** The technique's success across different LLMs highlights its versatility.


- **Accuracy Focus:** Emphasizing answer accuracy from LLMs using Reprompting prompts demonstrates the method's effectiveness.


####


Breakthrough Findings


The study shows Reprompting surpasses both human-made CoT prompts and other prompt optimization methods. This indicates its potential to significantly boost the reasoning abilities of LLMs across a range of tasks and models, offering insights into optimized prompt adaptability and efficiency.


####


Addressing Current Limitations


This research identifies the shortcomings of existing CoT prompting and in-context learning methods, underlining the importance of Reprompting in advancing the field.


####


Concluding Insights


Reprompting stands out as a robust, automated solution for crafting and refining CoT prompts.


It notably improves LLM reasoning skills without relying on manual input, facilitating better model performance on specific tasks and allowing fairer model comparisons.


####


The Future Impact


Reprompting marks a major step forward in machine learning, reducing the dependency on human involvement in LLM training and broadening their problem-solving capacity. This advancement opens new research and application possibilities, moving us closer to creating more intelligent and autonomous AI systems.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
