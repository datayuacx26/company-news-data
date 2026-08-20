---
schema_version: "1.0.0"
document_id: "f7b34510a206878190c14f5f50be4dc8608d311fd62d6a9c302c6bb93242624c"
company_key: "yc-datasaur"
company: "Datasaur"
source_id: "yc-datasaur-news-import-f7082d4871d2"
canonical_url: "https://datasaur.ai/blog-posts/june-feature-updates-smarter-search-faster-reviews-and-better-label-organization"
published_at: "2025-06-30T00:00:00+00:00"
first_seen_at: "2026-07-25T01:27:49.393704+00:00"
fetched_at: "2026-07-28T21:59:46.813241+00:00"
content_hash: "sha256:025e1716fcafbb1430dd1733ba4d9dd0f19ec21dc615396d7419b667d96f82f4"
---

# June Feature Updates: Smarter Search, Faster Reviews, and Better Label Organization

We’ve added a set of improvements designed to make your Span Labeling workflows smoother, faster, and more accurate. From smarter search to better reviewer insights, here is what’s new with Data Studio and LLM Labs.


## Data Studio


### Clearer Review: Advanced Search Now Shows Unresolved and Rejected Labels


We’ve expanded the Advanced Search in the Span Labeling extension to include **unresolved** and **rejected** labels. This gives reviewers a clearer view of what’s been previously reviewed—whether by themselves or others—and highlights areas that still need attention. It’s a more efficient way to ensure clean, validated annotations without missing a step.


[Learn more](https://docs.datasaur.ai/advanced/extensions/search#advanced-search)


### Bulk Review: Accept or Reject All Conflicted Labels in a Line


Speed up your review process with new right-click options to **accept or reject all conflicted labels in a line** . This also works for labels that span across multiple lines, so you no longer have to handle each one individually.


### Visible Attribution: Enhanced Tooltips Show Reviewer Actions


In Span Labeling, label tooltips now display reviewer actions—such as **“Accepted by”** or **“Rejected by”** —when you hover over a label. This added context helps reviewers understand whether a label was confirmed by another human or auto-accepted by the system, supporting smoother collaboration and clearer decision-making during review.


### Organized Labeling: Use Identical Labels Across Categories


You can now assign the same label name under different parent labels—like **“Credit Risk”** under both **Loan Analysis** and **Investment Assessment** . This makes it easier to organize related concepts without awkward workarounds, keeping your label sets clean while accurately reflecting their context.


[Learn more](https://docs.datasaur.ai/data-studio-projects/lets-get-labeling/label-sets#id-3.-hierarchical-label-set-in-span-labeling-projects)


## LLM Labs


### Enhanced Chunking Capabilities in Knowledge Base


Knowledge Base now offers more granular control over document chunking, giving you precise control over how your content is processed and stored. The enhanced chunking system allows you to fine-tune chunk sizes, overlap settings, and processing parameters at a much more detailed level than before.


The most significant improvement is the ability to configure chunking settings on a per-document basis. Different document types often require different chunking strategies for optimal performance.


For example, technical manuals might benefit from larger chunks to preserve context, while FAQ documents might work better with smaller, more focused chunks. Legal documents might need specific overlap settings to maintain regulatory context, while creative content might require different parameters entirely.


This document-specific configuration ensures that each piece of content in your Knowledge Base is processed using the most appropriate chunking strategy, leading to more accurate retrieval and better RAG performance. You can now optimize your knowledge base for maximum effectiveness across diverse content types, all within a single repository.


Try out these new features and discover how they can enhance your LLM development workflow!
