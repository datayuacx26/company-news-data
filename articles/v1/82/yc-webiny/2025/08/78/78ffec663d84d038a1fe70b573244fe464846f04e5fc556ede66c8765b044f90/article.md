---
schema_version: "1.0.0"
document_id: "78ffec663d84d038a1fe70b573244fe464846f04e5fc556ede66c8765b044f90"
company_key: "yc-webiny"
company: "Webiny"
source_id: "yc-webiny-news-import-09ea055e8444"
canonical_url: "https://www.webiny.com/blog/migration-vs-recreation-of-content-lessons-learned-from-enterprise-clients-replatforming"
published_at: "2025-08-18T15:27:37.310+00:00"
first_seen_at: "2026-07-22T19:34:09.199841+00:00"
fetched_at: "2026-07-28T21:27:42.276842+00:00"
content_hash: "sha256:922f548ac2a78a8a40a098ff00fdaa9a01bec2dfad9df2321619eb75b6656a0f"
---

# Migration vs. Recreation of Content: Lessons Learned From Our Enterprise Clients Replatforming | Webiny

When replatforming from a legacy CMS to Webiny, one of the earliest and the hardest decisions you will face is whether to migrate existing content or start over and recreate it all from scratch.


This is not just a technical debate. It’s a decision that will affect your architecture, your timelines, and your teams. Having helped enterprises to migrate from WordPress, Drupal, and Adobe Experience Manager to Webiny, we’ve learned first-hand that the answer isn’t always obvious and there is no one-size-fits-all approach, but hopefully in this article, we can share some learnings with you that can help you on your journey with Webiny or any other new system implementation.


### **🚚 Migration: When You Bring It All With You**


For teams with


**massive libraries of content** or demanding compliance requirements, content migration might seem like the only path forward. One of our clients, for example, had


**hundreds of thousands of posts, millions of media assets, and metadata** tied to SEO performance and editorial workflows. Recreating all of these anew looked like an impossible task. Also their content is their


**product.** The more posts they have online, the better. They didn’t want to rationalise the legacy content, nor simplify, nor archive older posts, because the existing articles were driving a huge amount of traffic to their website, so they decided to migrate it all.


Here is some of the thinking behind this decision. It could be applicable to you too.


### **✅ Pros:**


- **Reduces Editorial Rework** — avoids reformatting or rewriting hundreds / thousands of articles, especially when they only need minor or no tweaks.


- **Preserves Audit Trails & Historical Data -** retaining original content / author details can help with legal / compliance traceability — especially important in regulated industries, in this specific case because of copyrights.


- **Supports Redirect & SEO Continuity** — original URLs, metadata, and embedded links can be retained and mapped, preserving SEO equity.


- **Content Familiarity for Internal Teams -** editors and marketers already understand the structure and taxonomy of the articles, which lowers the learning curve.


- **Backwards-compatible Integrations** — existing API consumers (e.g., front-ends, search, feeds) can often continue working with adapted schemas with the new system.


- **Justifies CMS Investment to Business Stakeholders** — “We didn’t lose anything — we just made it better” is an easier sell than “We threw it all out and started over.”


### **❌ Cons:**


- **Legacy Schema Problems and Technical Debt Come Along** — risk carrying over poor content modelling decisions, inconsistent naming conventions, or deprecated fields — which can pollute your new system and limit future flexibility. Some analysis and rationalisaton had to take place to avoid the high impact of this.


- **Complex Mapping Logic Across Fields, Media, and Relationships** — migrating structured content often involves deeply nested models, cross-referenced entries, and asset associations — all of which require careful mapping and transformation logic to preserve integrity.


- **Time-Intensive Validation and QA** — even after successful migration scripts, extensive testing is required to ensure every piece of content renders correctly — especially across localized versions, rich text fields, and dynamic widgets.


- **Greater Risk of Edge-Case Failures** — older content often contains inconsistencies, unclosed tags, orphaned entries, deleted/ missing assets, or outdated formatting. These can surface as unpredictable bugs in the new front-end or APIs, especially when deeply nested or dynamic content is involved.


