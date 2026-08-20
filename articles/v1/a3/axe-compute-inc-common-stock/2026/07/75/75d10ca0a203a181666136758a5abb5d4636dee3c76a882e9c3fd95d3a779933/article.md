---
schema_version: "1.0.0"
document_id: "75d10ca0a203a181666136758a5abb5d4636dee3c76a882e9c3fd95d3a779933"
company_key: "axe-compute-inc-common-stock"
company: "Axe Compute Inc."
source_id: "axe-compute-inc-common-stock-rss-ffaba1d535a1"
canonical_url: "https://axecompute.com/ai-compute-pyramid/"
published_at: "2026-07-16T05:21:31+00:00"
first_seen_at: "2026-07-26T23:17:51.553128+00:00"
fetched_at: "2026-07-28T20:41:19.924237+00:00"
content_hash: "sha256:b487ad6a3072cde165eb384d4349a165ee35ddee5bf14fe8d920e084209c16b2"
---

# The AI Compute Pyramid

Access to AI compute scales with capital. Scaling laws make model capability a predictable function of compute, so the frontier is a capital race by construction. Compute is also the one AI input that behaves like physical property: detectable, excludable, quantifiable. Its supply chain, moreover, narrows to a single firm at almost every layer. The result is the AI compute pyramid: four tiers of access where capability tracks compute and, in turn, compute tracks capital.


**Key finding:** AI capability is, to a first approximation, a function of compute, and compute concentrates with capital. Training compute for the largest models doubled roughly **every six months** through the deep-learning era, Epoch AI projects frontier runs above **$1 billion by 2027** , and the chip supply chain narrows to a single dominant firm at every layer. Overall, the outcome is a four-tier compute pyramid in which altitude tracks balance sheet, not the quality of the work.


## The AI Compute Pyramid at a Glance


The AI compute pyramid: four tiers of access, and the four moats that hold the apex in place. Source: Axe Compute analysis of Epoch AI, the GovAI compute-governance report, OECD, and industry disclosures. ~6 months


Training-compute doubling time, deep-learning era


> $1B


Projected frontier training run by 2027 (Epoch AI)


> 90%


One firm’s share of data-center GPUs; one foundry; one EUV maker


## Capability Is a Function of Compute


Start with the science, because it sets everything else in motion. In 2020, Kaplan and colleagues at OpenAI showed that the loss of a language model falls as a smooth power law in the compute, data, and parameters used to train it. As a result, performance improves in step with the resources applied, across many orders of magnitude.


Two years later, the Chinchilla study from DeepMind (Hoffmann et al. 2022) refined the recipe. Specifically, for compute-optimal training, model size and training data scale together: double the parameters, and double the tokens. The lesson for anyone at the frontier is blunt. Consequently, cleverness cannot substitute for hardware, and a better model generally means a bigger compute bill.


The frontier has also moved quickly. Epoch AI’s analysis of three eras of machine learning (Sevilla et al. 2022) found that training compute for the largest models doubled roughly every six months during the deep-learning era. By contrast, the Moore’s-law period before 2010 doubled about every twenty months. Capability tracks compute, and compute is climbing far faster than hardware efficiency alone can offset. In turn, that single relationship turns the AI frontier into a capital race.


## Compute Is the One Input You Can Own


AI has three core inputs: compute, data, and algorithms. Compute is the one that behaves like physical property. Researchers at OpenAI and the Centre for the Governance of AI wrote the 2024 report *Computing Power and the Governance of AI* , with co-authors including Yoshua Bengio. Specifically, it describes compute as detectable, excludable, and quantifiable, with an extremely concentrated supply chain behind it. Algorithms leak and data copies, but a GPU cluster is a rivalrous physical asset that one party holds and another does not.


Those same properties make compute concentrate. In fact, the report documents the chokepoints, and they are stark. Extreme-ultraviolet lithography machines, essential for leading-edge chips, come from a single company, ASML. One foundry, TSMC, dominates leading-edge fabrication at roughly 90 percent of the pure-play market. Equally, one firm, NVIDIA, holds more than 90 percent of data-center GPU design, by the UK competition authority’s estimate. As a result, capital, not merit, decides the order of service when every layer narrows to one firm.


## The Four Tiers of the AI Compute Pyramid


