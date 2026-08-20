---
schema_version: "1.0.0"
document_id: "17556e8aa4947faccb8695c2fe6836bc86e70a6169bebb2c0ae7fdcd121c25f1"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/chain-of-thought-prompting-elicits-reasoning-in-large-language-models"
published_at: "2023-01-10T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:29.187426+00:00"
content_hash: "sha256:1ab7626816f6c5fc70a05515d9acf156aaa05d47bd573a5fb5498d91b86a7a57"
---

# Chain-of-Thought Prompting Elicits Reasoning in Large Language Models

Do not index


Original Paper


[https://arxiv.org/abs/2201.11903](https://arxiv.org/abs/2201.11903)


Blog URL


[blog.athina.ai /chain-o...e-models](https://blog.athina.ai/chain-of-thought-prompting-elicits-reasoning-in-large-language-models)


**Original Paper:**[https://arxiv.org/abs/2201.11903](https://arxiv.org/abs/2201.11903)


**By:**[Jason Wei](https://arxiv.org/search/cs?searchtype=author&query=Wei%2C%20J) ,[Xuezhi Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang%2C%20X) ,[Dale Schuurmans](https://arxiv.org/search/cs?searchtype=author&query=Schuurmans%2C%20D) ,[Maarten Bosma](https://arxiv.org/search/cs?searchtype=author&query=Bosma%2C%20M) ,[Brian Ichter](https://arxiv.org/search/cs?searchtype=author&query=Ichter%2C%20B) ,[Fei Xia](https://arxiv.org/search/cs?searchtype=author&query=Xia%2C%20F) ,[Ed Chi](https://arxiv.org/search/cs?searchtype=author&query=Chi%2C%20E) ,[Quoc Le](https://arxiv.org/search/cs?searchtype=author&query=Le%2C%20Q) ,[Denny Zhou](https://arxiv.org/search/cs?searchtype=author&query=Zhou%2C%20D)


**Abstract:**


> We explore how generating a chain of thought -- a series of intermediate reasoning steps -- significantly improves the ability of large language models to perform complex reasoning. In particular, we show how such reasoning abilities emerge naturally in sufficiently large language models via a simple method called chain of thought prompting, where a few chain of thought demonstrations are provided as exemplars in prompting. Experiments on three large language models show that chain of thought prompting improves performance on a range of arithmetic, commonsense, and symbolic reasoning tasks. The empirical gains can be striking. For instance, prompting a 540B-parameter language model with just eight chain of thought exemplars achieves state of the art accuracy on the GSM8K benchmark of math word problems, surpassing even finetuned GPT-3 with a verifier.


---


###


Summary Notes


####


Enhancing Language Models with Chain-of-Thought Prompting


Language models are at the forefront of advancements in natural language processing (NLP). As these models grow, their ability to perform a wide array of tasks also increases. Yet, complex reasoning remains a challenge.


This blog post introduces chain-of-thought prompting, a method that significantly improves language models' reasoning abilities, offering a new avenue for AI engineers in enterprise companies.


####


What is Chain-of-Thought Prompting?


Chain-of-thought prompting mimics human problem-solving by breaking down complex issues into simpler steps.


This technique prompts language models with examples that lay out a step-by-step reasoning process towards a solution.


It essentially provides models with a roadmap for tackling and solving complex problems, making them more than just answer generators.


####


Empirical Evidence of Success


Tests across various reasoning tasks—arithmetic, commonsense, and symbolic reasoning—demonstrate that chain-of-thought prompting surpasses traditional prompting methods.


For example, the PaLM 540B model, using chain-of-thought prompting, achieved record-breaking results on the GSM8K benchmark for math word problems.


**Detailed Findings:**


- **Arithmetic Reasoning:** Matched or exceeded the performance of models fine-tuned for arithmetic.


- **Commonsense Reasoning:** Showed effectiveness across diverse datasets.


- **Symbolic Reasoning:** Excelled in tasks requiring symbolic manipulation, even on new, longer sequences.


####


Discussion


Chain-of-thought prompting is a powerful method for drawing out detailed reasoning from large language models.


It uses a model's inherent knowledge, guiding it through a problem-solving process with just a few examples.


This method opens the door to further research on minimizing example needs, understanding its limits, and exploring its application in other reasoning tasks.


####


Conclusion


Chain-of-thought prompting significantly boosts large language models' reasoning capabilities on complex tasks without the need for extensive retraining or specialized datasets. It offers a promising path for AI engineers to enhance the intelligence and versatility of AI solutions.


####


Future Directions


The journey of exploring chain-of-thought prompting continues with several potential areas of research:


- Scaling this approach to larger models and more tasks.


- Automating the creation of chain-of-thought prompts.


- Investigating the impact of model size on effectiveness and seeking efficiency optimizations.


For AI engineers in enterprise companies, keeping up with these advancements is essential. As language models evolve, mastering chain-of-thought prompting will be crucial for harnessing AI's full power, driving innovation, and solving complex problems more efficiently.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
