---
schema_version: "1.0.0"
document_id: "b56737e0d1af5c07a2b78cbf7da6d3d9877a1f73332b06dc0b0ac28683ad49bf"
company_key: "yc-waypoint"
company: "Waypoint"
source_id: "yc-waypoint-rss-69637758f48b"
canonical_url: "https://www.usewaypoint.com/changelog/api-endpoint-batch-message-report/"
published_at: "2025-01-11T00:00:00+00:00"
first_seen_at: "2026-07-26T05:13:45.651737+00:00"
fetched_at: "2026-07-28T22:01:05.353137+00:00"
content_hash: "sha256:936cdacaf30eb5360589c0fba689137d4ff0c423e0c067233166c6d79747f6c4"
---

# API endpoint: batch message report

# API endpoint: batch message report


With our new[batch email messages API endpoint](https://www.usewaypoint.com/docs/api-reference#retrieve-an-email-batch) , you can now get a report with all of the email messages that were sent through an[email batch](https://www.usewaypoint.com/docs/sending-in-batches) . See the full context for each of your emails in your batch including delivery events like bounces, clicks, and complaints.


GET


` v1/batches/:id/email_messages`


Terminal window


```text
curl     "  https://live.waypointapi.com/v1/batches/batch_RmETGV42naFpgHjr/email_messages  "     \    -H   "  Content-Type: application/json  "     \    -u   "  API_KEY_USERNAME:API_KEY_PASSWORD  "
```
