---
schema_version: "1.0.0"
document_id: "58a6c9b3eda52899c035f358daf6340d3239aaeaa697267f17b344cdc0cf2221"
company_key: "galaxy-digital-inc-class-a-common-stock"
company: "Galaxy Digital Inc."
source_id: "galaxy-digital-inc-class-a-common-stock-news-import-39d9e80cc0e4"
canonical_url: "https://www.galaxy.com/insights/perspectives/inference-market-and-micro-structure"
published_at: null
first_seen_at: "2026-08-12T21:59:30.362897+00:00"
fetched_at: "2026-08-12T21:59:35.658729+00:00"
content_hash: "sha256:68d279243bc5749bc3f0f459273bd960e033007df7fac8691bd4f6f73a23dcb9"
---

# Inference Market (and Micro) Structureim

### TLDR


-


**Intelligence is commoditizing through open-source models.** Open source models are almost at parity with the frontier and usage is up 350× since January 2025. Since serving inference for open-source models is permissionless, a competitive market for inference is emerging.


-


**The inference “market structure” is somewhat like a trading venue.** There are buyers of tokens (Cursor, Lovable, enterprises), a marketplace (OpenRouter), and sellers of tokens (Fireworks, Baseten, Together AI) quoting a one-sided order book on a model’s tokens.


-


**Inference providers are like market makers:** they quote continuously, hold inventory in the form of compute, earn a spread, and vie for token flow. **The marketplace is like an exchange:** OpenRouter earns a “matching” fee (5.5% take rate) with potentially room to charge the “market makers” for token flow PFOF.


-


**GPU-hours are the homogenous input** into producing every token and serve as the layer for transferring risk for inference token production. Compute futures emerge as a mechanism to manage the 130% volatility of GPU-hours and to protect inference provider’s COGS


A few trends are playing out in real time:


-


**Intelligence is commoditizing and democratizing rapidly.** Since GPT-4, no frontier model has held the lead for more than 100 days. New models are shipping every 41 days . Open source models are catching up to the best frontier lab models within months vs years. This changes frontier labs economics substantially since they only have 4 months to amortize training capex before spend migrates towards open source models.


Source: Epoch AI's ECI benchmark data\[2\] (CC-BY, credit Epoch AI) Source: Epoch AI's benchmark data\[3\] (CC-BY, credit Epoch AI)


-


**Jevons’ Paradox is playing out in real time.** Cost savings achieved by switching from frontier models to open-source models are being reinvested into more token spend. Thus, token consumption compounds even if budgets stay flat. This is evidenced through OpenRouter routing 125× more tokens last week than it did in January 2025 with 75% of volume being open source models vs 27% earlier.


Source: OpenRouter datasets API, as of Jul 22 2026


-


The proliferation of open source models is leading to higher short-to-medium term GPU rentals. **Utilization and rental prices have shot up** across the Ornn H100 index after the recent open source model releases


Source: OpenRouter’s weekly rankings , Ornn’s H100 SXM index.


-


Venture investment in inference providers, GPU rental marketplaces, and routing platforms has **heated up intensely** over the last three months.


With token flows migrating to open-source models, token buyers growing more price-conscious, and inference being permissionless, a competitive market for serving inference is emerging. Competitive markets tend to develop mature market structures. The inference serving stack roughly resembles a trading venue and a deeper exploration reveals some interesting intricacies.


### Inference Market Structure


There are three core participants in the inference trading analogy: Buyers of Tokens, Sellers of Tokens, and a Marketplace.


Source: https://openrouter.ai/docs/faq), (https://sacra.com/c/fireworks-ai/)


**Customers/Buyers of Tokens**


Consumers chatting with ChatGPT, engineers running Cursor, apps like Lovable and Harvey serving models behind their own features, agent fleets doing background research, and enterprises wiring LLM APIs into production workflows – all are buyers of tokens. These users are split across a distribution of token-price elasticity:


-


15% are highly elastic. For e.g., Developer experimentation


-


25% medium elasticity. For e.g., Consumer chat


-


40% low elasticity. For e.g., Enterprise SaaS


