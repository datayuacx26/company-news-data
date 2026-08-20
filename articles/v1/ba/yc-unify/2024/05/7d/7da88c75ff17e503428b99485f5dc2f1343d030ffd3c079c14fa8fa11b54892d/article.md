---
schema_version: "1.0.0"
document_id: "7da88c75ff17e503428b99485f5dc2f1343d030ffd3c079c14fa8fa11b54892d"
company_key: "yc-unify"
company: "Unify"
source_id: "yc-unify-rss-3d614756e1ae"
canonical_url: "https://unifyai.substack.com/p/the-deep-dive-issue-25"
published_at: "2024-05-16T16:34:55+00:00"
first_seen_at: "2026-07-24T05:11:59.311365+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:6be193312766dcdde8357b828461707a77d1f2feddbcc279d54d5b3797bf3804"
---

# The Deep Dive Issue #25

# The Deep Dive Issue #25


### AutoGroq, extending transformers, and AI for science


[Nassim](https://substack.com/@nas07)


May 16, 2024


Another announcements season, another skirmish on the frontier of AI. This time, all-times favorites (by lack of contestants 🤷) OpenAI and Google enter the multimodal ring with a chance to win a bigger piece of that shiny, precious end-consumer data.


On the menu for this brawl, not much tech upgrade beyond existing SOTA, aside from the usual faster-cheaper-better that is. It’s an all-out fight over who makes sure you have zero chance of spending one day without talking to your new life-coach copilot buddy (let’s spare ourselves yet another comparison with “her” shall we).


Who wins? Place your bets, we’ll know in a few months when we start counting how many times “GPT” and “Gemini” were uttered globally. Seems to be the new KPI now.


## Agents on the fly


Agent-building frameworks let you program and orchestrate agent LLMs, but fine-grained edits during the output generation process often isn't an option. This makes agent systems black box structures. AutoGroq allows you to refine and steer agentic systems with a simple interface.


**Why would you care?** - Ever thought your agents were a bit flimsy and quick to go off on tangents? Or just that building agents is still a tad too complex? Now you can start building and tweaking agent workflows with natural words!


High level structure of the AutoGroq interface. One request automatically spins up dozens of co-ordinated agents to work on the task. Source:


[Repository README](https://github.com/jgravelle/AutoGroq?tab=readme-ov-file)


**How does it work?** - AutoGroq provides a user-friendly interface for generating and managing a team of AI agents tailored to a specific request. It leverages LLMs through the Groq API (duh) to generate agents, design workflows, and handle interactions between the agents. AutoGroq also allows you to download the workflow to be plugged into agent frameworks like Autogen and CrewAI, simplifying the initial system design step.


At a high level, you only need to pass in the general prompt task. The prompt is sent to an LLM for rephrasing to optimize it for further LLM processing. The optimized prompt is then analyzed to come up with a list of relevant agents that would be working on the task, where each agent has a name, role, skills and tools at its disposition.


A workflow structure is set-up from this list of agents. The workflow defines how the agents interact, who manages the overall task, and how each agent contributes to the final solution. Finally, additional input can be provided to refine the thought process of each agent separately in order to steer their behavior.


The project is still in active development but new features are quickly added!


Check out the


[repository](https://github.com/jgravelle/AutoGroq) to get started.


[Share](https://unifyai.substack.com/p/the-deep-dive-issue-25?utm_source=substack&utm_medium=email&utm_content=share&action=share)


## The Lab


#### Tokeneyezer


LLMs excel at language-based tasks but struggle with spatial reasoning. Where humans use "mental images" to reason spatially, no rigorous equivalent for LLMs has been developed to date.


[Visualization-of-Thoughts (VoT)](https://arxiv.org/pdf/2404.03622) explores spatial reasoning in LLMs. VoT prompts the LLM to generate visualizations of its reasoning process at each step. These visualizations act like a mental sketchpad, helping the LLM keep track of spatial information. For e.g, given a task to navigate a maze, VoT asks the LLM to describe the scene at every time period. Then, the LLM is asked to represent this "mental image" using simple characters, like drawing a basic map with "|", "-", and "+" symbols.


Based on a number of tests designed to measure spatial understanding, VoT is shown to significantly improve the spatial reasoning abilities of LLMs, outperforming other methods on the tested tasks.


#### Cache-cache


[You-Only-Cache-Once (YOCO)](https://arxiv.org/pdf/2405.05254) is a novel decoder-decoder architecture that consists of two main components: a self-decoder and a cross-decoder.


The self-decoder efficiently processes the input sequence and generates a set of key-value pairs that capture the essential information. These key-value pairs are then cached and shared with the cross-decoder. The cross-decoder, similar to the decoder in a standard Transformer, uses cross-attention to attend to the cached key-value pairs and generate the output sequence. This design allows YOCO to retain the global attention capabilities of the Transformer while significantly reducing the memory footprint and improving inference speed.


YOCO achieves comparable performance to traditional Transformer models on various language modeling tasks while demonstrating substantial reductions in GPU memory usage and inference time, making it a promising solution for deploying efficient and scalable LLMs.


#### Selection rounds


Contrasting with traditional fine-tuning, a


[recent paper](https://arxiv.org/pdf/2405.02774) explores two-stage fine-tuning as a means to improve the tuning process. The first stage, called pre-fine-tuning, involves selecting and using relevant samples from a large pool of freely available, unlabeled data to fine-tune the LLM. This pre-fine-tuned model is then further refined in the second stage using a smaller set of task-specific, labeled data, known as targeted fine-tuning.


The key innovation lies in the method for selecting data during the pre-fine-tuning stage. Instead of choosing data that closely matches the target task's data distribution, as done in previous methods, the approach selects data that gradually shifts the LLM's pre-training data distribution closer to the target distribution. This approach is based on the idea that pre-training already exposes the model to a diverse range of data, and nudging it towards the specific task domain can be more effective than directly aligning with the target distribution.


The method is shown to consistently outperform other data selection methods, and achieves comparable performance to conventional fine-tuning while requiring significantly less task-specific labeled data.


[Share The Deep Dive](https://unifyai.substack.com/?utm_source=substack&utm_medium=email&utm_content=share&action=share)


#### The Pulse


**I Bet My copilot is better** - IBM has released the


[Granite](https://github.com/ibm-granite/granite-code-models) , a set of open-sourced LLMs specifically designed for programming tasks, trained on a massive dataset of code and natural language, and licensed under Apache 2.0. The models are trained on code from 116 programming languages, range from 3 to 34 billion parameters. They support many uses cases, from building complex applications to on-device memory-constrained tasks.


**Not your average scientific calculator** - Intel's


[Aurora supercomputer](https://www.intel.com/content/www/us/en/newsroom/news/intel-powered-aurora-supercomputer-breaks-exascale-barrier.html) , has achieved exascale computing speeds, surpassing 1 exaflop and becoming the world's fastest AI-focused system. Powered by Intel's Xe GPU architecture and Xeon CPU Max Series processors, Aurora is designed to accelerate scientific discoveries by enabling researchers to leverage generative AI models in complex experiments.


**AI Origins** - Google DeepMind has released


[AlphaFold 3](https://blog.google/technology/ai/google-deepmind-isomorphic-alphafold-3-ai-model/#life-molecules) that predicts the structure and interactions of biological molecules with higher accuracy. AlphaFold now models a wide range of biomolecules including DNA and RNA. This can revolutionize drug discovery, accelerate genomics research, and enable the development of biorenewable materials and more resilient crops, among others.


And that’s all for this edition, we hope you enjoyed reading through!


The Unify Dev Team.
