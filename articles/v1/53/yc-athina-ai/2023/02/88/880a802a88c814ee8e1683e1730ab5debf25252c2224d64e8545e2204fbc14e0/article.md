---
schema_version: "1.0.0"
document_id: "880a802a88c814ee8e1683e1730ab5debf25252c2224d64e8545e2204fbc14e0"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/scalable-prompt-generation-for-semi-supervised-learning-with-language-models"
published_at: "2023-02-18T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:29:11.084734+00:00"
content_hash: "sha256:92bfb99ae802654e9d6f7058ac89389f8d827f7751771e9714724d83a75b87cc"
---

# Scalable Prompt Generation for Semi-supervised Learning with Language Models

Do not index


Original Paper


[https://arxiv.org/abs/2302.09236](https://arxiv.org/abs/2302.09236)


Blog URL


[blog.athina.ai /scalabl...e-models](https://blog.athina.ai/scalable-prompt-generation-for-semi-supervised-learning-with-language-models)


**Original Paper:**[https://arxiv.org/abs/2302.09236](https://arxiv.org/abs/2302.09236)


**By:**[Yuhang Zhou](https://arxiv.org/search/cs?searchtype=author&query=Zhou%2C%20Y) ,[Suraj Maharjan](https://arxiv.org/search/cs?searchtype=author&query=Maharjan%2C%20S) ,[Beiye Liu](https://arxiv.org/search/cs?searchtype=author&query=Liu%2C%20B)


**Abstract:**


> Prompt-based learning methods in semi-supervised learning (SSL) settings have been shown to be effective on multiple natural language understanding (NLU) datasets and tasks in the literature. However, manually designing multiple prompts and verbalizers requires domain knowledge and human effort, making it difficult and expensive to scale across different datasets. In this paper, we propose two methods to automatically design multiple prompts and integrate automatic verbalizer in SSL settings without sacrificing performance. The first method uses various demonstration examples with learnable continuous prompt tokens to create diverse prompt models. The second method uses a varying number of soft prompt tokens to encourage language models to learn different prompts. For the verbalizer, we use the prototypical verbalizer to replace the manual one. In summary, we obtained the best average accuracy of 73.2% (a relative improvement of 2.52% over even the previous state-of-the-art SSL method with manual prompts and verbalizers) in different few-shot learning settings.


---


###


Summary Notes


##


Making Semi-supervised Learning Easier with Automated Prompts


In the field of natural language understanding (NLU), using large pre-trained language models (PLMs) has become common.


These models are great but need fine-tuning for specific tasks, which can be hard and time-consuming.


This is especially true in semi-supervised learning (SSL), where designing the right prompts and verbalizers is essential for good results.


###


The Problem with Manual Prompts in SSL


Semi-supervised learning is useful because it allows us to use a lot of unlabeled data. One method, called Pattern-exploiting Training (PET), uses manually created prompts to help with this. But, making these prompts by hand is hard for several reasons:


- **Expert Knowledge Needed** : You need to really understand the domain and the model.


- **Time-Consuming** : It takes a lot of work, which gets worse as you have more data or more complex tasks.


- **Inflexible** : Once you make a prompt, changing it for new tasks or domains isn't easy.


###


A New Solution: Automated Prompts and Verbalizers


To solve these issues, a new framework has been developed that makes prompt and verbalizer creation automatic, reducing the need for manual work. It introduces two major innovations:


1. **Automatic Prompt Generation** : This creates diverse prompts automatically, making the process easier and more adaptable.


1. **Automatic Verbalizers** : This automatically links predicted tokens to class labels, avoiding manual mapping.


###


How It Works: Training and Testing


The framework uses a cross-entropy loss for training with the automatically generated prompts and fine-tunes the final classifier to make the best use of soft labels from unlabeled data.


###


Proof It Works: Testing the Framework


This new approach was tested on several datasets, including AG's News and Yahoo Answers. It did better than manual methods and matched or exceeded other top methods, showing that automated prompt and verbalizer design in SSL is possible and effective.


###


Limitations and What's Next


The main drawbacks are that it's only been tested in English and might require more computing power than some researchers have. Future work will look at how this can be used for other languages and in settings with less computing resources.


###


Conclusion


Automating prompt and verbalizer generation in SSL is a big step forward for NLU. It makes the process less dependent on manual work and expertise, and more scalable and adaptable.


There's still more to do, especially in making it work for more languages and reducing the need for high computing power, but this progress brings us closer to fully unlocking the potential of PLMs in NLU and beyond.


This development marks an exciting path towards easier and more efficient semi-supervised learning models, showing promise for broader applications and impacts in the future.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
