---
schema_version: "1.0.0"
document_id: "0e995a8bfa688f6f855cc35d689969d30a3ec923d6b47d801505f75c82f03a8e"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/evoprompting-language-models-for-code-level-neural-architecture-search"
published_at: "2023-02-28T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:28.379811+00:00"
content_hash: "sha256:dc7409be48f11b30fdb5d96daa6ffb07fe1f013344cf5f782b44540a5541ebb9"
---

# EvoPrompting: Language Models for Code-Level Neural Architecture Search

Do not index


Original Paper


[https://arxiv.org/abs/2302.14838](https://arxiv.org/abs/2302.14838)


Blog URL


[blog.athina.ai /evoprom...e-search](https://blog.athina.ai/evoprompting-language-models-for-code-level-neural-architecture-search)


**Original Paper:**[https://arxiv.org/abs/2302.14838](https://arxiv.org/abs/2302.14838)


**By:**[Angelica Chen](https://arxiv.org/search/cs?searchtype=author&query=Chen%2C%20A) ,[David M. Dohan](https://arxiv.org/search/cs?searchtype=author&query=Dohan%2C%20D%20M) ,[David R. So](https://arxiv.org/search/cs?searchtype=author&query=So%2C%20D%20R)


**Abstract:**


> Given the recent impressive accomplishments of language models (LMs) for code generation, we explore the use of LMs as adaptive mutation and crossover operators for an evolutionary neural architecture search (NAS) algorithm. While NAS still proves too difficult a task for LMs to succeed at solely through prompting, we find that the combination of evolutionary prompt engineering with soft prompt-tuning, a method we term EvoPrompting, consistently finds diverse and high performing models. We first demonstrate that EvoPrompting is effective on the computationally efficient MNIST-1D dataset, where EvoPrompting produces convolutional architecture variants that outperform both those designed by human experts and naive few-shot prompting in terms of accuracy and model size. We then apply our method to searching for graph neural networks on the CLRS Algorithmic Reasoning Benchmark, where EvoPrompting is able to design novel architectures that outperform current state-of-the-art models on 21 out of 30 algorithmic reasoning tasks while maintaining similar model size. EvoPrompting is successful at designing accurate and efficient neural network architectures across a variety of machine learning tasks, while also being general enough for easy adaptation to other tasks beyond neural network design.


---


###


Summary Notes


##


EvoPrompting: Transforming Neural Architecture Design with Language Models


The field of artificial intelligence is continuously advancing, with engineers and researchers exploring new methods to create neural architectures.


Traditional methods have their limits, and that's where **EvoPrompting** comes into play. This innovative approach is reshaping what's possible with language models (LMs) in neural architecture design.


This blog post will break down how EvoPrompting works, its fundamental concepts, and the impressive outcomes it has achieved, offering insights into the future of AI development, especially for AI Engineers at enterprise companies.


###


**Understanding EvoPrompting**


EvoPrompting introduces a novel method: starting with manually created program seeds. These seeds are the basis for generating new candidate architectures through a code-pretrained LM.


By training and evaluating these architectures, the best performers are selected as seeds for the next round.


This cycle of generation, evaluation, and improvement continues, driven by prompt-tuning the LM according to performance feedback until the best architectures are found.


####


**EvoPrompting Explained: Step-by-Step**


1. **Initialization** : Begin with manually created program seeds.


1. **Generation** : Generate candidate architectures using a pretrained LM.


1. **Evaluation** : Train and evaluate these architectures.


1. **Selection** : Pick the top performers as the next cycle's seeds.


1. **Prompt-Tuning** : Refine the LM's prompts using performance insights.


1. **Iteration** : Repeat the cycle, improving architecture designs progressively.


###


**Core Concepts**


EvoPrompting combines key concepts to effectively navigate neural architecture design:


- **In-context Prompting** : Tailor the LM's output to specific tasks via custom prompts without needing updates at inference.


- **Evolutionary Search** : Use evolutionary algorithms to explore the vast design space efficiently, focusing on the most promising designs.


- **Prompt-Tuning** : Utilize performance feedback to enhance the prompts, making the LM more effective with each iteration.


###


**Experiments and Achievements**


The effectiveness of EvoPrompting is highlighted through its performance on multiple benchmarks:


1. **MNIST-1D Dataset** : EvoPrompting surpassed both human-crafted models and basic prompting techniques, demonstrating higher accuracy and smaller model sizes.


1. **CLRS Algorithmic Reasoning Benchmark** : It excelled by generating innovative graph neural network architectures that outperformed existing models in 21 out of 30 tasks, showcasing its potential for innovation.


###


**Contributions**


EvoPrompting's impact goes beyond its experimental success, offering a new direction for neural architecture design:


- **Combining Evolutionary Search with LM Prompting** : This integration advances how LMs are guided in architecture design, moving past the constraints of straightforward prompting.


- **Progress in Graph Neural Networks** : The creation of groundbreaking architectures through EvoPrompting not only challenges but often exceeds the current best designs, marking significant progress in machine learning.


###


**Background and Inspiration**


EvoPrompting builds upon significant previous work, including:


- **LMs for Code Generation** : The expansion of Transformers has greatly enhanced LMs' capabilities in processing natural language and technical tasks.


- **Prompting Techniques** : The emergence of various prompting strategies aims to improve LM performance without the need for retraining.


- **Evolutionary Algorithms** : The creative adaptation of these algorithms in neural architecture search (NAS) demonstrates the flexibility and promise of EvoPrompting in this area.


###


**Conclusion**


EvoPrompting marks a major advancement in neural architecture design. By harnessing the strength of language models with evolutionary search and prompt-tuning, it presents a dynamic and efficient strategy for surpassing conventional model limitations. Its success across different benchmarks underscores its potential to significantly push forward the machine learning development process, paving the way for further exploration and innovation in AI.


###


**Acknowledgements**


The development of EvoPrompting was made possible through the collaborative efforts and support from our colleagues at New York University, OpenAI, and the Google Student Researcher program, whose contributions have been crucial.


EvoPrompting isn't just a new method; it's a landmark in AI development, promising a future where designing neural architectures is bound only by our imagination.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
