---
schema_version: "1.0.0"
document_id: "b1ec8a89d061c901680520961a8dc09886e3917eaced68fc31ec92c738378813"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/docprompting-generating-code-by-retrieving-the-docs"
published_at: "2023-02-18T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:28.379811+00:00"
content_hash: "sha256:8ab47ba178147e5d9354be75e1382fc76b4679aa31d4a76c71ae2aac360bdd95"
---

# DocPrompting: Generating Code by Retrieving the Docs

Do not index


Original Paper


[https://arxiv.org/abs/2207.05987](https://arxiv.org/abs/2207.05987)


Blog URL


[blog.athina.ai /docprom...the-docs](https://blog.athina.ai/docprompting-generating-code-by-retrieving-the-docs)


**Original Paper:**[https://arxiv.org/abs/2207.05987](https://arxiv.org/abs/2207.05987)


**By:**[Shuyan Zhou](https://arxiv.org/search/cs?searchtype=author&query=Zhou%2C%20S) ,[Uri Alon](https://arxiv.org/search/cs?searchtype=author&query=Alon%2C%20U) ,[Frank F. Xu](https://arxiv.org/search/cs?searchtype=author&query=Xu%2C%20F%20F) ,[Zhiruo Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang%2C%20Z) ,[Zhengbao Jiang](https://arxiv.org/search/cs?searchtype=author&query=Jiang%2C%20Z) ,[Graham Neubig](https://arxiv.org/search/cs?searchtype=author&query=Neubig%2C%20G)


**Abstract:**


> Publicly available source-code libraries are continuously growing and changing. This makes it impossible for models of code to keep current with all available APIs by simply training these models on existing code repositories. Thus, existing models inherently cannot generalize to using unseen functions and libraries, because these would never appear in the training data. In contrast, when human programmers use functions and libraries for the first time, they frequently refer to textual resources such as code manuals and documentation, to explore and understand the available functionality. Inspired by this observation, we introduce DocPrompting: a natural-language-to-code generation approach that explicitly leverages documentation by (1) retrieving the relevant documentation pieces given an NL intent, and (2) generating code based on the NL intent and the retrieved documentation. DocPrompting is general: it can be applied to any programming language and is agnostic to the underlying neural model. We demonstrate that DocPrompting consistently improves NL-to-code models: DocPrompting improves strong base models such as CodeT5 by 2.85% in pass@1 (52% relative gain) and 4.39% in pass@10 (30% relative gain) in execution-based evaluation on the popular Python CoNaLa benchmark; on a new Bash dataset tldr, DocPrompting improves CodeT5 and GPT-Neo1.3B by up to absolute 6.9% exact match.


---


###


Summary Notes


##


Enhancing Automated Code Generation with Documentation


As AI technology continues to progress, generating code from natural language instructions has become a key focus for developers at major corporations.


Traditional automated code generation models are quite capable but often run into problems with new or unfamiliar library functions or APIs.


This challenge limits their effectiveness and application range. To overcome this, a new method called "DocPrompting" has been developed, which uses documentation to improve code generation significantly.


###


**How DocPrompting Works**


DocPrompting is based on a straightforward idea: since documentation provides detailed information on libraries and functions, it could be a powerful tool for code generation. The process involves two main steps:


- **Finding the Right Documentation** : The model looks through documentation to find sections that match the user's request in natural language.


- **Code Creation** : Using the information from both the user's request and the selected documentation, the model generates accurate and functional code.


This method uses the wealth of knowledge in documentation to fill the gap between vague user requests and the specific needs of programming languages.


###


**Implementing DocPrompting**


DocPrompting combines document retrieval and code generation:


- **Document Retrieval** : It uses both sparse (e.g., BM25) and dense (e.g., SimCSE) methods to find relevant documentation.


- **Code Generation** : Various models like GPT-Neo, T5, and CodeT5 are used to create code, with strategies adapted to each model.


Its effectiveness is proven through new benchmarks focused on how well it can generate code based on documentation, showing clear advantages over older models.


###


**Test Setup and Results**


Tests used include the tldr dataset for Bash and an updated CoNaLa dataset for Python, concentrating on how well the system works with new functions and libraries.


Metrics like exact match and token-level F1 score were used for evaluation.


Results show that DocPrompting significantly beats traditional models in various metrics, proving its success in using documentation to generate code.


###


**Why It's Successful**


DocPrompting's success lies in its mimicry of how humans code: by consulting documentation.


Documentation connects the dots between what the user wants and how to code it, providing necessary context and details for accurate code snippets.


###


**Looking Ahead**


DocPrompting marks a big step forward in automated code generation. Future work could improve document retrieval, expand the languages and libraries used, and adapt the method for other coding tasks. This opens up new research and development possibilities.


###


**Conclusion**


DocPrompting offers a powerful way to improve automated code generation by utilizing documentation, effectively narrowing the gap between natural language instructions and code requirements.


With potential for further advancements, this approach could make AI-driven coding tools even more effective and versatile.


####


Thanks


Special thanks to Amazon AI and the Air Force Research Laboratory for their support in developing DocPrompting.


This technique, supported by both practical insights and empirical data, is paving the way for advanced coding assistants capable of handling more complex programming tasks with higher efficiency and accuracy.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models


[https://arxiv.org/abs/2207.05987](https://arxiv.org/abs/2207.05987)
