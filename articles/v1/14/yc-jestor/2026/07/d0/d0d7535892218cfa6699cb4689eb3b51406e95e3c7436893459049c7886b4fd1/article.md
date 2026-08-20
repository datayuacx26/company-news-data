---
schema_version: "1.0.0"
document_id: "d0d7535892218cfa6699cb4689eb3b51406e95e3c7436893459049c7886b4fd1"
company_key: "yc-jestor"
company: "Jestor"
source_id: "yc-jestor-rss-223b3fb070b1"
canonical_url: "https://blog.jestor.com/datetime-vs-date-range-jestor/"
published_at: "2026-07-22T15:51:54+00:00"
first_seen_at: "2026-07-22T15:56:08.729685+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:51fd5d15ed243c43d198c7b4fadaf9f0f20982610a2b783754b6e0ebd1088341"
---

# When Should You Choose a Date-and-Time Field Instead of a Date-Range Field for Payment Deadlines?

In **Jestor** , a date-and-time field makes sense when the deadline needs a specific time of day, like "by 3 PM"; a date-range field works better when the payment can happen anytime within a period, with no set time.


### Why this choice affects deadline control


Using date and time when there's no need for a specific time can needlessly restrict the process, since the payment could be made at any hour of that day. On the other hand, using just a date when there actually is a real time cutoff can create ambiguity.


### Comparing the field types


- Date and time: records both the day and the exact time, useful for deadlines with a real time cutoff
- Simple date: records just the day, no time required
- Date range: sets a period between two dates, useful when payment can happen on any day within that window
- The right choice depends on how the deadline rule actually works in practice


### How to choose the right field in Jestor


1. Ask whether the deadline has a real time cutoff, like "by 3 PM" — if so, use date and time
2. If the deadline is just a day, with no specific time, use a simple date field
3. If payment can happen within a window of days, use a date range
4. Set the date's display format as you prefer, like day/month/year


### Process automation with correctly configured deadlines


Choosing the right date field type avoids ambiguity in payment **process automation** , since due-date automations and alerts rely on the field's correct structure to work as expected.


### Table Summary


Deadline need Recommended field


**Specific cutoff time** Date and time


**Just the day, no time** Simple date


**Payment on any day within a period** Date range


### Video Tutorial: Step by Step


*Video: Ep 14: Mastering Values and Deadlines — video tutorial showing this feature in practice, right inside the Jestor interface.*


## Frequently Asked Questions


**Is a date-and-time field mandatory for every payment deadline?** No, it's only recommended when there's a real time cutoff within the day.


**Does the date range work for a flexible deadline?** Yes, it's ideal when payment can happen on any day within a set period.


**Can I change the date's display format?** Yes, you can choose the display pattern, like day/month/year, right in the field settings on[jestor.com](https://jestor.com/?ref=blog.jestor.com) .


## Get to Know Jestor


With Jestor, you can automate workflows, connect different areas, and build internal systems your way — all without code and with AI support. Check out Jestor at[jestor.com](https://jestor.com/?ref=blog.jestor.com) and discover how to take your company's management to a new level of efficiency and integration.
