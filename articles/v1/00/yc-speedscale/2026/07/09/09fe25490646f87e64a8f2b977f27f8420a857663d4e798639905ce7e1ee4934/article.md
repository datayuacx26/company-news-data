---
schema_version: "1.0.0"
document_id: "09fe25490646f87e64a8f2b977f27f8420a857663d4e798639905ce7e1ee4934"
company_key: "yc-speedscale"
company: "Speedscale"
source_id: "yc-speedscale-rss-29bb6cbf6f6f"
canonical_url: "https://speedscale.com/blog/the-best-model-is-a-routing-decision/"
published_at: "2026-07-27T00:00:00+00:00"
first_seen_at: "2026-07-27T19:24:32.239548+00:00"
fetched_at: "2026-07-28T20:32:33.872616+00:00"
content_hash: "sha256:75a020ad194387990da0ecdb645d043a4f95a1c14066c3c7414199a1c2a390a3"
---

# The Best Model Is a Routing Decision

I spent the day at NVIDIA thinking I’d be learning about all the newest models and RAM throughput achievements. Instead, a version of the future started to crystallize where AI really is good enough, pervasive and cheap(er). Let me share some of my observations and conclusions.


🎯 Key Takeaways


- Model selection is becoming a per-task routing decision, not a one-time commitment to a single flagship model.
- “Good enough” local and specialized models handle most work; frontier models become an escalation path for the hard 20%.
- The new craft is designing the factory — routing, context, and verification — not producing every line of code by hand.


Nemotron is advancing quickly. But the interesting part is not whether Nemotron 3 Super beats a frontier model on every test. It does not need to.


The interesting part is that NVIDIA is building a family of specialized models for reasoning, safety, retrieval, multimodal work, and voice. These models are designed to work together.


The model is becoming a component. The system around it is becoming the product.


## Latency Changes the Product