- **Rebuilding Previews and Front-End Behaviors Takes Longer** — migrated content might not align 1:1 with your new front-end architecture, leading to the need for additional custom logic to render previews or support new experiences.


- **Slower Onboarding for New Editors** — migrated content structures can retain complexity that made sense in the old CMS, and older editors were used to the structure, but it could confuse new editors working in a cleaner Webiny environment.


Migrating into Webiny often requires


**custom mapping scripts, content model restructuring** , and a rigorous test loop. Our engineering and product teams are always available to help with these, but it is essential that you have analysed your situation thoroughly and you have made the right decision for your organization and data to migrate this way.


Also with the power of AI it is possible to build the migration script quickly and easily and we already took this approach with one of our clients to shorten the timeline for migration. Some manual fixes were required, but the majority of the migration script was built with the help of AI.


### **🧼 Recreation: A Clean Slate — at a Cost**


Some teams opt to


**start fresh** , especially when:


- Content is outdated or inconsistent, and you want to recreate it to fix it.


- Legacy models don’t align with the new ways of working and data structure the organization is striving for.


- There’s a strategic push for content and / or organizational restructuring.


### **✅ Pros:**


- **Cleaner, Modern Content Models -** starting fresh allows you to design purpose-built schemas aligned with your current use cases, editorial workflows, and front-end needs — without legacy baggage.


- **Simplifies Use of Webiny’s Extensibility and Multi-Tenancy -** new content structures make it easier to leverage Webiny features like plugins, environments, roles, and tenant isolation without hacking around old constraints.


- **Improves Front-End Performance and Indexing Logic -** well-designed, consistent data models reduce the complexity of GraphQL queries and OpenSearch indexing, which directly improves performance and reliability, especially if you are operating in a multi-tenant context (where sharing indexes across tenant is possible).


- **Opportunity to Rethink UX, SEO, and Content Strategy -** you can align content structure with your new brand, audience segmentation, and multi-channel delivery goals — a rare but valuable strategic moment.


- **Reduces Future Migration Costs -** a cleaner system is easier to extend, maintain, and export in the future — reducing lock-in and long-term platform risk.


- **Clean up** - It is like moving homes, it is a great opportunity to get rid of all the unnecessary content, get a sleeker, more modern, and targeted website, instead of carrying all the junk that is not adding value.


### **❌ Cons**


- **Much Slower Time to Launch -** the recreating content even with automation takes time and coordination across editorial, development, and QA teams.


- **Much Higher Internal Effort for Content Review and Rewrite -** old content often needs reformatting, rewriting, or revalidation, fitting it into the newer structure, which takes manual effort and stakeholders collaboration.


- **Requires Change Management and Team Re-Training -** editors and stakeholders must adapt to a new UI, model structure, and editorial flow, which can create resistance or learning curves.


- **Potential Loss of Legacy Data or Metadata -** incomplete recreation processes can result in loss of SEO metadata, internal references, or historically important content, unless carefully planned.


- **Needs Early Buy-in from Stakeholders -** rebuilding content feels like more work up front, so you will need strong justification for the long-term benefits.


### **🧠 Have a think!**


- Do the analysis of your data structure and content ahead of time. It is a lot easier to look back and connect the dots - if your content has been growing organically now you can review it, identify the right pattern and rebuild from scratch OR if you content strategy is bringing you success, then just tweak it to optimize it for the new AI-driven world.


- Discuss internally what are the goals you want to achieve when switching to a new CMS system and based on these see which approach suits you better. Are you ready for full on restructuring of content and teams or you want to do a transition that helps you stay current and adapt to the ever changing world with minimum disturbance?


### **🛠 Next for Webiny: Migration Tooling**


We are now testing AI tools to make this easier:


- A


**toolkit** to automate your public website analysis and build a content model in Webiny so that you can decide which approach is best for you.


- Automation of migration scripting to help you move your content so that if you pick migration you do it quickly and reliably.


### **🎯 Choose Wisely**


The migration vs. recreation decision isn’t one-size-fits-all. But choosing with your eyes open — and using the right tooling and expertise can save months of effort and unlock the full potential of a modern CMS like Webiny.


If you’re planning a large migration or redesign, we’re happy to share more details from our clients’ lessons learned.
