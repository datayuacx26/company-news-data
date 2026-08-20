---
schema_version: "1.0.0"
document_id: "36d009b4460f3e0fb7fb7a1b7c8c77c09fa7ee89ca12e40f6842f3d1c80a753e"
company_key: "yc-adaptional"
company: "Adaptional"
source_id: "yc-adaptional-news-import-c2112bd5ddd4"
canonical_url: "https://www.adaptional.com/blog/why-loss-runs-are-the-hardest-document-in-insurance-automation"
published_at: null
first_seen_at: "2026-07-21T04:38:31.952963+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:2a2d78476c03a457aa906fd57c6d12e069811389c9d097bf6cd488afea464191"
---

# Why Loss Runs Are the Hardest Document in Insurance Automation

### Why Loss Runs Break Even the Best Extraction Systems


Loss runs are simple.


They are structured, consistent tables listing every historical loss for an account—the sort of document a large-context model should be able to parse and extract from without issue.


In practice, though, this is far from the truth.


‍


*Figure 1. Loss Run Example*


Loss runs are precisely where extraction systems break. The documents are long, inconsistent, and unforgiving.


They arrive as lengthy PDFs with multiple layouts stitched together. Sometimes everything is in one file; other times, if the insured had multiple carriers, you receive multiple loss runs, each with its own format and quirks. Some pages contain clean tables. Others contain wrapped rows, partial rows, or summaries that look identical to real claims.


‍


Page headers masquerade as data. Totals repeat across sections. A single claim might appear three times, each with slightly different values. Missing even one major loss wipes out the credibility of the entire pipeline.


*Figure 2: The Illusion of Structured Loss Runs*


The problem isn’t unique to LLMs. Underwriters face the same issues with inconsistent reporting formats, which can be confusing even by human standards.


This brings us to the central question: if humans struggle to reason through loss runs, how can we teach an LLM to do the same?


Let’s explore the two main challenges for LLM reasoning with loss runs.


### Context Window Pressure


*Figure 3: How Long Documents Break LLM Reasoning*


Even when an entire PDF fits inside a model’s context window, accuracy drops as the document grows. Long reports cause models to:


- Treat summaries as standalone losses
- Lose track of where a claim first appeared
- Produce inconsistent values for the same fields
- Miss subtle differences between rows


You cannot simply dump a 100-page PDF into a model. That is not a solution. It’s a stress test.


You need structure before you need scale.


The context window problem naturally leads to the second major issue: criticality.


‍


### The Real Bottleneck: Criticality


Loss runs have no tolerance for error.


If a property schedule is slightly off, someone can fix it. If a driver list misses a low-severity detail, the impact is contained.


Loss runs do not offer that luxury.


Missing a large loss is a pricing error. In some cases, it becomes a regulatory problem. The system must therefore be designed around *not missing* . It doesn’t matter how elegant the model is if it drops a seven-figure claim buried halfway through the file.


This matters even more because explainability and reliability are already pressure points for AI adoption. Loss runs hit both. They contain financial history, determine risk appetite, and shape premium decisions. When stakes are high, tolerance for mistakes collapses.


*Figure 4: High-Stakes Errors in Loss Run Extraction*


So how do you reduce mistakes in a system that is this critical, this long, and this unstructured?


‍


### A System That Mirrors How Underwriters Actually Read


The extraction pipeline becomes far more reliable when it follows the habits of an underwriter.


Start with structure. Separate tables from summaries. Merge rows split across pages. Remove repeated totals. Once the layout is normalized, extraction becomes predictable.


Then assign stable identities to each claim. Use claim numbers when available. When they’re missing or unreliable, anchor on combinations of fields such as date, description, and location. The goal is simple: every appearance of the same loss collapses into one record.


Next, apply a secondary model-based validation pass. Feed the extracted output back into an LLM and ask it to proofread for internal consistency—mismatched totals, duplicated claims, missing fields, or values that contradict the source document. This mirrors a junior reviewer scanning for obvious issues before escalation.


Finally, validate high-severity losses independently. Scan for anything above a dollar threshold. Reconcile claim-level totals with carrier-level totals. If something looks off, surface it immediately for human review.


As criticality increases, accountability becomes non-negotiable. When choosing between an LLM and a human, responsibility must remain with the human.


Naturally, loss runs should include human review and flags at intermediate steps. This doesn’t reduce inherent criticality. It enforces responsibility.


‍


### Closing Thought


The ethos behind automation is efficiency.


But efficiency, like any metric, has tradeoffs. Not every task should be pushed toward full end-to-end automation simply because the tools allow it. The goal is meaningful impact and not automation for its own sake.


It’s easy to claim that higher precision solves the problem. It doesn’t. Criticality isn’t a model issue; it’s an inherent property of the document. Loss runs matter because the information inside them matters. High-value documents create high-stakes extraction.


While we’ve outlined ways to improve consistency, reconciliation, and context handling, everything rests on one premise:


**The system cannot miss.**


‍


### Acknowledgements


Jeffrey Xie


‍


‍
