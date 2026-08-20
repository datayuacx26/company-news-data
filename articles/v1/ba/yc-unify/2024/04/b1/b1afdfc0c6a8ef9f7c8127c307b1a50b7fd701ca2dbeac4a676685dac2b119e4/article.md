---
schema_version: "1.0.0"
document_id: "b1afdfc0c6a8ef9f7c8127c307b1a50b7fd701ca2dbeac4a676685dac2b119e4"
company_key: "yc-unify"
company: "Unify"
source_id: "yc-unify-rss-3d614756e1ae"
canonical_url: "https://unifyai.substack.com/p/the-deep-dive-issue-19"
published_at: "2024-04-05T13:16:46+00:00"
first_seen_at: "2026-07-24T05:11:59.311365+00:00"
fetched_at: "2026-07-28T22:26:02.237076+00:00"
content_hash: "sha256:a389d5d66c2b22ffd0461c4fc2b54be1fe3b6a53d37e34bb07593dfc3897a00b"
---

# The Deep Dive Issue #19

# The Deep Dive Issue #19


### LLMLingua, token optimizations and cognitive AI


[Nassim](https://substack.com/@nas07)


Apr 05, 2024


There's a new AI in town with emotional intelligence. An "


[empathic voice interface](https://www.hume.ai/blog/series-b-evi-announcement) " claimed to understand and adapt to the tone of your voice. It's interesting how we're trying to humanize AI. We want to give it our shape with humanoid robots, and infuse emotion in their tokens.


Looks like we're getting to the point where interacting with AI will feel like talking to your bud'. I mean, it's not like there's no potential for this to turn into a gaslighting factory. But if at least chatbots can tell when you're not in the mood for chit-chat that's still progress I guess.


### **When words are expensive, keep it to the point**


Training LLMs often requires lengthy prompts which slows down inference and increases costs. This makes balancing computational efficiency and detailed instructions challenging. LLMLingua proposes an efficient way to improve inference performance by using prompts efficiently.


**Why would you care?** LLMLingua achieves state-of-the-art performance on various benchmarks. Notably reasoning, in-context learning, conversation, and summarization tasks. It provides up to 20x compression with minimal performance loss, reducing computational costs.


*High level illustration of the LLMLingua prompt compressor. Source: [Original Paper](https://arxiv.org/abs/2310.05736)*


**How does it work?** - LLMLingua employs a coarse-to-fine prompt compression method that works as follows:


-


First, a budget controller allocates compression ratios to different parts of the prompt. This preserves essential information in the instructions and questions. It also allows for higher compression of potentially redundant demonstrations.


-


Next, an iterative token-level compression algorithm further reduces the prompt size. The algorithm considers the relationships between tokens to accurately understand the prompt's meaning. This better helps preserve key information compared to dropping less informative tokens.


-


Finally, instruction tuning aligns the distributions of the LLM with the smaller model used for compression. Doing so further improves the compressed prompt's effectiveness.


Check out the


[repository](https://github.com/microsoft/LLMLingua) to get started.


[Share](https://unifyai.substack.com/p/the-deep-dive-issue-19?utm_source=substack&utm_medium=email&utm_content=share&action=share)


### **The Lab**


**LLM Academy.**[CoLLEGe](https://arxiv.org/pdf/2403.15362.pdf) is a meta-learning framework that generates flexible LLM embeddings for new concepts. It uses example sentences and definitions to create an embedding that captures the semantic features of the concept.


CoLLEGe first encodes the sequences containing the new token using a masked language model. This process generates pooled sequence embeddings for each support sequence. These sequences are then aggregated, and projected into the input and output embedding space of the language model.


Despite accuracy issues, CoLLEGe enables efficient learning of new concepts with only a few examples.


**Token experts** - Using multiple expert LLMs within a single framework is intricate.


[Expert-Token-Routing (ETR)](https://arxiv.org/pdf/2403.16854.pdf) simplifies this by representing expert LLMs as special tokens in the vocabulary of a "meta" LLM. The meta LLM can then route tasks to the appropriate expert model by predicting its unique token.


The framework learns the strengths of each expert LLM by training token embeddings on a set of queries. Doing so enables ETR to leverage expert knowledge even when the expertise of individual LLMs is implicit.


ETR also allows for quick integration of new expert LLMs without modifying the framework.


**Another radical Yaeger -** LLMs struggle to prioritize contextual knowledge over internal knowledge when the two conflict. This makes it difficult to update or correct their knowledge without retraining.


[EREN](https://arxiv.org/pdf/2403.17431v1.pdf) offers a novel approach for editing LLMs that addresses this issue.


With EREN, the LLM first determines whether an input question relates to any previously stored edits. If the input is relevant to specific edits, the LLM generates an answer based on those, thus prioritizing contextual knowledge. If not, the LLM relies on its internal knowledge to answer the question.


By storing edits in natural text format, EREN can scale to a large number of edits without exceeding the LLM's input limits.


[Share The Deep Dive](https://unifyai.substack.com/?utm_source=substack&utm_medium=email&utm_content=share&action=share)


### **The Pulse**


**Stay SAFE** - DeepMind released


[SAFE](https://techxplore.com/news/2024-03-deepmind-safe-ai-based-app.html) , an AI-based fact-checking system for LLMs. SAFE breaks down LLM-generated claims and uses Google Search to verify their accuracy. Doing so, it achieves a 72% match with human fact-checkers, sometimes outperforming them. The code is


[available on GitHub](https://github.com/google-deepmind/long-form-factuality) .


**What's the deal with kitchen bots -** MIT researchers have developed a


[novel method](https://news.mit.edu/2024/engineering-household-robots-have-little-common-sense-0325) to equip household robots with common sense reasoning. The method combines motion data with LLMs to generate logical sub-tasks based on the robot’s physical coordinates. This enables the robot to adapt to disrupting changes and solve problems autonomously.


**SG-1** - Microsoft and OpenAI are


[collaborating](https://www.theinformation.com/articles/microsoft-and-openai-plot-100-billion-stargate-ai-supercomputer) on a massive AI project called "Stargate", a supercomputer capable of independent reasoning and problem-solving. The goal is to create an AI system that surpasses human capabilities in tasks like language processing, image recognition, and decision-making.


And that’s all for this edition, we hope you enjoyed reading through!


The Unify Dev Team.


---


Before leaving, we’d love to get your feedback on some of the content in the newsletter. We’re constantly iterating on the format to deliver what matters most to you so feel free to voice your preference below, thanks!


Loading...
