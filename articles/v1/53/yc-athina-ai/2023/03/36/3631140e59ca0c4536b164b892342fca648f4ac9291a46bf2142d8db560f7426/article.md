---
schema_version: "1.0.0"
document_id: "3631140e59ca0c4536b164b892342fca648f4ac9291a46bf2142d8db560f7426"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/art-automatic-multi-step-reasoning-and-tool-use-for-large-language-models"
published_at: "2023-03-16T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:28.379811+00:00"
content_hash: "sha256:59fab298ae358393a43563b3b5f2617fd5c00bb61ab2bdf9fd6d7babe4107161"
---

# ART: Automatic multi-step reasoning and tool-use for large language models

Do not index


Original Paper


[https://arxiv.org/abs/2303.09014](https://arxiv.org/abs/2303.09014)


Blog URL


[blog.athina.ai /art-aut...e-models](https://blog.athina.ai/art-automatic-multi-step-reasoning-and-tool-use-for-large-language-models)


**Original Paper:**[https://arxiv.org/abs/2303.09014](https://arxiv.org/abs/2303.09014)


**By:**[Bhargavi Paranjape](https://arxiv.org/search/cs?searchtype=author&query=Paranjape%2C%20B) ,[Scott Lundberg](https://arxiv.org/search/cs?searchtype=author&query=Lundberg%2C%20S) ,[Sameer Singh](https://arxiv.org/search/cs?searchtype=author&query=Singh%2C%20S) ,[Hannaneh Hajishirzi](https://arxiv.org/search/cs?searchtype=author&query=Hajishirzi%2C%20H) ,[Luke Zettlemoyer](https://arxiv.org/search/cs?searchtype=author&query=Zettlemoyer%2C%20L) ,[Marco Tulio Ribeiro](https://arxiv.org/search/cs?searchtype=author&query=Ribeiro%2C%20M%20T)


**Abstract:**


> Large language models (LLMs) can perform complex reasoning in few- and zero-shot settings by generating intermediate chain of thought (CoT) reasoning steps. Further, each reasoning step can rely on external tools to support computation beyond the core LLM capabilities (e.g. search/running code). Prior work on CoT prompting and tool use typically requires hand-crafting task-specific demonstrations and carefully scripted interleaving of model generations with tool use. We introduce Automatic Reasoning and Tool-use (ART), a framework that uses frozen LLMs to automatically generate intermediate reasoning steps as a program. Given a new task to solve, ART selects demonstrations of multi-step reasoning and tool use from a task library. At test time, ART seamlessly pauses generation whenever external tools are called, and integrates their output before resuming generation. ART achieves a substantial improvement over few-shot prompting and automatic CoT on unseen tasks in the BigBench and MMLU benchmarks, and matches performance of hand-crafted CoT prompts on a majority of these tasks. ART is also extensible, and makes it easy for humans to improve performance by correcting errors in task-specific programs or incorporating new tools, which we demonstrate by drastically improving performance on select tasks with minimal human intervention.


---


###


Summary Notes


####


Blog Post: Revolutionizing AI with ART: A New Horizon for AI Engineers


The world of artificial intelligence (AI) is witnessing rapid advancements, with Large Language Models (LLMs) like GPT-3 transforming how machines mimic human-like text interactions.


From chatbots to automated content creation, the applications are vast.


Yet, these LLMs struggle with complex reasoning and accessing external data, leading researchers to seek innovative solutions.


###


Introducing the ART Framework


The ART (Automatic Reasoning and Tool-use) framework is a groundbreaking solution designed to empower LLMs with enhanced reasoning capabilities and the ability to use external tools.


This approach aims to address the limitations of LLMs by enabling them to perform complex reasoning tasks more effectively.


####


Key Features of the ART Framework


- **Task and Tool Libraries** : ART uses a structured library of task demonstrations and tools, selecting the most appropriate ones for each task.


- **Dynamic Multi-Step Reasoning** : It can dynamically create reasoning programs by linking relevant tasks and tools, adapting to different task requirements.


- **Integration with External Tools** : ART can integrate outputs from external tools into its reasoning process, expanding its knowledge base.


- **Human-in-the-Loop Enhancement** : It incorporates human feedback for updating its libraries, allowing continuous improvement.


###


ART in Practice: Experimental Insights


The ART framework's performance was tested using benchmarks like BigBench and MMLU, utilizing models like InstructGPT and Codex, and various external tools.


####


Key Findings


- **Enhanced Performance** : ART significantly outperformed traditional models in reasoning and tool use.


- **Impact of Human Feedback** : Even minimal human feedback led to substantial improvements in ART's capabilities.


###


Conclusion: Unlocking Advanced AI Reasoning


The ART framework is a pivotal development in enhancing LLMs' reasoning abilities. By facilitating complex reasoning tasks and incorporating external tools, ART sets the stage for more sophisticated AI applications.


Its adaptability and human-in-the-loop feature ensure it can evolve to meet new challenges.


For AI Engineers, ART represents a powerful tool to leverage the full capabilities of LLMs, enabling them to solve more complex problems and create more intelligent systems.


This step forward in AI development promises to drive innovation and transform our interaction with technology.


####


Visual Aids


- **Figure 1:** Demonstrates ART reasoning for a task.


- **Table 1:** Compares ART to other reasoning methods.


- **Table 2:** Shows ART's performance metrics on benchmarks.


This exploration of the ART framework underscores the ongoing progress in AI, aiming to enhance reasoning and tool use.


As we advance, the synergy between humans and AI is set to open new possibilities, reshaping our technological interactions both daily and professionally.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
