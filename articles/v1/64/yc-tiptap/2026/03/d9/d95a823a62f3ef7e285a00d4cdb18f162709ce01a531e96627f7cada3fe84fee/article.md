---
schema_version: "1.0.0"
document_id: "d95a823a62f3ef7e285a00d4cdb18f162709ce01a531e96627f7cada3fe84fee"
company_key: "yc-tiptap"
company: "Tiptap"
source_id: "yc-tiptap-news-import-73c8cbb5613b"
canonical_url: "https://tiptap.dev/blog/insights/q1-26-hackathon-recap"
published_at: "2026-03-19T00:00:00+00:00"
first_seen_at: "2026-07-22T16:45:41.534648+00:00"
fetched_at: "2026-07-28T21:26:23.229623+00:00"
content_hash: "sha256:8d6859491df38b77b6f4471ead179370c9a3278a7122d919d4c6819a9d45d073"
---

# Q1 ’26 Hackathon recap

## Berlin HQ, one focused sprint


Remote-first doesn’t mean remote only. Once a quarter, we bring the team together in one place to build faster, align on what’s next, and spend time together outside of Slack.


This time, the work clustered around AI-driven document actions, reviewable editing with Tracked Changes, and higher-fidelity document conversion.


---


## Roadmap and highlights from the week


During the Hackathon, we refined our product roadmap based on customer conversations and what we’re seeing in real-world implementations. We also did some behind-the-scenes infrastructure hardening to improve reliability for production workloads.


The outcome is simple: clearer priorities, better sequencing, and fewer surprises for teams building with Tiptap.


- Consolidated customer needs and feedback into concrete roadmap themes
- Aligned across product and engineering on sequencing and scope
- Defined a clearer package of near-term priorities to improve the overall experience


[→ View roadmap](https://tiptap.dev/roadmap)


### AI Toolkit: deeper integration and better building blocks


We focused on making AI-driven editing feel more reliable in real review workflows.


- Integrated the AI Toolkit with Tracked Changes
- Integrated server-side AI Toolkit with Tracked Changes
- Continued customer conversations to validate needs and priorities
- Published a Tiptap skill for AI coding agents (Claude, Cursor, and similar tools) to improve developer experience and help agents use the right Tiptap APIs and extensions
- Implemented AI server-side streaming (more responsive agent UX, faster feedback loops)


[→ Read the docs](https://tiptap.dev/docs/content-ai/capabilities/ai-toolkit/overview)


### Tracked Changes: more precise reviewable edits


We moved Tracked Changes forward with improvements focused on more precise tracking.


- Improvements including mark tracking and mark-attribute tracking
- Internal alignment on next steps (collaboration compatibility, branching, and getting other alpha features onto main)


[→ Read the docs](https://tiptap.dev/docs/tracked-changes/getting-started/overview)


### Document conversion: accuracy and compatibility improvements


We focused on making DOCX conversion more predictable in real-world documents. Some of these improvements land at the conversion layer first, and become visible in the editor as the relevant extensions and rendering capabilities catch up.


- Better tables and overall layout fidelity
- More reliable headers and footers (including multi-section documents)
- Cleaner handling of notes, page breaks, and images
- More flexible conversion APIs and upload workflows (including pre-built DOCX uploads)


[→ Read the docs](https://tiptap.dev/docs/conversion/getting-started/overview)


### UI templates: import, layout, and review


We started building use-case specific UI templates to turn our capabilities into polished, end-to-end flows.


- Conversion and Page Layout: Import Word files, choose page formats (size and margins), edit headers and footers with a richer UI, and preview how the document will look in Word.
- Tracked Changes plus AI Toolkit: Chat with AI while including specific document sections as context, receive suggestions as reviewable proposals, and iterate on them in a dedicated suggestion chat.


---


## Experiments


A quick look at two prototypes we built to explore what Tiptap documents can enable in new contexts.


### Voice: hands-free document editing


We built a voice agent demo on top of the Tiptap AI Toolkit, designed to turn speech into precise, reviewable document edits.


- Voice-driven agent demo for document actions and edits
- Two modes: transcription with precise voice commands, and a voice assistant that proposes edits (including Tracked Changes).


### Beyond text documents: real-time collaboration in a music app


We built a small experimental iOS music app prototype to explore what Tiptap’s document infrastructure for collaboration looks like outside the classic text editor. Two people can work on the same project at the same time, with changes syncing instantly and merging cleanly, from arrangement and track settings to audio snippet references and metadata.


---


## Team moments between the commits


In Berlin, we made space for the parts of teamwork that don’t show up in a diff: shared lunches, long conversations, and a few low-key team activities after work.


Highlights included an afterwork at the office with snacks and a cozy movie night, a VR World session where two teams went head-to-head, a mandatory stop at Rueyam for vegetable doener, a karaoke night, and a final Friday wrap-up with drinks and a BBQ.


---


## Until next time


We’ll keep pushing these threads forward, turning this week’s prototypes and milestones into finished, production-ready capabilities as we follow the roadmap.


Next up: our annual team trip in Portugal, where we’ll bring everyone together again, including teammates joining from farther away.


‍
