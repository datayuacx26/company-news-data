---
schema_version: "1.0.0"
document_id: "acfad3fe2e162fc0bf1d7028bc3860ed0d628e7ec0b57403f38224d8eed3cfc6"
company_key: "yc-waypoint"
company: "Waypoint"
source_id: "yc-waypoint-rss-69637758f48b"
canonical_url: "https://www.usewaypoint.com/changelog/api-endpoint-email-message-timeline-events/"
published_at: "2024-10-09T00:00:00+00:00"
first_seen_at: "2026-07-26T05:13:45.651737+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:979d81190441d135089a3c7c2a7bbc127bf5363045a5c0db2a027d2b005a848a"
---

# API endpoint: email message timeline events

Our new[email message events API endpoint](https://www.usewaypoint.com/docs/api-reference#list-email-events) gives teams the ability to access the full timeline of events on an individual email message. These events include when the message was created, every open, click, bounce, or any other event found in our[email event message logs](https://www.usewaypoint.com/docs/email-event-logs) .


This is especially helpful for teams that want to show this timeline of events to their own customers. For example, a CRM software platform may want to show these delivery events to help their customers better understand deliverability.


GET


` /v1/email_messages/EMAIL_MESSAGE_ID/events`