-


5% price inelastic. For e.g., Real-time response needs


Source: arXiv 2603.21690


Normalizing for the model used, elasticity is somewhat a function of who waits on the inference response. Human-facing demand is inelastic while agent-originated batch demand is elastic. For example, identical tokens cost half as much if you accept a 24-hour batch window instead of a synchronous reply.


The aggregate numbers overstate how sticky demand is: a 10% price cut buys only a 0.5–0.7% bump in usage, but that average blends enterprises that will pay almost anything for mission-critical work with hobbyists and dev pipelines that flock to whatever is cheapest. Closed models serve the trusted high-cost work and open models absorb the cheap high-volume flow.


As such, the average token is not elastic, but the marginal token is very elastic. Most of the volume already flowing today is bought by a person waiting on the answer: a chat reply, a Cursor completion. That buyer barely notices price and nobody switches models mid-conversation to save cents. However, the next phase of token-demand is bought by software: agent loops, batch research jobs, CI pipelines. Thus, the marginal token satisfies an objective function that maximizes quality while minimizing cost and one in which times plays a lesser role. OpenRouter already optimizes for this future demand as its default router splits a model’s traffic across providers weighted by 1/price² . Thus, a provider that halves its quote roughly quadruples its share of the flow. And since over 70% of routed tokens already come from CLIs and agents, almost all incremental token demand is likely elastic demand.


### Inference Providers/Sellers of Tokens


These are your sellers of tokens (Fireworks, Baseten, Together AI, and so forth). They burn GPU-hours to produce tokens and quote their token prices on the marketplace. Inference providers can differ on several price-based and performance-based axes. We will discuss them more in depth in the microstructure section.


### Marketplace/Exchange


This platform connects token buyers with inference providers and charges a fee for offering that service. For tokens, the clearest example here is OpenRouter.


However, the token marketplace differs from a trading venue in that it’s not two-sided. You can buy tokens from an inference provider but you cannot sell them back to the provider or to any third party. This is inherent to the nature of the token which is manufactured on demand and consumed on delivery, which leaves nothing behind to warehouse or resell. A financial product that structurally cannot have a bid-side has limited avenues for price discovery.


So, what does the absence of price discovery look like in practice?


Below is OpenRouter's live provider board for DeepSeek V4 Pro. One open model, eighteen sellers, quoted simultaneously:


Three things stand out:


-


**Firstly, token quotes cluster at an administered anchor.** At the bottom of the board sit five providers (Weights & Biases, Together, Fireworks, Baseten, and Parasail) quote the exact same price: $1.74/$3.48. The same pattern holds on every flagship open model. On Kimi K2.6, seven hosts quote exactly $0.95/$4.00 (including Moonshot itself):


And on GLM-5.2, six hosts quote exactly $1.400 (including Z.ai) while six more run promotional discounts off that same administered price.


Interestingly, the first-party prices are not always the floor. DeepSeek posts the cheapest V4 Pro endpoint, but on K2.6, GLM-5.2, and V4 Flash, third parties undercut the anchor.


-


**Secondly, prices can range highly and delivery quality remains questionable.** In the DeepSeek example, the quotes span from $0.435 to $1.74 (a 4x gap) across 17 providers. Disclosures around quantization are self-reported and often incomplete (3/5 hosts at the same price list don’t list their quantization). Thus, a cheap quote can be confused for a degraded model vs. an efficient provider for new entrants.


-


**Thirdly, mechanisms for price convergence do not exist in token-land.** Since the order book is one-sided and secondary marketplaces don’t exist tokens cannot be resold and there are no venues for arbitrage.


Thus, tokens themselves are not tradeable today but exploring the microstructure for all parties in a token transaction reveals a two-sided marketplace somewhere else.


### Inference Market Microstructure


By microstructure, we mean the mechanics of the token transaction itself: what instrument each participant actually holds, how their margins are constructed, and what risks sit on their “books.”


### Buyers


Margin=Value created from token use-Token Cost


Buyers operate on a value-based business model. They ingest tokens and output a product of some value. Thus to maximize their margins, they must optimize for lower token costs. They are beginning to do this by:


