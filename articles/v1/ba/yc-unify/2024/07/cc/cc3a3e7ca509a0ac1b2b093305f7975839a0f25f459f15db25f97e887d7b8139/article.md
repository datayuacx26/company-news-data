---
schema_version: "1.0.0"
document_id: "cc3a3e7ca509a0ac1b2b093305f7975839a0f25f459f15db25f97e887d7b8139"
company_key: "yc-unify"
company: "Unify"
source_id: "yc-unify-rss-3d614756e1ae"
canonical_url: "https://unifyai.substack.com/p/the-deep-dive-issue-35"
published_at: "2024-07-25T16:34:52+00:00"
first_seen_at: "2026-07-24T05:11:59.311365+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:7f0a8f5dc656b0679833921f9946597263ed3191841364a7c414b78494c9a411"
---

# The Deep Dive Issue #35

# The Deep Dive Issue #35


### Mem0, robust LLM workflows, and frontier models


[Nassim](https://substack.com/@nas07)


Jul 25, 2024


We live in a time where cutting-edge tech is released every other day and AI lets you build stuff and experiment at unparalleled speed. Somehow though, the global economy can still be put on its knees in a single moment of carelessness. Thinking about it, the contrast is so striking that it feels like we’re talking about two different worlds.


Now, last week’s massive bug is probably unrelated to AI per se (it can’t be a copy-paste from an LLM-generated code, right?), but it does serve as a good reminder that you can never test things enough, especially when you’re dabbling in the hallucination-prone black-box sorcery that is AI.


---


Small interlude to announce that we just started a new series where we’ll occasionally be featuring posts from various guest writers on everything related to LLMs.


What better to kick off this series than a deep dive into the meat of the topic with a detailed explanation of the Transformers architecture, brought to you by


[Daniel Warfield](https://iaee.substack.com/) . Enjoy the read!


[Transformers - Intuitively and Exhaustively Explained](https://unifyai.substack.com/p/transformers-intuitively-and-exhaustively)[Daniel Warfield](https://substack.com/profile/151611310-daniel-warfield)


·


July 25, 2024


[Read full story](https://unifyai.substack.com/p/transformers-intuitively-and-exhaustively)


---


## Personal Memories


Retrieval-Augmented Generation (RAG) has significantly improved how we access and leverage information from documents. Despite being modular, RAG is a static process that often stumbles when it comes to maintaining context and handling dynamic information. Mem0 offers a context-aware memory solution that mitigates these shortcomings.


**Why would you care?**


**-** By actively managing information, incorporating recency and decay mechanisms, and learning from user feedback, Mem0 makes information retrieval personalized and adaptable to your own usage preferences.


```text
import os
from mem0 import Memory


os.environ["OPENAI_API_KEY"] = "xxx"


# Initialize Mem0
m = Memory()


# Store a memory from any unstructured text
result = m.add("I am working on improving my tennis skills. Suggest some online courses.", user_id="alice", metadata={"category": "hobbies"})
print(result)
# Created memory: Improving her tennis skills. Looking for online suggestions.


# Retrieve memories
all_memories = m.get_all()
memory_id = all_memories[0]["id"] # get a memory_id
print(all_memories)


# Search memories
related_memories = m.search(query="What are Alice's hobbies?", user_id="alice")
print(related_memories)


# Update a memory
result = m.update(memory_id=memory_id, data="Likes to play tennis on weekends")
print(result)


# Get memory history
history = m.history(memory_id=memory_id)
print(history)
```


*Minimal example of a Mem0 workflow. Source: [Repository README](https://github.com/mem0ai/mem0/?tab=readme-ov-file#basic-usage-open-source)*


**How does it work? -** Mem0 acts like a long-term memory for AI agents by storing and retrieving information dynamically, following a series of repeatable steps:


1.


**Initialization:** Mem0 reads configuration settings from a config file, including the type of vector store (e.g., Qdrant), LLM (e.g., OpenAI), and embedder (e.g., SentenceTransformers) to use. It then sets up the components based on the configuration and connects to a SQLite database for tracking memory changes.


2.


**Adding Memories:** When you add data to Mem0, it first uses the embedder to convert the text into a numerical vector representation. You can optionally provide a prompt for the LLM to extract additional memories or facts from the input. Once logged, Mem0 uses the LLM to analyze the new memory, existing similar memories, and extracted facts. The LLM then decides whether to create a new memory, modify an existing memory based on the new information, or remove an outdated memory.


3.


**Retrieving Memories:** Mem0 can fetch memories by their unique ID from the vector store, or return a list of memories filtered by user, agent, or run ID. It can also perform similarity search based on an embedded query, only returning the most relevant memories.


Mem0 leverages the power of LLMs and vector embeddings to provide a dynamic and context-aware memory solution. It goes beyond simple storage and retrieval by actively using the LLM to curate and manage the stored information, making it a powerful tool for building intelligent agents that learn and evolve over time.


Check out the


[repository](https://github.com/mem0ai/mem0/) to get started.


[Share](https://unifyai.substack.com/p/the-deep-dive-issue-33?utm_source=substack&utm_medium=email&utm_content=share&action=share&token=eyJ1c2VyX2lkIjoxNTY2ODI5ODUsInBvc3RfaWQiOjE0NjUwMjc4MSwiaWF0IjoxNzIxMjkzNTI5LCJleHAiOjE3MjM4ODU1MjksImlzcyI6InB1Yi0xOTk5MjYzIiwic3ViIjoicG9zdC1yZWFjdGlvbiJ9.ttt1H-j4VUY_QCi79BIcdr9eM4n8RiJPaKkVmP9aOy4)


## The Lab


#### Graph Checker


Detecting LLM hallucination is an on-going challenge. Current methods struggle to provide clear explanations for the choices made by the model and how they connect to the overall reasoning process.


[GraphEval](https://arxiv.org/pdf/2407.10793) is a novel two-stage framework designed to address these limitations. In the first stage, GraphEval takes the LLM output and restructures it into a knowledge graph (KG). This KG acts like a concept map, transforming the text into a network of entities (key ideas) and their relationships (connections between those ideas). This structured representation allows for a more thorough analysis of the LLM's response.


The second stage involves systematically checking the accuracy of the information presented in the KG. GraphEval examines each relationship within the KG, and compares it against the original context provided to the LLM. This comparison is done using a pre-existing natural language inference (NLI) model, acting as a fact-checker. The NLI model determines the probability of each relationship statement being consistent with the given context. If any relationship within the KG is flagged as inconsistent or likely a hallucination by the NLI model, the entire LLM output is deemed unreliable.


GraphEval, when used with existing NLI models, improves their accuracy in identifying hallucinations in LLM-generated summaries on several benchmarks and, importantly, highlights the specific parts of the response that are likely incorrect.


#### LLM Habilis


LLMs can be augmented with external tools to provide more robust and diverse outputs, but it is often difficult to effectively convey the functionality of a tool to the model


[MetaTool](https://arxiv.org/pdf/2407.12871) focuses on enhancing the tool understanding of LLMs before training them on specific tasks. MetaTool uses a self-supervised data augmentation technique that involves automatically generating a large amount of training data by having the LLM interact with the tools in a simulated environment. During these interactions, the LLM is presented with a series of "meta-tasks." These meta-tasks are designed to mimic the way humans learn about tools, focusing on understanding cause and effect, and the boundaries of what a tool can and cannot do.


The meta-tasks include predicting the outcome of using a tool in a given situation (Effect), determining which tool to use to achieve a desired outcome (Decision-making), and identifying situations where a tool cannot be used (Input Boundary). The data generated from these meta-tasks is then used to further train the LLM, improving its ability to reason about and utilize tools effectively.


MetaTool significantly improves the performance of open-source LLMs on tool-oriented tasks, making them comparable to advanced models like GPT-4 in certain scenarios.


#### Overflowing


Manually creating workflows for LLM agents to solve complex tasks is time-consuming and requires expert knowledge, hindering large-scale AI agent development and deployment.


[AutoFlow](https://arxiv.org/pdf/2407.12821) , is a new framework that automatically generates workflows for AI agents using natural language. AutoFlow represents workflows as natural language programs, making them easily understandable and interpretable by LLMs. The framework uses a workflow optimization process that iteratively improves the generated workflows. It offers two distinct generation methods: fine-tuning-based and in-context-based.


The fine-tuning approach tailors workflow generation to particular tasks and domains by adjusting LLM parameters. Conversely, the in-context-based method leverages contextual information to guide generation without extensive fine-tuning, suitable for both open and closed-source LLMs. AutoFlow employs reinforcement learning, where a workflow generator LLM creates workflows based on user queries, and a workflow interpreter LLM executes these workflows. The interpreter LLM evaluates the workflow's performance on a dataset, providing feedback used to refine the generator LLM through iterative updates.


AutoFlow successfully generates robust and reliable agent workflows that can outperform manually designed ones in complex task-solving scenarios.


[Share The Deep Dive](https://unifyai.substack.com/?utm_source=substack&utm_medium=email&utm_content=share&action=share)


## The Pulse


**Winning horse** - Meta has


[released](https://ai.meta.com/blog/meta-llama-3-1/) Llama 3.1, its largest and most capable open-source LLM to date. The 405B parameter model rivals the performance of top closed-source LLMs, showcasing advanced capabilities in tasks like knowledge and tool use. The release also includes updated 8B and 70B models with expanded context length (128K), multilingual support, and enhanced reasoning.


**Om(i)ni model** - OpenAI has


[released](https://openai.com/index/gpt-4o-mini-advancing-cost-efficient-intelligence/) GPT-4o mini, a cost-efficient small model that significantly outperforms previous small models in resoning and multimodal tasks, among others. Priced at 15 cents per million input tokens and 60 cents per million output tokens, GPT-4o mini is an order of magnitude more affordable than comparable models.


**Pareto size** - Mistral AI has


[released](https://mistral.ai/news/mistral-large-2407/) Mistral Large 2, a new generation of its flagship language model. Mistral Large 2 is designed for single-node inference with long-context applications, achieving high performance and cost-efficiency. The model excels in instruction-following, conversation, and alignment, showcasing enhanced accuracy and reasoning capabilities. Mistral Large 2 is available for research and non-commercial use under the Mistral Research License.


And that’s all for this edition, we hope you enjoyed reading through!


The Unify dev team.
