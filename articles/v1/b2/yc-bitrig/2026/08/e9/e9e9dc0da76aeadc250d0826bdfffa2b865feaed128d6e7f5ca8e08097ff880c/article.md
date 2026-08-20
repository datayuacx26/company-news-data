---
schema_version: "1.0.0"
document_id: "e9e9dc0da76aeadc250d0826bdfffa2b865feaed128d6e7f5ca8e08097ff880c"
company_key: "yc-bitrig"
company: "bitrig"
source_id: "yc-bitrig-news-import-267282887e75"
canonical_url: "https://bitrig.com/blog/add-push-notifications"
published_at: "2026-08-18T00:00:00+00:00"
first_seen_at: "2026-08-19T05:59:10.313468+00:00"
fetched_at: "2026-08-19T05:59:12.507306+00:00"
content_hash: "sha256:00109a7dc20c5802feeabe2754ee1add12d4dc687d52a6f99824e6160271ff09"
---

# Bitrig Can Now Add Push Notifications to Your Apps

In the last two releases, we made it easy to add a[CloudKit](https://bitrig.com/blog/bitrig-builds-apps-with-cloudkit) or[Supabase](https://bitrig.com/blog/bitrig-builds-apps-with-supabase) backend.


Now your app can keep data in sync across devices, let people collaborate, manage user accounts, run code on a server, and respond to events even when it isn’t open.


But when something changes outside your app, you still need a way to tell your users about it. That’s what push notifications are for.


Someone commented on your post. An item was added to your grocery list. Your order is ready for pickup.


The notification itself is simple, but setting it up is tedious.


You need to configure the right Xcode capabilities, prompt the user for permission, register each device with APNs and your backend, account for APNs’ development and production environments, create and store an Apple push key, and write the server code that sends each notification.


It’s exactly the kind of fragmented busywork that breaks your flow.


Today, that changes:


**Bitrig can now automate your app’s entire push notification setup, from Xcode to your backend.**


That means:


-


**Send from CloudKit.** Bitrig creates rules that define which CloudKit changes should trigger a notification, and CloudKit handles delivery. No push key or notification server to run.


-


**Send from your custom backend.** With Supabase or another backend, Bitrig stores device tokens, creates the sending code, wires up the events that should trigger a push, and puts the required Apple credentials directly into your backend’s secrets.


-


**Keep your app up to date.** Use pushes to update Live Activities, refresh widgets and Apple Watch complications, or fetch new data in the background before someone opens your app.


-


**Test the whole path.** Bitrig handles development and production correctly, so you can test notifications in the simulator, on devices, through TestFlight, and on the App Store.


Each part of push notification support can be pieced together by hand. But it’s easy to lose your momentum bouncing between your app, Apple’s developer websites, and your backend dashboard, trying to make sure every capability, token, environment, identifier, and secret lines up.


Bitrig takes care of wiring it all together, end to end.


[Open Bitrig](https://bitrig.com/download) and add push notifications to your app today.