-


**Efficient model routing:** Coinbase has saved 90% of their dollar spend on tokens by routing prompts to OSS models , holding AI costs flat while growing token usage exponentially. Lindy cut 100% of its traffic over to DeepSeek and saved millions. Cognition and Factory already offer model routing within their harness and Cursor launched their own router yesterday to offer users 60% costs savings. This suggests, OSS models are capturing “token flow” from the frontier labs for non high-value tasks.


-


**Restructuring workloads:** Batch jobs price cost half the live price , and caching helps cut costs by 60-80% for repeated prompts. For instance, Sail Research looks to solve for longer-running background agents and claims up to 12× more tokens per dollar for work that can wait minutes instead of milliseconds.


-


**Professionalizing procurement:** Routing policies, model-mix budgets, and substitution playbooks are emerging as COGS management tools. Cloud spend experienced a similar transformation when AWS bills got big enough and FinOps teams emerged to manage spend. Token spend is undergoing a similar transformation


### Exchange


An exchange’s margin looks like:


Margin=Spread+Fees+Axuillary Sources


For token marketplaces like OpenRouter, the spread is non-existent since there is no markup — provider prices are passed through at list. However, OpenRouter charges a 5.5% fee when you buy credits – $1B in GPV accrues $55M in net revenue.


The 5.5% is a convenience fee. OpenRouter offers model aggregation (sign up for one account, use 400+ models), easier billing, failover guarantees, strong UI/UX, and most recently it introduced intelligent model selection through openrouter/auto (similar to the model router Coinbase built internally). One of the core premise of using OpenRouter is because as a user you get all capacity available. Fallback is a big value proposition. Even if an app or company builds internal routing or aggregation themselves, providers would prioritize flow from OpenRouter.


Since aggregators/marketplaces own the end user experience, they can extract those rents from both sides of the table – not only the buyers. As such, OpenRouter may charge a tax on the inference providers as well and charge them a share of their margins for providing them token flow – similar to how Robinhood charges Citadel for order flow.


That said, switching costs for OpenRouter are high but not insurmountable. $10M of token spend involves a $550k rent to OpenRouter. An exchange charging 550bps per trade while taking on zero risk sounds a bit egregious and preserving such high margins while taking no principal risk may be difficult. It is not farcical to imagine loss-leading competitors arising but they will have to compete on UX.


That said, there are ways for the exchange to preserve its take. Token quality is invisible at the point of purchase and OpenRouter is the intermediated trusted party between the buyers and sellers that records quantization, uptime, latency, throughput, and data usage policies across every inference provider. It can charge rent for that trust and for verification similar to how a ratings agency does.


### Inference Providers


Inference providers like Baseten, Fireworks, and Together AI own or rent GPUs and run model inference on them. Most inference providers can only run open-source models, while Anthropic and OpenAI inference can only be provided by the labs themselves and Infra-as-a-Service partners such as Amazon Bedrock and Google Vertex.


The margins for an inference provider look like:


PNL=Proceeds from tokens sold-Cost to produce tokens


A market maker quotes prices continuously, carries inventory, and earns a spread between what its inventory costs and what market orders pay. Similarly, an inference provider quotes prices for tokens they produce, holds token-production inventory in the form of GPU-hours, and earns the spread between what it costs for them to “make” a token and what the market pays for them.


Different inference providers also carry different “grades” of tokens. The same model can differ across providers on the non-price axes:


-


Quantization (FP8 vs FP4 vs INT8): Fewer bits per weight is cheaper and faster to serve, at some cost to model quality.


-


Time to first token (TTFT): How long before the reply starts. Important for human-facing chat


-


Tokens per second (TPS): How fast the reply streams. Important for agentic workloads.


-


Uptime: Provider blackout during urgent requests can be quite detrimental


For an inference provider, revenue is flow × price, and flow on a marketplace goes to the most competitive quote (sorted by 1/price² on OpenRouter). Defending margin at a competitive quote thus requires the lowest cost to produce. Since the cheapest quote gets picked off first on OpenRouter’s marketplace, loss-leader dynamics might naturally evolve if normalized for quality.