The industry already named the top and bottom of this structure. In 2023, the market was split into the “GPU-rich” and the “GPU-poor,” a phrase that stuck because it described something real. In the two years since, the split has hardened into a four-tier pyramid, and the capital a company can commit to compute sets its tier almost entirely. The diagram above maps the tiers; the table below summarises them.


Table 1: The four tiers of the AI compute pyramid


Tier Who sits here How they get compute Typical wait


Apex: Sovereign Hyperscalers, frontier labs, nations Own data centers and chip allocation None


Reserved Large AI-native firms, specialist providers Multi-year forward contracts Weeks


Procurement Funded enterprises Standard channels, reserved cloud 36 to 52 weeks


Base: Spot Startups, researchers, mid-market Spot and on-demand, job by job Minutes, when available


The apex owns its supply. Meanwhile, the reserved tier locks supply in under long contracts. Below it, the procurement tier waits in a queue that now runs 36 to 52 weeks. Hyperscaler forward orders for Blackwell GPUs consumed most of the available allocation, and TSMC packaging bookings run into 2027. At the same time, the base holds many of the field’s best ideas and the least guaranteed access to the hardware those ideas need.


### Rubin Shows the Queue Forming Again


The pattern is repeating in real time with Rubin, NVIDIA’s successor architecture to Blackwell. NVIDIA declared the platform in full production in January 2026. Partner availability opens in the second half of the year, yet the largest cloud platforms have already claimed the first wave of NVL72 systems. Allocation, in other words, closes before general availability begins. Therefore, early reservation moves a team up a tier, and we opened that route in[Vera Rubin early access](https://axecompute.com/vera-rubin-early-access/) .


## Four Moats Hold the Apex in Place


Four reinforcing moats hold the pyramid in place, each documented in the research.


**Capital.** Frontier training cost has grown about 2.4 times per year since 2016, and Epoch AI projects the largest runs will exceed a billion dollars by 2027, a level it notes only the most well-funded organisations can sustain. In practice, each rung up the pyramid costs an order of magnitude more than the last.


**Packaging and memory.** The binding constraint is no longer raw arithmetic. Gholami and colleagues (2024), writing in IEEE Micro, found that peak server compute has scaled about 3.0 times every two years. By comparison, memory bandwidth has scaled only 1.6 times and interconnect 1.4 times, a growing gap they call the memory wall. That gap puts the value in high-bandwidth memory and advanced packaging, which is exactly where supply runs tightest. TSMC’s CoWoS packaging order book runs full into 2027, and NVIDIA has reportedly reserved more than half of it, according to TrendForce and DigiTimes.


### Power and People Close the Loop


**Power.** Even unlimited capital meets a physical ceiling. Epoch AI estimates that frontier AI training is on track to demand gigawatt-scale power within this decade, which restricts the apex to those who can secure grid connections and energy at scale. We examine that constraint in[The Power Problem](https://axecompute.com/data-center-energy-constraints/) .


**Concentration of talent and scrutiny.** The divide compounds itself. Besiroglu and colleagues (2024) document a measurable “compute divide” that has pushed academic-only teams out of compute-intensive research such as foundation models. Earlier work in *Science* (Ahmed, Wahed, and Thompson 2023) found industry steadily taking over the compute, data, and talent that frontier work requires. As a consequence, the organisations with the compute attract the people who can use it, which secures more compute.


## Why Cheaper Compute Does Not Flatten the Pyramid


The intuitive objection is that falling prices will democratise access. The evidence points the other way. This is the Jevons paradox, first described for coal in 1865: when a resource becomes cheaper to use, total consumption rises rather than falls, because cheaper access opens far more uses.


The numbers fit the pattern. Gartner projects that performing inference on a one-trillion-parameter model will cost more than 90 percent less in 2030 than in 2025. At the same time, Goldman Sachs Research projects token consumption rising 24 times over the same window, to roughly 120 quadrillion tokens per month. Each hardware generation repeats the move. NVIDIA lists up to 10 times lower cost per generated token on Vera Rubin than on Blackwell. Every such drop has widened usage, not reduced spending. Consequently, the price per unit collapses while total demand explodes, and the organisations positioned to deploy at scale capture most of the new volume. Notably, capital keeps pooling at the top: the OECD reports that AI firms took 61 percent of all global venture capital in 2025, double their 2022 share. Cheaper compute widens the base of the pyramid without lowering its peak.


## How to Buy Altitude Without Buying a Fab


Axe Compute operates across 200+ locations worldwide on 400,000+ existing GPUs.


No enterprise can out-build the supply chain. ASML, TSMC, and NVIDIA sit where they sit, and the apex will keep its place. What a company can do is route around the access bottleneck rather than wait at the back of it. Instead, independent, distributed capacity pools GPUs that already exist and reallocates them to the workloads that need them, which breaks the link between a balance sheet and a place in the queue. The economics of that asset-light model are the subject of[Neoclouds and Why the Asset-Light Model Wins](https://axecompute.com/asset-light-gpu-cloud-neocloud-market/) .


### Build Moves the Decision Back to the Client


For a buyer, the practical questions are about choice and transparency rather than price alone. Which regions, which GPU types, and which network fabric can the provider give you, and how fast. That is the design of Build, the Axe Compute program for client-specified capacity. The client sets the specification, and Axe Compute sources and configures against it. As a result, a base-tier or procurement-tier team that secures distributed bare-metal capacity can operate as though it sat several tiers higher, choosing what it needs instead of designing around whatever happens to be available. Planning that allocation across training and inference is the focus of[Enterprise GPU Strategy in 2026](https://axecompute.com/enterprise-gpu-strategy-in-2026/) .


The scaling laws are not going to relax, and the supply chain is not going to widen. The companies that compound advantage are the ones that stop trying to climb the pyramid on someone else’s terms and instead secure independent capacity that behaves as though they sat at the top. Access is the constraint. It is also the choice.


About Axe Compute


Axe Compute Inc. (NASDAQ: AGPU) is a neocloud AI infrastructure platform built on a fundamental premise: AI innovation should not be constrained by hardware choice or inventory limitations. Axe Compute gives enterprises and AI innovators choice across hardware, geography, and deployment speed through two delivery models: Axe Compute Access, providing the latest GPU compute options in as fast as 48 hours across numerous global locations, and Axe Compute Build, enabling enterprises to access large-scale dedicated AI factories, all backed by enterprise-grade SLAs and support. Axe Compute is headquartered in Pittsburgh, Pennsylvania. For more information, visit[axecompute.com](https://axecompute.com/)


.


You cannot out-spend the apex. You can route around it.


[Reserve Compute](https://portal.axecompute.com/)
Contact info@axecompute.com


## Frequently Asked Questions


### Why does access to AI compute scale with capital?


Two findings combine. First, scaling laws (Kaplan et al. 2020; Hoffmann et al. 2022) show that model capability improves predictably as more compute is applied, so reaching the frontier requires exponentially more compute. Second, the 2024 report Computing Power and the Governance of AI shows compute is detectable, excludable, and quantifiable, and flows through a highly concentrated supply chain. Capability therefore tracks compute, and compute tracks capital. Axe Compute provides an independent route to that hardware without the capital or the queue.


### What do scaling laws say about compute and AI capability?


Kaplan et al. (2020) found that language-model loss falls as a smooth power law in compute, data, and parameters. Hoffmann et al. (2022), the Chinchilla study, showed that compute-optimal training scales model size and training data together. Epoch AI’s analysis (Sevilla et al. 2022) found training compute for the largest models doubled roughly every six months in the deep-learning era. Together these mean capability is, to a first approximation, bought with compute, which is why Axe Compute focuses on making that compute accessible.


### What is the difference between GPU-rich and GPU-poor organisations?


GPU-rich organisations, such as hyperscalers and frontier labs, own or control large reserved GPU fleets. GPU-poor teams, including startups, researchers, and most enterprises, depend on spot capacity and procurement queues. By 2026 the split has widened into a four-tier pyramid, with access set by capital rather than by the quality of the work. Distributed bare-metal providers such as Axe Compute exist to loosen that link.


### Why is the AI compute supply chain so concentrated?


Every layer narrows to one or two firms. Extreme-ultraviolet lithography machines come from a single company (ASML), leading-edge fabrication is dominated by one foundry (TSMC, around 90 percent of the pure-play market), and data-center GPU design is dominated by one firm (NVIDIA, over 90 percent share). The 2024 governance report Computing Power and the Governance of AI documents these chokepoints. Concentration upstream is why access downstream is rationed by capital, and why Axe Compute exists to widen that access.


### Will falling AI compute prices democratise access?


The evidence points the other way. Gartner projects that inference on a one-trillion-parameter model will cost more than 90 percent less in 2030 than in 2025, while Goldman Sachs Research projects token consumption to rise 24 times over the same window. Falling unit prices raise total consumption, a pattern known as the Jevons paradox, and the organisations positioned to deploy at scale capture most of the new volume. Cheaper compute widens the base of the pyramid without lowering its peak.


### How can a company access GPU compute above its capital tier?


Independent, distributed providers pool and reallocate GPU capacity outside the hyperscaler forward-order queue, so a company does not need apex-level capital to reach apex-grade hardware. Axe Compute provides bare-metal GPU infrastructure across 200+ locations in 93 countries with zero egress fees, and pricing significantly below hyperscaler rates. Through the Build program, the client sets the specification and Axe Compute sources and configures against it, including early access to new architectures such as NVIDIA Vera Rubin.


### Sources


- [Kaplan et al., “Scaling Laws for Neural Language Models,” arXiv:2001.08361 (2020): model performance improves as a power law in compute, data, and parameters](https://arxiv.org/abs/2001.08361)
- [Hoffmann et al., “Training Compute-Optimal Large Language Models” (Chinchilla), arXiv:2203.15556 (2022): model size and training tokens should scale together](https://arxiv.org/abs/2203.15556)
- [Sevilla et al. (Epoch AI), “Compute Trends Across Three Eras of Machine Learning,” arXiv:2202.05924 (2022): training compute doubled roughly every six months in the deep-learning era](https://arxiv.org/abs/2202.05924)
- [Sastry, Heim, Belfield et al., “Computing Power and the Governance of AI,” arXiv:2402.08797 (2024): compute is detectable, excludable, quantifiable, and produced via a concentrated supply chain (ASML, TSMC, NVIDIA)](https://arxiv.org/abs/2402.08797)
- [Gholami et al., “AI and Memory Wall,” IEEE Micro / arXiv:2403.14123 (2024): compute has outpaced memory bandwidth, making memory the binding bottleneck](https://arxiv.org/abs/2403.14123)
- [Epoch AI, “How Much Does It Cost to Train Frontier AI Models?” (2024): cost grows ~2.4x per year; over $1 billion by 2027](https://epoch.ai/blog/how-much-does-it-cost-to-train-frontier-ai-models)
- [Besiroglu et al., “The Compute Divide in Machine Learning,” arXiv:2401.02452 (2024): academic-only teams pushed out of compute-intensive research](https://arxiv.org/abs/2401.02452)
- [Ahmed, Wahed, and Thompson, “The Growing Influence of Industry in AI Research,” Science (2023)](https://www.science.org/doi/10.1126/science.ade2420)
- [Gartner press release, March 25, 2026: inference on a 1-trillion-parameter LLM will cost over 90% less by 2030](https://www.gartner.com/en/newsroom/press-releases/2026-03-25-gartner-predicts-that-by-2030-performing-inference-on-an-llm-with-1-trillion-parameters-will-cost-genai-providers-over-90-percent-less-than-in-2025)
- [Goldman Sachs Research (May 2026): token consumption projected to rise 24x between 2026 and 2030, to roughly 120 quadrillion tokens per month](https://www.goldmansachs.com/insights/articles/ai-agents-forecast-to-boost-tech-cash-flow-as-usage-soars)
- [NVIDIA press release, January 5, 2026: Rubin platform in full production; first cloud deployments committed ahead of partner availability in the second half of 2026; up to 10 times lower cost per generated token than Blackwell](https://nvidianews.nvidia.com/news/rubin-platform-ai-supercomputer)
- [OECD (2026): AI firms captured 61% of global venture capital in 2025, double the 2022 share](https://www.oecd.org/en/about/news/announcements/2026/02/ai-firms-capture-61-percent-of-global-venture-capital-in-2025.html)
- [TrendForce: TSMC CoWoS advanced packaging fully booked into 2027](https://www.trendforce.com/news/2025/12/08/news-tsmcs-cowos-l-s-reportedly-fully-booked-osat-partners-step-up-with-ases-cowop-in-focus/)
