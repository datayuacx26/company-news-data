---
schema_version: "1.0.0"
document_id: "a2af1eeeb66c78b7784f96699f8c73ccb8d561eab52b6f6549e4e4fe8b0366c4"
company_key: "yc-unify"
company: "Unify"
source_id: "yc-unify-rss-3d614756e1ae"
canonical_url: "https://unifyai.substack.com/p/the-deep-dive-issue-18"
published_at: "2024-03-29T14:11:57+00:00"
first_seen_at: "2026-07-24T05:11:59.311365+00:00"
fetched_at: "2026-07-28T22:26:02.237076+00:00"
content_hash: "sha256:83600d4b0de332c13788fcbe155bcfa0f77bdfd3243681f0f4cfc3c57e8819b3"
---

# The Deep Dive Issue #18

# The Deep Dive Issue #18


### LLAMAFACTORY and guided tuning


[Nassim](https://substack.com/@nas07)


Mar 29, 2024


Okay folks, stuff is actually getting real. We've all been hyped with self-aware assistants, autonomous robots and all sorts of futuristic tech but now we're getting telepathy as well. Few people had serious expectations about it, but it looks like


[Neuralink](https://www.theguardian.com/technology/2024/jan/29/elon-musk-neuralink-first-human-brain-chip-implant) successfully pulled-off their brain-chip implant project.


It may still be under clinical trial period but who knows, it may not be too long until you can converse with LLMs from the comfort of your cortex. Until then, you can't help but appreciate how life-changing this could be for anyone suffering from paralysis.


## **LLaMA Corp**


Fine-tuning has become a mainstream way to improve the performance of AI models, particularly LLMs, but remains slightly technical and tricky to calibrate for different types of models. LLAMAFACTORY proposes a unified framework that integrates various efficient LLMs fine-tuning methods through a single interface.


**Why would you care?** - If you've been looking for a no-code, standardized fine-tuning UI, LLAMAFACTORY comes equipped with LLAMABOARD to help you fine-tune models without the coding hassle.


*Overview of the LLAMAFACTORY pipeline. Source: [Original Paper](https://arxiv.org/pdf/2403.13372.pdf)*


**How does it work?** - LLAMAFACTORY minimizes dependencies between models, datasets, and training methods, providing a comprehensive solution for fine-tuning over 100 LLMs with a wide range of techniques. LLAMAFACTORY is composed of three core modules, namely a:


1.


Model Loader: Which prepares various architectures for fine-tuning using an AutoModel pipeline, extended with techniques like monkey patching to enable flash attention, and quantization to compress models.


2.


Data Worker: Which processes data from different tasks through a comprehensive pipeline supporting more than 50 datasets.


3.


Trainer: Which unifies fine-tuning methods to adapt models on different tasks and datasets.


Being implemented with PyTorch, the framework benefits from SOTA LLM open-source libraries, such as Transformers, PEFT, and TRL. The design of the framework allows these modules to be reused across different training approaches, significantly reducing the integration cost of new fine-tuning methods.


Check out the


[repository](https://github.com/hiyouga/LLaMA-Factory) to get started.


[Share](https://unifyai.substack.com/p/the-deep-dive-issue-18?utm_source=substack&utm_medium=email&utm_content=share&action=share)


## **The Lab**


### **LLM-Compliant**


Pre-trained LLMs are often further tuned to mitigate toxicity and harmfulness. However, this alignment may not capture the unique and contextual desired behaviors required for specific industries, sectors, or use cases.


[Alignment Studio](https://arxiv.org/pdf/2403.09704.pdf) provides a framework for sector-specific AI governance, offering more complex values, behaviors, and regulators than a simple temperature control system. Alignment Studio is composed of several modules, including:


-


Framers that identify the essential knowledge from the policy documents and create fine-tuning data for the LLM.


-


Instructors that use supervised fine-tuning and reinforcement learning fine-tuning to instill the desired values and behaviors in the LLM, based on the data created by Framers.


-


Auditors that evaluate the performance of the aligned LLM, both during training and after deployment.


Alignment Studio allows for more fine-grained control and better conflict resolution between values expressed by different regulations.


### **Mind your KPIs**


Continual instruction tuning (CIT) is an effective LLM tuning method used to calibrate the behavior of language models. But CIT still faces the problem of catastrophic forgetting, where LLMs may overly fit to previously seen instructions and become confused on held-out tasks.


[Key-part Information Gain (KPIG)](https://arxiv.org/pdf/2403.10056.pdf) is a new method for efficient LLM CIT. It consists of three main components:


-


In the task definition and notations section, a task is defined as a stream of time-indexed task sets used to finetune the LLM. After T-step training, the performance of the LLM is evaluated on the tests of seen tasks and held-out tasks.


-


The instruction diversity module is designed to address the issue of scarce instructions for a given task, using GPT-4 to generate parts of the seed instruction.


-


Information gain (IG) is used to measure the task-aware ability of the LLM for a specific task.


By addressing the “half-listening” phenomenon of CIT, KPIG improves the consistency of this instruction method.


### **More cutting-edge research**


**#Latent_Embedding** -


[SelfIE](https://arxiv.org/pdf/2403.10949.pdf) replaces input tokens with the latent embeddings from an LLM to interpret the information they contain, by inserting the target embedding into a separate forward pass of the same LLM through an interpretation prompt. By generating a natural language description for the hidden embeddings, SelfIE enables new modes of precise control over the model's behavior.


**RAG-and-drop** -


[Adaptive Retrieval-Augmented Generation (Adaptive-RAG)](http://arxiv.org/pdf/2403.14403v1) extends the capabilities of RAG by dynamically selecting the most suitable RAG strategy for a given query. It first assesses the complexity of the query using a classifier model and then chooses the appropriate strategy from a range of options. In doing so, Adaptive-RAG enables efficient and dynamic handling of queries of varying complexities.


**In the zone** -


[StateFlow](http://arxiv.org/pdf/2403.11322v1) is a novel framework that conceptualizes complex task-solving processes backed by LLMs as state machines. Within each state, StateFlow allows execution of a series of actions including the generation of LLM's responses and the use of external tools. State transitions are controlled by specific rules or decisions made by the LLM, allowing for a dynamic and adaptive progression through the task's pre-defined StateFlow model.


[Share The Deep Dive](https://unifyai.substack.com/?utm_source=substack&utm_medium=email&utm_content=share&action=share)


### **The Pulse**


**The journey continues** - Midjourney introduced a highly anticipated


[feature](https://venturebeat.com/ai/midjourney-debuts-feature-for-generating-consistent-characters-across-multiple-gen-ai-images/) that allows users to produce consistent characters across various generated images. With the introduction of the "--cref" tag, users can maintain character attributes such as facial features, body types, and clothing by referencing a URL containing the desired character.


**Deflection** - Microsoft has


[pulled in](https://blogs.microsoft.com/blog/2024/03/19/mustafa-suleyman-deepmind-and-inflection-co-founder-joins-microsoft-to-lead-copilot/) Mustafa Suleyman, co-founder of Infection AI and DeepMind, to head its new AI division, Microsoft AI. Suleyman and his colleague, Karén Simonyan, will bring their expertise in AI research and product development to Microsoft, particularly in enhancing the capabilities of Microsoft's Copilot, Bing, and Edge.


**{insert_letter}1** - Open Interpreter released


[O1](https://www.openinterpreter.com/01) , an open-source hardware device that functions as a portable voice interface, enabling users to control their home computer via natural language commands. It operates remotely and handles tasks like calendar checks, email sends, and AI skill training. The Rabbit r1 had already made noise a couple weeks ago and this new device could signal an expanding market for voice-controlled computing.


And that’s all for this edition, we hope you enjoyed reading through!


The Unify Dev Team.
