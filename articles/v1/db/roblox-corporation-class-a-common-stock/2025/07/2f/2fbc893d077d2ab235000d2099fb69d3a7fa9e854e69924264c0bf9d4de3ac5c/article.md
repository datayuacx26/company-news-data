---
schema_version: "1.0.0"
document_id: "2fbc893d077d2ab235000d2099fb69d3a7fa9e854e69924264c0bf9d4de3ac5c"
company_key: "roblox-corporation-class-a-common-stock"
company: "Roblox Corporation"
source_id: "roblox-corporation-class-a-common-stock-news-import-f52851b7bf16"
canonical_url: "https://about.roblox.com/zh-hans/newsroom/2025/07/roblox-guard-advancing-safety-for-llms-with-robust-guardrails"
published_at: "2025-07-22T21:37:00+00:00"
first_seen_at: "2026-07-23T23:14:17.357493+00:00"
fetched_at: "2026-07-28T21:59:46.813241+00:00"
content_hash: "sha256:adb1b2a07e6d64b18707bec418cea90852664fe83295833688e40589bc36e0f0"
---

# 尖端LLM助力保障Roblox上无限文本生成功能的安全

分享


[工程](https://about.roblox.com/zh-hans/newsroom?filter=%E5%B7%A5%E7%A8%8B)[新闻](https://about.roblox.com/zh-hans/newsroom?filter=%E6%96%B0%E9%97%BB)


# 尖端LLM助力保障Roblox上无限文本生成功能的安全


Roblox Guard 1.0：通过强大的防护措施提升安全性


作者


马赫什·南德瓦纳、亚当·麦克法林和尼什查伊·卡纳


已发布


2025年7月22日


- 今天，我们宣布推出 Roblox Guard 1.0，这是一个面向开发者和平台的开源安全工具包。
- [Roblox Guard](https://github.com/Roblox/RobloxGuard-1.0) 的首项[功能](https://github.com/Roblox/RobloxGuard-1.0) ——一款用于大型语言模型（LLM）安全的最先进（SOTA）防护模型——现已发布，在各大主流安全基准测试中树立了新标杆。
- 我们同时发布了[Roblox Guard-Eval](https://huggingface.co/datasets/Roblox/RobloxGuard-Eval) ，这是一个用于安全基准测试的数据集。


##### 挑战


我们最近发布了一款[文本生成 API](https://devforum.roblox.com/t/beta-introducing-text-generation-api/3556520) ，开发者可以利用大型语言模型（LLM）的强大功能，在体验中生成文本，从而打造更丰富、更沉浸式的体验。例如，开发者可以创建一个完全互动的 NPC，或提供关于如何玩游戏的互动教程。


自平台创立之初，我们就[积极对](https://about.roblox.com/newsroom/2025/07/roblox-ai-moderation-massive-scale) Roblox 上的[大部分内容进行审核](https://about.roblox.com/newsroom/2025/07/roblox-ai-moderation-massive-scale) ，以确保我们的产品符合 Roblox 严格的安全与文明标准。在发布文本生成 API 之前，我们首先研究了如何构建安全机制。我们开发了一个新模型，用于同时保护输入（用户提供的提示）和输出（API 生成的文本）。


##### 创新


Roblox Guard 1.0 工具包的首项功能是一个经过指令微调的 SOTA 大型语言模型（LLM），旨在帮助保障我们的文本生成 API 的安全。该模型在提示词和响应两个层面进行安全分类，判断每个输入或输出是否违反我们的政策。这种双层评估对于审核用户查询和模型自身生成的输出都至关重要。


在标准基准测试中，我们的 LLM 目前表现优于 Meta 的 Llama Guard、Google AI 的 ShieldGemma、NVIDIA NeMo Guardrails、OpenAI 的 GPT-4o 等主流 LLM 防护模型。Roblox Guard 1.0 LLM 在具有未知分类体系的域外数据集上也展现出强大的泛化能力。 我们已将首项功能所用的[LLM 权重](https://huggingface.co/Roblox/Llama-3.1-8B-Instruct-RobloxGuard-1.0) 以及[Roblox Guard-Eval](https://huggingface.co/datasets/Roblox/RobloxGuard-Eval) 基准测试数据集开源。


我们系统的核心是一个基于 Llama-3.1-8B-Instruct 模型微调而成的 LLM。在训练过程中，我们特别注重高质量的指令微调，以优化安全判断性能。这一过程中至关重要的一步是精心筛选提示词和响应，以反映多样化的现实世界安全场景。


我们的指令集未使用任何专有数据，仅结合了合成数据（由LLM生成）和开源数据，这使我们能够更轻松地扩展训练数据并利用规模法则，从而使这款首款Roblox Guard LLM成为当前最先进（SOTA）的模型。在整合各种开源和合成数据集时，我们发现采用数据集特有的分类法是构建指令的最佳方法，因为任务多样性有助于LLM在不同类型的提示词上进行训练。 由此构建出的模型表现稳健，能够泛化至不同的安全分类体系。此外，我们还在指令集中融入了“思维链”推理机制，引导模型清晰阐述其推理过程。这些中间推理步骤为模型提供了更强有力的语境支撑。


##### 结果


我们的安全团队基于 Roblox 的内容安全分类体系（涵盖 25 个子类别）开发了一套定制的高质量评估数据集。该评估数据集通过内部红队测试生成，即我们通过模拟对抗性攻击来测试系统并查找漏洞，其中不包含用户生成内容或个人数据。 该评估数据集包含提示词与响应词对，其中响应词由政策专家团队人工标注以确保质量。数据集涵盖了广泛的违规类型，有助于我们为评估创建更精准且有意义的标签。最终评估集包含 2,873 个示例。我们已将该评估数据集开源，其具备可扩展的安全分类体系，旨在帮助对大型语言模型（LLM）的防护机制和内容审核系统进行基准测试。


我们在涵盖提示词和响应内容的全面开源数据集以及 Roblox Guard-Eval 数据集上对模型进行了基准测试。这使我们能够同时评估模型在域内和域外数据集上的表现。 我们以二元分类（违规/不违规）的F1分数报告结果。在上表中，我们将我们的性能与几个知名模型进行了比较。这一首个Roblox Guard功能在泛化到域外数据集时表现优于其他模型。


我们正在持续改进安全系统，包括 Roblox Guard 1.0 工具，并计划在不久的将来发布更多功能。请关注我们在[HuggingFace](https://huggingface.co/datasets/Roblox/RobloxGuard-Eval) 和[GitHub](https://github.com/Roblox/RobloxGuard-1.0) 上的页面，了解未来的更新、改进以及开源版本的发布。
