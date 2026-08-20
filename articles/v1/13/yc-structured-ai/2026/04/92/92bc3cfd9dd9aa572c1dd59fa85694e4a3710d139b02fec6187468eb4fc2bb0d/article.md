---
schema_version: "1.0.0"
document_id: "92bc3cfd9dd9aa572c1dd59fa85694e4a3710d139b02fec6187468eb4fc2bb0d"
company_key: "yc-structured-ai"
company: "Structured AI"
source_id: "yc-structured-ai-news-import-70ac70f80ba3"
canonical_url: "https://getstructured.ai/blog/aec-bench-results/"
published_at: "2026-04-13T00:00:00+00:00"
first_seen_at: "2026-07-24T02:36:35.541532+00:00"
fetched_at: "2026-07-28T21:25:38.770750+00:00"
content_hash: "sha256:cc6d1f65ebad0f98fa49b4dc8a34d8afb3fbc4578fad7c83c796b823e4a79645"
---

# We Ran AEC-Bench. Here Are the Full Results.

AEC-Bench is the first open-source benchmark designed to test whether AI agents can actually work with construction documents. Not summarise them. Not chat about them. Work with them: navigate drawing sets, trace cross-references, verify submittals against specifications, catch coordination failures across hundreds of pages.


The benchmark was published in March 2026, and it is freely available for any company to run. 196 real task instances, nine task families, three complexity levels, scored against ground truth defects placed by domain experts. It tests the work your team does every day.


We ran it. **As of today, our Document Agents hold the highest score in every complexity category on AEC-Bench, and on several task types the gap is not close.** Here are the full results.


## Why we ran it


If you have sat through a demo of an AI tool for construction and thought "that looks great, but does it actually work on a real drawing set?" then you understand why AEC-Bench matters.


The AEC industry does not have a shortage of AI claims. What it has is a shortage of evidence. Every vendor can cherry-pick a result, define their own evaluation criteria, and report whatever makes the pitch look good. AEC-Bench removes that option. The tasks are defined. The ground truth is fixed. The scoring is automated. You either get it right or you do not.


We ran it because we wanted to know, with certainty, where our Document Agents stand.


## What AEC-Bench tests


The benchmark covers nine task types across three complexity levels.


Scope Task Type Tasks What It Tests


Intra-Sheet Detail Technical Review 14 Localised technical questions about a single detail view


Detail Title Accuracy 15 Whether detail titles match what is actually drawn


Note Callout Accuracy 14 Whether callout text corresponds to referenced elements


Intra-Drawing Cross-Reference Resolution 51 Finding cross-references that point to non-existent targets


Cross-Reference Tracing 24 Tracing all locations that reference a given detail


Sheet Index Consistency 14 Checking sheet index entries match actual sheets


Intra-Project Drawing Navigation 12 Locating the correct file, sheet, and detail for a query


Spec-Drawing Sync 16 Finding conflicts between specifications and drawings


Submittal Review 36 Evaluating submittals for spec and drawing compliance


If you have ever spent a Thursday evening tracing a detail reference across a 300-page drawing set, you will recognise these tasks.


## Our results


### Overall: 84.7% across all 196 tasks


Best configuration shown per agent family. Nomic Agent published aggregate scope scores only (no per-task breakdown). "Next Best" is the highest score from any configuration in the paper. Results as of 12 April 2026.


### By complexity scope


Best configuration shown per agent family. Results as of 12 April 2026.


The widest gap is on Intra-Project tasks: cross-document coordination, submittal review, spec-drawing sync. These require holding multiple documents in view simultaneously, a drawing set, a specification, a submittal package, and catching where they diverge. That is the work that takes a coordinator days, and where errors carry the most consequence.


## Submittal review: the standout result


With 36 task instances, submittal review is the largest task family in the benchmark and the most commercially relevant. Evaluating submitted product data against project specifications and drawings is high-stakes coordination work: a false positive wastes your team's time, a false negative creates real risk.


The highest score in the AEC-Bench paper across all models and configurations was 23.1%. Our Document Agents scored 75.0%.


The paper explains why this task is so difficult. Agents "tend to over-generate findings, leading to a high rate of false positives." The goal is not coverage. The goal is accuracy you can trust.


## What this means if you are evaluating AI for your practice


AEC-Bench gives the industry something it has not had before: a common, reproducible test. If you are comparing AI tools for your firm, you now have sharper questions to ask:


- **"What is your AEC-Bench score?"** If a vendor has not run it, or will not publish the results, that tells you something.
- **"How do you handle cross-document tasks?"** Single-page text extraction is largely solved. The hard problems are the ones that require holding a full drawing set, a specification, and a submittal package in view simultaneously.
- **"Can you show me the agent's reasoning?"** A number on a benchmark is a starting point. What matters is whether you can see how the tool arrived at its answer and verify it yourself.


If you want to see how our Document Agents handle your specific document types,[book a demo](https://getstructured.ai/contact/) .


**Source:**[AEC-Bench: A Benchmark for AI Agents in Architecture, Engineering, and Construction](https://arxiv.org/abs/2603.29199) (arXiv 2603.29199, March 2026)


[Read the full paper →](https://arxiv.org/abs/2603.29199)


Want to see how Structured AI can work for your team?


[Book a Demo](https://getstructured.ai/contact/)
