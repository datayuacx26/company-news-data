---
schema_version: "1.0.0"
document_id: "f1b1947ef34e4346f99e17f5936c150941922bae4319c36ed897c11976969d22"
company_key: "roblox-corporation-class-a-common-stock"
company: "Roblox Corporation"
source_id: "roblox-corporation-class-a-common-stock-news-import-13cc9c892e31"
canonical_url: "https://about.roblox.com/newsroom/2026/08/roblox-open-source-safety-models-roost"
published_at: "2026-08-19T12:00:00+00:00"
first_seen_at: "2026-08-19T22:58:12.628913+00:00"
fetched_at: "2026-08-19T22:58:15.386515+00:00"
content_hash: "sha256:bef27f526350627974f509b452661e11ec12c8dbb76fe0761baac577c3b393c4"
---

# Roblox Brings Open-Source Safety Models to ROOST Model Community

Share


[Engineering](https://about.roblox.com/newsroom?filter=engineering)[News](https://about.roblox.com/newsroom?filter=news)


# Roblox Brings Open-Source Safety Models to ROOST Model Community


Open-Sources a New Benchmark and Updated PII Classifier, Voice Safety Classifier, and Roblox Sentinel


By


Naren Koneru, Vice President of Trust and Safety Engineering


Published


Aug 19, 2026


Roblox is contributing three open-source safety models to the Robust Open Online Safety Tools (ROOST) Model Community. These include updates to our open-source[PII Classifier](https://huggingface.co/Roblox/roblox-pii-classifier-v2) ,[Roblox Sentinel](https://github.com/Roblox/Sentinel) , and our latest[voice safety classifier](https://huggingface.co/Roblox/voice-safety-classifier-v3) . We’re also open-sourcing a new evaluation dataset that other companies can use to benchmark their own classifiers. Versions of these models already run on Roblox, detecting attempts to solicit or share personal information, flagging early signs of child endangerment, and moderating voice chat in real time.


We joined ROOST as a[founding member](https://about.roblox.com/newsroom/2025/02/becoming-a-founding-partner-of-open-source-initiative-key-to-online-safety-approach) in early 2025, alongside Google, OpenAI, and others. Sharing these models gives other platforms a robust starting point to train and tune their own moderation tools. In turn, we hope to benefit from the learnings and feedback of other companies as they share their work.


## Keeping Personal Information Personal


The[Roblox PII Classifier](https://about.roblox.com/newsroom/2025/11/open-sourcing-roblox-pii-classifier-ai-pii-detection-chat) is trained to detect players’ attempts to share or request personally identifiable information (PII) or move players to other platforms, where protections may not be as robust. That includes efforts to bypass detection through misspellings, coded words, or obscure references to other platforms. Rather than searching for exact word matches, it’s built to understand conversational context, helping it catch cryptic requests that standard filters might miss.


[Version 2.0](https://huggingface.co/Roblox/roblox-pii-classifier-v2) brings several major updates: Training and evaluation data now incorporate surrounding conversational context, rather than isolated messages that could seem ambiguous. Evaluating messages in the context of broader conversation helps detect intent to share or solicit PII. Training on an LLM-generated set of synthetic examples of more complex bypass scenarios and rare multilingual cases helped us scale the languages we support from 17 to 189. We’ve also paired LLMs with context-fetching to pull relevant samples from a curated set of labeled data. Today, version 2.0’s precision and recall improves upon traditional moderation techniques—particularly on more sophisticated bypass attempts. For example, if someone is asked about a server on another platform, they might obscure their answer:


User: “it’s dscrd (dot) gg (slash) rblx-vibe-99,”


User: “now replace (dot) with . and (slash) with /.”


Version 2.0 can understand the context of those messages together and prevent the sharing of PII that could move a player off of Roblox. Thanks to automated red-teaming and targeted synthetic data generation, the model is even better at distinguishing nuanced PII sharing from casual conversation while preserving a natural user experience. This work raised our F1 score from 63.41 to 90.52—a significant improvement over version 1.0 that achieves top scores across internal and open-source benchmarks.


## Sharing an Evaluation Data Set


[Roblox PII Classifier Benchmark](https://huggingface.co/datasets/Roblox/roblox-pii-safety-for-chat-benchmark) is a new evaluation data set for PII classifiers with synthetic samples of English-language multiuser chats modeled on techniques often used to evade PII filters. The samples are LLM-generated simulations of players asking for or sharing PII, or directing players off platform. Strategies included phonetic bypasses, character and visual substitution, coded language, and information split across conversational turns. We also trained on hard negatives, such as chats that are benign but look similar to conversations that include PII sharing or solicitation.


This data set reflects realistic multiuser online chat, as opposed to existing data sets that often focus on named-entity extraction—a small subset of what platforms see. In practice, attempts to solicit PII often appear well before any detectable PII string—for example, a player asking another, “wuts ur @ on the app with the ghost?” We’re sharing this benchmark so the community can test and improve upon Roblox’s PII Classifier, helping us all move toward even stronger online protections.


## Detecting Risk as Early as Possible


[Roblox Sentinel](https://about.roblox.com/newsroom/2025/08/open-sourcing-roblox-sentinel-preemptive-risk-detection) is designed to detect early signals of potential child endangerment, well before messages become explicit enough for other systems to detect. Built on contrastive learning, Sentinel is trained on patterns of both benign and eventually harmful conversations. It compares new conversations with those patterns to flag subtle warning signs for review before they can escalate. Sentinel lets human reviewers prioritize the chats most likely to require attention, so they can quickly take action on bad users’ accounts and report problematic users to the appropriate authorities. As of the 12 months ended August 7, 2026, nearly 70% of the cases we detected were due to Sentinel's early detection, often catching these cases much sooner than other detection methods.


Today, we’re[releasing version 2](https://github.com/Roblox/Sentinel/blob/main/V2_FEATURES.md) , which increases accuracy by offering more ways to combine scores, expanding from two combining functions to six, each suited to a different type of data. Incorporating feedback from the ROOST community, version 2 also adds new ways to evaluate which function best fits a given use case, including explanations of why a source scored the way it did. And by eliminating the need to recompute evaluation data for each setting, version 2 makes searching across those choices practical. In one example, version 2 was able to complete a sweep across 324 configurations in less than three minutes—that sweep would previously have taken nearly an hour. It also raised ROC-AUC (a standard measure of ranking quality) from 0.894 with the default settings to 0.996 with the best configuration identified. **** These optimizations should make this new open-source tool easier to test and tune for specific use cases.


## Moderating Voice Safety in Real Time


Notification that appears after a player has broken the rules more than once, letting them know that voice chat is temporarily suspended and giving them an opportunity to appeal.


Our[voice safety classifier](https://about.roblox.com/newsroom/2024/07/deploying-ml-for-voice-safety) is built to detect and flag inappropriate speech in real time. When a player breaks one of our rules, they see a notification giving them a warning and sharing information on which policy they violated. If they continue to break the rules, chat access will be temporarily suspended for up to five minutes. Players may be subject to harsher consequences for larger violations or if other players file reports against them. Our classifier has been downloaded more than 72,000 times since we open-sourced it in 2024.


Recently released,[version 3](https://about.roblox.com/newsroom/2026/06/upgrading-voice-safety-classifier-22-languages-sharper-detection-capabilities) moderates voice chat across a total of 30 languages and eight violation categories with even higher recall and precision. Across all 30 languages, it achieves 61% recall at a strict 1% false-positive rate. It also adds built-in language detection. These gains were driven by more labeled training data across all 30 supported languages—machine labeling for volume, then human labeling for quality. Even though the underlying model increased from 94.6 million to 320 million parameters, we used model distillation to keep it fast enough for real-time detection.


Safety is a shared responsibility—no company can solve it alone. Our intent in contributing to ROOST is to help other companies improve their own safety classifiers. We will continue sharing and learning from the ROOST community as we advance automated detection together.
