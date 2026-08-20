---
schema_version: "1.0.0"
document_id: "4f342c7ac0c660739c3835bf156f99093e17496e71da915198d86441b7df9923"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/explaining-emergent-in-context-learning-as-kernel-regression"
published_at: "2023-05-22T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:26.378360+00:00"
content_hash: "sha256:17c57b1660dae0af54139fa60278b9f95ddfcad8184f50b8f9b25610cd91932b"
---

# Explaining Emergent In-Context Learning as Kernel Regression

Do not index


Original Paper


[https://arxiv.org/abs/2305.12766](https://arxiv.org/abs/2305.12766)


Blog URL


[blog.athina.ai /explain...gression](https://blog.athina.ai/explaining-emergent-in-context-learning-as-kernel-regression)


**Original Paper:**[https://arxiv.org/abs/2305.12766](https://arxiv.org/abs/2305.12766)


**By:**[Chi Han](https://arxiv.org/search/cs?searchtype=author&query=Han%2C%20C) ,[Ziqi Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang%2C%20Z) ,[Han Zhao](https://arxiv.org/search/cs?searchtype=author&query=Zhao%2C%20H) ,[Heng Ji](https://arxiv.org/search/cs?searchtype=author&query=Ji%2C%20H)


**Abstract:**


> Large language models (LLMs) have initiated a paradigm shift in transfer learning. In contrast to the classic pretraining-then-finetuning procedure, in order to use LLMs for downstream prediction tasks, one only needs to provide a few demonstrations, known as in-context examples, without adding more or updating existing model parameters. This in-context learning (ICL) capability of LLMs is intriguing, and it is not yet fully understood how pretrained LLMs acquire such capabilities. In this paper, we investigate the reason why a transformer-based language model can accomplish in-context learning after pre-training on a general language corpus by proposing one hypothesis that LLMs can simulate kernel regression with internal representations when faced with in-context examples. More concretely, we first prove that Bayesian inference on in-context prompts can be asymptotically understood as kernel regression
>
>
> y^=∑iyiK(x,xi)/∑iK(x,xi) as the number of in-context demonstrations grows. Then, we empirically investigate the in-context behaviors of language models. We find that during ICL, the attention and hidden features in LLMs match the behaviors of a kernel regression. Finally, our theory provides insights into multiple phenomena observed in the ICL field: why retrieving demonstrative samples similar to test samples can help, why ICL performance is sensitive to the output formats, and why ICL accuracy benefits from selecting in-distribution and representative samples.


---


###


Summary Notes


####


Simplified Blog Post: Understanding How Large Language Models Learn New Tasks


###


Introduction


In the world of Natural Language Processing (NLP), Large Language Models (LLMs) like GPT-3 have revolutionized how machines understand and generate human language.


A key feature of these models is their ability to learn new tasks from a few examples, known as In-Context Learning (ICL). This post explores how LLMs might be using a method similar to kernel regression to achieve this adaptability.


###


Background on In-Context Learning


####


What is In-Context Learning?


ICL is a process that allows LLMs to take a handful of examples included in their input and quickly learn from them to perform new tasks without needing to be retrained.


####


Theories Behind ICL


Although researchers have suggested theories like Bayesian inference and gradient descent to explain ICL, these remain speculative.


LLMs, through extensive pre-training, develop abilities such as reasoning and understanding multiple languages, indicating they're capable of complex behaviors.


###


How LLMs Might Be Learning


We start with two assumptions:


- The data LLMs are trained on can be seen as a mix of models, each for a specific task.


- LLMs use special tokens to differentiate between tasks based on the examples provided.


####


Kernel Regression as an Explanation


Our theory suggests that as LLMs get more examples, their learning process resembles kernel regression. This means they calculate the output by taking a weighted sum of the outputs from similar examples, where weights depend on how similar each example is to the input being tested.


####


Key Takeaways


- The success of ICL heavily relies on how relevant and well-distributed the input examples are.


- The predicted output is greatly influenced by the examples' labels, emphasizing the need for consistency in the labels during training and testing.


###


What We Found Through Experiments


Our tests support the theory, showing that:


- LLMs focus on similar examples when making predictions, akin to the weighting in kernel regression.


- Certain parts of the LLM, specifically some layers and attention heads, play a significant role in this process.


###


Conclusion


Our investigation suggests that LLMs, like GPT-3, might be using a method similar to kernel regression to quickly learn new tasks from a few examples.


This understanding opens up new research opportunities, especially in improving how these models handle example ordering and respond to incorrect labels.


For AI Engineers in the industry, grasping these mechanisms is essential for leveraging LLMs to their full potential in various NLP applications, keeping them at the edge of innovation.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
