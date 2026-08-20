---
schema_version: "1.0.0"
document_id: "da22d1f19e38c68b6bd88b366322effecfa1b7f660155b2ddfcad6b89c77313f"
company_key: "yc-castle-2"
company: "Castle"
source_id: "yc-castle-2-news-import-b99476926256"
canonical_url: "https://docs.castle.io/changelog/phone-call-detection"
published_at: "2026-05-19T00:00:00+00:00"
first_seen_at: "2026-07-21T12:42:16.969450+00:00"
fetched_at: "2026-07-28T21:43:32.355791+00:00"
content_hash: "sha256:980f4d1bcd4ba7a236fa83625d22a961c8ee7a937cbb3727b3f4bcbbe8df7741"
---

# Phone call detection

[Back to All](https://docs.castle.io/changelog)


Added


Castle can now detect when a user is on a phone call during sensitive actions like login or transactions. This helps you catch social engineering attacks, where a scammer calls the victim and walks them through actions while simultaneously taking over their account.


Three new fields are available under the` device.call` section of the event and API response payload:


```text
"device": {
"call": {
"state": "active",
"type": "cellular",
"direction": "incoming"
}
}
```


- **` device.call.state`** :` active` ,` ringing` ,` dialing` or` on_hold`
- **` device.call.type`** (Android):` cellular` or` voip`
- **` device.call.direction`** (iOS):` incoming` or` outgoing`


All three fields are searchable in Explore and available as policy filter conditions.


###


New signal: Phone Call Active


A new **Phone Call Active** signal fires on every event where the user's device has an active phone call. On Android, it only fires for cellular calls (not VoIP), since a VoIP call is more likely someone on a Teams or Zoom call while logging in.


Use this signal in your policies to challenge or deny high-risk actions performed during a phone call, a strong indicator of social engineering.


> ℹ️
>
>
> Phone call detection is available on Android SDK 3.1.3+ and iOS SDK 4.0.0+. Data is collected automatically by the SDKs with no integration changes needed.
