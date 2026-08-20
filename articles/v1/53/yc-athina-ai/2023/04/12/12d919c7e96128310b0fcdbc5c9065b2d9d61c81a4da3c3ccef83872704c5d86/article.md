---
schema_version: "1.0.0"
document_id: "12d919c7e96128310b0fcdbc5c9065b2d9d61c81a4da3c3ccef83872704c5d86"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/decomposed-prompting-a-modular-approach-for-solving-complex-tasks"
published_at: "2023-04-11T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:27.423621+00:00"
content_hash: "sha256:47a6d087121501d3039d0e52db44e2f57043f4d2163f5cbf6393f2c379cc2e5e"
---

# Decomposed Prompting: A Modular Approach for Solving Complex Tasks

Do not index


Original Paper


[https://arxiv.org/abs/2210.02406](https://arxiv.org/abs/2210.02406)


Blog URL


[blog.athina.ai /decompo...ex-tasks](https://blog.athina.ai/decomposed-prompting-a-modular-approach-for-solving-complex-tasks)


**Original Paper:**[https://arxiv.org/abs/2210.02406](https://arxiv.org/abs/2210.02406)


**By:**[Tushar Khot](https://arxiv.org/search/cs?searchtype=author&query=Khot%2C%20T) ,[Harsh Trivedi](https://arxiv.org/search/cs?searchtype=author&query=Trivedi%2C%20H) ,[Matthew Finlayson](https://arxiv.org/search/cs?searchtype=author&query=Finlayson%2C%20M) ,[Yao Fu](https://arxiv.org/search/cs?searchtype=author&query=Fu%2C%20Y) ,[Kyle Richardson](https://arxiv.org/search/cs?searchtype=author&query=Richardson%2C%20K) ,[Peter Clark](https://arxiv.org/search/cs?searchtype=author&query=Clark%2C%20P) ,[Ashish Sabharwal](https://arxiv.org/search/cs?searchtype=author&query=Sabharwal%2C%20A)


**Abstract:**


> Few-shot prompting is a surprisingly powerful way to use Large Language Models (LLMs) to solve various tasks. However, this approach struggles as the task complexity increases or when the individual reasoning steps of the task themselves are hard to learn, especially when embedded in more complex tasks. To address this, we propose Decomposed Prompting, a new approach to solve complex tasks by decomposing them (via prompting) into simpler sub-tasks that can be delegated to a library of prompting-based LLMs dedicated to these sub-tasks. This modular structure allows each prompt to be optimized for its specific sub-task, further decomposed if necessary, and even easily replaced with more effective prompts, trained models, or symbolic functions if desired. We show that the flexibility and modularity of Decomposed Prompting allows it to outperform prior work on few-shot prompting using GPT3. On symbolic reasoning tasks, we can further decompose sub-tasks that are hard for LLMs into even simpler solvable sub-tasks. When the complexity comes from the input length, we can recursively decompose the task into the same task but with smaller inputs. We also evaluate our approach on textual multi-step reasoning tasks: on long-context multi-hop QA task, we can more effectively teach the sub-tasks via our separate sub-tasks prompts; and on open-domain multi-hop QA, we can incorporate a symbolic information retrieval within our decomposition framework, leading to improved performance on both tasks. Datasets, Code and Prompts available at
>
>
> [this https URL](https://github.com/allenai/DecomP)


---


###


Summary Notes


####


Blog Post: Unraveling the Complexity of AI Tasks with Decomposed Prompting


In the ever-evolving landscape of artificial intelligence, the introduction of Decomposed Prompting (DECOMP) stands out as a significant leap forward, especially for AI engineering in large enterprises.


Traditional methods have struggled with complex problems, but DECOMP offers a new, more effective way to tackle these challenges. This blog post explores what makes Decomposed Prompting a game-changer in the realm of AI.


####


Current Prompting Strategies: Where They Fall Short


Despite impressive progress in AI, existing prompting methods often hit a wall when faced with complex, multifaceted tasks. These methods, including the Chain of Thought (CoT) prompting, are designed to map out reasoning steps.


Yet, they begin to falter as the complexity of tasks increases, leading to performance issues and inaccuracies. This highlights a clear need for a more capable approach.


####


Introducing Decomposed Prompting


Decomposed Prompting shines as a solution to these problems. It breaks down complex tasks into smaller, more manageable sub-tasks.


Each sub-task is then handled by a model or function specifically optimized for it. This not only improves performance on individual aspects of the problem but also allows for more flexible and scalable solutions.


####


Key Components of DECOMP


- **Decomposer Model** : Sits at the core of DECOMP, breaking down tasks into sub-tasks.


- **Sub-Task Handlers** : Specialized models or functions that tackle the sub-tasks efficiently.


- **Execution and Control** : A controller ensures the seamless integration of sub-tasks, guiding the process to a successful outcome.


####


DECOMP in Practice


Consider the application of DECOMP in complex scenarios like multi-hop question answering. Traditional methods might struggle, but DECOMP divides the task into digestible parts (e.g., data retrieval, reasoning, answer synthesis), each handled by experts in those areas. This approach not only simplifies the process but also enhances accuracy and speed.


####


Why DECOMP Outperforms Others


Empirical studies and practical applications have demonstrated DECOMP's effectiveness over traditional methods. It excels in tasks that require deep reasoning or deal with extensive information, setting a new standard for AI performance.


####


The Future of Decomposed Prompting


Decomposed Prompting marks a significant milestone in AI engineering. It promises not just immediate improvements in handling complex tasks but also lays the groundwork for future advancements.


Optimizing decomposition strategies and sub-task handlers could unlock even greater AI capabilities.


####


Conclusion: A New Era in AI Problem-Solving


Decomposed Prompting represents a major stride in overcoming the limitations of current AI approaches. It offers a scalable, efficient solution for complex problem-solving, paving the way for a new era of AI that is more adaptable and accurate.


For AI engineers and industry professionals, adopting DECOMP means staying at the cutting edge of technology and unlocking the full potential of AI to turn challenges into opportunities for innovation.


The journey toward mastering complex AI tasks has taken an exciting turn with Decomposed Prompting, heralding a future where AI can achieve unprecedented levels of performance.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