The demo I kept returning to was[Nemotron 3 VoiceChat](https://developer.nvidia.com/nemotron-voicechat-early-access) .


VoiceChat is a 12-billion-parameter, full-duplex speech model. It combines speech recognition, language processing, and speech generation in one architecture.


That sounds technical. The experience is simple.


It talks while you talk.


It can interject filler words while you are explaining something. You can interrupt it. It responds quickly enough that the awkward walkie-talkie rhythm of most voice assistants starts to disappear.


NVIDIA is targeting[less than 300 milliseconds of end-to-end latency](https://developer.nvidia.com/blog/building-nvidia-nemotron-3-agents-for-reasoning-multimodal-rag-voice-and-safety/) . At that speed, latency stops being a benchmark number. It changes the interface.


A slower model might produce a marginally better answer. It can still feel like talking to an automated phone tree.


The “best” voice model is not necessarily the smartest one. It is the one that can reason well enough without making you wait.


## Good Enough Keeps Getting Better


Frontier models remain astonishing. I use them every day. They are still where I send the work that requires the most judgment.


But the range of work that requires them is shrinking.


Nemotron shows where the rest of the industry is heading. NVIDIA now has specialized models for long-context reasoning, content safety, voice, retrieval, and multimodal understanding. Nemotron 3 Super itself uses a mixture-of-experts architecture that[activates only 12 billion parameters per pass](https://developer.nvidia.com/blog/building-nvidia-nemotron-3-agents-for-reasoning-multimodal-rag-voice-and-safety/) .


The goal is not to win every frontier benchmark. The goal is to complete useful work with predictable latency and cost.


One prediction I heard at NVIDIA was that, within 12 months, local models running on a desktop system such as a DGX Spark could be capable enough to accomplish most everyday work.


That may prove optimistic. Hardware companies have been known to feel good about hardware.


But the direction feels right.


If a local or inexpensive model handles 80% of a workflow, the frontier model only needs to handle the difficult 20%. The economics of the whole system change.


## Frontier Becomes an Escalation Path


We still build AI applications as if model selection were a marriage.


Pick a provider. Pick a flagship model. Send everything to it. Hope the invoice remains a private matter between you and finance.


That architecture is already aging.


Research projects such as[RouteLLM](https://github.com/lm-sys/RouteLLM) route simpler queries to cheaper models and reserve stronger models for harder work. Its published benchmarks report cost reductions of up to 85% while maintaining 95% of GPT-4 performance.


Treat those numbers as workload-specific, not universal. The larger point is harder to dismiss.


Model selection can happen per task.


A fast local model can classify the issue. A coding model can make the change. A reasoning model can inspect the architecture. A safety model can check the output. A frontier model can resolve the weird edge case that survived everything else.


The best model is whichever one should receive the next unit of work.


## The Monolith Is Already Dead


The frontier labs are moving in this direction too.


What looks like one assistant from the outside is increasingly an orchestrated system. Different models plan, search, write, critique, summarize, and operate tools behind the interface.


Open and local ecosystems are building the same architecture in public. It is the same shift I wrote about in[the AI software factory](https://speedscale.com/blog/ai-software-factory-context-layer/) : the model stops being the product and the system around it takes over.


Tools such as[Hermes Agent](https://hermes-agent.nousresearch.com/docs/) and[OpenClaw](https://docs.openclaw.ai/help/faq) make the idea accessible. They combine persistent memory, tools, schedules, skills, and multiple model providers. They are useful entry points because they expose the machinery.


You quickly stop asking, “Which model does my agent use?”


You start asking:


- Which model should plan?
- Which model should write code?
- Which work can stay local?
- Which tasks justify frontier pricing?
- What happens when a provider fails?
- Who verifies the output?


That is the beginning of a software factory. It is also, as[Conway’s Law predicted](https://speedscale.com/blog/conways-law-ai-factory/) , a mirror of the org chart that builds it.


## Local Is About Control


Local models are often presented as a cost-saving strategy. I am not convinced that is their best argument.


Hardware is expensive. Engineers are expensive. Maintaining inference infrastructure is not free.


I heard about well-funded Silicon Valley companies giving new software engineers a DGX Station running GLM 5.2 as part of their standard loadout. That is wildly impractical for the average company.


It is also a useful directional signal.


A local model can keep source code, customer data, and internal context inside an environment the company controls. It can continue operating without an external API. Its weights and deployment configuration can be inspected, pinned, and governed.


For financial services, healthcare, government, defense, and companies operating across data-residency boundaries, those characteristics can outweigh a simple token-price comparison.


Local is not only about paying less.


It is about deciding where intelligence is allowed to run.


## Code Is Cheap. Operations Aren’t.


We are approaching a world where a marketing professional with no traditional programming background can build an application used by thousands of people.


That is amazing.


It is also approximately where the easy part ends.


Someone must keep the application secure. Someone must test it against real behavior. Someone must understand why yesterday’s model update changed an output. Someone must handle deployment, observability, incidents, data retention, and the dependency that vanished during a weekend acquisition.


Google’s 2025 DORA research found that AI adoption was associated with[higher software-delivery throughput but continued pressure on delivery stability](https://cloud.google.com/blog/products/ai-machine-learning/announcing-the-2025-dora-report) . AI did not remove the weaknesses in an organization’s delivery system. It amplified them.


Software engineering at the leading edge is moving away from artisanal production of every line of code.


The new craft is designing the factory.


That means funneling work to the right agents. Giving them the right context. Routing each task to the lowest-cost model capable of completing it. Verifying the output. Feeding production evidence back into the loop. Keeping the system running continuously without allowing cheap code to become expensive chaos.


Verifying the output is the part that stays hard. A model that writes code cannot reliably tell you whether that code still behaves correctly against real traffic. That is an evaluation problem, not a generation one, and it is exactly where[improving the reliability of AI-generated code](https://speedscale.com/blog/a-developers-guide-to-improving-ai-code-reliability/) gets stuck. Deterministic guardrails are what make routing safe:[proxymock](https://speedscale.com/proxymock/) records real production requests and replays them against AI-generated changes, so “does this output work?” becomes a reproducible check instead of a judgment call. The cheaper the model doing the work, the more that verification layer earns its keep.


I arrived at NVIDIA asking which model would win.


The better question is who can keep the factory running while every model underneath it changes.


---


## Sources


- [NVIDIA Nemotron 3 VoiceChat Early Access](https://developer.nvidia.com/nemotron-voicechat-early-access) — VoiceChat is a 12B-parameter, full-duplex speech-to-speech model combining ASR, LLM, and TTS; 2026.
- [NVIDIA Nemotron 3 technical overview](https://developer.nvidia.com/blog/building-nvidia-nemotron-3-agents-for-reasoning-multimodal-rag-voice-and-safety/) — VoiceChat targets sub-300ms latency; Nemotron 3 Super activates 12B parameters per pass; 2026.
- [RouteLLM](https://github.com/lm-sys/RouteLLM) — Project benchmarks report up to 85% cost reduction while maintaining 95% of GPT-4 performance on MT-Bench.
- [Hermes Agent documentation](https://hermes-agent.nousresearch.com/docs/) — Documents persistent memory, skills, scheduled automation, subagents, and multiple model endpoints; 2026.
- [OpenClaw documentation](https://docs.openclaw.ai/help/faq) — Documents persistent memory, scheduled agents, skills, sandboxing, and model switching; 2026.
- [Google Cloud’s 2025 DORA summary](https://cloud.google.com/blog/products/ai-machine-learning/announcing-the-2025-dora-report) — AI adoption was associated with higher delivery throughput while maintaining a negative relationship with delivery stability; 2025.
