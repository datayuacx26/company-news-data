---
schema_version: "1.0.0"
document_id: "8d695b71fd964a3fd04744cbba3ea8c61a07f6582e1875125df1187153c0d5e8"
company_key: "yc-convoy-2"
company: "Convoy"
source_id: "yc-convoy-2-news-import-6ac8b1bcd379"
canonical_url: "https://www.getconvoy.io/changelog/inbound-event-filtering-and-idempotency-overrides"
published_at: null
first_seen_at: "2026-07-25T00:37:26.853065+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:b7ae8dabbc49a3dde343c8922fcacf0e0482ad3d54871a9cc87426668d3e1a21"
---

# Inbound Event Filtering & Idempotency Overrides

Here's what shipped in May.


**Inbound event filtering**


Subscription filters now match on query parameters and the request path, not just the body and headers. Define rules on a subscription and Convoy drops events that don't match before they reach your endpoints, so you only deliver what you care about. Less noise, fewer wasted deliveries.


Filtering is a Premium feature (advanced subscriptions).


**Idempotency header overrides**


By default, Convoy stamps each delivery with its own` X-Convoy-Idempotency-Key` so your endpoint can dedupe retries and replays. If your receiver already has its own idempotency scheme, you can now set that header yourself in the event's` custom_headers` , and Convoy forwards your value instead of overwriting it. When you don't set one, Convoy's default still applies, so existing endpoints keep working unchanged.


**Auth and outbound hardening**


We closed a set of gaps in Convoy's auth boundaries and outbound paths. Project and portal permissions are now enforced consistently: event creation, replays, and portal writes all check that the project is enabled and that you own the endpoint before anything is queued. Outbound OAuth2 token requests go through the same protected transport as the rest of Convoy's egress, so untrusted input can't steer them at internal addresses. Secret generation also moved to a cryptographic RNG.


If you self-host with the JWT realm enabled, Convoy now requires` CONVOY_JWT_SECRET` and` CONVOY_JWT_REFRESH_SECRET` and refuses to start without them. Set both before upgrading.


**Dashboard on Angular 21**


The dashboard moved to Angular 21 with a flat ESLint config and lint in CI. Less of a payoff you'll see directly, more that the UI now ships faster and with fewer regressions.


Add a filter to any subscription and only the events you want get through.
