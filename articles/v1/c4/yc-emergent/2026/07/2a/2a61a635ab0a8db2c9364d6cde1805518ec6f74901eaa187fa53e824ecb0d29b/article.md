---
schema_version: "1.0.0"
document_id: "2a61a635ab0a8db2c9364d6cde1805518ec6f74901eaa187fa53e824ecb0d29b"
company_key: "yc-emergent"
company: "Emergent"
source_id: "yc-emergent-news-import-16a7bf482038"
canonical_url: "https://emergent.sh/news/kimi-k3-launch"
published_at: "2026-07-17T21:27:00+00:00"
first_seen_at: "2026-07-21T18:00:01.007987+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:68c75f039af8ff6e96742b8fb0778133005dfc876843ca4e2d029b8aeaa9d799"
---

# Kimi K3: Moonshot AI's 2.8T Frontier Model Is Live

Most frontier AI models are locked behind closed APIs. You can use them, but you can't download them, modify them, or run them on your own terms. Moonshot AI just challenged that with Kimi K3, a 2.8 trillion parameter model released as open-weight, making it the largest model of its kind ever shipped. It launched on July 16, 2026, and it is designed to go head-to-head with the most capable proprietary models available.


For anyone building with AI, this is worth paying attention to, not just because of the scale, but because of what open-weight access at this level could mean for cost, flexibility, and how quickly new tools get built on top of it.


## What Kimi K3 Actually Is


