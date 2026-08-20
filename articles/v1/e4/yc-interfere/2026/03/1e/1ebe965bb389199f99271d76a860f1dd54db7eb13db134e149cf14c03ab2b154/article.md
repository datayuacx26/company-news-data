---
schema_version: "1.0.0"
document_id: "1ebe965bb389199f99271d76a860f1dd54db7eb13db134e149cf14c03ab2b154"
company_key: "yc-interfere"
company: "Interfere"
source_id: "yc-interfere-news-import-5665c5c9f81b"
canonical_url: "https://interfere.com/changelog/consent-management"
published_at: "2026-03-12T04:00:00+00:00"
first_seen_at: "2026-07-23T12:15:39.285313+00:00"
fetched_at: "2026-07-28T21:26:25.193690+00:00"
content_hash: "sha256:fcb2fbbd37d988ed460777ea515b08cd67b09a8c4d90e5194c09fd9ddea9fffc"
---

# Introducing Consent Management | Interfere › Interfere

We've added consent gating to our SDKs, giving you full control over which features activate based on your users' consent preferences, with native support for all modern consent platforms: c15t, CookieYes, OneTrust, or your own.


This brings Interfere in-line with modern requirements from audit frameworks like SOC 2 Type II and GDPR. You can learn more by reading the SDK's[README](https://www.npmjs.com/package/@interfere/next/v/0.2.0-alpha.6) .


# How it works


Pass a` consent` prop to` InterfereProvider` and only the categories your users have agreed to will run. Error tracking is always on — it's treated as a necessary function. Analytics (page events, rage clicks, fingerprint) and session replay are independently toggleable.


layout.tsx


Additionally, consent state is reactive. When a user updates their preferences through your consent banner or by logging in, call` consent.set()` and the SDK will load or tear down plugins in real time.


layout.tsx


# What's next


We recently rolled out the` identify` feature, allowing you to associate visitors with persistent users in your app across surfaces. That means that a user of your iOS app and a user of your Next.js app are just one user – even across different authentication providers, giving you total visibility into your user's sessions across your entire product surface area.
