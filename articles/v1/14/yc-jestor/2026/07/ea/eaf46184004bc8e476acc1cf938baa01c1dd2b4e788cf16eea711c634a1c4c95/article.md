---
schema_version: "1.0.0"
document_id: "eaf46184004bc8e476acc1cf938baa01c1dd2b4e788cf16eea711c634a1c4c95"
company_key: "yc-jestor"
company: "Jestor"
source_id: "yc-jestor-rss-223b3fb070b1"
canonical_url: "https://blog.jestor.com/replace-formula-clean-phone-number-whatsapp-automation/"
published_at: "2026-07-19T05:41:19+00:00"
first_seen_at: "2026-07-20T23:23:58.972603+00:00"
fetched_at: "2026-07-28T20:37:08.491459+00:00"
content_hash: "sha256:a9bee97e9c2a33d22ff8e5bc7e9755236e93aed678549da600c5db3e52b187ba"
---

# How do you use the replace formula to strip spaces and characters from a phone number before a WhatsApp automation?

In **Jestor** , the **replace** formula lets you substitute spaces, plus signs, and dashes with nothing inside a text field, which is especially useful for cleaning up phone numbers before using them in WhatsApp automations.


## How the replace formula works


The formula works by substituting a specific piece of text with something else — including nothing, which effectively removes that character. In the case of phone numbers, it's common for people to type them with spaces, dashes, or a plus sign before the country code; using replace, you can strip out each of those characters before sending the number to an automation, ensuring the format stays clean and compatible.


Character to remove Using replace


**Space** Replaces the space with nothing, joining the number together


**Plus sign (+)** Removes the symbol before the country code


**Dash (-)** Removes separators used when typing the number


## How to set up the replace formula


1. Create a formula field in the table where the phone number is stored.
2. Use the replace function to substitute spaces, dashes, and plus signs with nothing.
3. Use this already-cleaned field as the phone number source in the WhatsApp automation.
4. Apply the same logic to other text fields that need cleanup before an automation.


## Why choose Jestor


- **More reliable WhatsApp automations** : numbers arrive standardized, reducing sending failures.
- **A versatile formula** : replace can also help with ID generation or cleaning up other text fields.
- **No dependence on external tools** : the data cleanup happens right inside Jestor's own table.


### Does replace only work for phone numbers?


No, it can be used to clean up any kind of text, such as when generating IDs or cleaning other fields.


### Do you need a separate formula field to use replace?


Yes, the formula is applied inside a dedicated formula field, which can then be referenced in your automations.


### Does replace only remove one type of character at a time?


Each application of the formula handles one specific substitution; to remove several types of characters, you can chain multiple replaces together as needed.


## Reference video


Check out more hands-on tutorials on the Jestor YouTube channel.


## Get to know Jestor


With Jestor, you can automate workflows, connect teams, and build internal systems your way — all without code and powered by AI.


Discover Jestor at[jestor.com](https://www.jestor.com/?ref=blog.jestor.com) and see how to take your company's operations to a new level of efficiency and control.
