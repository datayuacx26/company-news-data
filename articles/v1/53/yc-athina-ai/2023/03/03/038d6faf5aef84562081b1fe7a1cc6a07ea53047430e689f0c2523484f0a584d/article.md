---
schema_version: "1.0.0"
document_id: "038d6faf5aef84562081b1fe7a1cc6a07ea53047430e689f0c2523484f0a584d"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/dynamic-prompt-learning-via-policy-gradient-for-semi-structured-mathematical-reasoning"
published_at: "2023-03-02T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:28.379811+00:00"
content_hash: "sha256:30ed9bba09d43ceb212d010cdeb2fb53a4e0b44da0dad6bc88e6ccbb1d75e774"
---

# Dynamic Prompt Learning via Policy Gradient for Semi-structured Mathematical Reasoning

Do not index


Original Paper


[https://arxiv.org/abs/2209.14610](https://arxiv.org/abs/2209.14610)


Blog URL


[blog.athina.ai /dynamic...easoning](https://blog.athina.ai/dynamic-prompt-learning-via-policy-gradient-for-semi-structured-mathematical-reasoning)


**Original Paper:**[https://arxiv.org/abs/2209.14610](https://arxiv.org/abs/2209.14610)


**By:**[Pan Lu](https://arxiv.org/search/cs?searchtype=author&query=Lu%2C%20P) ,[Liang Qiu](https://arxiv.org/search/cs?searchtype=author&query=Qiu%2C%20L) ,[Kai-Wei Chang](https://arxiv.org/search/cs?searchtype=author&query=Chang%2C%20K) ,[Ying Nian Wu](https://arxiv.org/search/cs?searchtype=author&query=Wu%2C%20Y%20N) ,[Song-Chun Zhu](https://arxiv.org/search/cs?searchtype=author&query=Zhu%2C%20S) ,[Tanmay Rajpurohit](https://arxiv.org/search/cs?searchtype=author&query=Rajpurohit%2C%20T) ,[Peter Clark](https://arxiv.org/search/cs?searchtype=author&query=Clark%2C%20P) ,[Ashwin Kalyan](https://arxiv.org/search/cs?searchtype=author&query=Kalyan%2C%20A)


**Abstract:**


> Mathematical reasoning, a core ability of human intelligence, presents unique challenges for machines in abstract thinking and logical reasoning. Recent large pre-trained language models such as GPT-3 have achieved remarkable progress on mathematical reasoning tasks written in text form, such as math word problems (MWP). However, it is unknown if the models can handle more complex problems that involve math reasoning over heterogeneous information, such as tabular data. To fill the gap, we present Tabular Math Word Problems (TabMWP), a new dataset containing 38,431 open-domain grade-level problems that require mathematical reasoning on both textual and tabular data. Each question in TabMWP is aligned with a tabular context, which is presented as an image, semi-structured text, and a structured table. There are two types of questions: free-text and multi-choice, and each problem is annotated with gold solutions to reveal the multi-step reasoning process. We evaluate different pre-trained models on TabMWP, including the GPT-3 model in a few-shot setting. As earlier studies suggest, since few-shot GPT-3 relies on the selection of in-context examples, its performance is unstable and can degrade to near chance. The unstable issue is more severe when handling complex problems like TabMWP. To mitigate this, we further propose a novel approach, PromptPG, which utilizes policy gradient to learn to select in-context examples from a small amount of training data and then constructs the corresponding prompt for the test example. Experimental results show that our method outperforms the best baseline by 5.31% on the accuracy metric and reduces the prediction variance significantly compared to random selection, which verifies its effectiveness in selecting in-context examples.


---


###


Summary Notes


##


Enhancing AI in Math Reasoning with Dynamic Prompt Learning


The challenge of teaching machines to understand and solve complex math problems, especially when data comes in mixed formats like text and tables, is a significant one in artificial intelligence (AI).


The development of the Tabular Math Word Problems (TabMWP) dataset is a big step forward. It's designed to test AI's ability to work with this kind of semi-structured data. This post explores the TabMWP dataset and a new method called dynamic prompting via policy gradient, aimed at improving how machines tackle these problems.


###


The TabMWP Dataset: A Closer Look


The TabMWP dataset is a key part of this research, offering a range of problems that blend text and tables and require mathematical reasoning to solve. Here’s why it’s noteworthy:


- **Task Design** : Every problem pairs a semi-structured table with a question, demanding a deep understanding and multiple steps to find the right answer.


- **Dataset Details** : Built with diversity in mind, it pulls from various sources and is annotated with detailed solutions, shedding light on the required reasoning.


- **Volume and Variety** : With 38,431 problems across different sets, it covers numerous question types and complexity levels, providing a solid base for AI training and testing.


###


Methods for Improvement


Researchers proposed two main methods to address the dataset's challenges:


- **Few-Shot GPT-3** : Uses GPT-3's ability to learn from a few examples to predict answers for new problems.


- **Dynamic Prompting via Policy Gradient (PROMPT PG)** : This new method uses a policy gradient strategy to dynamically choose the best in-context examples for the task, aiming to improve accuracy and model stability.


###


Experiment Results


The study tested these methods against standard models through comprehensive evaluations, focusing on accuracy.


The key finding was that PROMPT PG notably outperformed all baseline methods, proving the value of the policy gradient method in dealing with complex, semi-structured data.


###


Broader Context


This research adds to the ongoing efforts in AI to enhance mathematical reasoning and semi-structured data processing. It addresses current limitations and sets a new benchmark for future studies in these areas.


###


Conclusion


Introducing the TabMWP dataset and the dynamic prompt learning method via policy gradient represents a significant leap in AI and machine learning, particularly in solving complex reasoning tasks.


This approach of selecting optimal in-context examples can considerably boost language model performance, offering new directions for AI evolution.


###


Acknowledgments


This achievement is the result of collaboration among various academic and research entities, complemented by expert feedback.


It highlights the importance of collective effort in advancing AI capabilities.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
