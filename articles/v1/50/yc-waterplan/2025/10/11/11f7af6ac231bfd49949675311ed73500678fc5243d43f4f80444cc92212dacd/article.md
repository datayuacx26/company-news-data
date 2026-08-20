---
schema_version: "1.0.0"
document_id: "11f7af6ac231bfd49949675311ed73500678fc5243d43f4f80444cc92212dacd"
company_key: "yc-waterplan"
company: "Waterplan"
source_id: "yc-waterplan-news-import-8432a9416678"
canonical_url: "https://www.waterplan.com/blog/building-a-multi-agent-risk-stewardship-assistant-at-waterplan"
published_at: "2025-10-20T00:00:00+00:00"
first_seen_at: "2026-07-24T06:52:56.269796+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:527f8ea2846e3c8e855989a48e1a3ff5292e450678232f8aa79c6f5dbb16ffe8"
---

# Building a Multi-Agent Risk & Stewardship Assistant at Waterplan

## The Problem We Wanted to Solve


To effectively assess and manage water risk, it is necessary to integrate various data sources. At waterplan we gather information at different granularity levels: this involves considering global data, local data, regulations and many other data sources. These are a few examples of pieces of information at different levels:


### Local Data


Data sourced from specific local information, like regional reports, news, or measurements, offering a focused view on hazards in a particular area.


### Global Data


Data sourced from comprehensive global datasets that provide a worldwide perspective on potential hazards.


Careful analysis of this diverse information, utilizing established methodologies and industry standards, is crucial for informed decision-making. The primary objective is not merely data collection, but a comprehensive understanding of multifaceted risks, facilitating improved management and sustainable water stewardship.


We initially attempted a singular approach, using one "agent" with a vast set of instructions to manage all information. However, the diverse nature of information and rules quickly overwhelmed this single agent, leading to confusion and a decline in result quality, which worsened with the addition of more data and rules. The inherent complexity of the problem made it clear that a monolithic plan would be ineffective.


Our solution involved developing a system with multiple specialized "agents" working collaboratively under a main "orchestrator." This design effectively breaks down complex problems into smaller, more manageable tasks, with each agent focusing on a specific aspect of the analysis. This significantly improves system performance and accuracy.


Below is a simple diagram showing the high level architecture of the agent. The different pieces will be explained in more detail later:


This collaborative approach ensures that our answers are grounded in real data and established rules, eliminating speculation and incorrect information. Each specialized agent contributes unique skills, such as validating risk figures, interpreting regulations, and identifying site-specific details, while the orchestrator unifies their efforts to deliver comprehensive and coherent responses. This solution successfully addressed our operational needs.
