---
schema_version: "1.0.0"
document_id: "7a934c582086b37a55ff15444c2437c850facf363c7b42b7ebb004fc7beec5ea"
company_key: "yc-loops"
company: "Loops"
source_id: "yc-loops-news-import-d3d77458967f"
canonical_url: "https://loops.so/changelog/double-opt-in"
published_at: "2025-11-20T00:00:00+00:00"
first_seen_at: "2026-07-25T13:13:35.252230+00:00"
fetched_at: "2026-07-28T21:27:35.329570+00:00"
content_hash: "sha256:7df88973f2ea7ed8dbe3dd79541afe3d940145a52fb0adfe310d69d7eed9fd18"
---

# Double opt-in

#### Release: **Double opt-in**


Gain consent for email sending in a single click with double opt-in. When enabled, new contacts added via a form or form endpoint will be required to confirm their subscription by email before they’re added to your audience.


Loops handles the confirmation email, tracking, and audience updates for you, all you have to do is[opt-in](https://app.loops.so/settings?page=sending) .


Learn more in the[docs](https://loops.so/docs/contacts/double-opt-in) or enable it in your account today.


———


#### Release: **Preference center preview**


You can now easily preview the **preference center** based on one or more mailing lists to easily simulate what a contact sees when they’re subscribed to specific lists.


Learn more in the[docs](https://loops.so/docs/contacts/mailing-lists#preference-center)


———


### Improvement: **Reliable contact scaling**


We’ve been focused on making Loops more reliable for larger teams and higher volumes. Under the hood, we’ve made contact updates and related operations more efficient so you can push more data through Loops without slowing down day-to-day work.


Together, these changes make it easier to **scale reliably with Loops** as your team and audience grow to hundreds of millions of contacts and beyond.


———


### Improvement: **Better preview emails**


When sending preview emails you now can toggle the **Add preview tag** setting to prepend` \[PREVIEW\]` to the subject line.


This makes it easier to identify preview sends vs production messages in your inbox. Turn it off when you want to see the exact live subject line, just like your subscribers will.


———


### Improvement: **Track dedicated sending IP’s via endpoint**


For teams that care deeply about deliverability and reputation, Loops now exposes your **dedicated sending IP’s** via an API endpoint.


This is the same dedicated IP pool we monitor against providers like Spamhaus. You can use it to integrate deliverability and reputation checks into your own monitoring and alerting.


Learn more in the[docs](https://loops.so/docs/api-reference/dedicated-sending-ips) .


———


### Release: **Custom markdown element**


Under the hood, we now support c **ustom Markdown elements** with special tags like` loops-button` and` loops-columns` that render as full components in the editor.


That means you can paste Markdown that already includes structured elements (CTAs, simple two-column layouts while still getting the benefits of the visual editor, Components, and style controls.


Learn more in the[docs](https://loops.so/docs/creating-emails/editor#custom-markdown-tags) .
