---
schema_version: "1.0.0"
document_id: "dba0f037d915f6910dba38907786aa28c3d4ccbee5ffa7a6e2accc93aa9cf2f0"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/synthetic-prompting-generating-chain-of-thought-demonstrations-for-large-language-models"
published_at: "2023-02-01T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:29.187426+00:00"
content_hash: "sha256:be1adaa589a09445be657da21ed80b2ee8e339f5c6c198b1999ca8be48d68fde"
---

# Synthetic Prompting: Generating Chain-of-Thought Demonstrations for Large Language Models

Do not index


Original Paper


[https://arxiv.org/abs/2302.00618](https://arxiv.org/abs/2302.00618)


Blog URL


[blog.athina.ai /synthet...e-models](https://blog.athina.ai/synthetic-prompting-generating-chain-of-thought-demonstrations-for-large-language-models)


**Original Paper:**[https://arxiv.org/abs/2302.00618](https://arxiv.org/abs/2302.00618)


**By:**[Zhihong Shao](https://arxiv.org/search/cs?searchtype=author&query=Shao%2C%20Z) ,[Yeyun Gong](https://arxiv.org/search/cs?searchtype=author&query=Gong%2C%20Y) ,[Yelong Shen](https://arxiv.org/search/cs?searchtype=author&query=Shen%2C%20Y) ,[Minlie Huang](https://arxiv.org/search/cs?searchtype=author&query=Huang%2C%20M) ,[Nan Duan](https://arxiv.org/search/cs?searchtype=author&query=Duan%2C%20N) ,[Weizhu Chen](https://arxiv.org/search/cs?searchtype=author&query=Chen%2C%20W)


**Abstract:**


> Large language models can perform various reasoning tasks by using chain-of-thought prompting, which guides them to find answers through step-by-step demonstrations. However, the quality of the prompts depends on the demonstrations given to the models, and creating many of them by hand is costly. We introduce Synthetic prompting, a method that leverages a few handcrafted examples to prompt the model to generate more examples by itself, and selects effective demonstrations to elicit better reasoning. Our method alternates between a backward and forward process to generate new examples. The backward process generates a question that match a sampled reasoning chain, so that the question is solvable and clear. The forward process produces a more detailed reasoning chain for the question, improving the quality of the example. We evaluate our method on numerical, symbolic, and algorithmic reasoning tasks, and show that it outperforms existing prompting techniques.


---


###


Summary Notes


####


Blog Post: Boosting LLM Reasoning Skills with Synthetic Prompting


Large Language Models (LLMs) have transformed the AI landscape with their human-like text generation abilities.


Despite their prowess, these models often falter at complex reasoning tasks, a challenge that stems from their training on limited or non-diverse data.


Enter synthetic prompting, an innovative solution designed to enhance LLMs' reasoning without the need for extensive manual data curation.


###


Understanding the Challenge


LLMs are adept at various tasks but struggle with logical reasoning that requires step-by-step deduction.


Traditional training methods like few-shot learning don't consistently produce the diverse reasoning chains needed for complex problem-solving, limiting the models' ability to generalize.


###


Synthetic Prompting: The Innovative Solution


Synthetic prompting addresses these limitations by using the LLMs' own knowledge base to generate new, diverse examples for training, broken down into two phases:


####


Example Synthesis Phase


- **Backward Process:** The model creates a reasoning chain and then formulates a corresponding question, ensuring the question is coherent and answerable.


- **Forward Process:** The model refines the reasoning chain for the generated question, improving reasoning quality and complexity.


####


Inference Phase


- **Selection Scheme:** A complexity-based selection mechanism picks the most effective examples from the synthetically generated data, focusing on diversity and reasoning complexity to enrich the training set.


###


Proven Success


Testing across various reasoning tasks (numerical, symbolic, algorithmic) has shown significant improvements with synthetic prompting, recording up to 15.6% absolute gains in some areas.


The complexity-based selection method often outperforms other approaches, indicating its effectiveness in enhancing reasoning skills.


###


Looking Ahead


Synthetic prompting marks a significant advancement in LLM reasoning capabilities, offering a way to improve performance with limited seed examples. This method is crucial for the future of AI, enabling models to approach human-like reasoning and understanding.


###


Conclusion


Synthetic prompting is a breakthrough in AI, pushing LLMs closer to human intelligence levels by allowing them to generate their training data.


This approach not only improves LLMs' efficiency in reasoning tasks but also opens new research and development avenues in AI.


For AI professionals, leveraging synthetic prompting could revolutionize how LLMs are used in complex problem-solving, marking a new era in AI capabilities.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
