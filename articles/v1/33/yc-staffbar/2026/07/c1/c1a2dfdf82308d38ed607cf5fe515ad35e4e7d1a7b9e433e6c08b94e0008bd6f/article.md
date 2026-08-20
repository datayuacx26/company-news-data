---
schema_version: "1.0.0"
document_id: "c1a2dfdf82308d38ed607cf5fe515ad35e4e7d1a7b9e433e6c08b94e0008bd6f"
company_key: "yc-staffbar"
company: "Superwall"
source_id: "yc-staffbar-rss-5f8991137f5c"
canonical_url: "https://superwall.com/blog/whats-new-june-2026"
published_at: null
first_seen_at: "2026-07-20T23:20:38.930038+00:00"
fetched_at: "2026-07-28T21:08:32.413121+00:00"
content_hash: "sha256:3adc47db936993ec86169ac35ed0219c6446eb499942c7f43007b684a133c833"
---

# What's New in June 2026: AI Chat Builder, the Editor MCP, and full-funnel attribution

## AI Chat Builder: describe a paywall and watch it build itself


Describe the paywall or[Flow](https://superwall.com/docs/dashboard/dashboard-creating-flows/getting-started) you want in plain language - "make the trial toggle bigger, add a testimonials row, tighten the headline" - and[AI Chat Builder](https://superwall.com/docs/dashboard/dashboard-creating-paywalls/paywall-editor-ai-chat) builds it live in the editor while you watch. It works on a new paywall or one you have already shipped, and it covers Flows too, so the same loop runs your onboarding and cancellation surveys.


That is the shorter iteration loop fast-moving teams keep asking for: change the copy, ship the variant, read the result, without waiting on an app release.


[AI Chat Builder docs →](https://superwall.com/docs/dashboard/dashboard-creating-paywalls/paywall-editor-ai-chat) |[Blog post →](https://superwall.com/blog/vibe-code-paywalls-from-a-template-figma-or-terminal/)


## Editor MCP: give your coding agent the keys to a live editor session


Connect Claude Code, Cursor, or Codex to a live editor session with a pairing code, and your agent gets direct control of the paywall you have open - the real editor, updating as it works, not a static export. It is the only tool in the category that does this. Point it at a Figma frame and a brief, step away, and come back to a paywall ready to test.


Yes, it works on existing paywalls. And the[Superwall Skill](https://superwall.com/docs/dashboard/guides/superwall-skill) is the Editor MCP plus live docs and API helpers, so start with the Skill if you want your agent to understand Superwall, not just operate it.


[Editor MCP docs →](https://superwall.com/docs/dashboard/dashboard-creating-paywalls/paywall-editor-mcp) |[Superwall Skill docs →](https://superwall.com/docs/dashboard/guides/superwall-skill)


## wwdc.ai: every WWDC 2026 session, summarized and searchable


[wwdc.ai](https://wwdc.ai/) is a free site with AI summaries of all 114 WWDC 2026 sessions. No login, no paywall.


It is built for agents as much as for people: add` .md` to any session URL for the raw Markdown, point your tools at[llms.txt](https://wwdc.ai/llms.txt) , or install the WWDC skill so your coding agent pulls session context on demand.


[Visit wwdc.ai →](https://wwdc.ai/) |[Read the story →](https://superwall.com/blog/wwdc-ai-2026/)


## Singular and install matching: attribution that survives the install


Knowing which channel sent the user who converted is where most subscription data breaks. June closed part of that gap.


Pass a Singular Device ID to Superwall and installs and events line up cleanly in Singular. Separately, the SDK now matches installs to your performance-marketing integrations on its own and records it as an event you can chart and filter audiences on. With the[Adjust integration](https://superwall.com/docs/integrations/adjust) from May, the path from ad tap to renewal is finally continuous.


Singular Device ID needs iOS SDK 4.15.4+ or Android 2.7.18+. Install matching is on iOS 4.16.0+.


[Attribution integrations docs →](https://superwall.com/docs/integrations/singular) |[How full-funnel attribution works →](https://superwall.com/blog/superwall-and-adjust-integration-for-full-funnel-subscription-attribution/)


## Annual plans, billed monthly: a new lever for lower-friction pricing


You can now merchandise an annual App Store subscription as Apple's monthly-installment plan: the user commits for the year but pays month to month. That lower monthly number is often the difference between a scroll-past and a start.


It is a[per-product setting](https://superwall.com/docs/dashboard/products) , so you can test up-front billing against installments like any other variable and read the difference in your own experiment results.


Requires iOS SDK 4.16.0+.


[Products and billing plans docs →](https://superwall.com/docs/dashboard/products)


## Transaction abandon events: catch your highest-intent users at the exit


The users who opened the App Store purchase sheet and backed out are some of your highest-intent. Now you can reach them.


On Android, the[transaction abandon event](https://superwall.com/docs/dashboard/dashboard-campaigns/campaigns-standard-placements#transaction_abandon) is now exposed to paywalls, so you can show a different offer, some reassurance, or a discount the moment someone abandons a purchase, instead of losing them silently - the same trigger iOS has had for a while, now on both platforms.


Requires Android SDK 2.7.16+.


[Abandoned-transaction paywalls docs →](https://superwall.com/docs/dashboard/guides/tips-abandoned-transaction-paywall)


## EventTrackingBehavior: privacy control you can set at runtime


If you operate under GDPR, this one matters. A new[EventTrackingBehavior](https://superwall.com/docs/ios/sdk-reference/SuperwallOptions) setting controls exactly what event data the SDK sends: everything, Superwall only, or nothing. You can change it at runtime as consent changes, and it replaces the older isExternalDataCollectionEnabled option.


Requires iOS SDK 4.16.0+.


[SuperwallOptions docs →](https://superwall.com/docs/ios/sdk-reference/SuperwallOptions)


## Also shipped in June


- **Stability.** Fixed a crash from concurrent preloadAllPaywalls calls and corrected intro-offer eligibility during subscription changes. iOS 4.16.0.
- **Web redemption.** Paywalls now dismiss correctly after a web redemption. Android 2.7.17.
- **Config resilience.** Steadier behavior before config is retrieved. Android 2.7.16.
- **Expo fixes.** Paywalls with a custom purchase controller no longer get stuck on the loading spinner on cold start, and the compat layer no longer drops PaywallOptions like shouldPreload on iOS. Expo 1.1.5.


That's June. If something here unlocks a use case you have been sitting on, or raises a question, tell us. We say test everything, and we would rather hear the hard question now than after you ship.


## Ready to try it?


[Create your Superwall account →](https://superwall.com/sign-up?utm_source=blog&utm_medium=content&utm_campaign=whats-new-june-2026&utm_content=create-account) and build your first paywall today. It is free to get started, and everything above works from day one.


Want to see it live first?[Book a demo →](https://superwall.com/demo?utm_source=blog&utm_medium=content&utm_campaign=whats-new-june-2026&utm_content=book-demo) and we will walk you through the new AI, attribution, and billing tools, mapped to your app. A good fit if you are on Enterprise, scaling growth, or weighing a migration.


## FAQ


What is Superwall's AI Chat Builder? AI Chat Builder lets you describe a paywall or Flow in plain language and builds it live in the editor while you watch. It works on a new paywall or one you have already shipped, and it covers Flows too, so the same loop runs your onboarding and cancellation surveys.


What is the Superwall Editor MCP? The Editor MCP connects a coding agent like Claude Code, Cursor, or Codex to a live editor session with a pairing code, giving the agent direct control of the paywall you have open rather than a static export. It works on existing paywalls, and the Superwall Skill adds live docs and API helpers on top of it.


Can I offer an annual plan billed monthly with Superwall? Yes. You can merchandise an annual App Store subscription as Apple's monthly-installment plan, so the user commits for the year but pays month to month. It is a per-product setting, so you can test up-front billing against installments, and it requires iOS SDK 4.16.0 or later.


How does Superwall improve subscription attribution? You can pass a Singular Device ID to Superwall so installs and events line up cleanly in Singular. Separately, the SDK now matches installs to your performance-marketing integrations on its own and records it as an event you can chart and build audiences on, which, with the Adjust integration, makes the path from ad tap to renewal continuous.


Can I show a paywall when a user abandons a purchase on Android? Yes. The transaction abandon event is now exposed to paywalls on Android, the same trigger iOS has had for a while. You can show a different offer, some reassurance, or a discount the moment someone abandons a purchase instead of losing them silently. It requires Android SDK 2.7.16 or later.


How do I control what event data the Superwall SDK sends for GDPR? The new EventTrackingBehavior setting controls exactly what event data the SDK sends: everything, Superwall only, or nothing. You can change it at runtime as consent changes, and it replaces the older isExternalDataCollectionEnabled option. It requires iOS SDK 4.16.0 or later.
