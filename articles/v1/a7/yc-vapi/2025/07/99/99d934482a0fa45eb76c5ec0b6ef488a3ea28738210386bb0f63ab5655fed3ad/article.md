---
schema_version: "1.0.0"
document_id: "99d934482a0fa45eb76c5ec0b6ef488a3ea28738210386bb0f63ab5655fed3ad"
company_key: "yc-vapi"
company: "Vapi"
source_id: "yc-vapi-news-import-8dd0a49247bf"
canonical_url: "https://vapi.ai/blog/now-use-vapi-chat-widget-in-vapi"
published_at: "2025-07-02T00:00:00+00:00"
first_seen_at: "2026-07-24T05:51:02.715878+00:00"
fetched_at: "2026-07-28T21:27:44.796938+00:00"
content_hash: "sha256:185f2d0d272f1a914d32eecbd3c90f1423513b4a18dbf7a12923c09ed3ef0e47"
---

# Now Use Vapi Chat Widget In Vapi - Vapi AI Blog

Spinning up a chat UI shouldn’t be the hard part.


Today we’re launching the Vapi Chat Widget, a prebuilt, drop-in component that lets you embed your Vapi assistant directly on your site. Zero backend required. Fully session-aware. Configurable via public key.


### **Why We Built This**


Our chat API gives you full control but not everyone wants to own the UI.


Some teams are shipping fast and just want a working widget. Others are building platforms on top of Vapi and need something embeddable and white-labeled. Even developers building full apps often want a clean baseline they can fork and customize.


This widget exists for all of them.


### **What You Get**


- Drop-in embed for any site (React SDK available)
- Session-based context(24h) via Session API
- Optional voice toggle (chat or voice)
- Switch between chat + SMS with context continuity
- Public key config that works entirely client-side


Built on top of the same chat infra you already use. You don't need any special setup. If your assistant runs on chat, it runs here.


### **Why This Matters**


While you can roll your own UI, you don’t have to. This widget gives you:


- A fast path to production
- A reference implementation to fork or extend


### **Who It’s For**


- Developers who want to ship fast without sacrificing reliability, quality, or customizability
- Platforms that want to embed ai chat assistants across everything from static sites to dynamic React apps
- Teams already using Vapi voice agents who want to add chat support without duplicating infrastructure


### **What’s Next**


Cross-modality sessions (voice + chat memory sharing) are still in the works. Right now, chat and SMS share context; voice is separate. You can bridge them manually via API, but native continuity is coming soon.


### **Try It**


You’ll find the widget, source code, and docs[here](https://docs.vapi.ai/chat/web-widget) . Add it to your site in minutes. Then go as deep as you want.


We’re excited to see how you use it and what you build on top.
