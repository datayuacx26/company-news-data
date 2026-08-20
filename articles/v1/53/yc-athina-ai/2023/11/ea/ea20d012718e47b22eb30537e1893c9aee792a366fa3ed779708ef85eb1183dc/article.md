---
schema_version: "1.0.0"
document_id: "ea20d012718e47b22eb30537e1893c9aee792a366fa3ed779708ef85eb1183dc"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/large-language-models-and-prompt-engineering-for-biomedical-query-focused-multi-document-summarisation"
published_at: "2023-11-09T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:22.552077+00:00"
content_hash: "sha256:cc5aba8caacbf59d35f203a74634219b8899bcac3a7526525214f324853447bf"
---

# Large Language Models and Prompt Engineering for Biomedical Query Focused Multi-Document Summarisation

Do not index


Original Paper


[https://arxiv.org/abs/2312.04344](https://arxiv.org/abs/2312.04344)


Blog URL


[blog.athina.ai /large-l...risation](https://blog.athina.ai/large-language-models-and-prompt-engineering-for-biomedical-query-focused-multi-document-summarisation)


**Original Paper:**[https://arxiv.org/abs/2312.04344](https://arxiv.org/abs/2312.04344)


**By:**[Diego Mollá](https://arxiv.org/search/cs?searchtype=author&query=Moll%C3%A1%2C%20D)


**Abstract:**


> This paper reports on the use of prompt engineering and GPT-3.5 for biomedical query-focused multi-document summarisation. Using GPT-3.5 and appropriate prompts, our system achieves top ROUGE-F1 results in the task of obtaining short-paragraph-sized answers to biomedical questions in the 2023 BioASQ Challenge (BioASQ 11b). This paper confirms what has been observed in other domains: 1) Prompts that incorporated few-shot samples generally improved on their counterpart zero-shot variants; 2) The largest improvement was achieved by retrieval augmented generation. The fact that these prompts allow our top runs to rank within the top two runs of BioASQ 11b demonstrate the power of using adequate prompts for Large Language Models in general, and GPT-3.5 in particular, for query-focused summarisation.


---


###


Summary Notes


####


Blog Post: Enhancing Biomedical Summarization with Large Language Models


The field of Natural Language Processing (NLP) has seen remarkable advancements with the development of Large Language Models (LLMs) like GPT-3.5. These models have transformed text generation tasks in various domains.


However, the specialized field of biomedicine presents unique challenges for these models, especially when it comes to generating accurate and relevant summaries from multiple documents in response to specific queries.


This post looks into how prompt engineering can improve the use of LLMs for biomedical summarization, inspired by the latest research, including the 2023 BioASQ Challenge findings.


###


Understanding the Challenge


Biomedical summarization requires generating brief, accurate summaries from multiple documents based on a specific query.


The complexity of biomedical literature, filled with domain-specific jargon and detailed information, makes this task particularly challenging for LLMs.


These models often struggle to produce precise and contextually appropriate content and may generate irrelevant or incorrect information, which is a significant problem in a field where accuracy is crucial.


###


The Role of Prompt Engineering


Prompt engineering offers a strategic way to enhance LLM performance in specialized domains like biomedicine.


By carefully crafting the prompts given to these models, we can steer them towards producing more accurate and relevant text. There are two main types of prompts used:


- **Context-less prompts:** Include zero-shot prompts, with no prior examples, and few-shot prompts, which provide a few guiding examples.


- **Contextual prompts:** Incorporate relevant text snippets or summaries into the query, giving specific context to the model.


####


Insights from the BioASQ Challenge


The BioASQ Challenge provides valuable insights into the effectiveness of different prompting strategies. Research on the 2023 BioASQ 11b dataset shows that few-shot prompts generally perform better than zero-shot ones.


Notably, prompts that use retrieval-augmented generation (RAG), integrating relevant text snippets, significantly enhance the model's ability to generate relevant and accurate summaries.


###


Practical Implications


For AI engineers in enterprise companies, adopting a prompt engineering strategy, particularly with retrieval-augmented prompts, can greatly improve the quality of LLM-generated content in biomedicine.


This approach not only enhances information extraction and summarization tasks but also minimizes the risk of spreading incorrect information.


####


Key Takeaways:


- **Retrieval-Augmented Generation:** Using relevant text snippets in prompts significantly boosts model performance by providing necessary context.


- **Simplicity Over Complexity:** Simple, contextually supported prompts can outperform complex models without such context, emphasizing the importance of well-designed prompts.


- **Need for Ongoing Research:** While promising, further research is essential for creating automated systems to efficiently integrate relevant information into prompts.


###


Ethical Considerations and Limitations


Deploying these models requires careful consideration of ethical issues and limitations. The effectiveness of curated snippets for enhancing model accuracy raises questions about scalability and real-world applicability.


Ensuring the accuracy and relevance of generated content is crucial to prevent the spread of misinformation.


###


Conclusion


Advancements in LLMs for biomedical summarization through strategic prompt engineering represent a significant step forward in applying AI to domain-specific tasks. Continued exploration and refinement of these methods hold the potential to revolutionize biomedical research and healthcare.


However, navigating the technical and ethical challenges of AI deployment is essential for success. For AI professionals, leveraging prompt engineering and retrieval-augmented strategies offers an exciting path to achieving new levels of performance and accuracy in AI-generated biomedical content.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
