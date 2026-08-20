---
schema_version: "1.0.0"
document_id: "aaf6f1069331cc8a5f88798f7ffce86adebcca8a82d272427a373ef66709062f"
company_key: "yc-emergent"
company: "Emergent"
source_id: "yc-emergent-news-import-16a7bf482038"
canonical_url: "https://emergent.sh/news/ornith-1-0"
published_at: "2026-07-03T02:40:00+00:00"
first_seen_at: "2026-07-21T18:00:01.007987+00:00"
fetched_at: "2026-07-28T21:38:32.326002+00:00"
content_hash: "sha256:c56dab8df92f762caff3606ec4db632e6c89ffa356d24e9edf4abb3644ec811d"
---

# Ornith-1.0: The Open Model That Writes Its Own Agent

Most AI coding agents are two things bolted together: a model that writes code, and a human-designed scaffold that tells the model when to call a tool, how to recover from an error, and how to break a task into steps. The model is smart. The scaffold is rigid. And when a task drifts outside what the scaffold was built for, the whole agent stumbles.


On June 25, 2026, a research lab called DeepReinforce released[Ornith-1.0](https://deep-reinforce.com/ornith_1_0.html) , an open-source family of coding models built around a different idea: what if the model learned to write its own scaffold?


That is the entire premise, and it is a genuinely novel one. Ornith-1.0 does not just generate solutions. It generates the orchestration logic that produces those solutions, and it learns to do both at the same time during training. The result is a family of MIT-licensed models that punch well above their weight on agentic coding benchmarks, with the flagship surpassing Claude Opus 4.7 on two headline evaluations.


Here is what Ornith-1.0 actually is, how the self-scaffolding approach works, where it lands on benchmarks, and why it matters for anyone building or running coding agents.


## What is Ornith-1.0: The Open Model That Writes Its Own Agent


Ornith-1.0 is a family of open-source large language models built by DeepReinforce specifically for agentic coding. Per the company's[announcement](https://deep-reinforce.com/ornith_1_0.html) , it comes in four sizes (9B Dense, 31B Dense, 35B MoE, and 397B MoE), all released under the MIT license with no regional restrictions, and all post-trained on top of pretrained Gemma 4 and Qwen 3.5.


The name comes from the ancient Greek word for bird. The metaphor is deliberate: like a bird building its own nest, Ornith-1.0 learns to construct its own scaffolding before solving a coding task.


The models are available on[Hugging Face](https://huggingface.co/collections/deepreinforce-ai/ornith-10) and[GitHub](https://github.com/deepreinforce-ai/Ornith-1) , and expose an OpenAI-compatible interface with a 256K (262,144-token) context window.


## What "Self-Scaffolding" Actually Means


To understand why Ornith-1.0 is interesting, you have to understand what a scaffold is and why it usually holds agents back.


A scaffold (or harness) is the code that wraps a model and turns it into an agent. It handles the execution loop, decides when to call a tool, manages outputs, tracks memory, handles retries, and routes information between steps. For most coding agents, a human writes this scaffold once, and the model operates inside it. That works until the task changes in ways the scaffold was not designed for.


Ornith-1.0 flips this. Per DeepReinforce's technical write-up, instead of relying on a fixed, human-designed harness shared across a task category, Ornith-1.0 treats the scaffold as a learnable object that co-evolves with the model's policy during reinforcement learning.


Here is how the training loop works, per the announcement. Each RL step proceeds in two stages:


1. **Scaffold proposal.** Conditioned on a task and the scaffold previously used for it, the model first proposes a refined scaffold.
2. **Solution generation.** Conditioned on that scaffold and the task description, it then generates a solution rollout.


The reward from the rollout is propagated back to both stages. So the model is optimized not only to produce better answers, but to author the orchestration that elicits them. Over training, higher-reward scaffolds are continually mutated and selected, allowing per-task strategies to emerge automatically without any hand-engineered harness design.


In practical terms, as[MindStudio's breakdown](https://www.mindstudio.ai/blog/self-scaffolding-ai-models-ornith-1-0) describes it, Ornith-1.0 generates an inspectable Python harness as the first step in any task, with tool-aware logic built in from the start. You can read the scaffold it wrote. It is not a black box.


## The Reward Hacking Problem (and How DeepReinforce Handled It)


Letting a model write its own scaffold introduces an obvious risk: the model could learn to cheat. A self-generated scaffold might satisfy the verifier without actually doing the task, by reading visible test files and hardcoding expected outputs, touching the file the test checks for, or copying an oracle solution sitting in the environment.


This is one of the more thoughtful parts of the Ornith-1.0 design, and it is worth noting because reward hacking is a serious and well-documented failure mode in RL-trained coding models. Per the announcement, DeepReinforce defends against it in three layers:


1. **A fixed trust boundary.** The environment, the tool surface, and test isolation are immutable and outside the model's reach. The model evolves only the inner policy scaffold: its memory, error-handling, and orchestration logic.
2. **A deterministic monitor.** This flags any attempt to read withheld paths, modify verification scripts, or invoke actions outside the sanctioned tool surface, and assigns such trajectories zero reward.
3. **A frozen LLM judge.** Because intent-level gaming can happen entirely within the allowed tool surface, a separate frozen LLM acts as a veto on top of the verifier.


This matters for the industry because it is a concrete, replicable recipe for training self-improving agents without the model learning to game its own tests. Independent coverage from[MarkTechPost](https://www.marktechpost.com/2026/06/25/deepreinforce-releases-ornith-1-0-an-open-source-coding-model-family-that-learns-its-own-rl-scaffolds/) noted this three-layer defense as one of the more notable engineering contributions of the release.


## The Benchmark Numbers


Ornith-1.0's central claim is that it achieves state-of-the-art performance among open-source models of comparable size on agentic coding benchmarks. Here are the flagship results, all from DeepReinforce's[published tables](https://deep-reinforce.com/ornith_1_0.html) .


### The 397B Flagship


Benchmark Ornith-1.0-397B Claude Opus 4.7 Claude Opus 4.8 GLM-5.2-744B


Terminal-Bench 2.1 (Terminus-2) 77.5 70.3 85.0 81.0


Terminal-Bench 2.1 (Claude Code) 78.2 69.7 78.9 82.7


SWE-Bench Verified 82.4 80.8 87.6 —


SWE-Bench Pro 62.2 64.3 69.2 62.1


The headline: Ornith-1.0-397B scores 77.5 on Terminal-Bench 2.1 and 82.4 on SWE-Bench Verified, surpassing Claude Opus 4.7 on both. It also outperforms leading open-source models of similar size, including MiniMax M3 (66.0 on TB-2.1, 80.5 on SWE-Bench Verified) and DeepSeek-V4-Pro (67.9 on TB-2.1, 80.6 on SWE-Bench Verified).


The honest framing, which DeepReinforce's own numbers make clear: Ornith-1.0-397B beats Claude Opus 4.7, but it does not beat Opus 4.8 (Anthropic's current flagship) or the much larger GLM-5.2-744B on most benchmarks. This is state-of-the-art for open models at its size, not state-of-the-art overall.


### The 35B Middle Ground


The 35B MoE model is arguably the more interesting result for practical use. Per the announcement, Ornith-1.0-35B significantly outperforms similarly sized models including Qwen 3.5-35B, Qwen 3.6-35B, and Gemma 31B. Remarkably, despite having only 35B parameters, it surpasses Qwen 3.5-397B on Terminal-Bench 2.1 (64.4 vs 53.5) while matching its performance across several other coding benchmarks.


A 35B model beating a 397B model on a headline agentic benchmark is the clearest evidence that the self-scaffolding approach adds real capability, not just raw scale.


### The 9B Edge Model


The smallest model is built for edge and consumer hardware. Ornith-1.0-9B achieves 43.1 on Terminal-Bench 2.1 and 69.4 on SWE-Bench Verified. Per DeepReinforce, despite being a compact 9B-parameter model, it matches or exceeds much larger models such as Gemma 4-31B, demonstrating that strong agentic coding capabilities can be achieved even in resource-efficient deployments.


## Why the Model Sizes Matter


Ornith-1.0's four-size lineup is a deliberate spread across deployment scenarios, and understanding it helps you pick the right variant.


- **9B Dense:** Runs on a single 80GB GPU, or on consumer hardware like a gaming GPU or a MacBook Pro. This is the fully private, offline, no-API-cost option.
- **31B Dense:** A larger dense option (note: as of late June 2026,[LLM Reference](https://www.llmreference.com/model-family/ornith-1.0) reported the 31B variant's public weights were not yet accessible on Hugging Face, while 9B, 35B, and 397B were verified).
- **35B MoE:** The practical middle ground for most users. Strong performance, single-GPU deployable, punches above its weight class.
- **397B MoE:** The flagship for maximum performance in high-accuracy production agents. Requires sharding across multiple GPUs with tensor parallelism.


Every variant uses the same self-scaffolding RL training and exposes the same OpenAI-compatible interface. Per the[GitHub repo](https://github.com/deepreinforce-ai/Ornith-1) , Ornith-1.0 is a reasoning model, so by default the assistant turn opens with a thinking block before the final answer, and the serving recipes surface both the reasoning content and tool calls in OpenAI-compatible fields.


## What It Works With


Ornith-1.0 is optimized for terminal-native coding agents. Per the ecosystem documentation, it works directly with Claude Code, OpenHands, OpenClaw, and Hermes Agent out of the box. For local and self-hosted deployment, the models run through standard tooling like vLLM, Ollama, and LM Studio, with quantized (FP8) checkpoints available for lower-VRAM serving.


The recommended sampling parameters, per the repo, are temperature 0.6, top_p 0.95, and top_k 20 (with temperature 1.0 to reproduce the reported benchmark setup).


## Why This Matters


Ornith-1.0 is a meaningful release for three reasons, beyond the benchmark numbers.


**It is fully open and MIT-licensed.** Unlike frontier proprietary models, Ornith-1.0's weights are downloadable, its license permits commercial use, and there are no regional restrictions. For teams that need on-premise deployment, data residency, or freedom from API dependency, an open model that rivals Opus 4.7 on coding is genuinely useful.


**Self-scaffolding is a new scaling axis.** For the last few years, the dominant path to better coding agents has been bigger models and better hand-built harnesses. Ornith-1.0 demonstrates a third path: teaching the model to build its own harness. If this approach generalizes, it suggests that a meaningful chunk of agent capability can come from learned orchestration rather than raw parameter count. The 35B model beating a 397B model on Terminal-Bench is the proof point.


**It runs where you need it.** The 9B model on a MacBook Pro is a fully private, offline coding assistant with zero API cost. That is a different value proposition from any API-gated frontier model, and it matters for privacy-sensitive and cost-sensitive work.


The honest caveats: these are DeepReinforce's own published benchmarks, not independent reproductions. The self-generated scaffolds, while inspectable, introduce code-security considerations that teams should evaluate before running Ornith-1.0 agents on sensitive systems. And the flagship, while strong, does not match the current proprietary frontier (Opus 4.8, GLM-5.2-744B). Ornith-1.0 is the best open model at its size, which is a real and useful claim, not the best model overall.


## The Bottom Line


Ornith-1.0 is one of the more genuinely novel model releases of 2026. The self-scaffolding idea (training a model to write its own agent harness rather than operating inside a human-built one) is a real contribution, and the benchmark results back it up: the flagship beats Claude Opus 4.7 on Terminal-Bench 2.1 and SWE-Bench Verified, and the 35B model beats a 397B model on a headline agentic benchmark.


It is not the best coding model in the world. Opus 4.8 and GLM-5.2-744B still lead. But it is arguably the best open-source coding model at its size, it is MIT-licensed with no restrictions, and the 9B variant turns a laptop into a private offline coding assistant. For teams that value openness, on-premise control, and freedom from API dependency, Ornith-1.0 is worth a serious look.


More than that, it is a signal. If learned orchestration can make a 35B model outperform a 397B one, the next frontier in coding agents may not be about who has the biggest model, but about who teaches their model to build the best scaffold.