[Kimi K3](https://platform.kimi.ai/docs/guide/kimi-k3-quickstart) is a Mixture-of-Experts (MoE) model. In plain terms, that means it has 2.8 trillion total parameters but only activates a small subset of them, 16 out of 896 experts or roughly 104 billion parameters, for any given task. This design keeps the model efficient despite its massive size. It is built on two architectural components Moonshot developed internally: Kimi Delta Attention (KDA) and Attention Residuals (AttnRes), which the company says improve how information flows across long sequences and deep layers.


The headline specs, per Moonshot's official blog:


- 2.8 trillion total parameters, the first open model in the 3T class
- A 1-million-token context window
- Native vision capabilities, accepting text, image, and video input
- Always-on reasoning, defaulting to maximum effort. The reasoning_effort parameter also accepts low and high, though thinking cannot be switched off entirely
- An estimated 2.5x improvement in scaling efficiency compared to the previous Kimi K2 generation


The model is available on[Kimi.com](https://www.kimi.com/) , Kimi Work, Kimi Code, and the Kimi API. Full model weights were released publicly on July 27, 2026.


## How It Performs (According to Moonshot)


Moonshot published a detailed benchmark table alongside the launch. An important caveat: these are vendor-reported numbers. Independent verification from third-party evaluators is still catching up, though Moonshot has since published its technical report with further evaluation details.


[Source](https://www.kimi.com/blog/kimi-k3)


That said, the self-reported results are notable. On coding benchmarks, Kimi K3 scored 67.5 on DeepSWE, 88.3 on Terminal Bench 2.1, and 42.0 on SWE Marathon, the highest score among all models tested in that category. On agentic tasks, it posted a 91.2 on BrowseComp and 30.8 on Automation Bench, again leading the comparison set. Vision benchmarks show 81.6 on MMMU-Pro and 94.3 on MathVision.


[Source](https://www.kimi.com/blog/kimi-k3)


Moonshot is transparent about the model's position relative to the top tier. Their blog states directly that Kimi K3's overall performance "still trails the most powerful proprietary models, Claude Fable 5 and GPT 5.6 Sol," while consistently outperforming the other models they tested. That kind of self-assessment is useful because it sets realistic expectations rather than inflating claims.


## What It Can Actually Do


Beyond raw benchmark numbers, Moonshot showcased several practical demonstrations on the launch blog that highlight what this model looks like in action.


On the coding side, Kimi K3 built MiniTriton, a compact GPU compiler, from scratch. This included a custom intermediate representation layer, optimization passes, and a PTX code generation pipeline, with performance matching or exceeding established tools on supported benchmarks. In a separate test, K3 designed a chip to run a nano model built on its own architecture in a 48-hour autonomous session, closing timing at 100 MHz within a 4mm² footprint.


For knowledge work, Moonshot demonstrated K3 reproducing computational astrophysics research (the I-Love-Q universal relations) in roughly two hours, a task they estimate would typically take an experienced researcher one to two weeks. That workflow involved reviewing 20+ papers, evaluating 300+ equations of state, and producing 3,000+ lines of Python code with an interactive dashboard.


The model also handles creative and visual tasks. Moonshot showed K3 building playable 3D browser games using Three.js WebGPU, editing a teaser video from 56 source clips with beat synchronization, and creating interactive research reports with data visualizations. The company describes this as "vision in the loop," where the model iterates between writing code and evaluating screenshots of the output.


## Pricing and Access


Kimi K3 is available through Moonshot's API at the following rates, per their official documentation:


- $0.30 per million tokens for cache-hit input
- $3.00 per million tokens for cache-miss input
- $15.00 per million tokens for output


Moonshot notes that their inference architecture, Mooncake, achieves a cache hit rate above 90% in coding workloads, which would bring the effective input cost significantly closer to the $0.30 tier for repeated or iterative tasks.


The model is accessible through the Kimi app (iOS, Android, HarmonyOS),[Kimi Work](https://www.kimi.com/products/kimi-work) desktop for knowledge work,[Kimi Code](https://www.kimi.com/code) for terminal-based coding, and directly through the[Kimi API](https://platform.kimi.ai/) . At launch, the model ran at maximum thinking effort with no alternative setting. Lower effort modes have since shipped: reasoning_effort now accepts low, high, and max, with max remaining the default.


The[open-weight release on July 27](https://emergent.sh/news/kimi-k3-release-date) means developers can download and modify the model. This is a significant distinction from API-only access, as it opens the door for self-hosting, fine-tuning, and integration into custom workflows without ongoing API costs.


One detail worth reading before you build on it: the weights ship under a document Moonshot calls the Kimi K3 License rather than a standard open-source license. Internal use is unconditional. Anyone reselling inference or fine-tuning access to third parties needs a separate agreement with Moonshot once combined revenue passes $20 million over any consecutive 12 months, and products above 100 million monthly active users or $20 million in monthly revenue must display "Kimi K3" in the interface.


## Limitations Worth Knowing


Moonshot disclosed three specific limitations in their launch blog, which is a useful practice that not all labs follow.


First, K3 is sensitive to its thinking history. If an agent setup fails to pass back the full reasoning context, or if a session started with a different model gets switched to K3, output quality can become unstable. Second, the model tends to be "excessively proactive," meaning it may make decisions on behalf of users when instructions are ambiguous rather than asking for clarification. Third, Moonshot acknowledges a "noticeable gap in user experience" compared to the top proprietary models.


For builders, the proactiveness issue is particularly relevant. If you are building agents or workflows on top of K3, Moonshot recommends setting explicit behavioral constraints in the system prompt.


## What This Means If You Are Building with AI


The practical takeaway here is about access. Until now, frontier-class AI performance meant paying for closed APIs from a handful of providers. Kimi K3 puts a model in the same competitive range and, since the weights shipped on July 27, makes it available as a download anyone can run, modify, and build on top of. That changes the economics for developers and platforms building AI-powered tools.


If you are a non-technical builder, the direct impact is indirect but real. As open-weight models at this scale become available, the platforms and tools you use to build will get more capable, more affordable, and more flexible. The AI models powering your apps get better because the entire ecosystem has more options to choose from.


If you are exploring what you can build with AI right now,[Emergent](https://emergent.sh/) lets you turn ideas into working applications without writing code, powered by the latest AI models. The more capable these models become, the more capable your builds become.


Stay tuned to[Emergent News](https://emergent.sh/news) for more coverage of the AI tools and models shaping how products get built.
