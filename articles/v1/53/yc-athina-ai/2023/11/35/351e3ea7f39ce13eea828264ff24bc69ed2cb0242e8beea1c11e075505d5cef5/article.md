---
schema_version: "1.0.0"
document_id: "351e3ea7f39ce13eea828264ff24bc69ed2cb0242e8beea1c11e075505d5cef5"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/semi-structured-chain-of-thought-integrating-multiple-sources-of-knowledge-for-improved-language-model-reasoning"
published_at: "2023-11-13T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:29:09.893530+00:00"
content_hash: "sha256:2d6871faea7430f660e3bbfc050b3976513cc6aa580b771879d54ac51a35b272"
---

# Semi-Structured Chain-of-Thought: Integrating Multiple Sources of Knowledge for Improved Language Model Reasoning

Do not index


Original Paper


[https://arxiv.org/abs/2311.08505](https://arxiv.org/abs/2311.08505)


Blog URL


[blog.athina.ai /semi-st...easoning](https://blog.athina.ai/semi-structured-chain-of-thought-integrating-multiple-sources-of-knowledge-for-improved-language-model-reasoning)


**Original Paper:**[https://arxiv.org/abs/2311.08505](https://arxiv.org/abs/2311.08505)


**By:**[Xin Su](https://arxiv.org/search/cs?searchtype=author&query=Su%2C%20X) ,[Tiep Le](https://arxiv.org/search/cs?searchtype=author&query=Le%2C%20T) ,[Steven Bethard](https://arxiv.org/search/cs?searchtype=author&query=Bethard%2C%20S) ,[Phillip Howard](https://arxiv.org/search/cs?searchtype=author&query=Howard%2C%20P)


**Abstract:**


> An important open question in the use of large language models for knowledge-intensive tasks is how to effectively integrate knowledge from three sources: the model's parametric memory, external structured knowledge, and external unstructured knowledge. Most existing prompting methods either rely on one or two of these sources, or require repeatedly invoking large language models to generate similar or identical content. In this work, we overcome these limitations by introducing a novel semi-structured prompting approach that seamlessly integrates the model's parametric memory with unstructured knowledge from text documents and structured knowledge from knowledge graphs. Experimental results on open-domain multi-hop question answering datasets demonstrate that our prompting method significantly surpasses existing techniques, even exceeding those that require fine-tuning.


---


###


Summary Notes


####


Enhancing LLM Reasoning with the Semi-CoT Approach


The field of artificial intelligence (AI) has seen significant advancements with large language models (LLMs) leading the charge in natural language processing tasks.


Despite their progress, LLMs often face challenges with accuracy and hallucinations. Traditional solutions have partially addressed these issues, but a new method, the Semi-Structured Chain-of-Thought (Semi-CoT) approach, offers a promising way forward by integrating various knowledge sources more effectively.


####


Key Insights of Semi-CoT


The Semi-CoT approach boosts LLM reasoning by blending the model's internal knowledge, external databases, and unstructured data.


This method excels in breaking down complex questions into a structured process that smartly incorporates diverse information:


- **Parsing Questions** : Transforms questions into a structured format, leaving placeholders for specific data.


- **Incorporating External Knowledge** : Uses external tools to fill in these placeholders with relevant information from both structured and unstructured sources.


- **Finalizing with LLMs** : Fills any gaps using LLMs, utilizing their extensive knowledge and understanding.


####


Benefits of Semi-CoT


- **Efficiency** : Directly targets knowledge gaps, reducing unnecessary computations.


- **Integration** : Seamlessly combines different knowledge types, optimizing information use.


####


Contributions


- Introduces an efficient method for blending multiple knowledge sources during inference.


- Delivers top results on complex question-answering benchmarks.


- Offers open access to the developed code to encourage further research.


####


Methodology and Experiments


Focusing on multi-hop question answering, Semi-CoT parses questions into semi-structured chains and enriches them with data from various sources, including structured knowledge graphs and the LLM’s internal database:


- **Evaluation** : Conducted on the 2WikiMultihopQA, MuSiQue-Ans, and Bamboogle datasets using LLAMA 2 models.


- **Results** : Demonstrated superior performance, showcasing the value of structured integration of multiple knowledge sources.


####


Future Directions and Limitations


The next steps involve refining the parsing process and improving knowledge retrieval accuracy. However, the current focus on open-source LLAMA models and potential biases from Wikipedia-based sources are noted limitations.


####


Conclusion


The Semi-Structured Chain-of-Thought approach marks a significant leap in LLM reasoning, offering a sophisticated method for integrating diverse knowledge sources.


This not only enhances LLMs' performance in complex tasks but also paves the way for future advancements in AI, promising more accurate and efficient natural language processing technologies.
