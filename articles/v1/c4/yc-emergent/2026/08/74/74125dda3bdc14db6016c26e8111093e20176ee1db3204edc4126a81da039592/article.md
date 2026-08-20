---
schema_version: "1.0.0"
document_id: "74125dda3bdc14db6016c26e8111093e20176ee1db3204edc4126a81da039592"
company_key: "yc-emergent"
company: "Emergent"
source_id: "yc-emergent-news-import-16a7bf482038"
canonical_url: "https://emergent.sh/news/gpt-56-arc-agi-3-launch"
published_at: "2026-08-06T10:16:31+00:00"
first_seen_at: "2026-08-06T23:44:13.436472+00:00"
fetched_at: "2026-08-06T23:44:14.062043+00:00"
content_hash: "sha256:1303953adce0884e030b3941180bd718b3a95629e7c97fd5893e3566a6133c7f"
---

# GPT-5.6 ARC-AGI-3 Scores Triple With Two API Settings

OpenAI has disclosed a significant performance breakthrough for GPT-5.6, revealing that enabling two specific API settings tripled the model's scores on the challenging ARC-AGI-3 benchmark. The discovery centers on reasoning retention and compaction features, which together unlock substantially improved problem-solving capabilities without requiring model retraining or architectural changes.


## The ARC-AGI-3 Challenge and GPT-5.6 Baseline


The Abstraction and Reasoning Corpus for Artificial General Intelligence (ARC-AGI-3) represents one of the most demanding benchmarks for evaluating AI systems' ability to perform abstract reasoning and pattern recognition. Unlike conventional language tasks, ARC-AGI-3 requires models to identify underlying rules in visual grid puzzles and apply those rules to novel situations, testing true generalization rather than memorized patterns.


GPT-5.6's baseline performance on ARC-AGI-3 positioned it competitively among frontier models, but OpenAI researchers identified untapped potential in how the model processes multi-step reasoning tasks. The breakthrough came not from scaling compute or fine-tuning, but from adjusting how reasoning chains are managed during inference.


## Reasoning Retention: Preserving Cognitive Context


The first critical setting involves reasoning retention, a parameter that controls whether intermediate reasoning steps persist across API calls. When enabled, GPT-5.6 maintains its problem-solving context rather than treating each query in isolation. For ARC-AGI-3 tasks requiring iterative hypothesis testing, this continuity proved essential.


According to OpenAI's technical documentation, reasoning retention allows the model to:


- Build cumulative understanding of problem constraints across multiple attempts
- Reference earlier failed approaches to avoid repetitive errors
- Maintain working memory of partial solutions and intermediate insights
- Apply learned patterns from previous grid puzzles within the same session


This setting proved particularly effective on ARC-AGI-3's multi-part problems, where solutions require synthesizing insights from several related sub-tasks.


## Compaction: Streamlining Reasoning Efficiency


The second breakthrough setting, compaction, addresses how GPT-5.6 organizes and prioritizes reasoning tokens during extended problem-solving sessions. Rather than accumulating verbose reasoning chains that can dilute signal quality, compaction condenses intermediate steps into their essential logical components.


OpenAI reports that compaction improved both accuracy and efficiency, reducing token consumption while maintaining or enhancing solution quality. For developers working with API cost constraints, this dual benefit makes the setting particularly valuable beyond benchmark performance alone.


The compaction mechanism appears to function as an intelligent summarization layer, distilling lengthy reasoning processes into compact representations that preserve causal relationships and key decision points without carrying forward redundant elaboration.


## Combined Impact on ARC-AGI-3 Scores


When both settings were enabled together, GPT-5.6's ARC-AGI-3 scores increased roughly threefold compared to the baseline configuration. OpenAI has not disclosed the exact numerical scores, but the magnitude of improvement suggests a fundamental shift in the model's effective reasoning capacity on abstract tasks.


The synergy between reasoning retention and compaction creates a feedback loop: retained context becomes more valuable when efficiently compacted, while compaction becomes more effective when it can reference accumulated reasoning history. This interaction effect exceeded the additive gains from either setting in isolation.


## What This Means for AI Reasoning Capabilities


OpenAI's findings demonstrate that frontier model performance often depends as much on inference-time configuration as on training-time optimization. For developers building applications that require complex reasoning, multi-step problem solving, or abstract pattern recognition, these settings offer immediate performance gains without waiting for next-generation models. The research also suggests that current large language models may possess latent capabilities that remain inaccessible under default API configurations, pointing toward a broader opportunity for unlocking performance through smarter orchestration of existing systems.
