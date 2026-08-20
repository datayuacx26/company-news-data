---
schema_version: "1.0.0"
document_id: "552a1287eb0c51bbd00d1e24d35f25ab9188ff92fba28f18e34c83e84401d147"
company_key: "yc-reclaim-protocol"
company: "Reclaim Protocol"
source_id: "yc-reclaim-protocol-news-import-8b5fb76919cb"
canonical_url: "https://blog.reclaimprotocol.org/posts/moving-beyond-google-play-instant"
published_at: "2025-10-29T00:00:00+00:00"
first_seen_at: "2026-07-25T20:39:06.647660+00:00"
fetched_at: "2026-07-28T21:59:41.762292+00:00"
content_hash: "sha256:4db93db70466a896ee8f2fc923ffd80cdcfb1c567cacb2def0fb4452d90897be"
---

# Moving Beyond Google Play Instant

# Moving Beyond Google Play Instant


Sajjad Haider Sayed


Oct 29, 2025


Android


Google Play Instant


Deprecation


Product Update


There's something satisfying about building experiences that feel invisible - fast, seamless, and reliable. Our Google Play Instant–powered verification flow was one of those quiet wins users could generate proofs in seconds without having to visit the Play Store.


## What Was Google Play Instant?


Instant Apps were a Google Play feature that let users run lightweight versions of Android apps immediately - no full download, no icon on the home screen, and no redirects to the Play Store.


It was essentially an on-demand runtime environment managed by Google Play, and it powered our instant verification flow behind the scenes.


## What's happening


Google has announced that Google Play Instant will be discontinued starting December 2025. Per their guidance:


> Starting December 2025, Instant Apps cannot be published through Google Play, and all Google Play services Instant APIs will no longer work.


To keep proof generation smooth and uninterrupted, we're rolling out an updated Android verification flow the week of November 4, 2025.


## What's changing


The Reclaim experience itself remains the same – generating proofs still works exactly as before.


The only difference is for first-time users:


- **If you already have the Reclaim app** → nothing changes.
- **If you don't** → you'll be redirected once to the Play Store to install the app. After installation, it opens automatically and verification continues right away.


\[video\]


#### Before (Instant App)


Your browser does not support the video tag.


#### After (New Flow)


Your browser does not support the video tag.


Once the app is installed, everything stays just as fast and seamless as before.


## Our new approach


We've rebuilt our verification flow using Google's deferred deep link guidelines.


When a user installs the app during verification, they will be redirected back automatically — no extra taps, no restarts.


So while there's now a brief Play Store stop for first-time users, the overall experience remains smooth, automatic, and instant-feeling.


## Why this matters


This change aligns with Google's push for more consistent, secure, and performant app experiences. For us, this means a slightly different flow - but also a chance to make things more reliable across all Android devices.


You can expect:


- Fewer edge cases and errors
- Better stability
- Room to build new features we couldn't support before


Our goal stays the same: making verification easier and simple.


## Timeline & what to expect


- **Week of November 4, 2025** : New flow rolls out
- **December 2025** : Google discontinues Instant Apps


---


Questions about migration? Book a slot on[https://cal.com/abdul-reclaimprotocol](https://cal.com/abdul-reclaimprotocol)