**To some extent, inference providers run a crack spread strategy on GPU-hours.** In oil markets, a refiner’s “crack spread” is the margin between the crude it buys and the refined products (gasoline, diesel) it sells. The refiner is long crude, short products, and earns whatever the refinery in the middle can extract between the two prices. Similarly, inference providers are long GPU-hours forward, short tokens spot, with the serving stack acts as the refinery in between.


We can write the inference provider’s margin per GPU-hour as:


m=p⋅R⋅u-c


where:


-


p is the token price


-


R is the refining yield (tokens produced per GPU-hour)


-


u is utilization


-


c is the GPU-hour cost


It’s helpful to understand the above with an example. Let’s freeze our GPU/hr cost to $2.74/hr (Ornn H100 price on July 21) . A provider renting at the index and serving 1,000 tokens per second at $0.90 per million token produces 3.6M tokens per hour at full utilization and earns $3.24/hr against $2.74/hr of rent: a spread of +$0.50/hr. At 50% utilization, revenue halves to $1.62/hr and the spread turns negative to −$1.12/hr. Breakeven utilization on this “crack spread” sits at a utilization of 85%.


Two caveats. First, p and R pull against each other. Speed is sold by shrinking batch sizes, and smaller batches mean fewer tokens per GPU-hour. Serving users faster raises the price you can charge but lowers the yield you produce. That is why the same model on the same hardware sells for $0.56/M at 50 tok/s but $4/M at 125 tok/s. Providers often optimize their stack to serve a particular point on this curve.


*Anchor points from SemiAnalysis InferenceMAX\[13\] curve shape illustrative.


Secondly, the example’s 15% gross margin is much thinner than the reported 45–50% margin inference providers run today.


They are able to:


-


Charge higher prices (p ↑) by capturing flow from users further out on the throughput-cost curve.


-


Compress costs (c ↓) by signing longer-term GPU commitments


-


Drive utilization (u ↑) by dynamically managing or repurposing the GPU fleet (more on this below)


-


Push yield (R ↑) with the serving-stack optimizations (more on this below)


p is set by sell-side competition and c by the GPU market. R and u are the only two a provider have complete control over.


As such, providers seek to maximize R and achieve differentiation on those axes against other providers. Common techniques to optimize costs and maximize R are as follows:


-


**Quantization** (FP8 → FP4/INT4): Decoding a token is memory-bandwidth-bound: every output token requires streaming the model’s active weights from HBM. Quantization (using FP4 vs FP8) halves the bytes per weight and halves the bandwidth used. FP4 yields 1.8× over FP8 in production, and B200s running FP4 reach up to 3.65× the performance per dollar of H200s on FP8. However, quantization comes at the cost of model quality.


-


**Prefix/KV caching.** Most tool call an agent makes resends mostly the same prompt with a little new text at the end. Rather than re-process that prompt from scratch each time, the provider stores the computation from the last call and only processes what’s new. Coding agents reuse 85–97% of their prompt this way in production, and going from no caching to 90% reuse takes roughly 10× off the GPU bill. For agent traffic, cache-hit is the most important metric in determining the compute bill.


-


**Speculative decoding:** A small “draft” model guesses several tokens ahead, and the big model verifies the whole guess in a single pass. Checking is much cheaper than generating, so one read of the big model’s weights now yields several tokens instead of one.


-


**Prefill/decode disaggregation** : Prefill (reading a prompt) is bound my compute decode and (generating a reply) is bound by memory bandwidth. Yet they share the same GPUs. Splitting them onto separate pools, each tuned for the respective bottleneck, allows for 10–30% improvements and up to 4.4× per GPU for large MoE models on rack-scale systems.


-


**Parallelzing MoE experts:** Frontier MoE models activate only 3–5% of their parameters per token. Spreading the experts across 32–144 GPUs shrinks the weights each GPU holds, freeing HBM for larger batches, and expert load-balancing alone contributes a 1.5–2.5× throughput gain. DeepSeek runs its own fleet this way (prompts on 32-GPU units, generation on 144-GPU units ), SGLang reproduced the setup on 96 GPUs in the open-source engine, and NVIDIA’s rack-scale GB200 NVL72 systems are built around exactly this pattern.


