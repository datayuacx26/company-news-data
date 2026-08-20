---
schema_version: "1.0.0"
document_id: "40f5c7fee074dee8461bb0eb60d2edbd59d9f0a8fbe8fb6762a9f40f7e6b3990"
company_key: "yc-unify"
company: "Unify"
source_id: "yc-unify-rss-3d614756e1ae"
canonical_url: "https://unifyai.substack.com/p/the-deep-dive-issue-33"
published_at: "2024-07-11T17:14:56+00:00"
first_seen_at: "2026-07-24T05:11:59.311365+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:cb5fd68ed53088954713448c39485a9f8a16e552f26907f133bc2e01fae4463f"
---

# The Deep Dive Issue #33

# The Deep Dive Issue #33


### MicroAgents, improving LLM performance, and alternatives


[Nassim](https://substack.com/@nas07)


Jul 11, 2024


Not a week goes by without some news of a data breach at an AI company or a new LLM jailbreak being discovered, the latest of which being last week’s


[Skeleton Key](https://www.microsoft.com/en-us/security/blog/2024/06/26/mitigating-skeleton-key-a-new-type-of-generative-ai-jailbreak-technique/) . Of course, security has always been a thing in software, and these issues are often quickly patched before announced. Still, LLMs sometimes trained on unique chat activity are like modern data centers with treasures of information waiting to be excavated.


As AI gets integrated into more services, it’ll get harder to filter the kind of data we end up sending for training. Not much to do about it for now. Until tangible solutions emerge, responsible prompting should be the way forward.


## Technical Specs


Coding assistants have greatly improved shipping speed and efficiency. However, these tools often produce code that cannot be readily plugged into production pipelines without manual auditing. MicroAgents provides a way tool to automatically test and adjust generated code to ensure robustness.


**Why would you care?**


**-** If you’re using coding assistants or even just leveraging language models for coding tasks, MicroAgents can help you debug AI-generated code faster than having to copy-paste until it eventually works.


MicroAgents workflow versus traditional coding assistants. Source:


[MicroAgents Blog Post](https://www.builder.io/blog/micro-agent)


**How does it work? -** MicroAgent leverages unit tests as a guiding mechanism to ensure the generated code meets specific requirements. MicroAgent follows a structured workflow:


1.


**Function description:** You start by providing a natural language description of the desired function, module, or component you want to generate.


2.


**Tests generation:** Based on the description, MicroAgent begins by automatically generating unit tests that define the expected behavior of the function, including input-output examples.


3.


**Code generation:** MicroAgent then uses an LLM to write code in your chosen language that aims to pass the unit tests.


4.


**Automatic iteration:** If the tests fail, MicroAgent iteratively edits the code and re-runs the tests until all tests pass successfully.


Check out the


[repository](https://github.com/BuilderIO/micro-agent) to get started.


[Share](https://unifyai.substack.com/p/the-deep-dive-issue-33?utm_source=substack&utm_medium=email&utm_content=share&action=share)


## The Lab


#### Custom Commands


Current methods for automatically designing language models prompts are limited to finding a single set of well-crafted instructions. However, one set of instructions might not be enough to help the LLM solve all types of problems within a complex task.


[Mixture-of-Prompts (MoP)](https://arxiv.org/pdf/2407.00256) is a new technique to generate specialized prompts for various tasks. MoP first analyzes the task and divides it into smaller, more manageable sub-tasks. For each sub-task, MoP creates a specialized prompt, which includes both instructions and relevant example input-output pairs (demos). MoP decides which examples to include in each expert prompt by analyzing their meaning and grouping similar ones together. Then, MoP refines the instructions for each expert by testing different options and choosing the one that works best for its specific sub-task. Finally, when a new problem needs to be solved, MoP analyzes it to figure out which prompt is the most relevant to generate the solution.


MoP was tested on a large number of tasks and consistently outperformed traditional single-prompt methods, demonstrating its potential to better handle multi-dimensional tasks.


#### Memory Chunks


Recalling precise information from long text is still an issue for current LLMs. This prevents them from understanding and generating responses that are grounded in the entirety of the provided context.


To mitigate this,


[LARIMAR](https://arxiv.org/pdf/2407.01437) is a new memory-augmentation approach that enhances an LLM decoder with an external memory that can be dynamically updated to process long contexts efficiently. The method involves dividing the input text into smaller segments, encoding each segment, and storing these encodings in the external memory. To retrieve information, a query related to the text is encoded and used as a key to access the most relevant segment in the memory. This retrieved information then helps the LLM generate a response. Importantly, this approach allows the external memory to grow with the length of the input text, enabling LARIMAR to handle much longer contexts than those seen during its training. By storing the memory and performing these operations on a CPU, LARIMAR avoids overwhelming the GPU, which is typically a limiting factor for processing large amounts of data.


LARIMAR successfully demonstrates its ability to accurately recall information from contexts containing up to one million words without any task-specific training.


#### Shipping packages


Serving LLMs tailored to specific tasks can be challenging given the computational burden of switching between numerous models in real-time.


A new


[research](https://arxiv.org/pdf/2407.00066) explores the ability to better serve multiple LoRAs through efficient compression. The research proposes two main compression methods: individual compression and joint compression. Individual compression uses Singular Value Decomposition (SVD) to reduce the complexity of each LoRA model independently. Joint compression, on the other hand, aims to find a common structure shared by multiple LoRAs, representing them using a shared basis and unique scaling factors. This shared basis can be pre-loaded onto the GPU memory, reducing the need to constantly load and unload individual models. Both compression methods are designed to shrink the model size while minimizing the loss of information, ensuring the compressed models perform similarly to the original ones.


Experiments with up to 500 LoRAs show that these compression techniques enable serving over a thousand LoRAs efficiently, achieving a throughput comparable to serving a single, uncompressed LoRA.


[Share The Deep Dive](https://unifyai.substack.com/?utm_source=substack&utm_medium=email&utm_content=share&action=share)


## The Pulse


**Sonnet Metering -** Anthropic has introduced


[new features](https://www.anthropic.com/news/evaluate-prompts) to its Console that streamline the process of crafting high-quality prompts for AI applications. Users can now leverage Claude 3.5 Sonnet to generate prompts based on their tasks, generate test cases to evaluate prompt performance, and compare outputs from different prompts side-by-side.


**O for on par -** SenseTime has unveiled its upgraded


[SenseNova 5.5](https://www.sensetime.com/en/news-detail/51168278?categoryId=1072) large model, featuring China's first real-time multimodal model, SenseNova 5o. This model provides a new AI interaction experience comparable to GPT-4o's streaming capabilities. SenseNova 5.5 also boasts an enhanced edge-side large model, reducing the costs per device and making it more accessible for widespread deployment.


**Small, but deft** - Salesforce has


[unveiled](https://x.com/Benioff/status/1808365628551844186) a tiny model with just 1 billion parameters that outperforms much larger models in function-calling tasks. xLAM-1B was trained using a new approach to data curation, using an automated pipeline called APIGen to generate high-quality, diverse, and verifiable datasets for training AI models.


And that’s all for this edition, we hope you enjoyed reading through!


The Unify dev team.
