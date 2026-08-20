---
schema_version: "1.0.0"
document_id: "5e1b69746c76544b116d6e5f91fcb95cbae4951e750609e5779f987878e571e4"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/pal-program-aided-language-models"
published_at: "2023-01-27T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:29.187426+00:00"
content_hash: "sha256:5be8505eeeea69b518f21a5ddc2e65d1f25c1a9b06068ca56791d10a88d3f10b"
---

# PAL: Program-aided Language Models

Do not index


Original Paper


[https://arxiv.org/abs/2211.10435](https://arxiv.org/abs/2211.10435)


Blog URL


[blog.athina.ai /pal-pro...e-models](https://blog.athina.ai/pal-program-aided-language-models)


**Original Paper:**[https://arxiv.org/abs/2211.10435](https://arxiv.org/abs/2211.10435)


**By:**[Luyu Gao](https://arxiv.org/search/cs?searchtype=author&query=Gao%2C%20L) ,[Aman Madaan](https://arxiv.org/search/cs?searchtype=author&query=Madaan%2C%20A) ,[Shuyan Zhou](https://arxiv.org/search/cs?searchtype=author&query=Zhou%2C%20S) ,[Uri Alon](https://arxiv.org/search/cs?searchtype=author&query=Alon%2C%20U) ,[Pengfei Liu](https://arxiv.org/search/cs?searchtype=author&query=Liu%2C%20P) ,[Yiming Yang](https://arxiv.org/search/cs?searchtype=author&query=Yang%2C%20Y) ,[Jamie Callan](https://arxiv.org/search/cs?searchtype=author&query=Callan%2C%20J) ,[Graham Neubig](https://arxiv.org/search/cs?searchtype=author&query=Neubig%2C%20G)


**Abstract:**


> Large language models (LLMs) have recently demonstrated an impressive ability to perform arithmetic and symbolic reasoning tasks, when provided with a few examples at test time ("few-shot prompting"). Much of this success can be attributed to prompting methods such as "chain-of-thought'', which employ LLMs for both understanding the problem description by decomposing it into steps, as well as solving each step of the problem. While LLMs seem to be adept at this sort of step-by-step decomposition, LLMs often make logical and arithmetic mistakes in the solution part, even when the problem is decomposed correctly. In this paper, we present Program-Aided Language models (PAL): a novel approach that uses the LLM to read natural language problems and generate programs as the intermediate reasoning steps, but offloads the solution step to a runtime such as a Python interpreter. With PAL, decomposing the natural language problem into runnable steps remains the only learning task for the LLM, while solving is delegated to the interpreter. We demonstrate this synergy between a neural LLM and a symbolic interpreter across 13 mathematical, symbolic, and algorithmic reasoning tasks from BIG-Bench Hard and other benchmarks. In all these natural language reasoning tasks, generating code using an LLM and reasoning using a Python interpreter leads to more accurate results than much larger models. For example, PAL using Codex achieves state-of-the-art few-shot accuracy on the GSM8K benchmark of math word problems, surpassing PaLM-540B which uses chain-of-thought by absolute 15% top-1. Our code and data are publicly available at
>
>
> [this http URL](http://reasonwithpal.com/)


---


###


Summary Notes


##


Enhancing Language Models with Program-Aided Execution


Language models are a cornerstone of AI, making strides in numerous tasks like translation and content creation.


Yet, they struggle with tasks requiring logic or arithmetic, often making errors.


Program-Aided Language Models (PAL) offer a solution by blending the interpretive power of language models with the accuracy of programming, creating more reliable outputs.


####


Why PAL Matters


Large Language Models (LLMs) have been transformative in AI, but their ability to solve complex logical or arithmetic problems is lacking.


Despite advancements like "chain-of-thought" prompting, these models still fall short in accuracy for more challenging tasks.


####


Introducing Program-Aided Language Models (PAL)


PAL is a breakthrough approach that combines the strengths of LLMs with the precision of programming. Here’s how it works:


- LLMs generate a program that outlines the steps needed to solve a problem.


- An external interpreter then executes this program to produce the final answer.


This method allows LLMs to concentrate on understanding the problem and formulating it into a solvable program, leaving the precise calculation to a dedicated computational system.


####


Performance and Results


PAL has shown exceptional performance, outdoing traditional LLM methods and excelling in areas like the GSM 8K math problems. Its ability to handle complex calculations demonstrates a significant advancement over current models.


####


Why PAL Succeeds


A key observation is that LLMs often stumble not in the reasoning process but in performing the actual arithmetic. PAL addresses this by using programs for intermediate steps, achieving a higher level of accuracy and consistency.


####


Benefits of Using PAL


PAL brings several key advantages:


- **Computational Accuracy** : By delegating calculations to an interpreter, PAL avoids the common arithmetic mistakes of LLMs.


- **Versatility** : While showcased in arithmetic, PAL's approach is adaptable to various reasoning tasks, offering broader applications.


####


Looking Ahead


PAL's potential is vast, with possibilities including:


- Expanding its integration with more external computational tools to tackle diverse problems.


- Improving the interaction between LLMs and interpreters for even better results.


####


Conclusion


PAL represents a significant leap forward in AI's ability to handle complex reasoning tasks, combining LLMs' contextual understanding with the precision of program execution.


This innovation not only advances AI capabilities but also sets the stage for future developments in the field, promising a new era of highly accurate and versatile AI systems.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
