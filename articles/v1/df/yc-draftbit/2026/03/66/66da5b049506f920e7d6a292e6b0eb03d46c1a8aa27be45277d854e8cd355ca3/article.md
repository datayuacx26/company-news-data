---
schema_version: "1.0.0"
document_id: "66da5b049506f920e7d6a292e6b0eb03d46c1a8aa27be45277d854e8cd355ca3"
company_key: "yc-draftbit"
company: "Draftbit"
source_id: "yc-draftbit-news-import-aa5bd69b40d1"
canonical_url: "https://draftbit.com/whats-new/whats-new-march-19th-live-preview/"
published_at: "2026-03-19T14:21:10.075+00:00"
first_seen_at: "2026-07-21T16:58:44.160400+00:00"
fetched_at: "2026-07-28T21:56:54.694470+00:00"
content_hash: "sha256:3f19ff1fbdbbe49a234ac095990eae9de7064e1170b1c9125feb315e6874d117"
---

# Added Live Preview, Dashboard Revamp, Custom MCPs & More

We've been shipping fast this month — here's everything that's new in Draftbit. 🚀


## 📱 Live Preview on Your Dashboard


You can now preview your apps natively on your phone using the Draftbit Preview apps, available in both app stores. Just scan the QR code, and you'll be taken right into your app preview if you have the app, or to the App Store to download the app if you don't.


## 🏠 Revamped Dashboard


The dashboard got a major facelift. We've redesigned the layout with a cleaner look, a more prominent prompt area for starting new projects, and better overall app organization, as well as making it easier to start new projects. We now show all of your apps (v1 and v2) directly on your dashboard so you can access everything in one place, and we renamed "Import" to "Migrate" to better describe what's actually happening when you bring apps over from V1.


## 🎨 Styles Panel Improvements


The styles panel has reached full UI parity with V1 — all the controls you're used to are now available in the new builder. We've also added a **current vs. working toggle** , so you can compare your in-progress style changes against the committed version before saving. Section headers have been redesigned to make it easier to scan and find what you need at a glance.


## 🔌 Custom MCP Servers


This one's big for power users. If you're on a Pro plan, you can now connect your own **custom MCP (Model Context Protocol) servers** to Draftbit. That means your AI agent can access your own tools, APIs, and data sources — directly inside the builder. Think of it as giving your agent superpowers tailored to your specific workflow.


## 🧠 AI Model Upgrades & Additions


**GPT-5.4** is now the default model across the platform, paired with Codex as the default agent. But we didn't stop there:


- **GPT-5.4 Mini and Nano** have been added for lighter, faster tasks that don't need the full horsepower
- **OpenRouter model support** — use a wider range of models through OpenRouter with both Codex & Claude Code.
- **Claude Sonnet 4.6** is now available via updated Anthropic SDKs
- A new **model selector** in the chat view lets you switch models on the fly, mid-conversation
- A **reasoning effort dropdown** gives you control over how deeply the model thinks through problems — great for balancing speed vs. thoroughness
- We've also made our agents a ton more efficient, using significantly fewer credits for same or better outcomes.


## ⚡ Code Editing Speed Improvements


File edit latency has been reduced from **~3-10 seconds down to under ~500 milliseconds** . That's up to a 20x+ improvement. Edits from the agent and the code editor now feel nearly instant.


## 🎯 Improved Preview Selection Tool


The element picker in the live preview has been overhauled for better reliability and visual quality. Selecting elements is now more precise, with cleaner hover highlights and more consistent behavior across different component types. It's a small thing that makes a big difference when you're clicking around your app to style or inspect elements.


## 💳 Billing & Credits Visibility in the Builder


You no longer have to leave the builder to check your plan status. A new **billing plan indicator** lives right in the top bar, so you always know where you stand. If you're on a free plan, you'll see clear upgrade prompts when you try to access paid features like the model selector or agent chooser — no more guessing about what's available on your tier.


## 🏎️ Faster Sandboxes


Behind the scenes, we've made major infrastructure improvements to how sandboxes start, run, and recover. We fixed restart loops that could leave a sandbox stuck, reduced transaction timeouts, improved container lifecycle management, and optimized our prewarmed app pools so there's usually a sandbox ready to go before you even need one. The result: sandboxes start faster, stay stable longer, and use fewer resources overall — which also contributes to better credit efficiency.


## 📋 Changes to Free Plans


To keep the platform sustainable as we scale, we've introduced a **lifetime spend cap** on free daily credits. This means free accounts have a total credit ceiling over the life of the account. This does not affect **Paid plans** . w


## 🛠️ And a Whole Lot More


We've made a ton of improvements, fixed bugs, and made things faster overall -- and we have some even bigger news to share with you soon about massive upgrades in capabilities to Draftbit, so stay tuned.


Thanks for building with Draftbit — we're just getting started. 💜
