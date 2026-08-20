---
schema_version: "1.0.0"
document_id: "7229b2ef7b1342f4ad364da84693daf238547b73ab1291c3572f5117900a1047"
company_key: "yc-quivr"
company: "Quivr"
source_id: "yc-quivr-news-import-c3f3e0926723"
canonical_url: "https://www.quivr.com/blog/rag-finetuning-or-prompting-to-chat-with-your-knowledge"
published_at: null
first_seen_at: "2026-07-27T04:35:10.435547+00:00"
fetched_at: "2026-07-28T21:16:46.713740+00:00"
content_hash: "sha256:11e691a2ac0b9275f8ed5e7d616e0718165576ee2c84f53cdad45408577df418"
---

# The Current State of AI Language Models: RAG, Prompting, and Fine-Tuning

Feature


### RAG, Finetuning or Prompting to chat with your knowledge


# **The Current State of AI Language Models: RAG, Prompting, and Fine-Tuning**


You've probably heard about RAG, Prompting and Fine-Tuning a lot in the last year. You have seen hundreds of LinkedIn posts on how to build an external knowledge base chatbot, how to automate certain tasks. Each of them show-casing a certain way of doing it. But which one is the best for your use case. The goal of this article is to dive into what makes each of them interesting and when you should use it.


# **The Power of Prompting**


Prompting remains the foundation of working with language models. A well-crafted prompt is essential for achieving desired results, regardless of the approach used. For low-volume tasks, relying solely on prompts is often the most efficient solution. This approach allows for easy upgrades to newer models as they become available, potentially improving performance or reducing costs. It is however worth noting that prompt variance – the inconsistency in model outputs based on slight changes in prompts – is expected to decrease with future model generations. You currently tend to have prompts that work well with a model, but then changing for example between OpenAI and Anthropic reduces the quality of the answer. A well-rounded prompt can allow you to iterate quickly and deploy a proof-of-concept in a matter of minutes. There are a few things that you should however understand. LLMs have context windows — number of words the LLM can have as Input -- that differs from model to model. Some have 16k words or tokens and others up to 1.5M such as Google Gemini. Prompting is particularly efficient when you craft a prompt to work with new knowledge. For example, specific information that you gathered from a source that changes often. But, it comes at a price, you have to take the entire knowledge as input. It is usually very efficient for summarization or information extraction quickly. It is a concept you see on ChatGPT, where you explain to the model what you want and then copy-paste the knowledge. Prompt Engineering is a new field that recently made a bit of noise with high salaries, these experts can use techniques such as few shot prompting to create efficient prompt template that can be used anywhere. Prompt Engineering Techniques can be found all other the web, but the craft is still in the making as Generative AI became mainstream only on November 30th, 2023 with the release of GPT-3.5 from OpenAI. Now let's move to a more specific term that allows you to improve the efficiency of these language models. By giving them access to curated and specific data, reducing the context windows, increase the speed, reducing the computational resources required and improving the information retrieval.


# **Retrieval-Augmented Generation (RAG)**


Retrieval Augmented Generation or RAG is the concept of providing curated knowledge to a large language model via a concept of finding the relevant information and providing it to it. It allows you:


- Reduce the cost
- Reduce the latency
- Have more precise information RAG becomes necessary when dealing with:
- Frequently changing information
- Large corpora of data
- Third-party data sources This method involves:


1. Maintaining a quickly-accessible, preprocessed copy of the data
2. Implementing a system to select relevant information for the prompt Benefits of RAG include:


- Ability to provide citations for traceability
- Easy upgrades to new models
- Rapid adaptation to information changes RAG is particularly useful for applications involving ticketing systems, email management, customer support, personalization, and other communication platforms where up-to-date information is crucial. RAG is particularly efficient with new data and is the right approach when you have a specific task in mind that requires an optimization and retrieval of custom and external data. Retrieval Augmented Generation requires a bit more resources than just prompting. In order to do an efficient RAG, you need a vector database (vector store) that allows you to store the information for retrieval. The concept of storing new data in a RAG pipeline is simple:


