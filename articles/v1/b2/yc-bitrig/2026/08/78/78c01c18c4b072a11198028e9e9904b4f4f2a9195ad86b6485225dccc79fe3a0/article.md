---
schema_version: "1.0.0"
document_id: "78c01c18c4b072a11198028e9e9904b4f4f2a9195ad86b6485225dccc79fe3a0"
company_key: "yc-bitrig"
company: "bitrig"
source_id: "yc-bitrig-news-import-267282887e75"
canonical_url: "https://bitrig.com/blog/bitrig-builds-apps-with-cloudkit"
published_at: "2026-08-05T00:00:00+00:00"
first_seen_at: "2026-08-06T05:20:11.535942+00:00"
fetched_at: "2026-08-06T05:20:13.067880+00:00"
content_hash: "sha256:a3d867bcb5b45b119fd72685aa7e21bd4cbbab81e9245d585d9df804cfb8603f"
---

# Bitrig Can Now Build Apps with CloudKit Backends

With Bitrig, you can build an app that runs on[every Apple platform](https://bitrig.com/blog/bitrig-builds-apple-tv-and-vision-pro-apps) .


Doing that well means keeping your users’ data up to date on all of their devices.


Apple has set a high bar with its own apps, like Notes and Photos. I can pick up my iPhone and continue right where I left off on my Mac. My data is backed up without me thinking about it. It just works.


Behind much of that experience is CloudKit, Apple’s cloud backend service. There’s no server to build, operate, or scale. No sign-up, because your users are already signed into iCloud. And for most apps it’s free: your users’ private data lives in their own iCloud account, counted against their storage rather than yours.


Today, we’re excited to announce:


**Bitrig can now build apps that use CloudKit to securely sync your users’ data across all their devices.**


CloudKit has its sharp edges. There’s more than one way to use it, and the right choice depends on your app. Your schema lives in a web dashboard outside your repository. Your data model has to follow implicit rules, and schema changes require careful planning. Even experienced Apple developers can get tripped up by these details.


Bitrig now takes care of them for you:


-


**Sign in once.** The same secure Apple ID sign-in is shared across CloudKit, App Store Connect, RevenueCat, and Xcode downloads. Bitrig automatically manages the authentication tokens and API keys behind the scenes.


-


**Agentic CloudKit access.** Bitrig can inspect records, query data, validate schema changes, and diagnose CloudKit issues directly, instead of sending you off to the CloudKit Console.


-


**Schema as code.** Every important part of your app already lives in your repository. Now your CloudKit schema does too. Bitrig keeps it in sync with your code, making schema changes versioned, reviewable, and easy to evolve.


-


**Built-in CloudKit expertise.** Bitrig knows when to use SwiftData, Core Data, CKSyncEngine, or the raw CloudKit APIs. It catches common deployment mistakes, understands CloudKit’s development and production environments, and guides you through any steps Apple still requires you to do manually.


CloudKit is most commonly used for one person’s private data, but it also has a powerful sharing model. Users can invite other people to view or edit a single item or an entire collection. CloudKit handles the invitations, permissions, and synchronization while the data remains in its owner’s iCloud account.


That makes it a natural fit for shared schedules, trip planning, household lists, team checklists, and other apps where a small group collaborates on private data. Your users get the familiar sharing experience they know from Apple’s own apps, without you having to build accounts, access control, or a custom server.


Great apps don’t make people think about syncing. They just work.


Now yours can too.


[Open Bitrig](https://bitrig.com/download) and ask it to add CloudKit to your app today.
