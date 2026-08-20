---
schema_version: "1.0.0"
document_id: "3299119da44064c1be97d7df9547ac4c7099200f7e259b5344bc55fb5af02249"
company_key: "yc-athina-ai"
company: "Athina AI"
source_id: "yc-athina-ai-rss-7ac750909891"
canonical_url: "https://blog.athina.ai/large-language-models-are-zero-shot-reasoners"
published_at: "2023-01-29T00:00:00+00:00"
first_seen_at: "2026-07-24T17:13:32.301153+00:00"
fetched_at: "2026-07-28T22:26:29.187426+00:00"
content_hash: "sha256:237c226f9b4f06cbcff51c86c5f6daf2dffa1d849c71b33d04c8c7e03fbe8f50"
---

# Large Language Models are Zero-Shot Reasoners

Do not index


Original Paper


[https://arxiv.org/abs/2205.11916](https://arxiv.org/abs/2205.11916)


Blog URL


[blog.athina.ai /large-l...easoners](https://blog.athina.ai/large-language-models-are-zero-shot-reasoners)


**Original Paper:**[https://arxiv.org/abs/2205.11916](https://arxiv.org/abs/2205.11916)


**By:**[Takeshi Kojima](https://arxiv.org/search/cs?searchtype=author&query=Kojima%2C%20T) ,[Shixiang Shane Gu](https://arxiv.org/search/cs?searchtype=author&query=Gu%2C%20S%20S) ,[Machel Reid](https://arxiv.org/search/cs?searchtype=author&query=Reid%2C%20M) ,[Yutaka Matsuo](https://arxiv.org/search/cs?searchtype=author&query=Matsuo%2C%20Y) ,[Yusuke Iwasawa](https://arxiv.org/search/cs?searchtype=author&query=Iwasawa%2C%20Y)


**Abstract:**


> Pretrained large language models (LLMs) are widely used in many sub-fields of natural language processing (NLP) and generally known as excellent few-shot learners with task-specific exemplars. Notably, chain of thought (CoT) prompting, a recent technique for eliciting complex multi-step reasoning through step-by-step answer examples, achieved the state-of-the-art performances in arithmetics and symbolic reasoning, difficult system-2 tasks that do not follow the standard scaling laws for LLMs. While these successes are often attributed to LLMs' ability for few-shot learning, we show that LLMs are decent zero-shot reasoners by simply adding "Let's think step by step" before each answer. Experimental results demonstrate that our Zero-shot-CoT, using the same single prompt template, significantly outperforms zero-shot LLM performances on diverse benchmark reasoning tasks including arithmetics (MultiArith, GSM8K, AQUA-RAT, SVAMP), symbolic reasoning (Last Letter, Coin Flip), and other logical reasoning tasks (Date Understanding, Tracking Shuffled Objects), without any hand-crafted few-shot examples, e.g. increasing the accuracy on MultiArith from 17.7% to 78.7% and GSM8K from 10.4% to 40.7% with large InstructGPT model (text-davinci-002), as well as similar magnitudes of improvements with another off-the-shelf large model, 540B parameter PaLM. The versatility of this single prompt across very diverse reasoning tasks hints at untapped and understudied fundamental zero-shot capabilities of LLMs, suggesting high-level, multi-task broad cognitive capabilities may be extracted by simple prompting. We hope our work not only serves as the minimal strongest zero-shot baseline for the challenging reasoning benchmarks, but also highlights the importance of carefully exploring and analyzing the enormous zero-shot knowledge hidden inside LLMs before crafting finetuning datasets or few-shot exemplars.


---


###


Summary Notes


####


Unveiling the Power of Language Models: A Leap in AI's Zero-Shot Reasoning


The field of Natural Language Processing (NLP) is witnessing rapid advancements, thanks to the growth of Large Language Models (LLMs) like GPT-3. These models have transformed how AI understands and generates text similar to humans.


A particularly exciting development is their ability to tackle complex reasoning tasks directly, without needing specific training for each task. This capability is largely attributed to a method known as zero-shot learning.


This blog post explores Zero-shot Chain of Thought (Zero-shot-CoT), an innovative approach enhancing LLMs' reasoning abilities, drawing significant interest from AI Engineers in large companies.


###


Evolution of Prompting in LLMs


LLMs are known for producing responses that are both coherent and contextually relevant for a wide range of tasks. The technique of "prompting" these models with a question or context, followed by a cue for response, has been crucial.


Among these techniques, Chain of Thought (CoT) prompting has shown promise in enabling multi-step reasoning by guiding models through logical steps.


However, CoT's reliance on multiple, task-specific examples has limited its widespread use.


###


Zero-shot Chain of Thought (Zero-shot-CoT) Explained


Zero-shot-CoT stands out by streamlining the prompting process. It uses simple cues like "Let's think step by step" to initiate a reasoning path within the model, avoiding the need for task-specific examples. This makes it a more flexible and scalable approach compared to traditional CoT.


####


Key Features of Zero-shot-CoT:


- **Task-Agnostic** : Works across different reasoning tasks without custom tuning.


- **Simple Prompt Engineering** : Enables complex reasoning with straightforward prompts.


- **Enhanced Reasoning Ability** : Supports sophisticated multi-step reasoning processes.


###


Methodology: A Two-Stage Prompting Strategy


Zero-shot-CoT utilizes a two-stage prompting technique. Initially, it prompts the LLM to outline a reasoning path to approach a question. Then, a second prompt helps derive the final answer from this reasoning path.


This method has been tested on various reasoning tasks, including arithmetic and commonsense reasoning, showing its versatility.


###


Experimentation and Results


The effectiveness of Zero-shot-CoT is backed by solid evidence. It outperforms traditional zero-shot and few-shot learning methods across different datasets and matches the efficiency of few-shot CoT without needing task-specific examples.


####


Experimental Highlights:


- **Broad Applicability** : Shows significant performance improvement across numerous reasoning tasks.


- **Superior to Existing Methods** : Matches or exceeds the performance of few-shot learning methods, setting a new standard for zero-shot reasoning.


###


Implications and Future Directions


Zero-shot-CoT offers significant benefits for AI Engineers, especially in enterprise applications. It simplifies using LLMs for complex reasoning tasks and broadens the potential applications of these models.


####


Opportunities Unlocked:


- **Less Need for Task-Specific Tuning** : Reduces dependence on large, specific datasets for training.


- **Wider Application of LLMs** : Makes pre-trained models useful for more tasks, increasing their enterprise value.


###


Conclusion: The Impact of Zero-shot-CoT


Zero-shot-CoT marks a significant milestone in NLP, offering a powerful method for extracting advanced reasoning from LLMs.


It lowers the barriers to using sophisticated AI technologies in enterprises by making complex reasoning more accessible.


###


Acknowledgments


The development of Zero-shot-CoT was supported by leading institutions and advanced computational resources.


For those interested in the technical details, the original study's appendix provides an in-depth look at the experimental setups and findings, showcasing the thorough research behind this innovation.


As we enter a new era in NLP, Zero-shot-CoT highlights the potential of simple prompt engineering to unlock the reasoning capabilities of LLMs, setting a new benchmark for AI solutions in the business world.


---


###


How Athina AI can help


Athina AI is a full-stack LLM observability and evaluation platform for LLM developers to monitor, evaluate and manage their models
