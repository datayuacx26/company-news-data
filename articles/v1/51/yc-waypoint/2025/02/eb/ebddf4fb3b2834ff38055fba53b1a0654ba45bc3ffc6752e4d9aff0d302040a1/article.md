---
schema_version: "1.0.0"
document_id: "ebddf4fb3b2834ff38055fba53b1a0654ba45bc3ffc6752e4d9aff0d302040a1"
company_key: "yc-waypoint"
company: "Waypoint"
source_id: "yc-waypoint-rss-69637758f48b"
canonical_url: "https://www.usewaypoint.com/changelog/api-endpoint-list-emails/"
published_at: "2025-02-17T00:00:00+00:00"
first_seen_at: "2026-07-26T05:13:45.651737+00:00"
fetched_at: "2026-07-28T22:01:05.353137+00:00"
content_hash: "sha256:8b39a69ab99cf371570060f46d208386ffb9fcfd54300c6a32e4417980749f23"
---

# API endpoint: List emails

# API endpoint: List emails


Use our new[email messages API endpoint](https://www.usewaypoint.com/docs/api-reference#list-emails) to pull a report of your email messages, with optional date filters. Get the full picture, including delivery events like bounces, clicks, and complaints.


GET


` v1/email_messages`


Terminal window


```text
curl     "  https://live.waypointapi.com/v1/email_messages?createdAt.gt=2024-02-11T17:53:00Z  "     \    -H   "  Content-Type: application/json  "     \    -u   "  API_KEY_USERNAME:API_KEY_PASSWORD  "
```
