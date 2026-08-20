---
schema_version: "1.0.0"
document_id: "c11baf150fd248b56f2c8f3d2736c96d378eacede2bd84f94120b7d6546dba8f"
company_key: "yc-staffbar"
company: "Superwall"
source_id: "yc-staffbar-rss-5f8991137f5c"
canonical_url: "https://superwall.com/blog/whats-new-april-2026"
published_at: null
first_seen_at: "2026-07-20T23:20:38.930038+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:52e9ab98a34d7f21d3e18a6cc677cd9d849c8eb849041bd7eef0c76b2b92e2dc"
---

# What's New in April 2026: Flows Analytics, Custom Store Products, Web Checkouts, and AI in the dashboard. Plus a new iOS app.

## Flows Analytics: see exactly where users drop off and why


[Flows](https://superwall.com/blog/whats-new-march-2026) let you build multi-screen experiences right inside the Superwall editor — onboarding sequences, cancellation surveys, upsell paths, and feature walkthroughs — all with remote configuration and A/B testing.


Now you get a full picture of how users move through your flow. Inside your experiment results, the new **Flow Journey** view brings:


- **Drop-off by step** — user counts and median time spent on each step.
- **Branching view** — Sankey-style diagrams that show how users split across conditional paths.
- **Variant comparison** — test entirely different flow structures against each other.


The drop-off chart example


## Custom Store Products: sell from any payment system on any paywall


Superwall now supports custom store products, so you can attach products from Stripe, your own backend, or any payment system to a Superwall paywall. You get the same functionality you already rely on with native store products — price variables, trial copy, eligibility checks, and A/B testing.


That means you can place Stripe subscriptions alongside App Store subscriptions on the same paywall, sell lifetime access without StoreKit, and keep consistent experiment infrastructure across payment providers.


## Web Checkout: App2Web and one-time purchases


**App2Web.** iOS paywalls can now open Stripe Checkout in Safari for U.S. storefront customers outside the app, following Apple's external purchase guidelines. Entitlements sync automatically as soon as the user returns.


**One-time purchases.** Web checkout now supports Stripe one-time prices for lifetime access, credit packs, and consumables — no additional configuration required.


## AI Agent: ask anything about Superwall, right in the dashboard


There's now an AI agent built into the dashboard, accessible from the bottom right corner. The agent has access to Superwall's documentation and SDK codebase, so you can ask how a feature works, how to set something up, or why a paywall is behaving a certain way — and get linked answers you can follow up on.


AI Agent in your dashboard


## Superwall iOS App: every chart in your pocket, completely rebuilt


Version 2 is a complete rewrite, putting over 20 dashboard charts on your phone. Adjust dates, apply filters and breakdowns, and scrub across data points with your finger. Export anything to JSON or Markdown.


**Transactions redesign.** The overview and transaction history are now merged into one view, with detailed breakdowns including SDK events and purchase information.


**New home widgets.** MRR, ARR, and a Recent Transaction widget that shows the latest purchases for your selected app.


Revamped Superwall iOS App


## Apple Retention Messaging: control what users see when they cancel


Superwall now integrates with Apple's Retention Messaging API, letting you configure the messages subscribers see when they start a cancellation through the App Store. You can show text messages, image messages, alternate product offers, and promotional offers.


Set messages as defaults per product and locale, or configure them in real time based on the cancellation context. Apple invokes Superwall's callback URL, and Superwall returns the configured message.


Retention messaging example


## Also shipped in April


- **Priority Placements** — a single-toggle campaign prioritization that enables preload optimization.
- **Multipage paywall tracking** —` paywall_page_view` events on every page within multi-page paywalls, for step-by-step drop-off analysis.
- **SDK updates** — iOS 4.14.2–4.15.1, Android 2.7.10–2.7.13, Flutter 2.4.12, Expo 1.0.9–1.1.1, with performance improvements and new features.
- **Documentation updates** — code block tabs, Vibe Coding guides, and intro offer eligibility clarifications.
