---
schema_version: "1.0.0"
document_id: "486d38e7b4b38daf1d0aee49f04325ea8beb747454c7a6e0777ae91b4f175a46"
company_key: "yc-jestor"
company: "Jestor"
source_id: "yc-jestor-rss-223b3fb070b1"
canonical_url: "https://blog.jestor.com/currency-vs-text-field-jestor/"
published_at: "2026-07-22T15:51:52+00:00"
first_seen_at: "2026-07-22T15:56:08.729685+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:a7ab6fdc9f94deba29fdc3100071172f5b17b893fc433d4daaac0b897c0e9727"
---

# Why Does a Currency Field Let You Sum Values in Reports While a Text Field Doesn't?

In **Jestor** , a currency field stores the value as a number, letting formulas and automations sum those values; a text field stores the content as a string of characters, which can't be added up mathematically.


### Why this technical difference matters


Typing "$100" into a text field can look visually just like a currency field, but the system doesn't interpret that content as a number. When trying to sum installment values or generate financial reports, a text field simply won't add up, causing an error or a zeroed-out result.


### What a currency field offers on top


- Stores the value as a number, allowing sums in formulas and automations
- Lets you choose a currency symbol (dollar, euro, real, among others) in a standardized way
- Works with automatic reading via OCR, filling in the value extracted from an invoice
- Makes reporting easier for summing installments, total accounts payable, or revenue


### How to use a currency field correctly in Jestor


1. When creating the field, search for and select the "currency" type, not "text" or "number"
2. Choose the standardized currency symbol for that table
3. Use that field in formulas that need to sum or calculate values
4. Avoid text fields for any value that will need to be part of a calculation later


### Process automation with correct data from the start


Choosing the right field type from the beginning avoids rework in financial **process automation** , since fixing a text field into a currency one after many records have been created takes more work than setting it up correctly from the start.


### Table Summary


Field type Sums in formulas?


**Currency** Yes


**Number** Yes


**Text** No


### Video Tutorial: Step by Step


*Video: Ep 14: Mastering Values and Deadlines — video tutorial showing this feature in practice, right inside the Jestor interface.*


## Frequently Asked Questions


**Can I convert a text field into a currency field later?** It's best to configure it correctly from the start; later conversions may require manually adjusting existing data.


**Does the number field also sum values like the currency field?** Yes, but the currency field has the added benefit of standardizing the displayed currency symbol.


**Does the currency field work with automatic invoice reading?** Yes, Jestor's OCR can automatically fill that field from an attachment uploaded on[jestor.com](https://jestor.com/?ref=blog.jestor.com) .


## Get to Know Jestor


With Jestor, you can automate workflows, connect different areas, and build internal systems your way — all without code and with AI support. Check out Jestor at[jestor.com](https://jestor.com/?ref=blog.jestor.com) and discover how to take your company's management to a new level of efficiency and integration.
