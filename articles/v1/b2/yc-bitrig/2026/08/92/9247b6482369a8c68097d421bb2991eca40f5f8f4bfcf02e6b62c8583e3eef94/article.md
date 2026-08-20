---
schema_version: "1.0.0"
document_id: "9247b6482369a8c68097d421bb2991eca40f5f8f4bfcf02e6b62c8583e3eef94"
company_key: "yc-bitrig"
company: "bitrig"
source_id: "yc-bitrig-news-import-267282887e75"
canonical_url: "https://bitrig.com/blog/bitrig-builds-apps-with-supabase"
published_at: "2026-08-12T00:00:00+00:00"
first_seen_at: "2026-08-13T04:07:02.044586+00:00"
fetched_at: "2026-08-13T04:07:03.916578+00:00"
content_hash: "sha256:0f2ec917b26932f1b92cd57e262ca8882cf583153570c5b755a0f0973e5d4191"
---

# Bitrig Can Now Build Apps with Supabase Backends

Last week, we added support for building[CloudKit backends](https://bitrig.com/blog/bitrig-builds-apps-with-cloudkit) with Bitrig.


For many apps, CloudKit is exactly what you want: store your data in iCloud, keep it in sync across devices, and even share it between users.


But some apps call for more.


Sometimes you need a relational database. You need your own user accounts. You need to respond to webhooks.


For those apps, you want a more traditional backend.


Today, Bitrig can build that too:


**Bitrig can now build apps backed by Supabase.**


That means:


-


**Create Your Database.** Start from scratch or connect an existing Supabase project. Bitrig designs your Postgres schema, applies migrations, and writes your Swift models and queries.


-


**Add Authentication.** Let users sign in through popular social providers, by email, or anonymously. We've taught Bitrig to handle the entire Sign in with Apple setup for you.


-


**Secure Your Data.** Bitrig sets up database permissions and Row Level Security so users can access only the data they’re supposed to, then checks your backend for security and performance problems before you ship.


-


**Run Code on the Server.** Build and deploy Supabase Edge Functions to respond to webhooks, run scheduled jobs, and call third-party APIs without exposing your API keys and secrets.


-


**Prepare for App Review.** Bitrig sets up reviewer access for login-gated apps and inspects your Supabase backend to help fill out your app’s Privacy Nutrition Label.


Supabase gives every project a hosted Postgres database, along with authentication, file storage, realtime updates, and server-side functions. Supabase is free to get started and[affordable](https://supabase.com/pricing) to scale up if your app takes off.


If you’re building a personal journal app whose data should follow someone from their iPhone to their Mac, CloudKit is probably a great fit.


If you’re building a running club app with users, clubs, memberships, invitations, comments, and leaderboards, Supabase makes a lot more sense.


That used to mean jumping between your app code, server code, and developer dashboards to keep everything in sync.


Now, you can just[open Bitrig](https://bitrig.com/download) and tell it what you want to build.
