---
schema_version: "1.0.0"
document_id: "0968b0c2bb42b0f2946d6872bd8194f314e0897e53bcd37ac565e8358e024534"
company_key: "yc-taskade"
company: "Taskade"
source_id: "yc-taskade-rss-a662ed9a0141"
canonical_url: "https://www.taskade.com/blog/open-source-llm-history"
published_at: "2026-07-25T08:00:00+00:00"
first_seen_at: "2026-07-25T08:57:14.187719+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:824375f790c1bbbf66211a8dac0b0f33a9c10f0ce2f2a7a6a0a2ef949b5f4023"
---

# Open-Source LLM History: GPT-2 to Kimi K3 (2026)

[Blog](https://www.taskade.com/blog)


[AI](https://www.taskade.com/blog/ai)


Open-Source LLM History:…


On this page (20)


Open-source large language models went from "too dangerous to release" to **larger and more capable than most people thought possible in the open** — in about six years. In 2019, OpenAI held back the full weights of GPT-2, a 1.5-billion-parameter model, worried it was too risky to publish. In July 2026, a Chinese lab shipped **Kimi K3, a 2.8-trillion-parameter open-weight model** at frontier level — released in the open, its full weights set to follow within days for anyone to download.


How did we get from one to the other? Which models actually mattered, who built them, and what's the real difference between "open source" and "open weight" — a distinction almost every article gets wrong? This is the complete timeline. 🧬


> **TL;DR:** Open-source LLMs evolved from **GPT-2's cautious 2019 release** → the truly-open era of **EleutherAI and BLOOM (2021–22)** → **Meta's LLaMA (2023)** detonating the ecosystem → **Mistral** bringing mixture-of-experts to open weights → **DeepSeek's 2025 cost shock** → a Chinese open-weight surge ( **Qwen, Kimi, GLM** ) that shrank the gap to the closed frontier to a matter of months. Most "open" models are technically **open weight** , not fully open source.[See the 10 best open-source LLMs ranked →](https://www.taskade.com/blog/open-source-llms)


## What "Open-Source LLM" Actually Means (and Why "Open Weight" Is More Accurate)


Most models people call "open source" are actually **open weight** — a crucial difference. An **open-weight** model releases its trained parameters so you can download, run, and fine-tune it. A truly **open-source** model, by the[Open Source Initiative's](https://opensource.org/ai) 2024 definition, releases the weights *plus* the training code, information about the data, and a license with no usage restrictions.


The Linux Foundation's **Model Openness Framework** — created by its Global CTO of AI, Matt White — grades this on three levels, and it's the cleanest way to think about the spectrum:


Level What's released Term people use Examples


**Open Science (Class I)** Weights + code + raw training data + checkpoints + paper Fully open source BLOOM, OLMo, LLM360


**Open Tooling (Class II)** Weights + full training/eval code, no raw data Open source (loosely) Some research releases


**Open Model (Class III)** Just the weights + light docs **Open weight** Llama, DeepSeek, Qwen, Kimi, GLM


Almost every "open" model you've heard of — Llama, DeepSeek, Qwen, Kimi — sits in that bottom row: **open weight** . That's still enormously valuable (you can run and adapt them), but it's not the same as fully reproducible. Matt White's own worry, after touring China's labs in 2026, was about disclosure shrinking:


> "The research papers have shrunk and then become technical reports. And so there's not enough disclosure in a lot of these technical reports to be able to go and replicate the research."
>
>
> — Matt White, Linux Foundation


*Don't just read the open-source timeline — clone a living dashboard that tracks every model, license, and benchmark in minutes. One click, no code, free.*


## A Visual Timeline of Open-Source LLMs (2019 → 2026)


Here's the whole arc at a glance. Each era changed what "open" could mean — from a cautious weights-withheld release to trillion-parameter models anyone can download.


> **Last updated: July 2026** — refreshed for Kimi K3 (July 16, 2026) and the July 27 open-weight drop, plus DeepSeek V4 and the latest Qwen and GLM releases. Model dates, parameter counts, and licenses move fast and vary by source; where figures are disputed we say so. Check[Wikipedia](https://en.wikipedia.org/wiki/Llama_(language_model)) and each vendor's model card before quoting exact specs.


## 2019: GPT-2 and the Cautious Birth of Open Models


The modern open-model story starts with a decision *not* to release. In February 2019, OpenAI announced **GPT-2** — a 1.5-billion-parameter model — but held back the full weights, publishing a paper titled *Release Strategies and the Social Impacts of Language Models* and worrying openly that the model could be used to mass-produce fake text. It released the full 1.5B weights only in **November 2019** , after a staged rollout showed the feared harms didn't materialize at scale.


The myth-correction matters: **GPT-2 was not "fully open" at launch.** It was a careful, staged release — the first time a major lab treated a language model as something whose openness was a *policy question* , not a default. That framing has shaped every open-vs-closed debate since. (If you want the mechanics of how these models work at all, our explainer on[how large language models work](https://www.taskade.com/blog/how-do-llms-work) is the primer.)


## 2021–2022: The Truly-Open Era — EleutherAI, OPT, and BLOOM


The first genuinely open GPT-3-class models didn't come from a big lab — they came from a volunteer collective. **EleutherAI** released **GPT-Neo** (March 2021) and **GPT-J-6B** , then **GPT-NeoX-20B** (2022), reverse-engineering GPT-3-style capability and publishing everything. For the first time, anyone could download a large, capable model and its code.


Two 2022 releases pushed openness to its high-water mark:


- **Meta OPT-175B** — Meta's attempt to open-source a GPT-3-scale model, released with training logs and a research license.
- **BLOOM** — the crown jewel of the truly-open era. Built by **BigScience** , a collaboration of **over 1,000 researchers** , BLOOM was a **176-billion-parameter** model trained on **46 natural languages and 13 programming languages** , released with open weights, code, and full documentation. By the Model Openness Framework, BLOOM is one of the few models that reaches "Open Science" — the genuinely fully-open tier.


This era proved the community *could* build at scale. What it hadn't yet proved was that open models could be genuinely *useful* . That changed in 2023.


## 2023: Llama Changes Everything (and the "Open" Debate Begins)


**Meta's LLaMA** is the single most important release in this history, because it turned open weights into an *ecosystem* . **LLaMA 1** (February 24, 2023) came in 7B to 65B sizes and — after the weights leaked online — detonated a wave of community fine-tunes (Alpaca, Vicuna, and hundreds more). **Llama 2** (July 2023) made it official, shipping with a license that permitted commercial use.


But Llama also started the argument that still runs today.


### Is Llama Really Open Source?


**No, not by the strict definition.** The Open Source Initiative classifies Llama as **source-available** or **open weight** , not open source, for two reasons: the **Llama Community License** restricts use above **700 million monthly active users** , and it ships with an acceptable-use policy plus undisclosed training data. Critics call this "openwashing" — marketing a restricted release as open. Defenders point out that for 99.9% of developers, Llama was *effectively* open and did more to democratize LLMs than any purely-open model. Both are true, which is why the debate never fully resolves.


## 2023–2024: Mistral and the Mixture-of-Experts Moment


A French startup made the next leap. **Mistral AI** , founded by ex-Meta and DeepMind researchers Arthur Mensch, Guillaume Lample, and Timothée Lacroix, released **Mistral 7B** (September 2023) under the fully-permissive **Apache 2.0** license — no MAU clause, no strings. Then came **Mixtral 8x7B** (December 2023), which brought **mixture-of-experts (MoE)** to open weights: instead of running the whole model for every token, MoE activates only a few specialist "experts," delivering big-model quality at small-model cost.


MoE is the architecture that made everything after it possible. Every trillion-parameter open model since — DeepSeek, Kimi, GLM — is an MoE. Mistral proved you could ship it in the open, and that Europe had a frontier lab of its own.


## 2024: The Field Scales Up and Matures


By 2024 the open ecosystem professionalized. Meta shipped **Llama 3, 3.1 (a 405-billion-parameter flagship), 3.2 (multimodal), and 3.3** . Google entered with **Gemma** . And **Alibaba's Qwen** family began its rise to become the most prolific open-weight lineage of all — dozens of models across sizes and specialties under Apache 2.0.


## January 2025: The DeepSeek Shock


**DeepSeek R1** , released **January 20, 2025** , is the moment open weights went from "impressive" to "geopolitically significant." Built by a lab spun out of the Chinese hedge fund High-Flyer and led by **Liang Wenfeng** , R1 was a **671-billion-parameter MoE** reasoning model, released under the permissive **MIT license** , that matched top proprietary reasoning models on math and code — and briefly rocketed DeepSeek's app to **#1 on the US App Store** .


### How Much Did DeepSeek Cost to Train?


DeepSeek reported that the core training run cost **under $6 million** on roughly **2,000 Nvidia H800 GPUs** over about two months. That figure covers the final run, not the full research and infrastructure bill, so it understates the true investment — Anthropic's CEO argued DeepSeek "were not substantially more resource-constrained than US AI companies." But even discounted, it reset expectations about what frontier-class training *had* to cost.


### Is DeepSeek Open Source?


**The newer generation, largely yes.** DeepSeek's **V4** models ship both code and weights under **MIT** , permitting commercial use — one of the most permissive frontier-class releases anywhere. Earlier V3 and R1 releases used MIT for code but a more restrictive model license, which drew the same "is it really open?" scrutiny Llama faced. The trajectory has been *toward* openness, and DeepSeek's low-cost, MIT-licensed models pushed the whole market's prices down.


## 2025–2026: China's Open-Weight Surge — Qwen, Kimi, GLM, DeepSeek V4


Through 2025 and into 2026, the center of gravity for *open-weight* releases shifted markedly toward Chinese labs, each shipping under permissive licenses:


Lab Family License Signature strength


**Alibaba** Qwen (Qwen3, Qwen3-Coder) Apache 2.0 Most prolific; coding + multilingual


**DeepSeek** V4-Pro / V4-Flash MIT Cheap reasoning + code


**Moonshot AI**[Kimi K2 → K3](https://www.taskade.com/blog/moonshot-kimi-history) Modified MIT Agentic coding, long context


**Zhipu** GLM MIT Broad reasoning


The capstone landed in July 2026: **Kimi K3** , a **2.8-trillion-parameter** open-weight model — the **largest open-weight model to date** — with native vision and a 1-million-token context window, released July 16 with full weights scheduled for July 27. A model that big, released in the open, would have been unthinkable when GPT-2's 1.5B weights were considered too risky to publish six years earlier. (The full Moonshot story is worth its own read:[Moonshot AI & Kimi history](https://www.taskade.com/blog/moonshot-kimi-history) .)


## Why Are Chinese Open-Source Models Winning? The Debate


This is the most-discussed — and most-contested — question in the field, so let's be precise about what's argued versus what's settled.


**The popular thesis:** US GPU export controls (restricting the best chips to Chinese buyers) pushed Chinese labs toward **software-efficiency breakthroughs** — mixture-of-experts, latent attention, and cheaper training recipes — because "when you can't throw more compute at a problem, you find smarter solutions" (Matt White's framing). Layer on a culture of publishing and a **"borrow-and-build" chain** — one lab's technique (like DeepSeek's GRPO reinforcement-learning method) gets adopted and improved by the next — and you get rapid, compounding open progress.


**The dissent is serious and named.** Analysts including Gregory Allen (CSIS) argue that *motivation* to localize is not the same as *acceleration* , and that some Chinese chip-independence efforts predate the controls. Chris McGuire (CFR) argues the US chip lead is actually *widening* . And Anthropic's Dario Amodei argues DeepSeek's efficiency was "an expected point on an ongoing cost-reduction curve," not a resource-constraint miracle.


A careful reading of the download data supports humility on both sides. By one 2026 analysis, Chinese models edged ahead of US models in the share of downloads *of newly released models* — but that measures a rolling 12-month window, and on an all-time basis US models still dominate downloads. The honest summary: **Chinese labs drove much of the open-weight momentum in 2025–26, the technical contributions are real, and the geopolitical cause is genuinely debated.** (And a note on attribution: the widely used **Muon** optimizer originated in the international open-source community, not with any single lab or country — cite ideas by their lineage, not a flag.)


## Open vs Closed: How Big Is the Gap in 2026?


The gap has never been smaller. On public benchmarks, the best open-weight models trail the top proprietary systems by a **small and shrinking margin** — often described in 2026 as a matter of a few months rather than the year-plus gap of 2023. Independent reviewers who tested Kimi K3 at launch placed it at or near frontier level for agentic coding, and one summed up the moment bluntly: *"Open-source AI is no longer a few months behind the closed frontier models."*


Two honest caveats keep this from becoming hype. First, **the very hardest reasoning tasks** still tend to favor the top proprietary models. Second, **capability is not reliability** — open frontier models can be slower, more token-hungry, and flakier on long runs than their polished closed rivals. The gap in raw capability is nearly closed; the gap in *dependability* is still real, and it's the reason[AI-generated apps break](https://www.taskade.com/blog/why-ai-generated-apps-break) more often than their demos suggest.


## The Best Open-Source LLMs to Use Right Now


If you want a *ranked* answer to "which open model should I use in 2026," we keep a dedicated, continuously-updated guide: **[the 10 best open-source LLMs](https://www.taskade.com/blog/open-source-llms)** . The short version by job:


- **Coding:** Qwen3-Coder and the Kimi K2/K3 line lead agentic coding.
- **Reasoning and math:** DeepSeek V4 and the R1 lineage are the value picks.
- **Long context:** Kimi (1M tokens) and Llama 4 Scout (very long windows).
- **Running locally:** small Gemma, Qwen, and Llama variants fit a single GPU.


The larger point: you rarely want just one. The most effective setup keeps several models available and **routes each task to the best one** — a strategy we break down in[AI agent cost optimization](https://www.taskade.com/blog/ai-agent-cost-optimization) .


## Open-Source LLM License Cheat-Sheet


Licenses are where "open" gets real. Here's the current lay of the land (always verify the specific version's model card — licenses change within families in both directions):


```text
MODEL FAMILY        LICENSE               COMMERCIAL USE
──────────────────────────────────────────────────────────
Qwen                Apache 2.0            Yes, unrestricted
Mistral / Mixtral   Apache 2.0            Yes, unrestricted
DeepSeek (V4)       MIT                   Yes, unrestricted
GLM (Zhipu)         MIT                   Yes, unrestricted
Kimi (K2 family)    Modified MIT          Yes; attribution above
100M MAU / $20M/mo rev
Llama               Llama Community       Yes; restricted above
700M MAU + use policy
BLOOM               OpenRAIL              Yes; use restrictions
Rule of thumb: Apache 2.0 and MIT = the freest. Everything
else has a clause worth reading before you build on it.


```


## How to Use Open Models Without the Setup Headache


Here's the practical payoff of this whole history. You don't have to pick one open model, and you certainly don't have to rent GPUs to use them. The lesson of the last six years — GPT-2's caution, Llama's ecosystem, DeepSeek's cost shock, Kimi K3 in the open — is that **the frontier is becoming a commodity input.** The durable advantage is the layer that lets you *swap* models freely.


That's how[Taskade Genesis](https://www.taskade.com/blog/living-software-era) works. It gives you **15+ frontier models from OpenAI, Anthropic, Google, and open-weight providers — including DeepSeek, Qwen, and Moonshot's Kimi** — selectable per agent, with **Auto** picking a sensible default. Route cheap high-volume work to an open-weight model and hard reasoning to a premium one, all in one workspace, no lock-in.


Every AI agent gets **[34 built-in tools](https://www.taskade.com/agents)** , persistent memory, and **[100+ bidirectional integrations](https://www.taskade.com/automate)** . Your projects become the memory, your agents the intelligence, your automations the execution:


[Build your own AI app from a prompt →](https://www.taskade.com/create) — describe it once, and Taskade Genesis builds it as living software you can run on whichever models you like.


## What's Next for Open-Source LLMs


Three trends are already visible. **Trillion-parameter open models are becoming normal** — Kimi K3 (2.8T) and DeepSeek V4-Pro (~1.6T) put frontier-scale weights in the open. **The gap keeps shrinking** toward a few months, which changes the economics of building on open models. And attention is moving from raw chat quality to **agentic and tool-use capability** — models that can run long, multi-step tasks — plus **on-device** inference for privacy. The through-line: openness compounds, and the community keeps closing the distance.


> ▲ ■ ● The labs build the models; **you** build the apps that run on them. Don't just read the open-source timeline — clone a living model tracker, point it at any lab, and own a research workspace in minutes. Memory feeds Intelligence, Intelligence triggers Execution.[Clone the live tracker →](https://www.taskade.com/share/apps/tmnju1vsp3ggajo7) or[build your own from a prompt →](https://www.taskade.com/create) .


## 🔗 Related Reading


**Models & labs:**


- [The 10 best open-source LLMs, ranked](https://www.taskade.com/blog/open-source-llms)
- [Moonshot AI & Kimi history](https://www.taskade.com/blog/moonshot-kimi-history)
- [Anthropic & Claude history](https://www.taskade.com/blog/anthropic-claude-history)
- [OpenAI & ChatGPT history](https://www.taskade.com/blog/openai-chatgpt-history)
- [Google Gemini history](https://www.taskade.com/blog/google-gemini-history)
- [NVIDIA & Jensen Huang history](https://www.taskade.com/blog/nvidia-jensen-history)


**How it works:**


- [How large language models work](https://www.taskade.com/blog/how-do-llms-work)
- [What are reasoning models?](https://www.taskade.com/blog/reasoning-models)
- [What are world models?](https://www.taskade.com/blog/ai-world-models)
- [What is mechanistic interpretability?](https://www.taskade.com/blog/what-is-mechanistic-interpretability)
- [The history of computing](https://www.taskade.com/blog/history-of-computing)


> 🐑 **Before you go** — you don't have to choose one model. Inside[Taskade Genesis](https://www.taskade.com/create) you get:
>
>
> - **15+ frontier and open-weight models** (DeepSeek, Qwen, Kimi, Claude, GPT, Gemini)
> - **34 built-in tools** and persistent memory on every AI agent
> - **100+ bidirectional integrations** to automate the busywork
> - **Auto model routing** — the right model per task, no lock-in
>
>
> [Start free →](https://www.taskade.com/create) ·[Explore ready-made AI apps →](https://www.taskade.com/community)


## 💬 Frequently Asked Questions About Open-Source LLMs


What was the first open-source LLM?


There's no clean answer. GPT-2 (2019) is often cited, but its full weights were withheld until November 2019, so it wasn't fully open at launch. EleutherAI's GPT-Neo and GPT-J (2021) were the first genuinely open GPT-3-class models, and BLOOM (2022) was the most fully open early release.


What's the difference between open source and open weight?


Open weight means you get the downloadable parameters. Open source (by the OSI's 2024 definition) means weights plus training code, data information, and an unrestricted license. Most popular "open" models — Llama, DeepSeek, Qwen, Kimi — are technically open weight.


Is Llama open source?


Not strictly. The OSI classifies it as source-available/open weight because of the 700-million-MAU clause, an acceptable-use policy, and undisclosed training data — though it's freely usable for almost everyone.


Is DeepSeek open source?


The V4 generation ships code and weights under the permissive MIT license, so largely yes. Earlier V3 and R1 releases had a more restrictive model license. The trend has been toward greater openness.


What's the largest open-source LLM?


Kimi K3 (2.8 trillion parameters, July 2026) is the largest open-weight model to date, followed by DeepSeek V4-Pro (~1.6 trillion). Both are mixture-of-experts, so only a fraction of the parameters run per token.


Are open models as good as GPT-5 or Claude?


On many tasks, yes, and the gap is down to months. The top proprietary models still lead on the hardest reasoning and on reliability. Most teams use both and route per task.


Can I run these locally?


Small open-weight models run on a single GPU or laptop; the biggest (like Kimi K3) need data-center hardware even with open weights. Managed platforms like Taskade Genesis let you use them without any setup.


Who makes the top open models?


Meta (Llama), Mistral, DeepSeek, Alibaba (Qwen), Moonshot (Kimi), and Zhipu (GLM), plus pioneers EleutherAI, BigScience (BLOOM), and Google (Gemma).
