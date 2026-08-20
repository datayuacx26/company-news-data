---
schema_version: "1.0.0"
document_id: "2003db45283b21efb9de2410a18ee14a31f009717957f0f5b782066b8744700c"
company_key: "yc-taskade"
company: "Taskade"
source_id: "yc-taskade-rss-a662ed9a0141"
canonical_url: "https://www.taskade.com/blog/moonshot-kimi-history"
published_at: "2026-07-23T08:00:00+00:00"
first_seen_at: "2026-07-23T08:08:03.782494+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:2979bf66c2876a790315b3dc41f27df30310c45da6c98417edad3da8541908fd"
---

# Moonshot AI & Kimi History: From K2 to K3 (2026)

[Blog](https://www.taskade.com/blog)


[AI](https://www.taskade.com/blog/ai)


Moonshot AI & Kimi History:…


On this page (25)


Moonshot AI is a Beijing artificial intelligence company that builds the Kimi family of models with one stubborn belief: **open models can be as good as the closed frontier — and everyone should be able to download them.** In three years it went from a long-context chatbot nobody outside China had heard of to **Kimi K3, a 2.8-trillion-parameter open-weight model** that trades blows with the best proprietary systems on Earth, released in July 2026 and headed for a full open-weight drop days later.


How does a startup named after a Pink Floyd album end up shipping one of the most capable open models in the world? Who is Yang Zhilin, and why did Alibaba pour a billion dollars into his company? And what actually changed between Kimi K2, K2.5, and K3? Let's wind back the clock. 🌘


> **TL;DR:** Moonshot AI was founded in **March 2023** in Beijing by Yang Zhilin, Zhou Xinyu, and Wu Yuxin. Its Kimi chatbot broke out on long context; **Kimi K2** (July 2025) became the "open-weight DeepSeek moment"; **Kimi K3** (July 16, 2026) is a **2.8-trillion-parameter** open-weight flagship at frontier level. A 2026 round valued the company near **$20 billion** .[Clone a live AI-lab tracker →](https://www.taskade.com/share/apps/tmnju1vsp3ggajo7) and follow every model launch yourself.


## What Is Moonshot AI?


Moonshot AI is a Chinese AI company founded in **March 2023** in Beijing, best known for the **Kimi** family of large language models. It is counted among China's "six AI Tigers" — the cohort of well-funded startups racing to build frontier models — and it is the one that bet hardest on **open weights** : models whose parameters you can download, run locally, and fine-tune, rather than only rent through an API.


That bet is the whole story. Where much of the industry raced to lock capability behind paid endpoints, Moonshot's founder framed the mission differently at NVIDIA's GTC 2026 keynote:


> "One of our major pursuits is to build better open models and we believe in democratizing intelligence. Open models cannot be just open. They have to also be great."
>
>
> — Yang Zhilin, GTC 2026 keynote


*Don't just read Moonshot's history — clone a living dashboard that tracks any AI lab's models, funding rounds, and releases in minutes. One click, no code, free.*


### Moonshot AI at a Glance (2026)


Fact Detail


**Founded** March 2023, Beijing, China


**Founders** Yang Zhilin (CEO), Zhou Xinyu, Wu Yuxin


**Flagship product** Kimi model family + Kimi chatbot (kimi.com)


**Latest model** Kimi K3 (July 16, 2026) — 2.8T-parameter open-weight MoE


**Chinese name** 月之暗面 ("Dark Side of the Moon")


**Largest outside backer** Alibaba (~36% estimated stake)


**Latest valuation** ~$20 billion (2026 round)


**Total funding** ~$4 billion across multiple rounds


**License** Modified-MIT open weights (K2 family)


**Context window** Up to 1 million tokens (K3)


> **Last updated: July 2026** — refreshed for the July 16 Kimi K3 launch and the July 27 open-weight drop. Model generations, benchmark positions, and valuations in this space move fast and diverge across sources; where figures are disputed we say so. Check[Moonshot's blog](https://www.kimi.com/) before quoting any number in a deal memo.


## What Is Kimi?


Kimi is both **a chatbot** (the consumer app at kimi.com) and **a family of models** (Kimi v1 through Kimi K3). The two share a name because they share an origin: **Kimi is founder Yang Zhilin's English nickname.** The chatbot launched in late 2023 and made its name on one feature nobody else did as well at the time — reading enormous documents without losing the thread.


Think of it this way: the chatbot is the product a person opens in a browser; the model is the engine underneath, which Moonshot also publishes so developers can run it themselves. When people say "Kimi beat the previous best open model," they mean the model. When they say "I asked Kimi to summarize a 200-page contract," they mean the chatbot.


## Why the Names "Moonshot" and "Kimi"?


Moonshot AI's Chinese name is **月之暗面** (Yuè Zhī Ànmiàn), which literally means **"Dark Side of the Moon"** — a direct nod to the 1973 Pink Floyd album. The English name "Moonshot" carries the Silicon Valley meaning: an audacious, long-odds project aimed at a giant payoff. Put the two together and you get the company's self-image — an ambitious shot at something most people think is out of reach.


The model name is more personal. **Kimi is Yang Zhilin's own English nickname** , and it stuck to the chatbot and then to the whole model line. It's a rare case where the founder's name literally became the product — and a small reminder that behind the benchmark tables sits a specific person with a specific research history.


## Who Founded Moonshot AI? Meet Yang Zhilin


Moonshot AI was founded in **March 2023** by **Yang Zhilin** (杨植麟, CEO), **Zhou Xinyu** , and **Wu Yuxin** . Yang is the technical center of gravity, and his research pedigree is the reason serious investors took the company seriously from day one.


Yang earned his PhD at **Carnegie Mellon University** , advised by Ruslan Salakhutdinov and William Cohen, and did stints connected to Google Brain and Meta AI. Crucially, he co-authored two papers that anyone who studies language models will recognize:


- **Transformer-XL** — an architecture that extended how far a model could "remember" across long inputs, foreshadowing Moonshot's later obsession with long context.
- **XLNet** (NeurIPS 2019) — a pretraining method that **outperformed Google's BERT on 20 tasks** and became one of the most-cited NLP papers of its era.


Moonshot AI's Chinese name, 月之暗面, means "Dark Side of the Moon" — a Pink Floyd reference. Founder Yang Zhilin's English nickname, Kimi, became the model line's name. Photo: NASA / public domain.


The takeaway for the history: **Moonshot didn't stumble into long context and efficient architectures.** Those were Yang's research themes years before the company existed. When Kimi's first breakout feature turned out to be reading enormous documents, it was a founder building the thing he'd studied his whole career.


## Every Kimi Model Explained (v1 → K3)


Here is the single most useful reference in this piece: a clean, dated map of the Kimi lineage. Every competitor lists *some* versions; the point is to see the whole arc at once. (Parameter and context figures are Moonshot's reported specs; benchmark positions are discussed, and caveated, further down.)


Model Released Total / active params Context What changed


**Kimi v1** Late 2023 — ~200K→2M chars The long-context chatbot breakout


**Kimi K1.5** Jan 2025 — Long Reasoning ("thinking") arrives


**Kimi K2** Jul 11, 2025 1T / ~32B (MoE) 128K Open-weight breakout; agentic + coding


**K2 Instruct / Thinking** Sep–Nov 2025 1T / ~32B 256K Instruction-tuned + reasoning variants


**Kimi K2.5** Early 2026 1T-class 256K Native vision (early fusion) + agent swarm


**K2.6 / K2.7 Code** Apr–Jun 2026 1T-class 256K Agentic-coding specialization


**Kimi K3** Jul 16, 2026 **2.8T / ~16 experts** **1M** Frontier-level open-weight flagship


### Kimi v1 (2023): The Long-Context Breakout


The original **Kimi chatbot** launched in late 2023, and its calling card was **long context** — the ability to read and reason over very large documents in a single go. In a market where most assistants choked on anything past a few thousand words, "paste a whole book and ask questions" was a genuine differentiator, and it earned Moonshot an early, loyal Chinese user base. If you want the deeper mechanics of why context length matters so much, see[how large language models actually work](https://www.taskade.com/blog/how-do-llms-work) .


### Kimi K1.5 (January 2025): Reasoning Arrives


**K1.5** brought explicit **reasoning** — the model "thinks" through a problem in intermediate steps before answering, the same shift that reshaped the whole field around[reasoning models](https://www.taskade.com/blog/reasoning-models) . It was Moonshot signaling that it intended to keep pace with the reasoning wave, not just win on document length.


### Kimi K2 (July 2025): The Open-Weight "DeepSeek Moment"


**Kimi K2** , released **July 11, 2025** , is where Moonshot became a global name. It was a **1-trillion-parameter mixture-of-experts model** with roughly **32 billion active parameters** , and it shipped with **open weights** under a modified-MIT license. The reaction echoed the earlier shock of DeepSeek: here was near-frontier agentic and coding capability that anyone could *download* , not merely rent. Commentators called it an "open-weight DeepSeek moment," and it reframed Moonshot from "the long-context lab" into "the lab pushing the open frontier."


### Kimi K2 Instruct & K2 Thinking (September–November 2025)


Through late 2025 Moonshot iterated fast, shipping **instruction-tuned** and **reasoning** variants of K2 and extending context toward 256K tokens. This is the period where the K2 family hardened into a serious tool for agentic workflows rather than a research demo.


### Kimi K2.5 (Early 2026): Multimodal + the Agent Swarm


**K2.5** is the pivot most competitors under-cover, and it's the technical heart of everything that followed. Two things changed at once.


First, **native vision through "early fusion."** Instead of training a text model and bolting on an image module later ("late fusion"), K2.5 trained on vision and text tokens together from the start, giving one shared understanding. Moonshot described it as **the first open model with native joint vision-and-text capabilities** , and reported a striking result: with a strong text base, it needed essentially **zero vision fine-tuning data** to reach near-state-of-the-art on vision tasks.


Second, the **agent swarm.** Rather than one agent grinding through a task step by step, a lead orchestrator spawns **many parallel sub-agents** — researchers, coders, fact-checkers — that work simultaneously and report back. Yang's analogy in the GTC keynote was a company: "if we build a company, we need maybe a CEO to decompose and assign the task to different roles." We'll visualize it in a moment.


### Kimi K2.6 & K2.7 Code (April–June 2026): The Agentic-Coding Push


The **K2.6** and **K2.7 Code** releases specialized hard on **agentic coding** — long, multi-step programming tasks run with minimal supervision. On Moonshot's reported evaluations, the K2 family climbed to the top of open-weight agentic-coding leaderboards, and K2.6 became one of the models teams reached for when they wanted a cheap, capable open coder. (Kimi's K2.6 is one of the Moonshot models available in Taskade's model stack today.)


### Kimi K3 (July 2026): The 2.8T Open-Weight Flagship


**Kimi K3** , released **July 16, 2026** , is the payoff. It is a **2.8-trillion-parameter** mixture-of-experts model that activates only **16 of its 896 experts per token** — about **1.8% of its weights** — so a model that huge runs at a fraction of the compute a dense 2.8T model would need. It has **native vision** and a **1-million-token context window** , and Moonshot reports roughly **2.5× better scaling efficiency than K2** .


The API went live immediately, with **full open weights scheduled for July 27, 2026** . Pricing lands around **$0.30 per million cached input tokens, $3 input, and $15 output** — roughly comparable to a mid-tier proprietary model, which is remarkable for a model at this scale.


Independent reviewers who stress-tested K3 at launch reached a consistent verdict: it is at or near frontier level for agentic coding and long-horizon autonomy, trading blows with the strongest proprietary models on many tasks — while still showing rough edges (it can be slow and token-hungry). One reviewer's summary captured the moment: *"Open-source AI is no longer a few months behind the closed frontier models."* Moonshot's own release note is honest about the gap that remains — the model "still trails the most powerful proprietary models" on the very hardest tasks — but the direction is unmistakable. (Exact benchmark numbers are still settling post-launch and vary by source; we discuss the benchmark picture below rather than pinning contested figures.)


## How Kimi's Agent Swarm Works


The agent swarm is Moonshot's answer to a simple problem: some tasks are too big for one agent to finish in a reasonable time. Instead of a single model working sequentially, a **lead orchestrator decomposes the goal and spawns parallel sub-agents** — Yang described scaling to **100 or even 1,000** of them — each handling a slice of the work, then reports get merged into one result.


To train this behavior, Moonshot shaped three rewards so the swarm doesn't cheat or collapse: an **instantiation reward** (pay the model for spawning sub-agents so it doesn't quietly revert to a single agent), a **finish reward** (pay it only when sub-tasks actually complete, so it can't spawn junk it never finishes), and an **outcome reward** (did the overall task succeed). If the "orchestrator plus specialists" pattern sounds familiar, it's the same idea behind[multi-agent platforms](https://www.taskade.com/blog/best-multi-agent-platforms) and[inter-agent communication patterns](https://www.taskade.com/blog/inter-agent-communication-patterns) — Moonshot just baked it into the model's training.


## Kimi's Architecture in Plain English


You don't need a PhD to understand what makes Kimi efficient. Moonshot's whole pitch, laid out in Yang's GTC 2026 keynote, is that **scaling is the main driver of progress** — and that you can scale along more than just "train on more data." Kimi pushes three dimensions:


- **Token efficiency (MuonClip).** The optimizer decides how to nudge a model's weights each training step. Moonshot invested heavily in the **Muon** optimizer and its own **MuonClip** variant, which stabilizes training at trillion-parameter scale (past a certain size, internal scores "explode" and training diverges; a clipping trick caps them without hurting accuracy). Moonshot reports **roughly 2× the token efficiency of the long-standing Adam optimizer** — meaning the same limited pile of high-quality data teaches the model about twice as much. As Yang put it: *"Token efficiency is not just about efficiency. It's actually also about improving the upper bound of intelligence."*
- **Long context (Kimi Linear + Kimi Delta Attention).** A more efficient way to relate words across a very long input, so the model can hold a **1-million-token** window (about 700,000 words) without the cost exploding. This is the through-line from the original 2023 chatbot to K3.
- **Attention residuals.** A next-generation architectural idea Moonshot previewed at GTC, reported to add **~24% more token efficiency** . Combined with MuonClip, it's much of where K3's ~2.5× scaling gain comes from.


The important context-setting caveat: these efficiency figures (2×, 24%) are **Moonshot's own reported results** from its papers and keynote, not independent consensus — and the underlying Muon optimizer originated outside Moonshot (with researcher Keller Jordan in late 2024). Moonshot's contribution is scaling it. For a broader tour of these building blocks, our[open-source LLM history](https://www.taskade.com/blog/open-source-llm-history) walks the full lineage.


## How Moonshot AI Got Funded: Investors & Valuation


Moonshot AI is one of the best-funded startups in China, and the cap table reads like a who's-who of Chinese tech. **Alibaba is the largest outside backer** , with an estimated **~36% stake** and a reported billion-dollar-plus commitment, alongside Tencent, HongShan (the former Sequoia China), IDG Capital, 5Y Capital, Gaorong, Meituan, and China Mobile.


Round / event Approx. amount Notable backers Signal


**Seed & early (2023)** Tens of millions 5Y Capital, others Yang's XLNet pedigree draws first checks


**2024 mega-round** ~$1B+ Alibaba (lead), HongShan, others Valuation into the multi-billions


**Follow-on (2024–25)** Hundreds of millions Tencent, Gaorong, IDG Joins China's "AI Tigers" tier


**2026 round** ~$2B Multiple Valuation ~ **$20B** ; ~$4B total raised


*Valuations in this sector move fast and diverge across sources; the ~$20B figure is a 2026 snapshot, not a fixed number.* The strategic subtext matters: Alibaba's large stake ties Moonshot to one of China's biggest cloud and commerce platforms — distribution and compute in exchange for a frontier model partner. If you like tracking this kind of thing, it's exactly the sort of living dashboard you can[build from a single prompt](https://www.taskade.com/create) .


## Is Kimi Open Source? The Modified-MIT License, Explained


Here's the nuance almost every write-up gets muddy. The **Kimi K2 family shipped with open weights under a modified-MIT license** — you can download the parameters, run them locally, and fine-tune them. The single condition is an **attribution clause** : commercial products that cross **100 million monthly active users or $20 million in monthly revenue** must display "Kimi" prominently in their interface.


Two precisions worth keeping straight:


1. **Open weight ≠ fully open source.** Moonshot releases the *weights* , not the full training data or pipeline. "Open weight" is the accurate term; it's more open than an API-only model and less open than a fully reproducible release.
2. **License varies by version.** The headline modified-MIT terms are clearest for the K2 open-weight line. Later point releases have been reported differently across sources, so **check the specific license attached to each model version** before you build on it. When in doubt, read the model card.


This is also why the open-weight bet is more than ideology. When a model is downloadable, no single government or vendor decision can switch it off for you — a point that turned concrete in mid-2026, when[export-control action briefly suspended a US frontier model](https://www.taskade.com/blog/claude-fable-mythos) and self-hostable alternatives suddenly looked like insurance rather than idealism.


## Kimi vs DeepSeek: China's Two Open-Weight Leaders


Kimi and **DeepSeek** are the two names that come up whenever anyone discusses China's open-weight frontier, and the honest answer to "which is better" is **it depends on the job.**


Dimension Kimi (K3) DeepSeek (V4)


**Signature strength** Agentic coding, long-horizon autonomy Code generation, math reasoning


**Context window** Up to 1M tokens ~1M tokens


**Scale** 2.8T total (MoE) ~1.6T (Pro)


**Vibe** The agent that runs for hours The cheap, sharp coder


**License** Modified-MIT open weights MIT open weights


The productive takeaway isn't to crown a winner — it's that **you shouldn't have to pick permanently.** Teams increasingly keep both available and route each task to whichever model is strongest, a pattern we cover in[AI agent cost optimization](https://www.taskade.com/blog/ai-agent-cost-optimization) . For a head-to-head, see our[Kimi vs DeepSeek comparison](https://www.taskade.com/compare/kimi-vs-deepseek) and[Kimi vs Claude](https://www.taskade.com/compare/kimi-vs-claude) .


## Where Kimi Stands Against the Global Frontier


On the closed-vs-open axis, Kimi K3 is the strongest evidence yet that **the gap has nearly closed.** Independent reviewers who ran K3 through real agentic-coding and long-horizon build tasks at launch placed it at or near frontier level — competitive with the best proprietary models on many tasks, and clearly ahead of the previous best open model. Moonshot's own framing is measured: K3 "demonstrates frontier-level performance across their evaluation suite" while still trailing the very top proprietary systems on the hardest problems.


Two caveats keep this honest. First, **exact benchmark numbers are still settling** post-launch and diverge across sources, so we describe the *direction* (frontier-adjacent, best-in-open) rather than pin contested scores. Second, **capability isn't the same as reliability** — reviewers consistently noted K3 can be slow, token-hungry, and occasionally flaky on long runs. The frontier is close; the polish still favors the incumbents. That reliability-vs-capability gap is exactly the theme of[why AI-generated apps break](https://www.taskade.com/blog/why-ai-generated-apps-break) .


## Kimi's Products Today: Chat, Agents, and the API


Beyond the raw models, Moonshot ships a real product surface:


- **Kimi chatbot** at kimi.com — a free tier plus paid subscription tiers named after musical tempos: **Moderato, Allegro, Allegretto, and Vivace.**
- **Agent modes** — including an autonomous "computer-use" style agent and a desktop agent for longer, tool-using work.
- **The API** — priced (for K3) around $0.30 cached / $3 input / $15 output per million tokens, with open weights for teams that want to self-host.


For most people the fastest way to *use* Kimi isn't to stand up GPUs — it's to pick it inside a platform that already routes to it, which brings us to the part where this history becomes something you can act on.


## Use Kimi (and Any Frontier Model) Inside Taskade Genesis


Here's the connect-the-dots move. The lesson of the last three years — Kimi K2's open-weight breakout, K3 reaching the frontier, an export order briefly pulling a closed model overnight — all points to the same conclusion: **you don't want to be locked to one model.** You want the *workspace* to be the constant and the model to be swappable.


That's exactly how[Taskade Genesis](https://www.taskade.com/blog/living-software-era) works. It gives you **15+ frontier models from OpenAI, Anthropic, Google, and open-weight providers — including Moonshot's Kimi models** — selectable per agent. Route one agent to a Kimi model for long-context or agentic-coding work, another to a different model for reasoning, all in one place, with **Auto** picking a sensible default when you'd rather not choose.


Underneath, every AI agent gets **[34 built-in tools](https://www.taskade.com/agents)** , **persistent memory** , and **[100+ bidirectional integrations](https://www.taskade.com/automate)** — triggers pull events in, actions push data out. Your projects become the memory, your agents become the intelligence, your automations become the execution:


And because a workspace has[7 project views](https://www.taskade.com/ai/apps) , the same underlying data reshapes to whatever lens the task needs — no re-entry, no export:


```text
WHAT YOU'RE DOING            →   BEST TASKADE VIEW
─────────────────────────────────────────────────
Tracking model releases      →   Table
Planning a research sprint    →   Board
Mapping a model family tree   →   Mind Map
Scheduling a content series   →   Calendar
Sequencing a launch           →   Gantt (with Timeline)
Triaging tasks                →   List
Org-charting an AI team       →   Org Chart
Same data. Seven lenses. Zero re-entry.


```


[Build your own model-and-lab tracker from a prompt →](https://www.taskade.com/create) — describe it once, and Taskade Genesis builds it as living software you can clone, share, and keep updated.


## Controversies and Open Questions


No honest history skips the hard parts. A few worth naming:


- **Open weights vs. safety.** A downloadable frontier model can't be recalled. That's the whole appeal — and the whole worry. The debate over how open the frontier *should* be is genuine, and it's the same tension behind[AI guardrails](https://www.taskade.com/blog/ai-guardrails) and[Scientist AI](https://www.taskade.com/blog/scientist-ai-explained) .
- **How fast the valuation ran up.** Going from a 2023 seed to a ~$20B valuation in about three years is extraordinary — and, like every AI valuation right now, it prices in a future that hasn't fully arrived.
- **Benchmark trust.** Many of K3's headline numbers come from Moonshot's own slides or early third-party runs. The *direction* (open models reaching the frontier) is well-supported; specific leaderboard positions are not yet settled. Treat any single benchmark screenshot with the skepticism you'd apply to any vendor's launch chart.


## What's Next for Moonshot AI


Two things are on the near horizon: the **full Kimi K3 open weights (scheduled July 27, 2026)** , which will let the community host, fine-tune, and pressure-test the model directly; and continued speculation about a **public listing** as the company matures. The bigger arc is the one Yang keeps returning to — that open models won't just catch the frontier, they'll *set* part of it:


> "Agent swarms is not the end. And we are glad that we can move forward with the entire open source community to achieve better and better intelligence."
>
>
> — Yang Zhilin, GTC 2026 keynote


Whether or not any single benchmark holds, the structural point stands: **the frontier is becoming a commodity input.** The durable advantage is moving up a layer — to the workspace, the memory, and the routing that decide *which* model runs *which* task.


> ▲ ■ ● Moonshot builds the models. **You** build the trackers, research workspaces, and apps that run on them. Don't just read a Kimi K3 story — clone a living AI-lab tracker, point it at any company, and own a research workspace in minutes. Memory feeds Intelligence, Intelligence triggers Execution.[Clone the live tracker →](https://www.taskade.com/share/apps/tmnju1vsp3ggajo7) or[build your own from a prompt →](https://www.taskade.com/create) .


## 🔗 Related Reading


**AI labs & model history:**


- [Open-source LLM history: from GPT-2 to Kimi K3](https://www.taskade.com/blog/open-source-llm-history)
- [The 10 best open-source LLMs, ranked](https://www.taskade.com/blog/open-source-llms)
- [Anthropic & Claude history](https://www.taskade.com/blog/anthropic-claude-history)
- [OpenAI & ChatGPT history](https://www.taskade.com/blog/openai-chatgpt-history)
- [Google Gemini history](https://www.taskade.com/blog/google-gemini-history)
- [NVIDIA & Jensen Huang history](https://www.taskade.com/blog/nvidia-jensen-history)


**How the tech works:**


- [How large language models work](https://www.taskade.com/blog/how-do-llms-work)
- [What are reasoning models?](https://www.taskade.com/blog/reasoning-models)
- [What are world models?](https://www.taskade.com/blog/ai-world-models)
- [AI thinking modes explained](https://www.taskade.com/blog/ai-thinking-modes-explained)
- [Context engineering](https://www.taskade.com/blog/context-engineering)


**Agents & building:**


- [Best multi-agent platforms](https://www.taskade.com/blog/best-multi-agent-platforms)
- [AI agent cost optimization](https://www.taskade.com/blog/ai-agent-cost-optimization)
- [What is an AI agent harness?](https://www.taskade.com/blog/agent-harness-explained)


> 🐑 **Before you go** — the frontier is a moving target, but your workspace doesn't have to move with it. Inside[Taskade Genesis](https://www.taskade.com/create) you get:
>
>
> - **15+ frontier models** (Kimi, Claude, GPT, Gemini, and more), selectable per agent
> - **34 built-in tools** and persistent memory on every AI agent
> - **100+ bidirectional integrations** to automate the busywork
> - **7 project views** on the same data — no re-entry
>
>
> [Start free →](https://www.taskade.com/create) ·[Explore ready-made AI apps →](https://www.taskade.com/community)


## 💬 Frequently Asked Questions About Moonshot AI & Kimi


What is Moonshot AI?


Moonshot AI is a Chinese AI company founded in March 2023 in Beijing, best known for the Kimi family of large language models. It's one of China's "six AI Tigers" and the lab that bet hardest on open weights, culminating in the 2.8-trillion-parameter Kimi K3 in July 2026.


Who founded Moonshot AI?


Yang Zhilin (CEO), Zhou Xinyu, and Wu Yuxin founded the company in March 2023. Yang holds a PhD from Carnegie Mellon and co-authored the influential Transformer-XL and XLNet papers, the latter of which beat Google's BERT on 20 tasks in 2019.


Why is it called Moonshot and Kimi?


The Chinese name 月之暗面 means "Dark Side of the Moon," a Pink Floyd reference; "Moonshot" is the English rendering of an audacious goal. "Kimi" is founder Yang Zhilin's English nickname, which became the name of the chatbot and model family.


What was Kimi K2 and why did it matter?


Kimi K2 (July 11, 2025) was a 1-trillion-parameter open-weight mixture-of-experts model that delivered near-frontier agentic and coding performance you could download and run. It was widely called an "open-weight DeepSeek moment" and made Moonshot a global name.


What is Kimi K3?


Kimi K3 (July 16, 2026) is Moonshot's flagship: a 2.8-trillion-parameter MoE model activating ~16 of 896 experts per token, with native vision and a 1-million-token context window. The API launched immediately; full open weights were scheduled for July 27, 2026.


Is Kimi open source or open weight?


Open weight. The K2 family shipped under a modified-MIT license — you can download and fine-tune the model — with an attribution clause for very large commercial products. The training data and full pipeline aren't released, so "open weight" is more accurate than "fully open source," and the license can vary by version.


How much is Moonshot AI worth?


A 2026 round valued the company at roughly $20 billion, with nearly $4 billion raised in total. Alibaba is the largest outside backer, with an estimated 36% stake. AI valuations move fast, so treat that figure as a dated snapshot.


Is Kimi better than DeepSeek or Claude?


It depends on the task. Kimi K3 leans into agentic coding and long-horizon autonomy; DeepSeek is prized for cheap code and math; Claude and other proprietary models still lead on the hardest reasoning and on polish. The practical move is to keep several available and route per task — which is exactly what Taskade Genesis lets you do.


Can I use Kimi inside Taskade?


Yes. Taskade Genesis offers 15+ frontier models including Moonshot's Kimi models, selectable per agent, with Auto as a sensible default. Route long-context work to Kimi and reasoning to another model, all in one workspace, on a free tier with paid plans from $10/month billed annually.
