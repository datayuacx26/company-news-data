---
schema_version: "1.0.0"
document_id: "d541e3d8efbc8b816ffd84e2c92554a6495fcdec52de180175d1c8b91db4f0a3"
company_key: "yc-unify"
company: "Unify"
source_id: "yc-unify-rss-3d614756e1ae"
canonical_url: "https://unifyai.substack.com/p/the-deep-dive-issue-32"
published_at: "2024-07-04T17:25:17+00:00"
first_seen_at: "2026-07-24T05:11:59.311365+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:dab7defdc1ce04cbf83bbe61841179ffaa18b5aea33bfc5a4ac34f82938197a1"
---

# The Deep Dive Issue #32

# The Deep Dive Issue #32


### GPTCache, agent workflows, and new models


[Nassim](https://substack.com/@nas07)


Jul 04, 2024


As LLMs improve and make fewer mistakes, some argue our own role will just be to review the outputs and steer it with feedback. Here’s the catch though, if models continue to get smarter and process longer sequences, this also means errors will become harder and take longer to find, bummer.


[CriticGPT](https://openai.com/index/finding-gpt4s-mistakes-with-gpt-4/) lately showed how using GPT-4 to identify GPT-4’s errors (no biases here of course) helps human evaluators assess the accuracy of answers. This seems to be working well but also means that eventually, outsourcing part or all of the auditing process to other models could become a necessity.


The question is, how long until we get no choice but to accept the AI’s assessment if that too becomes difficult to sense-check. Remember last time you nervously nodded at someone when they asked if what they said made sense? At least we know how this will feel like.


## Not too different


Building on top of LLM APIs has become the standard approach to build and deploy LLM applications. As usage grows, this also translates into substantial increases in costs and latency when handling multiple requests. GPTCache infuses LLM APIs with semantic caching to improve the efficiency of LLM-based applications.


**Why would you care? -** If you’ve got scalability issues or just want to optimize applications that involve making lots of similar queries, GPTCache can quickly help minimize expenses and improve throughput.


Overview of the GPTCache architecture. Source:


[Documentation](https://gptcache.readthedocs.io/en/latest/#how-does-it-work)


**How does it work? -** Traditional cache systems use an exact match approach to determine if a requested piece of content is available in the cache. However, this approach is less effective for LLMs due to the variability of LLM queries. Instead, GPTCache uses semantic caching, which not only stores the query but also the underlying semantics, to increase the overall caching accuracy.


GPTCache employs embedding algorithms to convert queries into embeddings and uses a vector store for similarity search. This allows it to identify and retrieve similar or related queries from the cache storage. GPTCache also provides three metrics to gauge its performance: hit ratio, latency, and recall, which can be used to optimize the caching system.


The GPTCache architecture uses an:


-


**LLM Adapter** that integrates different LLMs by unifying request protocols


-


**Embedding Generator** that extracts embeddings for similarity search


-


**Cache Manager** that controls the operation of both Cache Storage and Vector Store, managing cache evictions and distributed caching


-


**Similarity Evaluator** that determines the similarity between the input request and cached requests, using various strategies like distance and model-based similarity


Check out the


[repository](https://github.com/zilliztech/GPTCache) to get started.


[Share](https://unifyai.substack.com/p/the-deep-dive-issue-32?utm_source=substack&utm_medium=email&utm_content=share&action=share)


## The Lab


#### Agent’s Log


Current LLMs are limited in their ability to automatically learn and improve from data after being deployed.


[Agent Symbolic Learning](https://arxiv.org/pdf/2406.18532) , is a framework that allows language agents to learn from data and evolve independently.


This framework draws parallels between language agents and how neural networks learn. An agent's workflow is divided into a series of steps, like a neural network's layers. Each step uses prompts and tools to process information. Agent Symbolic Learning mimics the learning process of neural networks by treating these prompts, tools, and the workflow itself as elements that can be optimized.


Throughout the process, the agent records its actions and outcomes in a trajectory. This trajectory is then assessed using a language-based loss function, which evaluates the agent's performance. Based on this evaluation, the framework generates textual feedback, highlighting areas for improvement in the agent's prompts, tools, and workflow. Finally, dedicated optimizers use this feedback to fine-tune the agent's components, enabling it to learn from experience and improve over time.


Agent Symbolic Learning, enables language agents to automatically refine their prompts, tools, and overall workflow through experience, resulting in self-evolving agents that continuously improve their performance on complex tasks.


#### Brains and brawn


Current LLMs exhibit remarkable performance but are impractical for some use cases due to their prohibitive computational costs and their reliance on expensive APIs.


To address the high cost of using large models,


[Data Shunt+ (DS+)](https://arxiv.org/pdf/2406.15471) proposes a collaborative framework where smaller, more manageable models work alongside larger models. DS+ employs two main strategies: Small Model for Large Model (S4L) and Large Model for Small Model (L4S).


S4L aims to refine the large model's input and prediction space using smaller models. It achieves this through Prompt Pruning (PP) and Prompt Transferring (PT). PP uses the small model's predictions to craft prompts for the large model, effectively narrowing down the potential outcomes and enhancing accuracy. PT breaks down complex tasks into sub-tasks, enabling small models to handle simpler aspects, allowing the large model to focus on more challenging tasks.


Conversely, L4S utilizes the large model's knowledge to enhance the smaller model's capabilities. This knowledge transfer is achieved through a 2-Stage Confidence Distillation method. This method selectively transfers knowledge from the large model to the smaller one based on the confidence levels of both models, preventing the smaller model from forgetting its initial training and minimizing the impact of potentially incorrect knowledge.


Data Shunt+ successfully improves the accuracy of LLMs on various tasks while significantly reducing the cost.


#### Large social models


Despite significant progress in LLM Multi-Agent Systems (LLM-MAS), information asymmetry remains a critical challenge when applying these systems to enhance human cooperation.


[Informative Multi-Agent Systems (iAgents)](https://arxiv.org/pdf/2406.14928) is a new paradigm that aims to overcome this information asymmetry. Unlike traditional systems that operate in a shared information environment, iAgents mirror human social networks, where agents act on behalf of their users. To facilitate effective communication and information exchange, iAgents employ a novel reasoning mechanism called InfoNav. InfoNav guides agents to proactively identify and request missing information necessary for task resolution by generating a dynamic plan that outlines required data.


This plan is constantly updated as information is exchanged between agents, ensuring focused and productive communication. Further, iAgents uses a mixed memory system comprising Distinct Memory and Fuzzy Memory. Distinct Memory stores human information in a structured format, enabling precise retrieval using methods like SQL queries, while Fuzzy Memory handles unstructured data through summaries and embedding-based searches. This dual-memory system allows agents to access and process diverse types of information effectively.


iAgents is designed to address information asymmetry in collaborative tasks, demonstrating its ability to solve complex problems within large social network simulations.


[Share The Deep Dive](https://unifyai.substack.com/?utm_source=substack&utm_medium=email&utm_content=share&action=share)


## The Pulse


**Virtual Language Machines -** Meta has announced the release of


[Meta LLM Compiler](https://huggingface.co/collections/facebook/llm-compiler-667c5b05557fe99a9edd25cb) , a suite of models built on Meta Code Llama that enhance code optimization and compiler capabilities. These models can emulate compiler behavior, predict optimal passes for code size, and disassemble code, and can be fine-tuned for new optimizations and compiler tasks.


**Gemini Party -** Google has made


[several updates](https://developers.googleblog.com/en/new-features-for-the-gemini-api-and-google-ai-studio/) to its Gemini AI platform, including expanding access to the 2 million token context window in Gemini 1.5 Pro, introducing code execution capabilities for Gemini 1.5 Pro and 1.5 Flash, and making Gemma 2 available in Google AI Studio. The company is also releasing Gemini 1.5 Flash tuning for developers through its API.


**Hello? GPT Here** - Kyutai has developed


[Moshi](https://kyutai.org/cp_moshi.pdf) , a publicly accessible AI assistant with natural conversational abilities, which can speak and listen in real-time with a latency of just 200-240 milliseconds. Moshi's architecture is based on an audio language model that treats compressed audio data like pseudo-words, allowing it to work directly with audio data and predict speech. Kyutai plans to open source the technology in the coming months.


That’s all for this edition, we hope you enjoyed reading through!


The Unify dev team.