1. Parse the new data to extract the knowledge from it
2. Embed this information with an embedding model — It creates a vector representation of your data
3. Store this embedding in the vector database Then when the user asks a question:
4. Embed the question of the user
5. Look at the closest chunks from the vector store to the embedded user question
6. Give these chunks as context to the LLM. ‍ With this technique, a knowledge base can be ingested and updated into a vector database very efficiently. Vector database can be simple PostgreSQL or Knowledge graph. The consensus is moving towards Knowledge Graph, however, as it allows you to do more things such as Entity extraction, metadata retrieval and optimize the exploration of the data. It is to this date the recommended system for efficiently answering questions. However, some people also envision using another system called fine-tuning to allow you to get a better answer. In the next part, we will look at Fine-tuning and what companies such as Hugging Face have done to help Generative AI in this field.


# **Fine-Tuning: Specialization and Efficiency**


Fine-tuning is the concept of taking a large language model and using a tuning process with training data or proprietary data to make is very efficient and good in a subject. Generative AI models are using huge models that are very expensive but have a high level of understanding and reasoning. Using them can be costly. Therefore, some people have turned to fine-tuning with their proprietary data sets to improve the efficiency and the cost. Fine-tuning is particularly useful for:


1. Achieving specific writing styles (e.g., mimicking a particular author's voice)
2. Optimizing high-volume, specific tasks Key points:


- Can make smaller models perform similarly to larger ones for specific tasks
- Potentially more cost-effective for high-volume applications (around 20M requests per month or less with GPU sharing)
- Challenges with GPU memory management when using shared resources However, fine-tuning has limitations:
- Less responsive to new information compared to RAG
- Unable to provide citations for its outputs
- Often requires combination with RAG for business applications


# **The Accuracy Dilemma**


Improving model accuracy through fine-tuning presents a challenging “valley of death” scenario:


- Existing models may not be accurate enough for deployment
- Without deployment, it's difficult to collect data for fine-tuning
- Paying for synthetic data creation may not provide a good return on investment, especially considering the rapid pace of model improvements Some big companies have fine-tuned their own models, but haven't seen awesome results. For now, fine-tuning should only be used if you know what you are doing, you are building something very specific, and you want the large language model to be very efficient in one task.


# **The Role of Model Size and Specialization**


An interesting observation is that a small, “dumb” model that has been fine-tuned can sometimes approximate the performance of a larger, more advanced model for specific tasks. This opens up possibilities for cost optimization in high-volume applications. However, the challenges of quickly loading fine-tuned models into GPU memory, especially in shared environments, can offset these potential savings for smaller use cases. Fine-tuned models tend to be more expensive to run because it requires GPU memory and can't be batched with other models. For example, OpenAI can have 1000s of GPUs running the same model and therefore can optimize based on the load. If you have a fine-tuned model but not enough users, you'll have to maintain one or multiple GPUs to host your model and that can be quite expensive, especially on services such as Hugging Face.


# **The Future of AI Language Models**


As the field rapidly evolves, several trends are emerging:


1. The potential for synthetic data to bridge the “valley of death” in fine-tuning
2. The impact of licensing restrictions on model selection and fine-tuning options
3. The ongoing improvements in base model capabilities, potentially reducing the need for specialized approaches Generative AI models are moving very quickly, each company is making significant improvements each week. The field has not yet stabilized and a lot of people are still moving at sight.


# **Conclusion**


The landscape of AI language models is complex and rapidly evolving. While RAG, prompting, and fine-tuning each have their place, the choice of approach depends heavily on the specific use case, volume of requests, and desired outcomes. As models continue to improve, the balance between these techniques may shift, potentially simplifying some aspects of AI deployment while introducing new challenges and opportunities. We recommend you to think a lot about what you want to do. If there is one thing you can take from this article, it is the following. **Iterate quickly:** Prompting **New or specific dataset per user:** RAG **Huge static dataset:** Fine-tuning At Quivr, we are leveraging Prompting and RAG to provide you with the best to answer your questions efficiently. After one year of building Quivr we are just looking at fine-tuning for some of our clients with huge data sets. Bear in mind that the field is evolving quickly and that these advices are only here to help you make an informed decision in this rapidly evolving space.
