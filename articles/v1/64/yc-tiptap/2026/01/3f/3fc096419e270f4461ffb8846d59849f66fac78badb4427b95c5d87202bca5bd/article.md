---
schema_version: "1.0.0"
document_id: "3fc096419e270f4461ffb8846d59849f66fac78badb4427b95c5d87202bca5bd"
company_key: "yc-tiptap"
company: "Tiptap"
source_id: "yc-tiptap-news-import-30112aa6d3bf"
canonical_url: "https://tiptap.dev/blog/release-notes/our-roadmap-for-2026"
published_at: "2026-01-13T00:00:00+00:00"
first_seen_at: "2026-07-22T16:45:40.643427+00:00"
fetched_at: "2026-07-28T21:27:02.153272+00:00"
content_hash: "sha256:6a752405e4a328adf1ffa2156545aa14c78589a1173e9e680af3f129facefbd5"
---

# Our roadmap for 2026

What that means for product teams building with Tiptap: your product can treat documents like first-class objects in your product’s data model, queryable, versioned, permissioned, and validated server-side. That unlocks structured content, reliable conversion, and real-time collaboration (including with AI), without mountains of glue code.


## Example: legal contracts


Imagine a legal contract platform (DocuSign-style) where every deal has a contract built from reusable, approved clauses.


When you treat the contract as a structured document (not a rich-text blob), the roles split cleanly:


- **Your backend** validates clauses and signatures, enforces permissions, and stores a queryable history.
- **Your editor UI** is one client that renders the model and lets people edit it.
- **AI agents** propose changes (with tracked changes), but they still go through the same rules and validation.
- **Conversion** to DOCX/PDF stays consistent because it’s built from the same structured blocks.


Less glue code for the product team, more reliability for users.


## Why we believe this is the right direction


Over the last two years, we learned that modern “documents” aren’t just UI. They’re living objects that need a defined schema (structure you can validate and query), reliable conversion, and collaboration, including with AI.


Building the **AI Toolkit** taught us how to safely let agents read and write documents in real time (in the browser) and asynchronously (in the backend). Investing in **document conversion** forced us to make that same document model portable and production-grade across **DOCX/PDF and page layout details like headers and footers** .


Why this matters: All innovation is worthless if applications can't embed themselves in the document process of their users! For a lot of real teams and industries, Microsoft Word is still their de facto standard. They send PDFs to customers, and need the output to match what legal and ops expect. We learned that the hard way by building with customers in the field. And shipping Flex **validated the platform bet** : with the infrastructure in place, a small team can build a complex, AI-native writing UI quickly.


Those lessons will shape what we build in 2026: a platform where building products that juggle documents in real time across users, agents, and platforms is as easy as \[INSERT SOMETHING REALLY EASY HERE\].


## How we’ll get there: three bets aligned under the mission


To execute on the mission, we’re investing in three aligned bets. Each is a pillar of the same strategy: together, they make documents programmable, portable, and collaborative.


### 1. Bet: AI in Documents


**Main question:** How do we collaborate with AI on documents and give agents safe, reliable access to content?


The two core features evolved in this bet are the **AI Toolkit** and the **Server AI Toolkit** , giving agents real-time access to a Tiptap document to create and edit it in the browser, or asynchronously in the backend with no browser needed.


The AI Toolkit is already working in customers' production apps at scale. The alpha version of the Server AI Toolkit is already available upon request and will be released as a stable version publicly later this year.


### 2. Bet: Document Conversion


**Main question:** How do we convert Tiptap documents into other file formats, and the other way around to make Tiptap more interoperable?


In 2025, we proved the basics: DOCX import and export, PDF export, and page layout fundamentals (including headers and footers). The 2026 bet is not “we keep shipping conversion.” It’s making conversion **workflow-complete** , so teams can treat it like a platform capability, not an isolated feature.


What’s missing today is the stuff real documents break on: higher fidelity (tabs, numbering, footnotes), stable **round-tripping** (export to Word, get redlines back, keep structure intact), and **metadata** like comments and tracked changes. Conversion matters because it’s the gate for regulated and enterprise workflows, and even for everyone else it becomes urgent the first time you need to send a doc outside your app.


### 3. Bet: Tiptap Flex


In 2025, Flex proved something simple: if the editor, Document Cloud, and AI Toolkit exist as real platform primitives, a small team can ship an AI-native writing UI fast. Two people built the first prototype in under 10 weeks.


The 2026 bet is to productize that prototype into a repeatable capability in the platform. In plain terms: make “AI in the editor” something teams can embed and ship, not just a demo.


Flex also ties directly to the mission’s “collaborative” pillar. When documents are programmable and real-time, users and agents can work in the same doc without duct tape.


And there’s a flywheel here: Flex dogfoods the platform. It surfaces the papercuts first, which forces improvements in collaboration, tracked changes, conversion, and the AI Toolkit. Those improvements flow back into Flex, which creates more real usage, which pushes the platform again.


## What we’ll ship:
The 2026 Roadmap!


Tiptap handles the hardest parts of document editing so your team can focus on building your product.[See where we’re heading with the roadmap.](https://tiptap.dev/roadmap)
