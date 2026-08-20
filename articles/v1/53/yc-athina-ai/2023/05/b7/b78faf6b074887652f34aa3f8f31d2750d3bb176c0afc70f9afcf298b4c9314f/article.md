---
schema_version: "1.0.0"
document_id: "b78faf6b074887652f34aa3f8f31d2750d3bb176c0afc70f9afcf298b4c9314f"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/plan-and-solve-prompting-improving-zero-shot-chain-of-thought-reasoning-by-large-language-models"
published_at: "2023-05-06T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:26.378360+00:00"
content_hash: "sha256:31dd9105a17147c491f32682ddf05b454ae0f5afe3260d3bcd6a663827a2df10"
---

# Plan-and-Solve Prompting: Improving Zero-Shot Chain-of-Thought Reasoning by Large Language Models

Do not index


Original Paper


[https://arxiv.org/abs/2305.04091v3](https://arxiv.org/abs/2305.04091v3)


Blog URL


[blog.athina.ai /plan-an...e-models](https://blog.athina.ai/plan-and-solve-prompting-improving-zero-shot-chain-of-thought-reasoning-by-large-language-models)


**Original Paper:**[https://arxiv.org/abs/2305.04091v3](https://arxiv.org/abs/2305.04091v3)


**By:**[Lei Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang%2C%20L) ,[Wanyu Xu](https://arxiv.org/search/cs?searchtype=author&query=Xu%2C%20W) ,[Yihuai Lan](https://arxiv.org/search/cs?searchtype=author&query=Lan%2C%20Y) ,[Zhiqiang Hu](https://arxiv.org/search/cs?searchtype=author&query=Hu%2C%20Z) ,[Yunshi Lan](https://arxiv.org/search/cs?searchtype=author&query=Lan%2C%20Y) ,[Roy Ka-Wei Lee](https://arxiv.org/search/cs?searchtype=author&query=Lee%2C%20R%20K) ,[Ee-Peng Lim](https://arxiv.org/search/cs?searchtype=author&query=Lim%2C%20E)


**Abstract:**


> Large language models (LLMs) have recently been shown to deliver impressive performance in various NLP tasks. To tackle multi-step reasoning tasks, few-shot chain-of-thought (CoT) prompting includes a few manually crafted step-by-step reasoning demonstrations which enable LLMs to explicitly generate reasoning steps and improve their reasoning task accuracy. To eliminate the manual effort, Zero-shot-CoT concatenates the target problem statement with "Let's think step by step" as an input prompt to LLMs. Despite the success of Zero-shot-CoT, it still suffers from three pitfalls: calculation errors, missing-step errors, and semantic misunderstanding errors. To address the missing-step errors, we propose Plan-and-Solve (PS) Prompting. It consists of two components: first, devising a plan to divide the entire task into smaller subtasks, and then carrying out the subtasks according to the plan. To address the calculation errors and improve the quality of generated reasoning steps, we extend PS prompting with more detailed instructions and derive PS+ prompting. We evaluate our proposed prompting strategy on ten datasets across three reasoning problems. The experimental results over GPT-3 show that our proposed zero-shot prompting consistently outperforms Zero-shot-CoT across all datasets by a large margin, is comparable to or exceeds Zero-shot-Program-of-Thought Prompting, and has comparable performance with 8-shot CoT prompting on the math reasoning problem. The code can be found at
>
>
> [this https URL](https://github.com/AGI-Edgerunners/Plan-and-Solve-Prompting)


---


###


Summary Notes


##


Elevating AI Reasoning with Plan-and-Solve Prompting: A Breakthrough for AI Engineers


The landscape of artificial intelligence (AI) is continually advancing, with large language models (LLMs) at the forefront of natural language processing (NLP) tasks.


These models excel in generating and understanding text in a human-like manner, paving the way for solving complex problems. However, their ability to perform intricate reasoning is often hindered by errors and misunderstandings.


The introduction of Plan-and-Solve (PS) Prompting is a major innovation, offering a new tool for AI engineers, particularly in enterprise environments, to enhance AI's reasoning capabilities.


###


What is Plan-and-Solve Prompting?


Plan-and-Solve Prompting is an innovative strategy aimed at improving LLMs' reasoning skills. It advances beyond the traditional Chain-of-Thought (CoT) prompting by:


- **Simplifying Complex Problems** : It breaks down tasks into smaller, more manageable parts.


- **Offering Clear Guidance** : Unlike CoT's manual example-based guidance, PS Prompting uses a specific trigger sentence to direct the model through the reasoning steps smoothly.


- **Enhanced Version (PS+)** : This variant includes precise instructions to boost calculation accuracy and reduce errors.


This approach not only tackles the limitations seen in CoT prompting but also expands the capabilities of AI models in reasoning with less need for manual example creation.


###


Insights from Experiments


PS Prompting has been thoroughly tested across various datasets, addressing arithmetic, commonsense, and symbolic reasoning. Compared to traditional methods like Zero-shot-CoT and few-shot approaches, PS Prompting has shown remarkable results:


- **Improved Performance** : It consistently surpasses Zero-shot-CoT, proving its effectiveness in enhancing reasoning.


- **Better Arithmetic Reasoning** : The PS+ variant, in particular, has shown a significant reduction in calculation mistakes.


- **Versatility** : Its strong performance across different types of reasoning tasks demonstrates its broad applicability.


These results highlight PS Prompting's potential to transform AI's approach to complex reasoning tasks, making it an invaluable tool for AI engineers.


###


How AI Engineers Can Utilize PS Prompting


For AI engineers, especially in large companies, PS Prompting offers profound benefits. Here are some tips for effectively using this method:


- **Break Down Complex Tasks** : Dividing complex problems into smaller parts makes them more manageable for the model.


- **Design Precise Prompts** : The effectiveness of PS Prompting relies heavily on the quality of the prompts, which must guide the model accurately.


- **Refine Through Experimentation** : Continuously testing different prompts and analyzing outcomes can help find the most effective approach for complex reasoning.


###


Conclusion: Charting the Future of AI Reasoning


Plan-and-Solve Prompting signifies a significant advance in AI reasoning capabilities. It not only overcomes the limitations of previous methods but also establishes a new benchmark for AI problem-solving with minimal human input.


For AI engineers in enterprise settings, PS Prompting provides a promising path to boosting their models' reasoning powers, fostering efficiency, and driving innovation.


As this method continues to be explored and refined, the prospects for AI reasoning capabilities are set to reach new heights.


By adopting PS Prompting, AI engineers are not just enhancing their models; they're contributing to the dawn of an era where AI can think, reason, and solve problems more akin to human beings.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
