---
schema_version: "1.0.0"
document_id: "a8002139900d988202f5de4083ba639eac90250cbd468fe4cf6d5c6ec9acfec0"
company_key: "yc-tiptap"
company: "Tiptap"
source_id: "yc-tiptap-news-import-30112aa6d3bf"
canonical_url: "https://tiptap.dev/blog/release-notes/recap-q1-2026"
published_at: "2026-03-10T00:00:00+00:00"
first_seen_at: "2026-07-22T16:45:40.643427+00:00"
fetched_at: "2026-07-28T22:00:58.612667+00:00"
content_hash: "sha256:5a2f16a910e1940fbe2e1579b157728d971ad76feed20b8b656198154afce112"
---

# Recap Q1 2026

As AI capabilities become a baseline expectation in content editing, we're introducing new features to help your Tiptap implementation stay ahead.


General-purpose AI tools are expanding into document workflows, but they don't understand your documents or your custom extensions. The result is generic output that doesn't fit and edits that break things. The features below help you bring AI into your application on your terms, with full awareness of how your documents actually work.


In Q1 we shipped tools for running AI workflows on your backend without the editor open, faster and more accurate in-editor AI editing, headers and footers for Tiptap Pages, and broader import/export support. We're also previewing Track Changes!


To explore how these align with your requirements and discuss integration approaches, please connect with us. We're happy to provide technical details and outline the best path forward for your implementation.


