---
schema_version: "1.0.0"
document_id: "57b2fee26c91574ccfdb7ce54be11e9e98e43367b1c6626f9deb997bc530978d"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/self-consistency-improves-chain-of-thought-reasoning-in-language-models"
published_at: "2023-03-07T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:29:11.084734+00:00"
content_hash: "sha256:fe8b764cd0693d4cf6a8db39873f06967db2a15e8a218cbc501ed8e0cbcc56b1"
---

# Self-Consistency Improves Chain of Thought Reasoning in Language Models

Do not index


Original Paper


[https://arxiv.org/abs/2203.11171](https://arxiv.org/abs/2203.11171)


Blog URL


[blog.athina.ai /self-co...e-models](https://blog.athina.ai/self-consistency-improves-chain-of-thought-reasoning-in-language-models)


**Original Paper:**[https://arxiv.org/abs/2203.11171](https://arxiv.org/abs/2203.11171)


**By:**[Xuezhi Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang%2C%20X) ,[Jason Wei](https://arxiv.org/search/cs?searchtype=author&query=Wei%2C%20J) ,[Dale Schuurmans](https://arxiv.org/search/cs?searchtype=author&query=Schuurmans%2C%20D) ,[Quoc Le](https://arxiv.org/search/cs?searchtype=author&query=Le%2C%20Q) ,[Ed Chi](https://arxiv.org/search/cs?searchtype=author&query=Chi%2C%20E) ,[Sharan Narang](https://arxiv.org/search/cs?searchtype=author&query=Narang%2C%20S) ,[Aakanksha Chowdhery](https://arxiv.org/search/cs?searchtype=author&query=Chowdhery%2C%20A) ,[Denny Zhou](https://arxiv.org/search/cs?searchtype=author&query=Zhou%2C%20D)


**Abstract:**


> Chain-of-thought prompting combined with pre-trained large language models has achieved encouraging results on complex reasoning tasks. In this paper, we propose a new decoding strategy, self-consistency, to replace the naive greedy decoding used in chain-of-thought prompting. It first samples a diverse set of reasoning paths instead of only taking the greedy one, and then selects the most consistent answer by marginalizing out the sampled reasoning paths. Self-consistency leverages the intuition that a complex reasoning problem typically admits multiple different ways of thinking leading to its unique correct answer. Our extensive empirical evaluation shows that self-consistency boosts the performance of chain-of-thought prompting with a striking margin on a range of popular arithmetic and commonsense reasoning benchmarks, including GSM8K (+17.9%), SVAMP (+11.0%), AQuA (+12.2%), StrategyQA (+6.4%) and ARC-challenge (+3.9%).


---


###


Summary Notes


##


Enhancing AI's Reasoning with Self-Consistency in Chain of Thought


Language models are reshaping our interaction with artificial intelligence (AI), taking on everything from basic queries to complex problem-solving.


Yet, when faced with detailed multi-step reasoning, these models often stumble. The introduction of chain-of-thought (CoT) prompting marked a significant step forward, encouraging models to work through problems step by step out loud.


Building on this, the concept of "self-consistency" has been introduced, offering a substantial boost in reasoning performance.


This blog explores the concept of self-consistency, how it works, its benefits, and what it means for AI professionals in large corporations.


####


Introduction to CoT Prompting and Self-Consistency


CoT prompting has changed the game for AI problem-solving by making models think and reason through steps like a human would. Self-consistency enhances this by:


- Creating multiple reasoning paths.


- Finding the most consistent answer across these paths.


- Serving as a "self-ensemble" method within a single model, reducing the need for multiple models and saving computational resources.


####


How Self-Consistency Works


- **CoT Prompting:** Guides models to detail their thought process, paving the way for solving complex problems.


- **Self-Consistency:** Improves upon CoT by:


####


Performance Improvements and Results


Tests on tasks like arithmetic and commonsense reasoning across four language models showed that self-consistency significantly enhances performance:


- **Improvements:** Notable upgrades in accuracy were observed, with self-consistency setting new standards.


- **Compared to Traditional Methods:** Self-consistency stood out against common decoding techniques such as greedy decoding and beam search, even outperforming methods requiring extra training or human input.


####


Boosting Model Robustness and Reliability


The key to self-consistency's success lies in the diverse reasoning paths it generates, which improve the model's stability and trustworthiness.


This approach overcomes the drawbacks of single-path decoding and extensive model training, marking a leap in tackling complex reasoning tasks.


It also opens the door to potential uses in open-ended text generation, broadening its application scope.


####


Conclusion: Self-Consistency as a Leap Forward


Self-consistency represents a major advance in enhancing language models' reasoning abilities. Its computational efficiency and easy integration with existing models make it a viable option for improving NLP tasks.


Looking ahead, applying self-consistency to various tasks and model types is an exciting research direction.


####


Ethical Considerations and Reproducibility


The study stresses careful implementation and testing to mitigate the risk of incorrect or biased results. It also provides detailed configurations for replication, encouraging further innovation in the field.


####


Looking Forward


Self-consistency in CoT reasoning signals a move towards more intelligent, dependable, and efficient language models. For AI engineers in large companies, this means new possibilities for deploying advanced NLP applications, improving efficiency, and better decision-making.


As we delve deeper into this promising field, the future of AI and machine learning appears increasingly bright, heralding an era of AI that closely mirrors human reasoning in problem-solving.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
