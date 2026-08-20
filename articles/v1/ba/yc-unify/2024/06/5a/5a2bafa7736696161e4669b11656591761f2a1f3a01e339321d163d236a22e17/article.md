---
schema_version: "1.0.0"
document_id: "5a2bafa7736696161e4669b11656591761f2a1f3a01e339321d163d236a22e17"
company_key: "yc-unify"
company: "Unify"
source_id: "yc-unify-rss-3d614756e1ae"
canonical_url: "https://unifyai.substack.com/p/the-deep-dive-issue-31"
published_at: "2024-06-27T17:08:08+00:00"
first_seen_at: "2026-07-24T05:11:59.311365+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:9941e57d84a2ea3c69e363e881e019ab491c11c4f3bae778c2ba533408360167"
---

# The Deep Dive Issue #31

# The Deep Dive Issue #31


### RAGFlow, multi-step reasoning, and accelerated AI infrastructure.


[Nassim](https://substack.com/@nas07)


Jun 27, 2024


The LLM king is dead, long live the LLM king. Claude-3.5 is now said to outrank GPT-4o on most tasks, at a fraction of the original cost. Better and cheaper models is now a familiar tune though. What’s probably more interesting is the introduction of “artifacts” that lets you run and visualize generated or content in-place.


This may not make API power-users bat an eye, but it’s yet another step that brings LLMs closer to the general public. Importantly, it gives a nice preview of how LLMs can be fully integrated into all sorts of interfaces to make edits on the fly. Eventually, we may end up not needing coding environments at all at some point. No more heated debates over the best IDE when that comes, might not be too bad of a change for once.


## Overflowing information


Retrieval Augmented Generation (RAG) is a powerful approach to query documents using LLMs. Building efficient RAG pipelines tailored to one’s documents remains tricky given the complexity of RAG tools. RAGFlow offers a streamlined RAG workflow that facilitates the use of LLMs to build adaptable RAG systems.


**Why would you care? -** RAGFlow lets you query your documents with an intuitive and modular interface that lets you quickly configure the embedding models, visualize and explore key references, and support all sorts of document formats.


RagFlow system architecture. Source: Repository


[README](https://github.com/infiniflow/ragflow?tab=readme-ov-file#-system-architecture)


**How does it work? -** RAGFlow’s backend exposes straightforward APIs to process documents, rank them for relevance, and query LLMs across selected sources. The information retrieval process is based on deep-document understanding to extract information from unstructured data.


Deep-document understanding uses


*Deep* Doc, a system designed to analyze documents from various domains and formats. It uses vision and parser components to achieve this. The vision component offers OCR for text extraction, layout recognition for identifying document structures like titles, figures, and tables, and Table Structure Recognition (TSR) for analyzing complex table layouts.


The parser component handles various document formats like PDF, DOCX, EXCEL, and PPT, providing structured data from text, tables, and figures.


*Deep* Doc's ability to handle diverse document formats and analyze their structure makes it a valuable tool for document processing and retrieval.


Check out the


[repository](https://github.com/infiniflow/ragflow) to get started.


[Share](https://unifyai.substack.com/p/the-deep-dive-issue-31?utm_source=substack&utm_medium=email&utm_content=share&action=share)


## The Lab


#### Preliminary thoughts


LLMs can be slow to respond when dealing with complex tasks that involve multi-step reasoning. This slow response time makes interacting with them feel sluggish and limits their usefulness in real-time applications.


[LiveMind](https://arxiv.org/pdf/2406.14319) is a new framework for simultaneous inference that proposes to address this latency issue. LiveMind allows LLMs to start working on a problem even before the user has finished typing the entire prompt. It breaks down the prompt into sentences and analyzes each sentence as it becomes available. This early analysis allows the LLM to perform preliminary reasoning and store the results. When the user finishes typing the prompt, the LLM combines these preliminary results with the complete prompt to quickly generate the final answer. This approach reduces the amount of work the LLM needs to do after the prompt is sent, significantly decreasing the perceived latency. Additionally, LiveMind can use a powerful LLM for the initial analysis and a smaller, faster LLM for the final answer generation. This collaborative approach further speeds up the process while maintaining good accuracy.


LiveMind achieves an average 59% reduction in response latency on the MMLU-Pro dataset compared to traditional methods, while collaborative inference, using a large LLM for analysis and a small LLM for output, further reduces latency by an average of 68% and improves accuracy by 5.5%.


#### Instructions curriculum


[Instruction Pre-Training](https://arxiv.org/pdf/2406.14491) is a new method developed to improve training of language models focused on instructions-following.


Instead of feeding raw text directly to the language model, this approach enriches the data by adding instructions and their corresponding answers. To achieve this, the method uses an instruction synthesizer tool that analyzes existing datasets containing various tasks and their solutions. By learning from these examples, the synthesizer can generate new instructions and answers based on any given text. The language model then learns from this enhanced dataset, containing both the original text and the added instructions with answers. This process allows the language model to better understand how to interpret instructions and solve tasks in a variety of formats.


This new pre-training method helps create language models that are significantly better at understanding and following instructions, leading to improved performance on a wide range of tasks.


#### Mindmapping


Problems that require many steps of thinking make LLMs more prone to incorrect deductions. To improve multi-step reasoning of LLMs,


[Q*](https://arxiv.org/pdf/2406.14283) guides the models’ reasoning using a search algorithm.


Q* treats the reasoning process as a sequence of steps. Each step represents a point on a map, and the goal is to find the most promising path to the correct answer. To do this, Q* uses a special function called a Q-value model, which predicts how good a step is based on how likely it is to lead to the right answer. This model is trained separately using a large set of example problems and their solutions. During problem-solving, for each step, Q* asks the LLM for several possible next steps. Then, it uses the Q-value model to estimate how promising each of these steps is. This helps Q* guide the LLM towards the most promising path, avoiding dead ends and ultimately finding the correct answer.


Q* significantly improves the accuracy of existing large language models on complex reasoning tasks such as math problem-solving and code generation.


[Share The Deep Dive](https://unifyai.substack.com/?utm_source=substack&utm_medium=email&utm_content=share&action=share)


## The Pulse


**Master of one -** Etched, a startup specializing in transformer-focused chips, has launched


[Sohu](https://www.etched.com/announcing-etched) , an ASIC designed specifically for AI LLM inference. Sohu claims to outperform Nvidia's H100 in performance, with a single 8xSohu server equaling the power of 160 H100 GPUs. By dedicating its hardware solely to transformer architecture, Sohu can allocate more transistors to AI compute, resulting in increased efficiency and speed.


**AI hyper veins -** Intel has unveiled its first


[fully integrated optical compute interconnect (OCI)](https://www.intel.com/content/www/us/en/newsroom/news/intel-unveils-first-integrated-optical-io-chiplet.html) chiplet, a significant advancement in high-speed data transfer technology. Intel's OCI chiplet, capable of transmitting data at 32 gigabytes per second over 100 meters of fiber optic cable, promises to revolutionize high-performance AI infrastructure by offering improved bandwidth, reduced power consumption, and increased reach.


**Text-to-science -** Meta's FAIR team is releasing


[several new AI models and tools for researchers](https://x.com/AIatMeta/status/1803107817345393136) , including JASCO, an audio generation model that uses text prompts and audio inputs to create music, and AudioSeal, a watermarking tool that identifies AI-generated speech. They are also releasing two sizes of their mixed-model input model, Chameleon, as well as multi-token prediction models.


That’s all for this edition, we hope you enjoyed reading through!


The Unify dev team.
