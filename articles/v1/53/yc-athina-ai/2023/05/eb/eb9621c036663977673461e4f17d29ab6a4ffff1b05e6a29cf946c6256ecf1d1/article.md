---
schema_version: "1.0.0"
document_id: "eb9621c036663977673461e4f17d29ab6a4ffff1b05e6a29cf946c6256ecf1d1"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/let-s-sample-step-by-step-adaptive-consistency-for-efficient-reasoning-and-coding-with-llms"
published_at: "2023-05-19T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:26.378360+00:00"
content_hash: "sha256:99b41c3f44213aa4920639868bd0c90a5fa01f61b185d5df55f7f4e74b840c44"
---

# Let's Sample Step by Step: Adaptive-Consistency for Efficient Reasoning and Coding with LLMs

Do not index


Original Paper


[https://arxiv.org/abs/2305.11860](https://arxiv.org/abs/2305.11860)


Blog URL


[blog.athina.ai /let-s-s...ith-llms](https://blog.athina.ai/let-s-sample-step-by-step-adaptive-consistency-for-efficient-reasoning-and-coding-with-llms)


**Original Paper:**[https://arxiv.org/abs/2305.11860](https://arxiv.org/abs/2305.11860)


**By:**[Pranjal Aggarwal](https://arxiv.org/search/cs?searchtype=author&query=Aggarwal%2C%20P) ,[Aman Madaan](https://arxiv.org/search/cs?searchtype=author&query=Madaan%2C%20A) ,[Yiming Yang](https://arxiv.org/search/cs?searchtype=author&query=Yang%2C%20Y) ,[Mausam](https://arxiv.org/search/cs?searchtype=author&query=Mausam)


**Abstract:**


> A popular approach for improving the correctness of output from large language models (LLMs) is Self-Consistency - poll the LLM multiple times and output the most frequent solution. Existing Self-Consistency techniques always generate a constant number of samples per question, where a better approach will be to non-uniformly distribute the available budget based on the amount of agreement in the samples generated so far. In response, we introduce Adaptive-Consistency, a cost-efficient, model-agnostic technique that dynamically adjusts the number of samples per question using a lightweight stopping criterion. Our experiments over 17 reasoning and code generation datasets and three LLMs demonstrate that Adaptive-Consistency reduces sample budget by up to 7.9 times with an average accuracy drop of less than 0.1%. Our code and data are available at
>
>
> [this https URL](https://www.sample-step-by-step.info/)


---


###


Summary Notes


####


Simplifying Adaptive-Consistency in LLMs for Efficient AI Solutions


Large Language Models (LLMs) have become a cornerstone in the field of artificial intelligence, transforming how machines understand and generate human-like text.


As these models grow, ensuring their outputs are both accurate and relevant is essential, yet challenging.


Traditional methods like Self-Consistency, which involves asking LLMs the same question multiple times to ensure accuracy, are effective but often too resource-intensive for practical use, especially in enterprise settings.


Enter Adaptive-Consistency, a smarter, cost-effective strategy that enhances how we interact with LLMs without compromising on quality. Let’s break down this innovative approach.


###


The Basics of LLMs


LLMs excel in adapting to new tasks through in-context few-shot prompting. This method uses example inputs to guide models in generating accurate answers.


However, as LLMs become larger, the computational cost of this process can be prohibitive.


###


What is Adaptive-Consistency?


Adaptive-Consistency revolutionizes the querying process of LLMs with three key features:


- **Dynamic Sample Adjustment** : It varies the number of samples based on how much the samples agree, optimizing computational resources.


- **Stopping Criterion** : This feature decides when enough samples have been taken, reducing unnecessary computations.


- **Confidence Quantification** : By using a Dirichlet distribution, it measures how confident we can be in the majority answer, ensuring decisions are made with precision.


This method not only makes the sampling process more efficient but also adapts in real-time to the model's responses, ensuring optimal use of computational resources.


###


Proven Efficiency and Accuracy


Testing on three different LLMs and 17 distinct tasks revealed that Adaptive-Consistency:


- **Cuts Down Samples** : It needed up to 7.9 times fewer samples than fixed-budget methods, highlighting its efficiency.


- **Keeps Accuracy High** : The quality of outcomes remained on par with more resource-intensive methods.


These results show that Adaptive-Consistency successfully reduces computational demands without sacrificing output quality, addressing a key concern for AI Engineers in enterprise environments.


###


Analyzing the Impact


Further analysis into Adaptive-Consistency's performance revealed:


- **Optimal Confidence Thresholds** : Setting higher confidence thresholds can lower sampling costs while maintaining accuracy.


- **Flexible Stopping Criteria** : The method’s adaptability to different computational and task demands showcases its flexibility.


###


The Road Ahead


Adaptive-Consistency marks a significant advancement in using LLMs more efficiently. Future research could explore even more effective stopping criteria and task-specific adjustments, broadening its application in AI tasks.


###


Conclusion


Adaptive-Consistency stands out as a viable solution for balancing accuracy with computational efficiency in the use of LLMs. This approach not only enhances the efficiency of querying these models but also opens up new possibilities for AI research and applications.


As AI continues to evolve, methods like Adaptive-Consistency will be key in achieving our goals efficiently and accurately.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
