---
schema_version: "1.0.0"
document_id: "66b2cebd6103910454b8dcaef8db0f7ddab7a5d6c45645145518302de9f8af69"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/successive-prompting-for-decomposing-complex-questions"
published_at: "2022-12-08T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:29.187426+00:00"
content_hash: "sha256:126b544a5d18025139d245fe5126764d15bc6a8c73492fcff0d5ee66e929cad9"
---

# Successive Prompting for Decomposing Complex Questions

Do not index


Original Paper


[https://arxiv.org/abs/2212.04092](https://arxiv.org/abs/2212.04092)


Blog URL


[blog.athina.ai /success...uestions](https://blog.athina.ai/successive-prompting-for-decomposing-complex-questions)


**Original Paper:**[https://arxiv.org/abs/2212.04092](https://arxiv.org/abs/2212.04092)


**By:**[Dheeru Dua](https://arxiv.org/search/cs?searchtype=author&query=Dua%2C%20D) ,[Shivanshu Gupta](https://arxiv.org/search/cs?searchtype=author&query=Gupta%2C%20S) ,[Sameer Singh](https://arxiv.org/search/cs?searchtype=author&query=Singh%2C%20S) ,[Matt Gardner](https://arxiv.org/search/cs?searchtype=author&query=Gardner%2C%20M)


**Abstract:**


> Answering complex questions that require making latent decisions is a challenging task, especially when limited supervision is available. Recent works leverage the capabilities of large language models (LMs) to perform complex question answering in a few-shot setting by demonstrating how to output intermediate rationalizations while solving the complex question in a single pass. We introduce \`\`Successive Prompting'', where we iteratively break down a complex task into a simple task, solve it, and then repeat the process until we get the final solution. Successive prompting decouples the supervision for decomposing complex questions from the supervision for answering simple questions, allowing us to (1) have multiple opportunities to query in-context examples at each reasoning step (2) learn question decomposition separately from question answering, including using synthetic data, and (3) use bespoke (fine-tuned) components for reasoning steps where a large LM does not perform well. The intermediate supervision is typically manually written, which can be expensive to collect. We introduce a way to generate a synthetic dataset which can be used to bootstrap a model's ability to decompose and answer intermediate questions. Our best model (with successive prompting) achieves an improvement of ~5% absolute F1 on a few-shot version of the DROP dataset when compared with a state-of-the-art model with the same supervision.


---


###


Summary Notes


##


Simplifying Complex Questions with Successive Prompting: A New Approach in AI


The field of artificial intelligence (AI) and natural language processing is always evolving, with a key challenge being the ability to accurately answer complex questions.


Traditional methods often fall short due to the complexity of these questions. However, a new method called "Successive Prompting" is making significant strides, particularly beneficial for AI engineers at enterprise companies aiming to improve their systems.


###


Understanding the Complexity


Complex questions are tricky because they involve multiple data points and layers of reasoning. Typically, large language models (LMs) try to answer these by generating intermediate steps in one go, which can be inefficient.


Successive Prompting changes the game by breaking down complex questions into easier sub-tasks, making them more manageable for AI systems.


###


How Successive Prompting Works


Successive Prompting transforms how AI systems tackle complex questions by:


- **Breaking down questions** into simpler parts, treating each as a separate query-answer situation.


- **Updating the context** with intermediate outcomes to better deal with complex dependencies.


- **Using synthetic data** to improve the model's learning, especially for new types of reasoning.


Tests on the DROP dataset have shown this method's potential to greatly improve question-answering abilities.


###


Training AI Models


When training AI models with Successive Prompting, there are several strategies:


- **In-context learning** , which includes examples directly in the prompt.


- **Fine-tuning** the models for specific reasoning steps.


- Creating **synthetic data** for more complex and varied training examples.


This flexible approach lets AI engineers customize their systems for better performance in answering complex questions.


###


Testing and Results


The DROP dataset was used to test Successive Prompting, using a few-shot learning approach with 300 manually annotated examples. The use of synthetic data, derived from Wikipedia tables, was key to its success. Results showed that this method outperformed traditional models, especially when synthetic data and fine-tuning were applied.


###


Conclusion


Successive Prompting is a big step forward in natural language processing, especially for answering complex questions.


By breaking down questions into simpler parts, this method not only boosts accuracy and clarity in responses but also allows for versatile training and application.


For AI engineers at enterprise companies, adopting Successive Prompting could be a game-changer, leading to more effective AI solutions.


As we push the boundaries of AI capabilities, Successive Prompting shines as a key innovation, paving the way for smarter, more adaptable AI systems.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
