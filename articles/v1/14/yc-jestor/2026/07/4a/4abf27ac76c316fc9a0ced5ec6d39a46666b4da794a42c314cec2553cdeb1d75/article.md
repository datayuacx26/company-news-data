---
schema_version: "1.0.0"
document_id: "4abf27ac76c316fc9a0ced5ec6d39a46666b4da794a42c314cec2553cdeb1d75"
company_key: "yc-jestor"
company: "Jestor"
source_id: "yc-jestor-rss-223b3fb070b1"
canonical_url: "https://blog.jestor.com/formula-vs-lookup-field-jestor/"
published_at: "2026-07-25T03:30:54+00:00"
first_seen_at: "2026-07-25T03:34:18.287744+00:00"
fetched_at: "2026-07-28T21:47:36.059629+00:00"
content_hash: "sha256:b4de2a14ceb5764b78e06177888285e0f1ba06bdc3678fab03c0188c2a9c7e49"
---

# Formula field vs. lookup field: when does each make more sense for bringing in connected data?

In **[Jestor](https://jestor.com/?ref=blog.jestor.com)** , a lookup field brings in a value that already exists in a connected table, without changing it, while a formula field processes a calculation — sum, multiplication, condition — from one or more fields, connected or not.


## The mistake of using the wrong field for the wrong data


When structuring relational data, it's common to wonder where to process the information: pull the raw data from the connected record, or already calculate something on top of it.


Using the wrong field creates two kinds of problems: needlessly complex formulas just to display a simple piece of data, or lookups that don't deliver the calculation the process actually needs.


## How to decide between the two


1. Use lookup when you just need to display a value that already exists in another table.
2. Use formula when you need a calculation, a condition, or a combination of values.
3. Combine both: bring in the data with lookup, then calculate something on top of it with formula.
4. Avoid recreating with formula something a lookup would already solve more simply.
5. Consider displaying the data directly on the card, which sometimes removes the need for the lookup.


Field type What it does Updates automatically?


Lookup Displays a value that already exists in the connected table Yes, if the source value changes


Formula Calculates something new, combining fields and values Yes, whenever the source fields change


## Why choose Jestor?


**[Jestor](https://jestor.com/?ref=blog.jestor.com)** also lets you display connected data directly on the card or in the form, without necessarily creating a lookup field — which sometimes frees up space in the table's structure.


### Frequently Asked Questions


**Can a lookup bring in data from more than one connection level?** Yes, you can bring in information from multi-level connected records, also displayed directly on the card.


**Can a formula use fields from different tables?** Yes, as long as there's a connection between the tables, the formula can combine connected fields.


**Is it worth removing old lookups if the information already shows up on the card?** Yes, if the information is already available through direct display on the card, the corresponding lookup can be removed to save space in the table.


## Reference Video


## Get to know Jestor


With **[Jestor](https://jestor.com/?ref=blog.jestor.com)** , you can automate workflows, connect departments, and build internal systems your way — all without code and with **AI** support.


Get to know[Jestor](https://jestor.com/?ref=blog.jestor.com) at jestor.com and see how to take your company's management to a new level of efficiency and integration.