These optimization levers compound multiplicatively. Despite models getting larger and GPU/hr prices increasing, inference costs have plummeted across generations. DeepSeek’s flagship output price fell ~8× since January 2025 (R1 at $2.19/M → V4 Flash at $0.28/M ), while the H100 index ended the same window 17% higher: $2.34 to $2.74.


Source: R1, Jan 2025 → V3.2-Exp’s −50% cut, Sep 2025 → V4 Flash, 2026 . GPU line: Ornn’s H100 SXM price index , daily, pulled Jul 21 2026.*


Inference providers differentiate by building proprietary depth in these optimization levers. Fireworks wrote FireAttention, a custom attention kernel that allows it to serve the same weights 4× faster than stock vLLM and excels at latency-sensitive flow. Together built ATLAS, a speculative-decoding system whose draft models learn from live traffic, designed to enable its speedups to compound with volume. Baseten trains custom EAGLE-3 draft heads per customer, and has excelled at dedicated deployments. SAIL tunes its whole stack for time insensitive work and has excelled at long-running background agents.


However, these optimizations eventually diffuse into the open-source serving engines like vLLM and SGLang. As such, the “alpha” for an inference provider decays and the decay/diffusion rate has been accelerating. Prefill/decode disaggregation took 11–16 months, Multi-head latent attention (MLA) took 3–8 months, EAGLE-3 took 1–2 months and by DeepSeek V4 the release included the most performant serving stack out of the gate.


The edge decay is structural. Not every optimization is published but the largest ones keep arriving as papers because the labs that invent them want their models served widely and cheaply on day one. Red Hat pays vLLM’s core maintainers and NVIDIA contributes to both vLLM and SGLang.


There is additional industry nuance here. Some open source labs work closely with inference clouds and let them access model weights pre-open launch in exchange for 15-30% of lifetime serverless revenue from that model. Inference providers basically give up a percentage of future revenue from that model to get a 10 day head start to optimize their gear and plan their capacity around the new model release. Providers can also unlock more storage to have higher cache hits to be able to attract more flow. Model life cycles are short and the initial surge in demand post model release is quite profitable – almost like market makers vying for flow during an IPO or a TGE.


A serving-stack edge is therefore temporally short-lived and like any trading alpha must be continuously re-earned.
As such, once the optimization stack (R) is exhausted, COGS management turns financial:


-


**Rebalancing capacity (u):** Provider’s often split their fleets in two parts: dedicated capacity, (where a customer reserves specific GPUs for its own model) and a shared serverless pool everyone else draws from. Dedicated capacity is paid for around the clock but is rarely busy around the clock and its idle hours are predictable. The unlock is that most customer fine-tunes are LoRA adapters: small weight deltas layered on the same base model that is already loaded on the GPU. So when a dedicated customer goes quiet overnight, providers can swap that customer’s adapter out and route serverless traffic onto the reserved GPUs and swap them back on when the customer wakes up. In this manner, the same GPU-hour essentially gets sold twice — once as a reservation, once as serverless flow — pushing effective utilization from 50% to 120%. This is not dissimilar to airlines selling more seats than the plane holds, because the no-shows are predictable. Here the no-show is a dedicated customer’s idle hour.


-


**Managing compute cost (c):** Compute is the largest cost item in the entire token delivery supply chain. Of every $100 of tokens created, roughly $50 is burnt in GPU-hours. **As such, GPU-hours are the homogeneous input for producing any token and thus the most integral part of the entire token lifecycle.** The all-in cost of an owned H100 in turn is as follows:


Today providers manage c with term contracts and short-term rental agreements while internalizing all depreciation, price and volatility risk. Financial derivatives like futures and forward curves for GPUs (and memory) can help inference providers transfer risk and manage their cost effectively.


In other words, as an inference provider’s software and hardware optimizations asymptote, it’s business begins to look more and more like a trading book.


