---
schema_version: "1.0.0"
document_id: "4252d5d022c284aacc0b339d88bcc45e4c2b803831b10296bc66669908d910f6"
company_key: "yc-docsum"
company: "Docsum"
source_id: "yc-docsum-news-import-2e6a6886c0eb"
canonical_url: "https://www.docsum.ai/blog/product-update-ai-legal-intake-playbook-generation"
published_at: "2026-05-12T00:00:00+00:00"
first_seen_at: "2026-07-25T01:48:08.975430+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:ead50a530d4296e5f32c3def752d8932c216c12b6fdcce7f82a4a7b3578df16e"
---

# 🚀 Product Update: AI Legal Intake & Playbook Generation

📢 **From capturing requests to actually completing them.**


Last cycle we gave you a place to capture legal requests. Now Docsum can start doing the work. This release introduces AI Legal Intake, an agent that delegates and auto-executes incoming requests, alongside AI-powered playbook generation with alignment scoring, and automatic document grouping that keeps your repository organized on its own.


### 🤖 AI Legal Intake: An Agent That Acts on Requests


**Requests don't just get tracked. They get done.**


We shipped the core architecture for AI-powered legal intake. A new orchestrator agent serves as the entry point for Slack-based requests, delegates work to the Docsum AI agent for auto-execution, and produces task artifacts: real output documents you can review and follow up on.


-


**An orchestrator agent at the front door.** Incoming Slack requests are interpreted, routed, and delegated to the Docsum AI agent to execute, not just logged.


-


**Task artifacts you can act on.** Completed tasks produce output documents, so intake delivers results, not just status updates.


-


**Built on a rebuilt Slack integration.** We rebuilt Slack from the ground up for speed and reliability, and the bot now follows entire threads, not just direct mentions, for natural, conversation-style interactions.


### 📋 Playbook Generation & Alignment: Build Standards From Your Own Documents


**Generate a playbook from your contracts, and measure every deal against it.**


Creating a negotiation playbook from scratch is slow. Now you can generate one. Upload source documents as the basis for playbook synthesis, and Docsum produces a structured playbook for you. Once it exists, alignment percentages show how closely any contract matches your standard positions. They're displayed across both the web app and the Word Add-in, so you can see deviation at a glance wherever you work.


### 🗂️ Automatic Document Grouping: A Repository That Organizes Itself


**Related documents group themselves on upload.**


Documents uploaded to your repository can now be automatically added to document groups, with new groups created when no match is found. This strengthens the clustering of related contracts and counterparties, making it far easier for both you and Docsum's agents to surface insights across a deal.


**Also in this release:**


-


**Unified notifications inbox.** A single place to track subscriptions and activity across all your documents and workflows.


-


**Repository tool in the Word Add-in.** Query and reference your repository from inside Microsoft Word.


-


**Unarchive documents.** Restore archived documents back to your active library.


-


**Export chat messages as DOCX.** Save any individual chat message as a Word document.


-


**The latest AI models.** Added support for GPT-5.4/5.5 and Claude Opus 4.7.


-


**Smarter agentic search.** Accent-insensitive matching (searching "e" now matches "é") and cross-field OR logic, so queries like "documents in New York OR with Net 30 terms" just work.


### 🚀 Why This Matters


This is the moment intake stops being a to-do list and starts being a teammate. Docsum can now take a request, do the work, and hand back a result, while generating the playbooks it measures against from your own documents. Combined with a rebuilt Slack experience and self-organizing document groups, your repository is becoming less of a filing cabinet and more of a working colleague.
