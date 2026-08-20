---
schema_version: "1.0.0"
document_id: "e2e7dccedfb75c97ca2514c8f5e41784d94bd28fec30ccdc2d1d75e8ba8b66df"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/privacy-preserving-prompt-tuning-for-large-language-model-services"
published_at: "2023-05-10T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:26.378360+00:00"
content_hash: "sha256:214b5bafc377dc56cf3dd3c0b598f419479f101679d89006348a93910489acad"
---

# Privacy-Preserving Prompt Tuning for Large Language Model Services

Do not index


Original Paper


[https://arxiv.org/abs/2305.06212](https://arxiv.org/abs/2305.06212)


Blog URL


[blog.athina.ai /privacy...services](https://blog.athina.ai/privacy-preserving-prompt-tuning-for-large-language-model-services)


**Original Paper:**[https://arxiv.org/abs/2305.06212](https://arxiv.org/abs/2305.06212)


**By:**[Yansong Li](https://arxiv.org/search/cs?searchtype=author&query=Li%2C%20Y) ,[Zhixing Tan](https://arxiv.org/search/cs?searchtype=author&query=Tan%2C%20Z) ,[Yang Liu](https://arxiv.org/search/cs?searchtype=author&query=Liu%2C%20Y)


**Abstract:**


> Prompt tuning provides an efficient way for users to customize Large Language Models (LLMs) with their private data in the emerging LLM service scenario. However, the sensitive nature of private data brings the need for privacy preservation in LLM service customization. Based on prompt tuning, we propose Privacy-Preserving Prompt Tuning (RAPT), a framework that provides privacy guarantees for LLM services. \\textsc{rapt} adopts a local privacy setting, allowing users to privatize their data locally with local differential privacy. As prompt tuning performs poorly when directly trained on privatized data, we introduce a novel privatized token reconstruction task that is trained jointly with the downstream task, allowing LLMs to learn better task-dependent representations. Despite the simplicity of our framework, experiments show that RAPT achieves competitive performance across tasks while providing privacy guarantees against adversaries.


---


###


Summary Notes


####


Keeping Data Private in AI with RAPT


The world of artificial intelligence (AI) is always growing, with large language models (LLMs) leading the way.


These models are great at understanding and creating text that sounds human. A common way to make these models do specific tasks is through prompt tuning, which adjusts the models with little extra training.


However, when businesses use cloud services for prompt tuning, they worry about keeping their sensitive data private. This blog introduces an innovative solution called RAPT that protects data privacy without losing the benefits of LLMs.


####


Introduction to RAPT: A New Hope for Data Privacy


RAPT, short for Privacy-Preserving Prompt Tuning, is a new tool designed to keep sensitive information safe while still making the most out of LLMs.


It uses a smart technique called local differential privacy in the prompt tuning process, making sure that sensitive data stays confidential.


####


Inside the RAPT Framework


- **Local Privacy Approach** : With RAPT, users can make their data private on their own devices by adding a bit of noise to it. This ensures the data remains private even when used to train LLMs.


- **Making Tokens Private Again** : To avoid losing performance with the added privacy, RAPT introduces a way to teach the LLM to guess the original content from its noisier version, helping it understand better.


####


How RAPT Works


Using RAPT involves a few steps:


- **Making Data Private** : Users first make their data private with differential privacy before sending it to the cloud.


- **Tuning LLMs with RAPT** : Then, they use the cloud-based LLMs' prompt tuning services with this private data.


- **Reconstructing Private Tokens** : The LLM tries to guess the original data from its private version, improving its performance.


Tests on models like BERT and T5, for tasks such as understanding emotions in text, show that RAPT is effective at protecting privacy without sacrificing quality.


####


Other Methods


Research into keeping LLMs private has looked at centralized methods, which rely on the cloud provider, and local methods, which keep more control with the user. RAPT offers a middle ground, providing strong privacy without depending on the provider.


####


Results


RAPT does a better job at protecting privacy compared to traditional prompt tuning, without losing much in performance.


It works well for different AI tasks and models, making it a big step forward for the field.


####


Conclusion


RAPT meets the critical need for privacy in using LLMs for personalized tasks. By combining local differential privacy with a technique to guess original content from its private form, it allows the safe use of cloud-based LLM services without risking sensitive data. Future research could expand this approach to more models and types of privacy threats.


####


Figures and Tables


- **Figure 1:** Shows how RAPT works with cloud-based LLMs.


- **Tables and Figures:** Provide detailed results, showing how effective RAPT is.


**Keywords:** Large Language Models, Privacy-Preserving, Prompt Tuning, Differential Privacy, Natural Language Processing.


RAPT represents a significant advancement in responsible AI development, offering a way for businesses to use cutting-edge technology while protecting sensitive information. This approach not only enhances security in AI deployments but also sets new standards for responsible AI in our data-centric world.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
