---
schema_version: "1.0.0"
document_id: "b95b89e5f9b0da55fd02efb4145b1111d81a089fdfc3d7b591f489484e9d5909"
company_key: "roblox-corporation-class-a-common-stock"
company: "Roblox Corporation"
source_id: "roblox-corporation-class-a-common-stock-news-import-f52851b7bf16"
canonical_url: "https://about.roblox.com/zh-hans/newsroom/2025/04/updating-ml-voice-safety-more-languages"
published_at: "2025-04-02T19:00:00+00:00"
first_seen_at: "2026-07-23T23:14:17.357493+00:00"
fetched_at: "2026-07-28T21:30:42.971376+00:00"
content_hash: "sha256:c7b9337b505814f6b50b5c2bc804c0a492b4e0d46667b7f01206861f7c36602e"
---

# 为我们的开源语音安全模型新增更多语言支持

分享


[工程](https://about.roblox.com/zh-hans/newsroom?filter=%E5%B7%A5%E7%A8%8B)[安全与文明](https://about.roblox.com/zh-hans/newsroom?filter=%E5%AE%89%E5%85%A8%E4%B8%8E%E6%96%87%E6%98%8E)[新闻](https://about.roblox.com/zh-hans/newsroom?filter=%E6%96%B0%E9%97%BB)


# 为我们的开源语音安全模型新增更多语言支持


作者


Naren Koneru，工程副总裁，以及 Janne Pylkkonen


已发布


2025年4月2日


- 我们正在更新开源[语音安全分类器](https://about.roblox.com/newsroom/2024/07/deploying-ml-for-voice-safety) ，将其参数从9460万增加到1.202亿，并新增支持七种语言。
- 自分类器首个版本发布以来，我们在1%误报率下，针对英语语音聊天数据的召回率已提升至59.1%。相较于上一版本30.9%的召回率，这一数据实现了92%的提升。


维护安全与文明始终是 Roblox 一切工作的基石。近二十年来，我们致力于构建强大的安全系统，并随着新技术的出现不断对其进行扩展和优化。2024 年，我们推出了超过 40 项安全改进，其中包括[对家长控制功能的全面升级](https://about.roblox.com/newsroom/2024/11/major-updates-to-our-safety-systems-and-parental-controls) ，而[今天](https://about.roblox.com/newsroom/2025/04/new-parental-controls-on-roblox) 我们将[再次](https://about.roblox.com/newsroom/2025/04/new-parental-controls-on-roblox) 对其[进行更新](https://about.roblox.com/newsroom/2025/04/new-parental-controls-on-roblox) 。 此外，我们还推出了业内首批开源[语音安全分类器](https://about.roblox.com/newsroom/2024/07/deploying-ml-for-voice-safety) 之一，其[下载](https://huggingface.co/Roblox/voice-safety-classifier) 量已超过23,000次。今天，我们发布了[更新](https://huggingface.co/Roblox/voice-safety-classifier-v2) 版本，该版本准确性更高，且支持更多语言。


包括该分类器在内的许多保护用户的安全系统，均由 AI 模型驱动。我们开源其中部分模型，因为我们深知[分享 AI 安全领域的进步](https://about.roblox.com/newsroom/2025/02/becoming-a-founding-partner-of-open-source-initiative-key-to-online-safety-approach) 将惠及整个行业。这也正是我们近期作为创始合作伙伴[加入 ROOST](https://about.roblox.com/newsroom/2025/02/becoming-a-founding-partner-of-open-source-initiative-key-to-online-safety-approach) 的原因——这是一个致力于通过推广开源安全工具来解决数字安全重要领域的新型非营利组织。


在管理全球平台上每天产生的海量内容和互动时，AI 是保障用户安全的关键要素。我们确信，我们构建的模型正在有效支持我们的需求。例如，在 2024 年第四季度，Roblox 用户上传了 3000 亿条内容。 在这数以亿计的视频、音频、文本、语音聊天、虚拟形象和 3D 体验中，仅有 0.01% 被检测为违反了我们的政策。而且，几乎所有这些违反政策的内容都在用户看到之前就被自动预筛查并移除了。


我们已更新了语音安全分类器的开源版本，以提升其准确性，并帮助我们对更多语言的内容进行审核。新模型：


- 通过多语言数据训练，新增了对七种语言（西班牙语、德语、法语、葡萄牙语、意大利语、韩语和日语）违规内容的检测能力。
- 整体召回率提升至59.1%，较上一版本30.9%的召回率提高了92%，且误报率较低。
- 该模型经过大规模部署优化，高峰期每秒可处理多达 8,300 次请求（其中大部分不包含违规内容）。


自首个模型发布以来，我们观察到美国用户每小时语音内容中的违规举报率降低了 50% 以上。该模型还帮助我们每天[对](https://about.roblox.com/safety-civility-resources?section=safety&article=enforcing-the-rules) 数百万分钟的语音聊天[进行审核](https://about.roblox.com/safety-civility-resources?section=safety&article=enforcing-the-rules) ，其准确度甚至超过了人工审核员。我们从未停止对安全系统的改进，并将继续更新开源版本。


### 高效的多语言语音安全分类器


我们最初的开源语音安全分类器基于 WavLM base+ 模型，并通过人工标注的英语语音聊天样本进行了微调。这种端到端架构取得的令人鼓舞的结果，促使我们进一步尝试定制化架构。我们利用知识蒸馏技术优化了模型的复杂度和准确率，这对于大规模推理服务具有显著优势。 我们的新分类器沿用了这些基础构建模块，并在数据利用和架构优化方面进行了扩展和深化。


通过多语言数据训练，我们的单一分类器模型可无缝处理我们支持的前八种语言中的任意一种。此外，训练方法的改进使得该模型不仅更准确，而且在典型的推理场景中运行速度比第一版快了20%至30%。


新的语音安全分类器仍基于 WavLM 架构，但层级配置与之前发布的版本以及 WavLM 预训练模型有所不同。特别是，我们增加了一个额外的卷积层，以降低变压器层的内部时间分辨率。 总体而言，新模型架构拥有1.202亿个参数，较上一版本的9460万个参数增加了27%。尽管参数数量增加，但在处理4至15秒的输入片段时，新模型的计算时间却减少了20%至30%。这是因为该模型将输入信号压缩成了比之前更短的表示形式。


#### 采用多种标注策略


端到端模型的监督式训练需要经过精心整理的音频与类别标签配对数据。我们对数据管道进行了重大改进，确保了标注数据的稳定供应。训练材料的基础是一个庞大的机器标注数据集，包含超过10万小时的涵盖所有支持语言的语音数据。 我们自动转录语音内容，并将其输入内部开发的基于文本的毒性分类器进行处理，该分类器采用了预设的政策和毒性分类标准。数据采集过程中，恶意内容的采样概率高于正常语音，以便更好地捕捉边界案例和较不常见的政策违规情况。


基于语音转录文本和文本分类的标签无法完全捕捉语音聊天内容中的细微差别。因此，我们利用人工标注的数据对前一训练阶段的模型进行了微调。虽然分类任务保持不变，但后期的训练阶段有助于优化决策边界，并增强模型对语音聊天特有表达的响应能力。这是一种课程学习（curriculum learning）的形式，有助于我们最大限度地利用宝贵的人工标注示例。


端到端模型训练面临的一个挑战是，如果标注政策随时间变化，目标标签可能会过时。因此，随着我们不断完善可接受语音政策，我们需要对采用旧标注标准的数据进行特殊处理。 为此，我们采用了一种多任务方法，使模型能够从不符合当前语音聊天政策的数据集学习。这包括为旧政策专门设置一个独立的分类头，从而允许模型主干从旧数据集学习，同时不影响目标标签或主分类头。


#### 便于部署的校准模型


使用该分类模型时，需确定工作点并根据任务要求调整分类器的灵敏度。为简化模型部署，我们对模型输出进行了校准，并针对语音聊天审核任务进行了微调。 我们基于保留集估算了分段线性变换，并针对每个输出头和支持的语言分别进行处理。这些变换在模型蒸馏过程中被应用，从而确保最终模型在原生层面上已校准。这消除了在推理过程中进行后处理的必要性。


我们很高兴将这个新的[开源模型](https://github.com/Roblox/voice-safety-classifier) 分享给社区，并期待在有新进展时及时分享。
