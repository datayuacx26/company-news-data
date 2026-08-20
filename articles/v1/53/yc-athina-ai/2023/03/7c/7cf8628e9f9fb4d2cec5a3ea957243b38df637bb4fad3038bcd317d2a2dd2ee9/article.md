---
schema_version: "1.0.0"
document_id: "7cf8628e9f9fb4d2cec5a3ea957243b38df637bb4fad3038bcd317d2a2dd2ee9"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/uprise-universal-prompt-retrieval-for-improving-zero-shot-evaluation"
published_at: "2023-03-15T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:29:11.084734+00:00"
content_hash: "sha256:01162c6c2785b1ee4b7f6c4e807d8b1f247f89195be2db573a3fe35a0fffc69f"
---

# UPRISE: Universal Prompt Retrieval for Improving Zero-Shot Evaluation

Do not index


Original Paper


[https://arxiv.org/abs/2303.08518](https://arxiv.org/abs/2303.08518)


Blog URL


[blog.athina.ai /uprise-...aluation](https://blog.athina.ai/uprise-universal-prompt-retrieval-for-improving-zero-shot-evaluation)


**Original Paper:**[https://arxiv.org/abs/2303.08518](https://arxiv.org/abs/2303.08518)


**By:**[Daixuan Cheng](https://arxiv.org/search/cs?searchtype=author&query=Cheng%2C%20D) ,[Shaohan Huang](https://arxiv.org/search/cs?searchtype=author&query=Huang%2C%20S) ,[Junyu Bi](https://arxiv.org/search/cs?searchtype=author&query=Bi%2C%20J) ,[Yuefeng Zhan](https://arxiv.org/search/cs?searchtype=author&query=Zhan%2C%20Y) ,[Jianfeng Liu](https://arxiv.org/search/cs?searchtype=author&query=Liu%2C%20J) ,[Yujing Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang%2C%20Y) ,[Hao Sun](https://arxiv.org/search/cs?searchtype=author&query=Sun%2C%20H) ,[Furu Wei](https://arxiv.org/search/cs?searchtype=author&query=Wei%2C%20F) ,[Denvy Deng](https://arxiv.org/search/cs?searchtype=author&query=Deng%2C%20D) ,[Qi Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang%2C%20Q)


**Abstract:**


> Large Language Models (LLMs) are popular for their impressive abilities, but the need for model-specific fine-tuning or task-specific prompt engineering can hinder their generalization. We propose UPRISE (Universal Prompt Retrieval for Improving zero-Shot Evaluation), which tunes a lightweight and versatile retriever that automatically retrieves prompts for a given zero-shot task input. Specifically, we demonstrate universality in a cross-task and cross-model scenario: the retriever is tuned on a diverse set of tasks, but tested on unseen task types; we use a small frozen LLM, GPT-Neo-2.7B, for tuning the retriever, but test the retriever on different LLMs of much larger scales, such as BLOOM-7.1B, OPT-66B and GPT3-175B. Additionally, we show that UPRISE mitigates the hallucination problem in our experiments with ChatGPT, suggesting its potential to improve even the strongest LLMs. Our model and code are available at
>
>
> [this https URL](https://github.com/microsoft/LMOps)


---


###


Summary Notes


####


Blog Post: Unlocking New AI Capabilities with Uprise: Streamlining Zero-Shot Learning


In the fast-paced world of artificial intelligence (AI), the ability of models to perform tasks they haven't been directly trained for, known as zero-shot learning, is increasingly critical.


This is especially true for Large Language Models (LLMs) that are expected to adapt and respond across a broad spectrum of tasks without specific training. Uprise represents a significant advancement in this area, enhancing zero-shot learning capabilities and broadening the potential applications of AI.


####


What is Uprise?


Uprise, short for Universal Prompt Retrieval for Improving Zero-Shot Evaluation, marks a significant step towards creating more adaptable AI. Unlike conventional methods that rely heavily on fine-tuning or creating specific prompts for tasks, Uprise automates the process of finding the most relevant prompts for any given zero-shot task.


This not only boosts the adaptability of LLMs across various tasks but also shows promise in reducing errors, such as hallucinations, in model outputs. Uprise's model and code are available on GitHub, encouraging open collaboration.


####


Main Features


Uprise brings several key advancements to the AI realm:


- **Universal Application:** Works across different models, demonstrating its wide applicability.


- **Enhanced Performance:** Proven to improve outcomes in tasks such as Reading Comprehension, Closed-book QA, and Paraphrase Detection.


- **Reduction in Hallucination:** Shows potential in decreasing hallucination issues in models like ChatGPT, leading to more accurate outputs.


####


How It Works


The essence of Uprise's approach involves training a prompt retriever that identifies the most suitable prompts from a predefined set for any zero-shot task. This process includes:


- Creating data from instruction templates.


- Scoring prompts based on their effectiveness.


- Tuning the retriever through contrastive learning to be effective across different tasks and models.


This methodology has proven Uprise's ability to enhance performance in a variety of tasks and models without needing additional tuning.


####


Experimental Insights


Uprise has demonstrated significant improvements in zero-shot learning, showing notable performance enhancements across different tasks and models when compared to traditional methods. While it has improved accuracy and reduced hallucination in models like ChatGPT, its impact on tasks inherently based on language modeling, such as Coreference Resolution and Commonsense Reasoning, has been more limited.


####


Future Directions and Limitations


Despite its successes, Uprise's effectiveness in language modeling tasks remains constrained. Future research may explore integrating multimodal information and enhancing performance in areas where Uprise currently shows limited benefits. This ongoing work aims to push AI capabilities further, making models more adaptable, reliable, and widely usable.


####


Ethical Considerations and Open Science


Uprise is developed with a commitment to ethical standards and open science, ensuring accessibility and transparency in AI research. By making datasets and language models publicly available, it invites broad participation and feedback to foster a more inclusive research community.


####


Acknowledgments


The development of Uprise was made possible through the collaborative efforts of many, including colleagues who contributed to debugging, discussions, paper review, and code enhancement. This collective effort highlights the importance of community in advancing AI technology.


Uprise is a pivotal development in the quest for more adaptable and reliable AI. By improving zero-shot learning across a range of tasks and models, it opens up new possibilities for the application and evolution of LLMs, moving us closer to a future where AI seamlessly understands and interacts with the world.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
