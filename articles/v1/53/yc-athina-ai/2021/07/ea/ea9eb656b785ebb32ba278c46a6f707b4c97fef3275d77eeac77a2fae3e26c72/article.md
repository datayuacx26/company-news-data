---
schema_version: "1.0.0"
document_id: "ea9eb656b785ebb32ba278c46a6f707b4c97fef3275d77eeac77a2fae3e26c72"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/pre-train-prompt-and-predict-a-systematic-survey-of-prompting-methods-in-natural-language-processing"
published_at: "2021-07-28T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:36.656438+00:00"
content_hash: "sha256:dce77b3fea4ab4ae0721c72b49729fae10b7e145739b7ed1c1051ab1e6c68eb0"
---

# Pre-train, Prompt, and Predict: A Systematic Survey of Prompting Methods in Natural Language Processing

Do not index


Original Paper


[https://arxiv.org/abs/2107.13586](https://arxiv.org/abs/2107.13586)


Blog URL


[blog.athina.ai /pre-tra...ocessing](https://blog.athina.ai/pre-train-prompt-and-predict-a-systematic-survey-of-prompting-methods-in-natural-language-processing)


**Original Paper:**[https://arxiv.org/abs/2107.13586](https://arxiv.org/abs/2107.13586)


**By:**[Pengfei Liu](https://arxiv.org/search/cs?searchtype=author&query=Liu%2C%20P) ,[Weizhe Yuan](https://arxiv.org/search/cs?searchtype=author&query=Yuan%2C%20W) ,[Jinlan Fu](https://arxiv.org/search/cs?searchtype=author&query=Fu%2C%20J) ,[Zhengbao Jiang](https://arxiv.org/search/cs?searchtype=author&query=Jiang%2C%20Z) ,[Hiroaki Hayashi](https://arxiv.org/search/cs?searchtype=author&query=Hayashi%2C%20H) ,[Graham Neubig](https://arxiv.org/search/cs?searchtype=author&query=Neubig%2C%20G)


**Abstract:**


> This paper surveys and organizes research works in a new paradigm in natural language processing, which we dub "prompt-based learning". Unlike traditional supervised learning, which trains a model to take in an input x and predict an output y as P(y|x), prompt-based learning is based on language models that model the probability of text directly. To use these models to perform prediction tasks, the original input x is modified using a template into a textual string prompt x' that has some unfilled slots, and then the language model is used to probabilistically fill the unfilled information to obtain a final string x, from which the final output y can be derived. This framework is powerful and attractive for a number of reasons: it allows the language model to be pre-trained on massive amounts of raw text, and by defining a new prompting function the model is able to perform few-shot or even zero-shot learning, adapting to new scenarios with few or no labeled data. In this paper we introduce the basics of this promising paradigm, describe a unified set of mathematical notations that can cover a wide variety of existing work, and organize existing work along several dimensions, e.g.the choice of pre-trained models, prompts, and tuning strategies. To make the field more accessible to interested beginners, we not only make a systematic review of existing works and a highly structured typology of prompt-based concepts, but also release other resources, e.g., a website
>
>
> [this http URL](http://pretrain.nlpedia.ai/)


---


###


Summary Notes


####


Blog Post: Understanding Prompt-Based Learning in NLP


Prompt-based learning is transforming the field of natural language processing (NLP) by utilizing pre-trained language models for various tasks.


This innovative approach allows AI engineers to adapt these models for specific needs efficiently, without the need for extensive training data.


This blog post breaks down the essentials of prompt-based learning, covering its key concepts, methods, applications, and future considerations.


###


Key Concepts


####


What is Prompt-Based Learning?


Prompt-based learning modifies input data into a format recognizable by pre-trained language models through textual prompts. These prompts serve as guides, enabling the model to generate predictions. This method capitalizes on the model's pre-learned knowledge for task adaptation.


####


The Role of Pre-Trained Language Models


Central to prompt-based learning are pre-trained language models, which are developed by training on large datasets to grasp general language features. These models, based on their training goals like autoregressive or denoising, play various roles in prompt-based learning.


###


Prompting Techniques


####


Crafting Prompts


- **Prompt Engineering:** Involves creating effective prompts manually or through automated systems to direct the model's output accurately.


- **Prompt Varieties:** Prompts can be simple text strings or complex embeddings, influencing the model's prediction direction.


####


Designing Answers


- **Answer Space Optimization:** Adjusting the answer space is crucial for aligning model outputs with task-specific requirements, through both manual and automated approaches.


####


Leveraging Multiple Prompts


Using several prompts or enhancing prompts with examples can improve model accuracy. Strategies explored include prompt ensembling and composition.


###


Practical Applications


Prompt-based learning is versatile, with applications including:


- **Knowledge Probing:** Examining models for factual and linguistic knowledge.


- **Text Classification & Inference:** Transforming classification tasks to fit prompt-based models.


- **Information Extraction:** Customizing prompts to identify relationships and entities in texts.


- **Question Answering & Text Generation:** Enhancing QA tasks and guiding text generation for summarization and translation.


###


Facing the Challenges


While promising, prompt-based learning encounters hurdles:


- **Effective Prompt Creation:** Crafting prompts that fit a wide range of tasks is challenging.


- **Optimizing Answers:** Finding the best answers, especially for complex tasks, requires further exploration.


- **Training Method Selection:** Understanding the impact of different training strategies is vital.


- **Ensuring Transferability:** Making prompts work across various models and tasks is essential for widespread application.


###


Conclusion


Prompt-based learning marks a significant step forward in NLP, providing a flexible and potent means of employing pre-trained language models for specific tasks.


Despite existing challenges, its potential to streamline and enhance NLP applications is substantial.


Continued research and experimentation in this area are expected to open new possibilities and deepen our comprehension of natural language processing.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
