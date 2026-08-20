---
schema_version: "1.0.0"
document_id: "5d2082b29c5c4919eb7a6e7bbf0e76829ffda4a966b00346ced5ca492cf1b0c1"
company_key: "yc-waypoint"
company: "Waypoint"
source_id: "yc-waypoint-rss-69637758f48b"
canonical_url: "https://www.usewaypoint.com/changelog/receive-webhooks/"
published_at: "2025-07-05T00:00:00+00:00"
first_seen_at: "2026-07-26T05:13:45.651737+00:00"
fetched_at: "2026-07-28T22:01:03.825556+00:00"
content_hash: "sha256:e74f6ffd0089995004cb79a52b254b35cf2b59f7844e245386d79edc9130f4f9"
---

# Send emails from third party webhooks

# Send emails from third party webhooks


Earlier this year, we released[sending webhooks](https://www.usewaypoint.com/changelog/webhooks) based on delivery events.


Today, we are introducing the opposite - **receiving webhooks events from third party services to send emails** .


Common event examples:


- A successful payment on Stripe.
- A new pull request on GitHub.
- A new user account created on Auth0.
- An new order on Shopify.
- A shipping status change on Shippo.


This is done by creating a webhook endpoint URL on Waypoint to listen for third-party events and trigger associated[workflows](https://www.usewaypoint.com/docs/workflow-basics/) (and resulting dynamic emails).


[View Stripe webhook example](https://www.usewaypoint.com/docs/trigger-workflows-via-webhooks/#example-stripe-webhook-event)