### Compute Futures: The Two-Sided Market


A tradeable instrument requires standardization (one unit substitutes for another) and stability (a contract written today describes the same good at maturity).


Tokens fail both today:


-


Standardization: A token is a product tuple across model × quantization × context × latency tier. Fixing the model, one ends up with a Caretsian bundle for the model’s grades – 3 quantizations × 2 context configurations × 3 latency tiers is already 18 distinct, non-fungible products of the “same” model.


-


Stability: The overton window for the newest model has been shrinking. New models now come out every 41 days, often rendering an entire class of older models redundant and plummeting demand for them.


Could a token index repair this? Only halfway. Token prices are set at the labs’ discretion and are manipulable by construction. Even the SIT proposal , which fixes the inputs, grades the model rather than the served inference, where the quality games actually live (identical weights score 76–100% on tool calls depending on the host).


As such, a token index can work as a reference rate but it cannot carry risk transfer. Risk transfer must happen one layer down in the homogenous input layer that is GPU-hours.


GPU-hours already have emergent indices for standardized price discovery such as Ornn’s Compute Price Index and two-sided liquid trading venues like the Architect Innovation Exchange (both Galaxy Ventures portfolio companies)


Why an inference provider needs to manage GPU price risk? An inference provider’s entire cost side is one volatile spot input – their cost of compute. In m=p⋅R⋅u-c, the provider controls R and u and competition sets p but c floats on a spot market whose realized volatility averages 130% annualized with current realized vol sitting at 312%.


In practice a provider’s book is a blend of owned GPUs inventory, 1–3 year term commitments, and short-term rentals at prices near the spot index. Left unhedged, the blend loses in both directions.


When rent spikes (May 2026: +101% off the trough), the spot leg’s cost doubles while token prices stay stagnant (token prices have been monotonically decreasing so far) and thus every marginal GPU-hour serves at a loss.


When rent collapses (the $1.63 trough), the longer-term commitments are over-paying above the spot market rate, and competitors renting at spot can quote token prices below the provider’s cost.


How would inference providers manage price risk in practice? Buy GPU-hour futures against the fleet’s forward rental needs. Locking the cost leg replaces the floating spot rent with the futures price plus the cost of running the hedge itself:


m=p⋅R⋅u-(F_0+f)


where F_0 is the futures price for the tenor being locked and f is the financing cost of the position (collateral tied up as initial margin plus daily variation margin). As with any future, you can have contango or backwardation. Let’s walk through with some examples of a dummy futures curve:


-


**Contango** (F_0>c_spot): Futures curve slopes up, so the buyer bears carry costs — m=p⋅R⋅u-(c_spot+"carry"+f). Take the February glut: spot sat at $1.63, but our dummy futures quoted above it at $2.05 for May delivery, $2.20 for further out because the market expected rents to mean revert. A provider hedging then paid $0.40–0.60 over spot for locking in the futures price. When the squeeze took spot to $3.28 in May, the hedged provider was still paying $2 and saving the $1.28 delta in operating costs.


-


**Backwardation** (F_0>c_spot): Futures curve slopes down, so the buyer pockets a discount (positive roll) — 𝑚=𝑝⋅𝑅⋅𝑢−(𝑐𝑠𝑝𝑜𝑡−discount+𝑓). Take May’s squeeze: spot hit $3.28, but our dummy futures quoted below it at $2.90 two months out because the market expected the shortage to pass. Whoever needed a GPU that week paid the full $3.28 while a provider buying next quarter’s hours got them at a discount to the panic price.


Either way, an open price risk becomes a known, contracted cost which provides margin stability and the only risks left are the two the provider is actually able to internalize, R and u.


There are still a few caveats to the above:


-


**Firstly, basis risk is not entirely removed** : The contract settles on index-spec H100 hours, while a real fleet is a mix of vintages, regions, and interconnects that won’t track the index one-for-one. The hedge covers the market move, not the provider’s fleet’s spread to it.


-


