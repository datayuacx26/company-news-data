---
schema_version: "1.0.0"
document_id: "f302ab852b317684f6cc743484136e2fd8fb66b8345ec12509ca7e3a3182200b"
company_key: "yc-staffbar"
company: "Superwall"
source_id: "yc-staffbar-rss-5f8991137f5c"
canonical_url: "https://superwall.com/blog/whats-new-july-2026"
published_at: null
first_seen_at: "2026-08-13T17:19:08.283821+00:00"
fetched_at: "2026-08-13T17:19:11.444958+00:00"
content_hash: "sha256:fa04df299dd0bc4b25caf155799efc1c323deca5e2a956fc49e62a0d27c8c06d"
---

# What's New in July 2026: Tangent joins Superwall, Superwall AI, a new CLI, and Managed Payments

## Tangent joins Superwall, and we raised $7.5M


The biggest news of the month: Superwall acquired[Tangent](https://superwall.com/blog/superwall-acquires-tangent-and-raises-7.5m/) , the monetization consultancy founded by Vahe Baghdasaryan, and raised $7.5M. Superwall can now design, test, and manage monetization testing end to end for any subscription app, with the software and the experts under one roof.


For Tangent's clients, it is business as usual: Tangent still works with any subscription app, whether or not you use Superwall. And if you want hands-on help with your paywall and pricing strategy, there is now a[services page](https://superwall.com/services) for that.


[Read the announcement →](https://superwall.com/blog/superwall-acquires-tangent-and-raises-7.5m/) |[Explore services →](https://superwall.com/services)


## Superwall AI: ask your dashboard a question


[Superwall AI](https://superwall.com/docs/dashboard/superwall-ai) is a built-in dashboard assistant that answers questions about how Superwall works, what is in your account, and what your data says, in plain English, with links to dig deeper.


It is the fastest way to close the gap between "I wonder why trial conversion dipped" and an actual answer: ask, read, follow the link, act. No SQL, no tab-hopping through docs.


[Superwall AI docs →](https://superwall.com/docs/dashboard/superwall-ai)


## Superwall CLI: the dashboard, from your terminal


The new[Superwall CLI](https://superwall.com/docs/dashboard/guides/superwall-cli) lets you manage products, campaigns, and paywalls, query your analytics data, and call the Superwall API right from your terminal. It is built for coding agents as much as for people: give Claude Code or Cursor the CLI and it can inspect and operate your Superwall setup as part of a task.


The[Superwall Skill](https://superwall.com/docs/dashboard/guides/superwall-skill) was updated alongside it, with a new` npx superwall skills` installer, so your agent stays current with the skills that ship today.


[Superwall CLI docs →](https://superwall.com/docs/dashboard/guides/superwall-cli) |[GitHub repo →](https://github.com/superwall/cli)


## Managed Payments: Stripe as your merchant of record


Selling on the web means someone has to handle sales tax and compliance. With[Managed Payments](https://superwall.com/docs/web-checkout/web-checkout-managed-payments) , Stripe now acts as merchant of record for eligible Stripe web checkout payments, handling tax calculation and compliance for you through Stripe Connect.


Later in the month we shipped clearer setup guidance: you can see when a Stripe environment is Configured and Enabled, how Superwall checks product eligibility before turning Managed Payments on, and how to override it per paywall.


[Managed Payments docs →](https://superwall.com/docs/web-checkout/web-checkout-managed-payments)


## OneSignal integration: subscription state in your messaging stack


The new[OneSignal integration](https://superwall.com/docs/integrations/onesignal) syncs Superwall subscription state to OneSignal as user tags, so your lifecycle and messaging team can segment and branch Journeys on trials, renewals, and cancellations without engineering work.


On the SDK side, iOS 4.16.1 clarified that the` onesignalId` integration attribute should be set to the OneSignal User ID.


[OneSignal integration docs →](https://superwall.com/docs/integrations/onesignal)


## Event tracking controls, now on Android and Expo


June brought GDPR-friendly event tracking control to iOS. July extended it across the stack: a new` eventTrackingBehavior` option on[Android](https://superwall.com/docs/android/guides/configuring) and[Expo](https://superwall.com/docs/expo/guides/configuring) lets you decide exactly what the SDK sends to Superwall: everything, Superwall-only, or nothing. You can switch it at runtime as users update their privacy consent, and it supersedes the older isExternalDataCollectionEnabled option, which is now deprecated.


Requires Android SDK 2.7.19+ or Expo SDK 1.2.0+.


[Android configuration docs →](https://superwall.com/docs/android/guides/configuring) |[Expo configuration docs →](https://superwall.com/docs/expo/guides/configuring)


## Font Size and Font Scale variables: respect the user's text settings


Users who crank up their system text size are telling you something. Paywalls can now adapt to it: new` Font Size` and` Font Scale` variables are available in the[paywall editor](https://superwall.com/docs/dashboard/dashboard-creating-paywalls/paywall-editor-variables) and in audience filters, so you can adjust layout or target an audience by their accessibility settings. On Android, SDK 2.7.21 adds the underlying device attributes.


[Paywall editor variables docs →](https://superwall.com/docs/dashboard/dashboard-creating-paywalls/paywall-editor-variables)


## Also shipped in July


- **Expo SDK 1.2.0.** Updates the underlying iOS (4.16.1) and Android (2.7.20) SDKs, adds getStoreFrontCountryCode() to read the device's store country, and supports passing a[Singular device ID](https://superwall.com/docs/integrations/singular) as a typed integration attribute.
- **Xcode 26.4.** The iOS SDK now builds cleanly under Xcode 26.4. iOS 4.16.1.
- **Loading spinner theming.** Theme the built-in purchase and restore spinner to match your brand with the new PaywallOptions.loadingColor, plus a new storeFrontCountryCode property and a web-redemption restoration fix for purchase-controller apps. Android 2.7.20.
- **Paywall resilience.** Paywalls now survive the app being backgrounded and reopened, concurrent paywalls no longer overwrite each other's experiment ID, and audience-filter evaluation is faster. Android 2.7.21.
- **International keyboards.** Fixed paywall text inputs silently dropping non-Latin keyboards (Cyrillic, Korean, Thai, emoji), and billing errors no longer propagate to the app when a user lacks billing. Android 2.7.23.


That's July. If something here unlocks a use case you have been sitting on, or raises a question, tell us. We say test everything, and we would rather hear the hard question now than after you ship.


## Ready to try it?


[Create your Superwall account →](https://superwall.com/sign-up?utm_source=blog&utm_medium=content&utm_campaign=whats-new-july-2026&utm_content=create-account) and build your first paywall today. It is free to get started, and everything above works from day one.


Want to see it live first?[Book a demo →](https://superwall.com/demo?utm_source=blog&utm_medium=content&utm_campaign=whats-new-july-2026&utm_content=book-demo) and we will walk you through Superwall AI, the CLI, and Managed Payments, mapped to your app. A good fit if you are on Enterprise, scaling growth, or weighing a migration.


## FAQ


Did Superwall acquire Tangent? Yes. In July 2026 Superwall acquired Tangent, the monetization consultancy founded by Vahe Baghdasaryan, and raised $7.5M. For Tangent's clients it is business as usual: Tangent still works with any subscription app, whether or not you use Superwall.


What is Superwall AI? Superwall AI is a built-in dashboard assistant that answers questions about how Superwall works, what is in your account, and what your data says, in plain English, with links to dig deeper.


What can I do with the Superwall CLI? The Superwall CLI lets you manage products, campaigns, and paywalls, query your analytics data, and call the Superwall API right from your terminal or coding agent. The Superwall Skill was updated alongside it with a new npx superwall skills installer.


What are Managed Payments in Superwall's web checkout? With Managed Payments, Stripe acts as merchant of record for eligible Stripe web checkout payments, handling tax calculation and compliance for you through Stripe Connect. You can see when a Stripe environment is Configured and Enabled, and override Managed Payments per paywall.


Can I control what event data the Superwall SDK sends on Android and Expo? Yes. The eventTrackingBehavior option, which came to iOS in June, is now on Android and Expo. You can choose everything, Superwall-only, or nothing, and switch it at runtime as users update their privacy consent. It requires Android SDK 2.7.19 or Expo SDK 1.2.0 or later.


What does Superwall's OneSignal integration do? It syncs Superwall subscription state to OneSignal as user tags, so your lifecycle and messaging team can segment and branch Journeys on trials, renewals, and cancellations without engineering work.
