---
schema_version: "1.0.0"
document_id: "0eb413960182e6a6095304a8ce82ed134d74b38fe54fc99576258c79d0f96377"
company_key: "yc-lamin"
company: "Lamin"
source_id: "yc-lamin-news-import-1018c6ad32c5"
canonical_url: "https://blog.lamin.ai/symbolic-memory"
published_at: null
first_seen_at: "2026-07-24T09:27:52.013300+00:00"
fetched_at: "2026-07-28T21:37:36.757263+00:00"
content_hash: "sha256:64fe58326d478da9d461b86d202b80de68bd6f22b0bea71975b672c69e09565e"
---

# Symbolic memory for biological R&D

# Symbolic memory for biological R&D¶


- [Alex Wolf](https://falexwolf.com/)


What should the shared memory layer for agents and humans look like? Will it live in embeddings or in records? A high-level note.


## The shared memory layer¶


In December 2025 I realized that large models would soon operate most of the small models in biological R&D. LLMs, via AI agents, started reliably executing work plans of remarkable complexity, in particular in software engineering, but also in data science, machine learning, and experimental design.


I started working on improving the interface of machine learning and data infra for scRNA-seq in 2016 with` scanpy


` &` anndata


` . The goal was to reduce the cognitive load & time scientists spend on wrangling data. After moving to lead computational teams at Cellarity, working closely with wetlab and automation, it became clear that the biggest gains were in how humans and models learn and improve together. I felt it was clear that this would best be achieved through a “shared memory layer”, a query layer that would allow teams to close larger-scale feedback loops.\[ 1 \] Now we have these very useful AI agents – which behave like a mix of junior colleagues, general experts, and small models – and I wonder whether this changed that premise.


For a moment, I worried the premise had changed and I’d been working on the wrong problem. But in the next moment it was clear that it didn’t because this new abundance of this peculiar intelligent agency is completely lost if it’s not exposed to the right context. On top of that, it suffers from short context windows & context rot. Similar to how junior colleagues benefit from supervision with rich & relevant context that lets them find the right way to think about a problem, agents perform remarkable work if they have context. And they produce unusable slop if they don’t.


## Good memory for agents¶


I don’t think anyone debates that this is the key bottleneck of current AI systems, so I want to spend most of this note on what an optimal “shared memory layer” for agents and humans could look like. I think that’s particularly relevant from the angle that a good part of the magic of agents is – unlike the merely intelligent compute machine that is an LLM – their ability to autonomously retrieve context if it’s *presented to them* in the right way. Agents can solve their own biggest bottleneck if “context engineering” is done right. In this note I want to refer to the “layer” that achieves this as the “shared memory layer”. This complements the procedural side of context engineering.


The past year revealed that for now the “right way to present context to agents” seems to be files in storage paired with established API-based systems of record. It doesn’t seem to be tensors, vector databases, or RAG systems. I found that interesting because I was never in the “symbolic camp” when it came to developing machine learning models. So I asked Gemini 3 to clear this up: *Can you disentangle why the symbolic representations in markdown notes & standard SQL databases, i.e., words and tables representing related concepts, are preferable for persisting thoughts whereas we use tensors for modeling/enabling thoughts in the first place?*


## Symbolic vs. tensorial representations¶


This is what it responded: *We use Tensors to think but Tables to remember. The distinction between the “Neural Thinking Engine” and the “Symbolic Memory” is essentially the digital version of the Dual Process Theory in psychology. Tensors in Neural Nets are for thinking because they are continuous, high-dimensional, and fuzzy. This makes them perfect for inference because it allows: (1) pattern matching & generalization, (2) compression & probabilism, (3) differentiability. Symbolic Systems like markdown files & SQL databases are for memory because they are discrete, low-dimensional, and rigid. They are preferable for persistence because they display: (1) Perfect Fidelity/ Zero Decay, (2) Addressability & Auditability, (3) Compositionality.*\[ 2 \]


To make this more concrete, I asked: *Is it valid to see “reasoning models” as a mix of neural nets trained on symbolic problem-solving tasks (like realizing that they made an error, calling a tool, etc.)?* And it says: *Modern Reasoning Models are Neural networks trained to simulate symbolic execution via two concepts. (1) Reinforcement Learning on Chain-of-Thought: The model is rewarded for following symbolic rules (e.g., “If I get an error, I must catch the exception and retry”). (2) Internal Symbolic Loops: The “Reasoning” tokens are essentially the model talking to itself in a symbolic language (natural language or code) to “hold” state. It uses the context window as a temporary symbolic scratchpad because its own weights are too fuzzy to hold complex logical state over many steps.*\[ 3 \]


I also remembered another angle to argue in favor of “symbolic systems” for holding memory: a 2016 paper from DeepMind on differentiable memory.\[ 4 \] Symbolic systems of record aren’t differentiable in the sense that you can write a tensor optimization loop, so differentiable tensorial memory fills a gap. On the other hand, a team that monitors updates and reviews changes, e.g. in the context of software development or R&D, also runs an optimization process against symbolic systems of record.\[ 5 \] And with AI agents & robotics, this becomes much more scalable than it was. Not as scalable as optimizing tensors on a GPU, but maybe scalable enough to merit the question of whether these symbolic systems of record are more optimal than memory stored in tensors, at least for present day real-world systems?


The “differentiable memory paper” from 2016 did, as far as I know, not make it into real world AI systems and I generally have the feeling that RAG systems and vector databases are much less hyped today than 2 years ago. Most recent approaches to increase context for LLMs revolve around better bookkeeping of markdown files, better tool calling to existing systems of record, and strategies in which LLMs recursively query storage systems. My understanding is that this is what agents should be doing in almost any setting and that it has been particularly optimized in the “Recursive Language Models” paper.\[ 6 \] Gemini 3 comments on this: *The reason those \[differentiable memory\] systems didn’t take over is twofold. (1) Scaling Laws: It turned out to be easier to just make the context window larger than to make the external memory differentiable. (2) The bottleneck is symbolic: The real world is symbolic. In R&D, for example, you deal with discrete entities: molecules, experiments, perturbations, sequences, files, etc. Attempting to force these into a continuous differentiable space for the sake of an optimization loop loses that discrete ground truth.*


## AI agents prefer symbolic memory¶


All of this gives me some comfort because it tells me that AI agents prefer the same kind of symbolic systems that humans prefer. And so I’m less worried about being out-of-the-loop on what matters most: I’ll always be able to review persisted work results in symbolic systems. This might be evident for language models given they’ve been trained on symbols, but because I’m coming from numerical data that’s inaccessible to humans, it seemed plausible that AI would soon store most results as tensorial embeddings.\[ 7 \]


All of this lets me also gain confidence in the simple intuitive idea of what “good memory” should be. Similar to how it seems that humans feel more comfortable & productive with recent symbolic systems of record (hybrids of note-taking & database systems like Notion or Obsidian), there should be similar systems that make agents “feel” comfortable & productive in formulating queries to retrieve the context they need.\[ 8 \] It’ll be interesting to observe which systems of record agents will ultimately prefer, and we’ll keep optimizing for that at Lamin.


## Acknowledgments¶


Sergei Rybakov and Sunny Sun for cross-reading and comments.
