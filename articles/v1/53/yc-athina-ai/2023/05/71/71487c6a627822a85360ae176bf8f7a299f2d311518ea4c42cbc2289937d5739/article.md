---
schema_version: "1.0.0"
document_id: "71487c6a627822a85360ae176bf8f7a299f2d311518ea4c42cbc2289937d5739"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/let-s-verify-step-by-step"
published_at: "2023-05-31T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:24.924529+00:00"
content_hash: "sha256:a436073bf3fd1fb78a305b0db7aa793e866500fce163118bfbb117b0ee9228c0"
---

# Let's Verify Step by Step

Do not index


Original Paper


[https://arxiv.org/abs/2305.20050](https://arxiv.org/abs/2305.20050)


Blog URL


[blog.athina.ai /let-s-v...-by-step](https://blog.athina.ai/let-s-verify-step-by-step)


**Original Paper:**[https://arxiv.org/abs/2305.20050](https://arxiv.org/abs/2305.20050)


**By:**[Hunter Lightman](https://arxiv.org/search/cs?searchtype=author&query=Lightman%2C%20H) ,[Vineet Kosaraju](https://arxiv.org/search/cs?searchtype=author&query=Kosaraju%2C%20V) ,[Yura Burda](https://arxiv.org/search/cs?searchtype=author&query=Burda%2C%20Y) ,[Harri Edwards](https://arxiv.org/search/cs?searchtype=author&query=Edwards%2C%20H) ,[Bowen Baker](https://arxiv.org/search/cs?searchtype=author&query=Baker%2C%20B) ,[Teddy Lee](https://arxiv.org/search/cs?searchtype=author&query=Lee%2C%20T) ,[Jan Leike](https://arxiv.org/search/cs?searchtype=author&query=Leike%2C%20J) ,[John Schulman](https://arxiv.org/search/cs?searchtype=author&query=Schulman%2C%20J) ,[Ilya Sutskever](https://arxiv.org/search/cs?searchtype=author&query=Sutskever%2C%20I) ,[Karl Cobbe](https://arxiv.org/search/cs?searchtype=author&query=Cobbe%2C%20K)


**Abstract:**


> In recent years, large language models have greatly improved in their ability to perform complex multi-step reasoning. However, even state-of-the-art models still regularly produce logical mistakes. To train more reliable models, we can turn either to outcome supervision, which provides feedback for a final result, or process supervision, which provides feedback for each intermediate reasoning step. Given the importance of training reliable models, and given the high cost of human feedback, it is important to carefully compare the both methods. Recent work has already begun this comparison, but many questions still remain. We conduct our own investigation, finding that process supervision significantly outperforms outcome supervision for training models to solve problems from the challenging MATH dataset. Our process-supervised model solves 78% of problems from a representative subset of the MATH test set. Additionally, we show that active learning significantly improves the efficacy of process supervision. To support related research, we also release PRM800K, the complete dataset of 800,000 step-level human feedback labels used to train our best reward model.


---


###


Summary Notes


##


Enhancing Language Models with Process Supervision: A Simplified Guide


The field of artificial intelligence, particularly natural language processing, is always advancing. One key goal is improving the accuracy and human-like reasoning of language models.


A promising method for achieving this is process supervision, especially useful for complex reasoning tasks.


###


The Challenge of Logical Errors


Even the best language models can make logical mistakes or "hallucinations" during complex reasoning. These errors can affect their reliability, particularly in critical uses.


Traditional training methods, focusing only on the final outcome, often fail to address these issues effectively.


###


Why Process Supervision Works Better


Process supervision offers a more effective solution by providing feedback at each step of the reasoning process. Here’s why it’s superior:


- **Detailed Feedback** : It identifies exactly where errors occur, allowing for more precise corrections.


- **Human-Like Reasoning** : By evaluating each step, it mimics human problem-solving, making models think more like us.


- **Efficient Learning** : It uses active learning to target the most misleading errors, improving training efficiency.


###


How to Implement Process Supervision


####


Gathering and Assessing Data


The first step is to collect data with human reviewers assessing the accuracy of each reasoning step in the model’s answers. They rate each step as positive, negative, or neutral, creating a detailed dataset for training.


####


Training the Generator


It’s crucial to train the generator to output solutions step-by-step. This makes it easier to review individual steps and fits the process supervision training format.


####


Using Large-Scale Supervision


The PRM800K dataset, with 800,000 step-level feedback labels, is invaluable for training a Process Reward Model (PRM) on a large scale. This rich dataset significantly boosts model performance.


####


Small-Scale Synthetic Supervision


For those with limited resources, small-scale synthetic supervision is a practical option. It uses a large-scale PRM to guide the training of smaller models, offering a cost-effective way to simulate extensive data collection.


####


Testing Generalization


Evaluating the model's performance on new, unseen tasks is essential. By testing on recent STEM tests not included in the training set, engineers can assess how well the model generalizes to new challenges.


###


Conclusion


Moving from outcome supervision to process supervision can greatly improve language models, making them more accurate and aligned with human reasoning.


The introduction of the PRM800K dataset is a game-changer, providing a wealth of data for ongoing research and development.


As the field progresses, the value of detailed feedback and human-like reasoning in training language models becomes increasingly clear.


Process supervision represents not just a new method, but a shift in our approach to training language models for complex reasoning tasks.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
