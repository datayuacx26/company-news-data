---
schema_version: "1.0.0"
document_id: "d640e81f1181bf927f899c3217c40359e2640fa6611a21054e31034af05e916f"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/connecting-large-language-models-with-evolutionary-algorithms-yields-powerful-prompt-optimizers"
published_at: "2023-09-15T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:23.018316+00:00"
content_hash: "sha256:7fb23d2eee45993392d26f58083a32b86a9b3f7c5115b2aae8b8eba967a659a7"
---

# Connecting Large Language Models with Evolutionary Algorithms Yields Powerful Prompt Optimizers

Do not index


Original Paper


[https://arxiv.org/abs/2309.08532](https://arxiv.org/abs/2309.08532)


Blog URL


**Original Paper:**[https://arxiv.org/abs/2309.08532](https://arxiv.org/abs/2309.08532)


**By:**[Qingyan Guo](https://arxiv.org/search/cs?searchtype=author&query=Guo%2C%20Q) ,[Rui Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang%2C%20R) ,[Junliang Guo](https://arxiv.org/search/cs?searchtype=author&query=Guo%2C%20J) ,[Bei Li](https://arxiv.org/search/cs?searchtype=author&query=Li%2C%20B) ,[Kaitao Song](https://arxiv.org/search/cs?searchtype=author&query=Song%2C%20K) ,[Xu Tan](https://arxiv.org/search/cs?searchtype=author&query=Tan%2C%20X) ,[Guoqing Liu](https://arxiv.org/search/cs?searchtype=author&query=Liu%2C%20G) ,[Jiang Bian](https://arxiv.org/search/cs?searchtype=author&query=Bian%2C%20J) ,[Yujiu Yang](https://arxiv.org/search/cs?searchtype=author&query=Yang%2C%20Y)


**Abstract:**


> Large Language Models (LLMs) excel in various tasks, but they rely on carefully crafted prompts that often demand substantial human effort. To automate this process, in this paper, we propose a novel framework for discrete prompt optimization, called EvoPrompt, which borrows the idea of evolutionary algorithms (EAs) as they exhibit good performance and fast convergence. To enable EAs to work on discrete prompts, which are natural language expressions that need to be coherent and human-readable, we connect LLMs with EAs. This approach allows us to simultaneously leverage the powerful language processing capabilities of LLMs and the efficient optimization performance of EAs. Specifically, abstaining from any gradients or parameters, EvoPrompt starts from a population of prompts and iteratively generates new prompts with LLMs based on the evolutionary operators, improving the population based on the development set. We optimize prompts for both closed- and open-source LLMs including GPT-3.5 and Alpaca, on 31 datasets covering language understanding, generation tasks, as well as BIG-Bench Hard (BBH) tasks. EvoPrompt significantly outperforms human-engineered prompts and existing methods for automatic prompt generation (e.g., up to 25% on BBH). Furthermore, EvoPrompt demonstrates that connecting LLMs with EAs creates synergies, which could inspire further research on the combination of LLMs and conventional algorithms.


---


###


Summary Notes


####


Revolutionizing AI Prompt Design with Evolutionary Algorithms


####


Introduction


In the fast-paced world of artificial intelligence (AI), the performance of Large Language Models (LLMs) is heavily influenced by the quality of prompts they're given. Traditionally, creating effective prompts has been a manual and expertise-heavy process.


The innovative solution? Using Evolutionary Algorithms (EAs) to automate and refine prompt design, enhancing LLM efficiency and simplifying the development workflow.


####


EvoPrompt Framework Explained


####


Overview


The EvoPrompt Framework ingeniously combines the strength of LLMs with the adaptability of EAs.


It automates prompt creation and optimization, working seamlessly with both closed and open-source LLMs across various datasets without needing internal model data.


####


How It Works


- **Initial Setup:** Begins with a wide range of initial prompts.


- **Evolutionary Operations:** Applies mutation and crossover operations using LLMs to generate new prompts.


- **Selection:** Chooses the most effective prompts based on their performance on development sets.


####


Applications


This framework is versatile, applicable to any LLM or task, including complex challenges like the BIG-Bench Hard tasks.


####


Experiments and Results


EvoPrompt outshines both manually crafted and other automated prompts in performance across diverse tasks and datasets.


Its scalability and ability to fine-tune prompts for various conditions highlight its effectiveness and wide applicability.


####


Advantages and Impact


- **Accessibility:** EvoPrompt simplifies the optimization process by not needing internal model parameters.


- **Efficiency:** It finds a balance between exploring new prompts and refining existing ones.


- **Readability:** The prompts are not only effective but also easy for humans to understand and modify.


The EvoPrompt Framework signifies a leap forward in integrating traditional algorithms with modern LLMs, enhancing AI system sophistication and reducing manual tuning dependency.


####


Looking Ahead


The potential of combining EAs with LLMs via the EvoPrompt Framework opens up thrilling new research avenues.


Future endeavors could explore further integration with other algorithmic techniques, broadening the scope of application to even more complex tasks.


####


Conclusion


The EvoPrompt Framework is a key advancement in automating and optimizing LLM prompt design through evolutionary algorithms. Its proven effectiveness, adaptability, and efficiency make it a crucial development in AI.


The future looks bright for expanding this approach and further integrating LLMs with traditional algorithms, promising continued innovation in AI capabilities.


####


Code Availability and Contributions


The EvoPrompt framework is open for access on GitHub, providing a platform for AI engineers and researchers to replicate results and contribute to ongoing advancements. This initiative not only showcases a novel way to lessen manual effort in prompt design but also encourages further research in merging machine learning models with algorithmic techniques.


AI professionals in enterprise settings are especially positioned to leverage this groundbreaking framework, potentially transforming AI development and application.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