**Second, locking c doesn’t lock demand** : If a provider hedges more hours than they end up utilizing the position flips from a cost hedge into a directional long on compute. The hedge ratio therefore is somewhat a function of the provider’s confidence in their future projection of their u.


Huge thanks to[Brett](https://x.com/BrettHarrison) and[Ishanee](https://x.com/Ishanee) from[Architect](https://architect.co/) ,[Kush](https://x.com/bavaria_kush) ,[Wayne](https://x.com/wayne_nelmz) , and[Jack](https://www.linkedin.com/in/jack-minor/) from[Ornn](https://ornn.com/) , and Vishwa from Anera in their help in reviewing and helping me with this piece. Architect and Ornn are both Galaxy Ventures portfolio companies.


Sources
1.[https://tomtunguz.com/three-years-in/](https://tomtunguz.com/three-years-in/) ↩
2.[https://epoch.ai/data-insights/gpt-4-longest-eci-lead](https://epoch.ai/data-insights/gpt-4-longest-eci-lead) ↩
3.[https://epoch.ai/data-insights/open-closed-eci-gap](https://epoch.ai/data-insights/open-closed-eci-gap) ↩
4.[https://openrouter.ai](https://openrouter.ai/) ↩ ↩
5.[https://data.ornn.com](https://data.ornn.com/) ↩ ↩ ↩ ↩
6.[https://arxiv.org/pdf/2603.21690](https://arxiv.org/pdf/2603.21690) ↩ ↩
7.[https://developers.openai.com/api/docs/guides/batch](https://developers.openai.com/api/docs/guides/batch) ↩ ↩
8.[https://openrouter.ai/docs](https://openrouter.ai/docs) ↩
9.[https://tomtunguz.com/inflation-deflation-ai/](https://tomtunguz.com/inflation-deflation-ai/) ↩
10.[https://x.com/Altimor/status/2062389885437366342](https://x.com/Altimor/status/2062389885437366342) ↩
11.[https://x.com/SpencerFarrar/status/2070230293429244094](https://x.com/SpencerFarrar/status/2070230293429244094) ↩
12.[https://openrouter.ai/docs/faq](https://openrouter.ai/docs/faq) ↩
13.[https://inferencex.semianalysis.com/](https://inferencex.semianalysis.com/) ↩
14.[https://github.com/deepseek-ai/open-infra- index/blob/main/202502OpenSourceWeek/day_6_one_more_thing_deepseekV3R1_inference_system_o verview.md](https://github.com/deepseek-ai/open-infra-%20index/blob/main/202502OpenSourceWeek/day_6_one_more_thing_deepseekV3R1_inference_system_o%20verview.md) ↩
15.[https://lmsys.org/blog/2025-05-05-large-scale-ep/](https://lmsys.org/blog/2025-05-05-large-scale-ep/) ↩
16.[https://pricepertoken.com/pricing-page/model/deepseek-deepseek-r1](https://pricepertoken.com/pricing-page/model/deepseek-deepseek-r1) ↩ ↩
17.[https://devtk.ai/en/blog/deepseek-api-pricing-guide-2026/](https://devtk.ai/en/blog/deepseek-api-pricing-guide-2026/) ↩ ↩
18.[https://technode.com/2025/09/30/deepseek-releases-v3-2-exp-experimental-model-cuts-api-prices-by- over-50/](https://technode.com/2025/09/30/deepseek-releases-v3-2-exp-experimental-model-cuts-api-prices-by-%20over-50/) ↩
19.[https://fireworks.ai/blog/fire-attention-serving-open-source-models-4x-faster-than-vllm-by-quantizing-with- no-tradeoffs](https://fireworks.ai/blog/fire-attention-serving-open-source-models-4x-faster-than-vllm-by-quantizing-with-%20no-tradeoffs) ↩


Legal Disclosure:


This document, and the information contained herein, has been provided to you by Galaxy Digital Inc. and its affiliates (“Galaxy Digital”) solely for informational purposes. This document may not be reproduced or redistributed in whole or in part, in any format, without the express written approval of Galaxy Digital. Neither the information, nor any opinion contained in this document, constitutes an offer to buy or sell, or a solicitation of an offer to buy or sell, any advisory services, securities, futures, options or other financial instruments or to participate in any advisory services or trading strategy. Nothing contained in this document constitutes investment, legal or tax advice or is an endorsement of any of the stablecoins mentioned herein. You should make your own investigations and evaluations of the information herein. Any decisions based on information contained in this document are the sole responsibility of the reader. Readers should consult with their own advisors and rely on their independent judgement when making financial or investment decisions.


Participants, along with Galaxy Digital, may hold financial interests in certain assets referenced in this content. Galaxy Digital regularly engages in buying and selling financial instruments, including through hedging transactions, for its own proprietary accounts and on behalf of its counterparties. Galaxy Digital also provides services to vehicles that invest in various asset classes. If the value of such assets increases, those vehicles may benefit, and Galaxy Digital’s service fees may increase accordingly. The information and analysis in this communication are based on technical, fundamental, and market considerations and do not represent a formal valuation. For more information, please refer to Galaxy’s public filings and statements. Certain asset classes discussed, including digital assets, may be volatile and involve risk, and actual market outcomes may differ materially from perspectives expressed here.


For additional risks related to digital assets, please refer to the risk factors contained in filings Galaxy Digital Inc. makes with the Securities and Exchange Commission (the “SEC”) from time to time, including in its Quarterly Report on Form 10-Q for the quarter ended September 30, 2025, filed with the SEC on November 10, 2025, available at www.sec.gov.


Certain statements in this document reflect Galaxy Digital’s views, estimates, opinions or predictions (which may be based on proprietary models and assumptions, including, in particular, Galaxy Digital’s views on the current and future market for certain digital assets), and there is no guarantee that these views, estimates, opinions or predictions are currently accurate or that they will be ultimately realized. To the extent these assumptions or models are not correct or circumstances change, the actual performance may vary substantially from, and be less than, the estimates included herein. None of Galaxy Digital nor any of its affiliates, shareholders, partners, members, directors, officers, management, employees or representatives makes any representation or warranty, express or implied, as to the accuracy or completeness of any of the information or any other information (whether communicated in written or oral form) transmitted or made available to you. Each of the aforementioned parties expressly disclaims any and all liability relating to or resulting from the use of this information. Certain information contained herein (including financial information) has been obtained from published and non-published sources. Such information has not been independently verified by Galaxy Digital and, Galaxy Digital, does not assume responsibility for the accuracy of such information. Affiliates of Galaxy Digital may have owned, hedged and sold or may own, hedge and sell investments in some of the digital assets, protocols, equities, or other financial instruments discussed in this document. Affiliates of Galaxy Digital may also lend to some of the protocols discussed in this document, the underlying collateral of which could be the native token subject to liquidation in the event of a margin call or closeout. The economic result of closing out the protocol loan could directly conflict with other Galaxy affiliates that hold investments in, and support, such token. Except where otherwise indicated, the information in this document is based on matters as they exist as of the date of preparation and not as of any future date, and will not be updated or otherwise revised to reflect information that subsequently becomes available, or circumstances existing or changes occurring after the date hereof. This document provides links to other Websites that we think might be of interest to you. Please note that when you click on one of these links, you may be moving to a provider’s website that is not associated with Galaxy Digital. These linked sites and their providers are not controlled by us, and we are not responsible for the contents or the proper operation of any linked site. The inclusion of any link does not imply our endorsement or our adoption of the statements therein. We encourage you to read the terms of use and privacy statements of these linked sites as their policies may differ from ours. The foregoing does not constitute a “research report” as defined by FINRA Rule 2241 or a “debt research report” as defined by FINRA Rule 2242 and was not prepared by Galaxy Digital Partners LLC. Similarly, the foregoing does not constitute a “research report” as defined by CFTC Regulation 23.605(a)(9) and was not prepared by Galaxy Derivatives LLC. For all inquiries, please email[\[email protected\]](https://www.galaxy.com/cdn-cgi/l/email-protection) .


©Copyright Galaxy Digital Inc. 2026. All rights reserved.
