---
schema_version: "1.0.0"
document_id: "90b6e287781ddca830b5b1ca5a2181ac346114baad84dec6646694f06a988c15"
company_key: "yc-loops"
company: "Loops"
source_id: "yc-loops-news-import-d3d77458967f"
canonical_url: "https://loops.so/changelog/columns"
published_at: "2024-10-07T00:00:00+00:00"
first_seen_at: "2026-07-25T13:13:35.252230+00:00"
fetched_at: "2026-07-28T21:33:00.470256+00:00"
content_hash: "sha256:60353d84c7f3e6f08023bf02a2d7c70b0a07c58d05dff0f5bfc38a369f293d75"
---

# Columns arrive at Loops

Chris from Loops here. This month we’re releasing a big feature request, Column support! Columns allow multiple images, text, buttons and more to be placed side by side, helping you create more dynamic and well-designed emails 🎨


### **Columns have arrived**


We did it! Column support is now rolled out in the editor.


And it’s very cool. You can control the **gap spacing** and the **width** of columns in the editor with a click and drag.


We also added support for stacking the columns on mobile or maintaining the columns side by side.


[Check out the docs](https://loops.so/docs/creating-emails/editor#columns) for more on our implementation or just[try the template below](https://app.loops.so/templates?templateId=cm1qt7ct4000008mi67bk1l8m) with columns fully built-out.


### **Improving deliverability with large institutions**


As Loops has continued to scale, we’ve faced various deliverability challenges and love solving them, because when we fix an issue for **one** customer, it benefits **all** our users.


Recently, we made changes to improve compliance with an older type of email gatekeeping around strict spam filters and legacy email protocols.


These are somewhat common at large corporations, and as a result of that effort we reworked how emails are composed on the backend.


For the technical details, our Senior Engineer Sam outlined the process we went through to improve deliverability across the Loops platform[in this article](https://loops.so/updates/a-deep-dive-into-rspamd) .


### **Other improvements**


1.


Names are now supported in Webhook integrations (Stripe and Clerk).


2.


We now support Mailing Lists in our Census integration.


3.


Fixed overflowing preview email modal with many emails.


4.


If an email has preview text added, the email will now open with the sending details fully expanded.
