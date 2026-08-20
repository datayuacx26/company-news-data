---
schema_version: "1.0.0"
document_id: "c8a1684fdda0c3e60e95863de6380a6899c0598d657111c100331f6062858a56"
company_key: "yc-unify"
company: "Unify"
source_id: "yc-unify-rss-3d614756e1ae"
canonical_url: "https://unifyai.substack.com/p/the-deep-dive-issue-27"
published_at: "2024-05-30T17:31:45+00:00"
first_seen_at: "2026-07-24T05:11:59.311365+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:04eebbcb6753ca3d6d8f8a6fe17da90e068e3694aadb8a0d24ce2f2155c98ba7"
---

# The Deep Dive Issue #27

# The Deep Dive Issue #27


### Guidance, LLM evaluation, and LLM add-ons.


[Nassim](https://substack.com/@nas07)


May 30, 2024


Seems like alternative architectures are coming into the scene almost as quickly as new LLMs. A while back, Mamba was shown to be a possible challenger to the Transformer status-quo, but inherent limits are still hindering mass adoption outside of the research space. More recently, new paradigms like KANs and xLSTMs propose to revisit some machine learning goldies with extra twists to make them competitive with the infamous attention mechanism.


While some results seem promising and driving interest, tests are still narrow and much less production-grade. Overall, it looks like good old Optimus is still here to stay, especially with multi-modal applications growing in demand. But what do you think? Is there really an LLM world beyond Transformers? 🤔


## **Fewer options, better choices**


Getting an LLM to output a precisely defined answer by tweaking the input prompt can be complex, all the more so given LLM outputs contain some level of randomness. There's loads of prompt optimization libraries and frameworks but few provide granular control over the expected output. Guidance is one such a few which lets you flexibly, and precisely specify the desired output structure.


**Why would you care?** - Guidance is super intuitive to use and lets you smoothly weave LLM input where needed, all in pure Python!


```text
from guidance import select


# a simple select between two options
llama2 + f'Do you want a joke or a poem? A ' + select(['joke', 'poem'])
```


*Minimal example where guidance is used to force a Llama model to select between two options given some context. Source: [Repository README](https://github.com/guidance-ai/guidance) .*


**How does it work?** - Guidance lets you write your code in pure Python and expand it by adding LLM capabilities through additional functions. With Guidance, you can enforce specific output structures using various methods including:


-


**Selects** , which limits the model's choices to a predefined set of options, as in the example above.


-


**Regular Expressions** , which let you define patterns the output should adhere to.


-


**Context-Free Grammars (CFGs)** , which create sophisticated grammars to structure complex outputs. For e.g, the


` one_or_more` CFG can be used on top of the


` select` method to select more than one option.


-


**Pre-Built Components** , such as substring and JSON. These components are utilities that come pre-packaged with the library.


Always in the spirit of Pythonic code, logic built using these primitives can be made into functions that can be called as decorators to augment other functions, making it easy to write bespoke re-usable components. The library also comes with tons of extra features like token healing to standardize starting / ending tokens, statefulness, cross provider support, etc.


Check out the


[repository](https://github.com/guidance-ai/guidance) to get started.


[Share](https://unifyai.substack.com/p/the-deep-dive-issue-27?utm_source=substack&utm_medium=email&utm_content=share&action=share)


## **The Lab**


### **Wisdom from experience**


The internal knowledge of LLMs quickly becomes outdated as time passes, this is typically handled with retraining or fine-tuning, both of which can be expensive solutions depending on the use-case.


[WISE](https://arxiv.org/pdf/2405.14768) proposes to keep LLMs up-to-date while avoiding expensive retraining. WISE introduces a new memory system for LLMs that uses both the model's original memory and a separate "side memory" specifically for storing edits. This side memory is like adding a notebook to the LLM where it can keep new information without changing its original knowledge base.


During the editing process, WISE divides new information into smaller chunks and stores them in different sections of the side memory. This prevents new information from interfering with previously learned information. Later, these chunks are carefully merged into a single, unified side memory, ensuring that different pieces of knowledge are integrated. Finally, when the LLM receives a query, a routing mechanism determines whether to access the main memory or the side memory based on the query's relevance to previously edited information.


WISE successfully updates LLMs with new information while preserving their original capabilities, and achieves significant improvements in accuracy and consistency over existing methods, particularly in handling a large number of edits over time


### **Multi-round selection**


Mixture of Experts can to generally improve the efficiency of large Transformer models. However, MoEs typically focus on optimizing the selection of expert models, assuming a pre-determined pool of expert models.


[Dynamic Mixture of Experts (DYNMOE)](https://arxiv.org/pdf/2405.14297) automates the selection process of expert groups through two key components. First, instead of using a fixed number of experts for every data point, DYNMOE introduces an extra gating to determine how many experts it needs by treating expert selection like a multi-label classification problem.


Secondly, DYNMOE incorporates an adaptive training process that dynamically adjusts the overall number of experts. If data points consistently fail to find suitable experts, new ones are added. Conversely, experts that remain unused are removed to maintain efficiency.


DYNMOE consistently achieves comparable or even surpasses the performance of manually tuned MoE models across various vision, language, and vision-language tasks, all while activating fewer parameters.


### **Do you even reflect?**


Current methods for evaluating LLMs often rely on external models or lag behind the rapid advancements in LLM capabilities.


Going beyond external evaluation,


[ProbDiff](https://arxiv.org/pdf/2405.10516) proposes a self-evaluation approach to LLM eval, leveraging the idea that a more capable LLM will exhibit a more consistent probability distribution in its responses to a given query.


ProbDiffs works by first prompting an LLM to generate a response to a query. Then, it asks the LLM to revise its initial response multiple times, each time calculating the difference in log probability between the original and revised responses. A larger difference in log probability suggests the LLM is less confident in its responses and therefore less capable of handling the query.


ProbDiff demonstrates consistent performance in evaluating various LLMs across diverse tasks, achieving results comparable to evaluations based on GPT-4.


[Share The Deep Dive](https://unifyai.substack.com/?utm_source=substack&utm_medium=email&utm_content=share&action=share)


## **The Pulse**


**Open Toolbox** - HuggingFace just announced adding


[support for tools](https://x.com/victormustar/status/1795405605605106044) on their HuggingChat platform. Using Cohere's Command R+ model, you can now access image editing / generation, document retrieval and other convenient extensions, through the same interface.


**More GPT in your GPT** - OpenAI is now giving free access to


[previously paid features](https://x.com/openai/status/1795900306490044479) directly into the standard ChatGPT, including web search, file upload, image understanding, data analysis and memory, with some limits.


**One report away -** Perplexity AI released


[Perplexity Pages](https://techcrunch.com/2024/05/30/perplexity-ais-new-feature-will-turn-your-searches-into-sharable-pages/) , a new feature which lets you turn search queries into shareable, customizable pages. Pages are also searchable through Google and can be asked follow-up questions like regular search sessions. The feature is currently available to a limited number of users, with plans to roll it out to all.


And that’s all for this edition, we hope you enjoyed reading through!


The Unify Dev Team.
