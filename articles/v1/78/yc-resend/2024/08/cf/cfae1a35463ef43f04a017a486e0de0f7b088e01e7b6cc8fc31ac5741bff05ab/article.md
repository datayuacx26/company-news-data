---
schema_version: "1.0.0"
document_id: "cfae1a35463ef43f04a017a486e0de0f7b088e01e7b6cc8fc31ac5741bff05ab"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/broadcast-schedule"
published_at: "2024-08-25T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T20:59:29.484597+00:00"
content_hash: "sha256:06b2a2f2863a757ffb1f3dc0a98f74bbe9499ce2183ae0eb18e91a7be7e7f686"
---

# Broadcast Schedule

Earlier this week, we introduced the ability to[schedule individual emails](https://resend.com/blog/introducing-the-schedule-email-api) programmatically using the Resend API with the` scheduled_at` parameter.


Today, we're excited to announce that you can now schedule broadcasts too.


## Schedule a broadcast


When you navigate to the[Broadcasts page](https://resend.com/broadcasts) , and click on the *"Create Email"* button, you will now see an option called *"When"* .


The *"When"* text input uses natural language to convert your date and time into a valid timestamp. For example, you can type` "in 1 hour"` or` "tomorrow at 9am"` or` "next Monday at 3pm ET"` .


If you don't specify a timezone, the broadcast will be scheduled in the user's local time. If you specify the timezone, the time will be calculated based on that timezone.


Different from the` scheduled_at` parameter, which has a short limit of how far in the future you can schedule an email, broadcast scheduling has a much more flexible range.


## Edit a scheduled broadcast


If you need to update a scheduled broadcast, you can do so by navigating to the specific broadcast.


You will be prompted with a confirmation dialog, and you can choose to update the scheduled time, update the content, or both.


## Get started


You can start scheduling broadcasts today by going to the[Broadcasts page](https://resend.com/broadcasts) .


If you have any questions or feedback, feel free to[reach out](https://resend.com/contact) .
