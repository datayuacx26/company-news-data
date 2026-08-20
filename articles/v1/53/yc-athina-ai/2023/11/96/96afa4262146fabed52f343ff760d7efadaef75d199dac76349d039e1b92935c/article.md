---
schema_version: "1.0.0"
document_id: "96afa4262146fabed52f343ff760d7efadaef75d199dac76349d039e1b92935c"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/black-box-prompt-optimization-aligning-large-language-models-without-model-training"
published_at: "2023-11-08T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:22.552077+00:00"
content_hash: "sha256:ec1999103893898511107444c5d3421090fa5e7fa75bacc8459d2ccd7e271f7a"
---

# Black-Box Prompt Optimization: Aligning Large Language Models without Model Training

Do not index


Original Paper


[https://arxiv.org/abs/2311.04155](https://arxiv.org/abs/2311.04155)


Blog URL


[blog.athina.ai /black-b...training](https://blog.athina.ai/black-box-prompt-optimization-aligning-large-language-models-without-model-training)


**Original Paper:**[https://arxiv.org/abs/2311.04155](https://arxiv.org/abs/2311.04155)


**By:**[Jiale Cheng](https://arxiv.org/search/cs?searchtype=author&query=Cheng%2C%20J) ,[Xiao Liu](https://arxiv.org/search/cs?searchtype=author&query=Liu%2C%20X) ,[Kehan Zheng](https://arxiv.org/search/cs?searchtype=author&query=Zheng%2C%20K) ,[Pei Ke](https://arxiv.org/search/cs?searchtype=author&query=Ke%2C%20P) ,[Hongning Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang%2C%20H) ,[Yuxiao Dong](https://arxiv.org/search/cs?searchtype=author&query=Dong%2C%20Y) ,[Jie Tang](https://arxiv.org/search/cs?searchtype=author&query=Tang%2C%20J) ,[Minlie Huang](https://arxiv.org/search/cs?searchtype=author&query=Huang%2C%20M)


**Abstract:**


> Large language models (LLMs) have shown impressive success in various applications. However, these models are often not well aligned with human intents, which calls for additional treatments on them, that is, the alignment problem. To make LLMs better follow user instructions, existing alignment methods mostly focus on further training them. However, the extra training of LLMs are usually expensive in terms of GPU compute; worse still, LLMs of interest are oftentimes not accessible for user-demanded training, such as GPTs. In this work, we take a different perspective -- Black-Box Prompt Optimization (BPO) -- to perform alignments. The idea is to optimize user prompts to suit LLMs' input understanding, so as to best realize users' intents without updating LLMs' parameters. BPO is model-agnostic and the empirical results demonstrate that the BPO-aligned ChatGPT yields a 22% increase in the win rate against its original version, and 10% for GPT-4. Importantly, the BPO-aligned LLMs can outperform the same models aligned by PPO and DPO, and it also brings additional performance gains when combining BPO with PPO or DPO. Code and datasets are released at
>
>
> [this https URL](https://github.com/thu-coai/BPO)


---


###


Summary Notes


####


Simplifying AI Alignment with Black-Box Prompt Optimization


In the fast-paced world of artificial intelligence (AI), aligning Large Language Models (LLMs) like GPT-3 and GPT-4 with human intent poses a significant challenge.


Traditional strategies, which involve retraining these models, are often costly and sometimes not feasible.


This is where Black-Box Prompt Optimization (BPO) comes in as an innovative and efficient solution.


###


The Challenge at Hand


For AI professionals working in large companies, adapting LLMs to meet specific requirements is essential but challenging. Traditional methods, such as Reinforcement Learning from Human Feedback (RLHF) and Direct Preference Optimization (DPO), have limitations.


They require extensive resources, access to the model's internal workings, and can introduce biases. These methods also struggle with scaling and are not universally applicable across different models.


Prompt engineering and tuning have been initial steps toward guiding LLMs more effectively, but they fall short of offering a scalable, model-agnostic solution without the need for retraining.


###


How Black-Box Prompt Optimization Works


BPO offers a promising solution by focusing on optimizing the prompts given to LLMs. This approach uses a prompt preference optimizer, trained on datasets of good and bad responses, to refine prompts and align the model's outputs with human preferences. Its key features include:


- Applicability across various models, both API-based and open-sourced, without needing direct access to the model's architecture.


- A model-agnostic optimization focus, enhancing its versatility and ease of use.


####


Evidence of BPO's Effectiveness


Empirical tests across different tasks have shown BPO's potential in improving response quality and alignment with human intent.


Experiments with models like GPT-3.5-turbo and GPT-4 have demonstrated notable improvements, making BPO a strong contender against traditional training methods.


###


Practical Applications and Achievements


BPO's real-world impact is significant, offering enhanced alignment of LLMs with specific enterprise needs:


- Experiments have shown a 22% increase in win rate for ChatGPT and a 10% increase for GPT-4 in achieving better alignment with human intent.


- BPO's flexibility allows it to be integrated with other methods, potentially leading to even more effective alignment strategies.


###


Looking Ahead: The Future of LLM Alignment


BPO represents a shift in aligning LLMs with human intent, focusing on prompt optimization for a scalable and efficient solution.


This method is especially beneficial for AI engineers looking to leverage LLMs without the drawbacks of traditional methods.


The potential for integrating BPO with automated prompt engineering and applying it in various scenarios is extensive. With available code and datasets, BPO encourages ongoing research and innovation in AI.


In summary, Black-Box Prompt Optimization offers a new and practical approach to aligning LLMs with human preferences, marking a significant advancement in the optimization of AI technologies.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