[→ Book a meeting here](https://calendar.google.com/calendar/appointments/schedules/AcZssZ0qImN6nFvlKn5RYrF7JCv5hDHckBTZanDBIf1GDZ_rqf_52smV0uEZ2h3-znpRwaeu-RxivS2U)


---


## AI Toolkit: AI that understands your documents


General-purpose AI tools can generate and edit text, but they don't understand your documents. The AI Toolkit gives your application AI capabilities that work within your document model, so edits are precise, valid, and fit the way your editor actually works.


The AI Toolkit offers two integration paths:


- **Client-side:** AI works alongside users in the editor. Changes stream in as suggestions that users can review and accept, while real-time collaboration continues uninterrupted.
- **Server-side:** AI reads and edits documents on your backend without the editor open in a browser. Run workflows from scheduled jobs, webhooks, or trigger them from the frontend and let them complete after users close the tab.


Both paths share the same core capabilities:


Capabilities Client-side Server-side


AI edit attribution ✅ ✅


AI collaboration in comments ✅ ✅


Custom component generation ✅ ✅


Stream AI responses into the editor ✅ ❌


Selection & caret awareness ✅ ❌


Human-in-the-loop review ✅ ❌


Client-side document editing ✅ ❌


Server-side document editing ❌ ✅


Token-optimized JSON compression ❌ ✅


### What's new in Q1


- **More accurate editing**
The AI now makes focused adjustments using fewer tokens, with significantly fewer diff mismatch errors. The same improvement applies to how comments are positioned in long documents.


- Optimized` tiptapEdit` ,` tiptapRead` , and` editThreads` tools for precision and token efficiency.


- ‍ **Non-blocking suggestions**
**‍** Previously, the AI had to wait for a user to accept or reject a suggestion before continuing. Now it can layer multiple rounds of changes while suggestions queue up for review. Suggestions are proposed edits – additions, deletions, replacements – shown inline in the document for a user to accept or reject individually.


- Suggestions now support stacking; the AI continues editing while prior suggestions await review.


- ‍ **Document templates**
**‍** Define which parts of a document are fixed and which the AI should fill. Generated content respects your document structure rather than starting from a blank page. Mark any node as a template slot, conditional section, or dynamic attribute.


- Template-aware generation with progressive streaming into the editor.
- Target the full document or a specific range.


- ‍ **Smarter diffs ‍** Compare two documents and get a clean list of changes. The new default diff mode identifies changed blocks first, then performs character-level diffing within them, so changes never cut across block boundaries.


- Options:` simplifyChanges` ,` changeMergeDistance` ,` groupInlineChanges` for fine-grained control. **‍**


- **Server-side AI**
**‍** The AI Toolkit now runs on your backend. Process documents at scale, run async workflows, and add your human into the loop to review the AI edits.


- Available as hosted REST API or on-premises deployment.
- Enterprise and Business customers can join the Pilot program.


- ‍ **Tiptap Shorthand ‍** A token-efficient format for representing your documents when working with the AI Toolkit. Shorthand reduces AI token costs by up to 80% compared to standard JSON without sacrificing accuracy.


- Currently available on all Server AI Toolkit endpoints.
- Use the same format value across all API calls in a conversation.


### Use cases


- **Batch processing:**
A publishing team uploads 2,000 product descriptions. Overnight, the AI standardizes tone, fixes formatting inconsistencies, and flags missing fields.
- **Async content generation:**
A user fills out a brief and hits "Generate." They close the tab and move on. When they return, a full first draft is waiting in their editor, structured to match the document template.
- **Document assistants:**
An AI agent works alongside users in the editor, suggesting improvements to content and structure as they write.
- **Compliance and review pipelines:**
Before a legal update goes live, the AI reviews every active contract for affected clauses and stages suggested rewrites. A compliance officer reviews and approves each change before it touches production content.
- **Structured document generation:**
A team generates contracts from templates. Legal boilerplate and branded headers stay fixed while the AI fills in deal-specific terms, pulling from a CRM integration. Individual sections can be regenerated without touching the rest.
- **Document comparison:**
Compare two document versions and surface changes as inline suggestions users can accept or reject individually.
- **Multi-document operations:**
A product name changes. A single workflow updates it across help docs, onboarding guides, and release notes – coordinated in one run, with every edit tracked.


### Get started today


[→ Technical documentation](https://tiptap.dev/docs/content-ai/capabilities/server-ai-toolkit/overview)


---


## **Conversion & Pages**


AI brings intelligent editing into your platform. But documents rarely live in a single application. They get created in one tool, reviewed in another, and exported for distribution somewhere else. For your editor to play a meaningful role in that workflow, it needs to handle the documents your users are already working with.


Tiptap Pages gives your editor a paginated layout so users can work with documents the way they expect: with page breaks, margins, headers, and footers. Convert Service handles the other half of the equation, importing documents from formats like DOCX and exporting them to PDF, Word, and more. Together, they let users bring documents into your application, work with them in context, and send them back out without losing fidelity.


### What's new in Q1


- ‍ **Headers and footers**
**‍** Users can now edit headers and footers directly in the editor view. Configure them to vary across your document: a different first page for title sheets, different odd/even pages for print layouts, or a single default that replicates throughout. These options combine to handle complex document structures, and the export feature fully supports them.


- Headers and footers support Tiptap extensions and custom nodes.
- Single editor instance reused across all headers and footers, so performance stays consistent even in long documents.


- ‍ **Convert Service**
**‍** Expanded format coverage and significantly improved formatting fidelity for getting content in and out of your editor.


- Export to PDF, ODT, DOC, DOCX, EPUB, and Markdown. Import from DOCX and Markdown.
- Improved paragraph spacing, indentation, line breaks, highlight colors, and table handling including column widths and cell borders.
- PDF export downloads fonts directly from Google Fonts, with custom font support for on-premises installations.


### **Use cases**


- **Print-ready documents:**
A legal team drafts contracts in the editor with branded headers on the first page and page numbers on every page after. They export to PDF and send it to the client, no post-processing in Word needed.
- **Content migration:**
A company moves 500 knowledge base articles from Word into their editor. Convert Service imports the DOCX files, preserving formatting, table structure, and layout so the content team can pick up where they left off.
- **Multi-format publishing:**
A documentation team writes once in the editor and exports to DOCX for clients who want Word files, PDF for print distribution, and Markdown for their developer portal.


### Get started today


[→ Technical documentation Pages](https://tiptap.dev/docs/pages/getting-started/overview)


[→ Technical documentation Conversion](https://tiptap.dev/docs/conversion/getting-started/overview)


---


## Redlining / Tracked changes


Whether edits come from an AI agent, a teammate, or an external reviewer, your users need a clear way to see what changed, who changed it, and whether to keep it. Track Changes brings redlining to your Tiptap editor, so proposed edits are visible inline until they are explicitly approved or rejected.


When a user edits in suggestion mode, changes are captured rather than applied immediately. Added text appears in the document but is visually distinguished as a proposal. Deleted text stays in the document as a proposed removal, keeping the original content accessible until the change is accepted. Every suggestion carries author attribution, so reviewers can filter by contributor or review changes from a specific collaborator.


### **Comments integration**


Track Changes works with the existing Comments extension. When a user creates a suggestion, a comment thread can be automatically linked to that change. Collaborators can discuss a specific edit in context, with the conversation anchored to the exact location of the proposed change.


[→ Technical documentation Tracked Changes](https://tiptap.dev/docs/editor/extensions/functionality/tracked-changes)


‍


---


## Coming next: Shape what we build 👀


- **AI Toolkit + Version History:**
Time travel for AI edits. Review, compare, and roll back AI changes alongside human edits.
- **Page-Aware AI:**
AI agents that understand page structure, enforce page count constraints, and restructure content based on page boundaries.
- **Conversion:**
Import and export more detailed headers & footers, tracked changes, and comments to and from DOCX.
- **Decorations API:**
Add inline, node, and widget decorations from your extensions with automatic position tracking and React/Vue support.


What do you need from your editor?
‍[→ Book a meeting here](https://calendar.google.com/calendar/appointments/schedules/AcZssZ0qImN6nFvlKn5RYrF7JCv5hDHckBTZanDBIf1GDZ_rqf_52smV0uEZ2h3-znpRwaeu-RxivS2U)
