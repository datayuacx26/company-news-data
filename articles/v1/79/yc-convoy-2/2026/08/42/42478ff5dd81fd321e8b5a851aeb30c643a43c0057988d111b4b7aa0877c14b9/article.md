---
schema_version: "1.0.0"
document_id: "42478ff5dd81fd321e8b5a851aeb30c643a43c0057988d111b4b7aa0877c14b9"
company_key: "yc-convoy-2"
company: "Convoy"
source_id: "yc-convoy-2-news-import-6ac8b1bcd379"
canonical_url: "https://www.getconvoy.io/changelog/generated-sdks-redesigned-dashboard-and-dynamic-event-acks"
published_at: null
first_seen_at: "2026-08-05T17:48:33.814764+00:00"
fetched_at: "2026-08-05T17:48:36.576436+00:00"
content_hash: "sha256:26417f7dcaa1565fde988febc5687527f5d5348a6e2031a310afaa88b5a36ba8"
---

# Generated SDKs, Redesigned Dashboard & Dynamic Event Acks

Here's what shipped in July across the gateway, dashboard, and SDKs.


**SDKs are now generated from the spec**


Our Go, Python, JavaScript, Ruby, and PHP clients have been around for years, but they were maintained by hand and had drifted behind the API. Each one is now generated from Convoy's OpenAPI spec and regenerated whenever the API changes, so the types and methods you get match the API you're actually calling.


Java is new. It's on Maven Central as` io.github.frain-dev:convoy` .


Signature verification is still hand-written in every client and tested against one shared set of vectors, so verifying a webhook behaves the same whichever language you receive it in.


**A redesigned dashboard**


The dashboard has a new look throughout: event deliveries, endpoints, subscriptions, portal links, billing, and settings.


**Verify dynamic events before accepting them**


` POST /events/dynamic` can now wait for Convoy to work out where the event is going before it responds. Turn on **Verify Dynamic Events Before Accepting** in your project's endpoint settings.


With it on, a 201 means the event was accepted *and* matched to an endpoint and subscription. If that doesn't finish in time you get a 504 rather than a success you can't rely on. It's off by default, so existing integrations are unaffected.


**Failure reasons on events**


If a dynamic event's URL matches none of your endpoint URL templates, Convoy rejects it. The event used to show a bare` Failure` with nothing explaining why. It now carries a reason, shown in the event's detail view.


Events in that state also used to retry forever and sit in` Processing` . They now fail cleanly the first time.


**Breaking:**` GET /events` no longer returns the` metadata` field. On dynamic events it exposed the raw payload, including your endpoint secret and any custom auth headers, in plaintext.


**Custom User-Agent**


Set` User-Agent` in an event's` custom_headers` and Convoy sends that on the outbound webhook instead of` Convoy/<version>` . Receivers see only your agent string. It needs a paid license; without one Convoy keeps sending` Convoy/<version>` .


**Per-project request ID header**


When you publish an event with an idempotency key, Convoy passes that key on to your receivers under` X-Convoy-Idempotency-Key` so they can deduplicate too. Outgoing projects can now pick a different header name for it. Once you do, every event you publish has to include an` idempotency_key` , since that's the value the header carries.


**Endpoint health at a glance**


The endpoints list now shows circuit breaker state, failing or recovering, in the Status column, with tooltips explaining what each one means. The failure rate covers a rolling 30 days and counts deliveries that are still retrying as failures, so an outage in progress is visible before its retries run out.


That percentage is now the same one Convoy acts on when it decides to break a circuit. It used to be read from somewhere else and could sit at 0% through a real outage.


**Trials and pricing**


Cloud now offers three plans, Pro, Premium, and Enterprise, each with its own highlights and a side-by-side comparison. Trials are available on both cloud and self-hosted. Cloud trials need a verified email address first.


**Reliability fixes**


- **Duplicate events** : sending the same idempotency key twice at the same moment could store and deliver both. Only one gets through now
- **Events with no delivery target** :` POST /events` used to accept a request naming neither` endpoint_id` nor` app_id` , return 201, then deliver nothing. It's now rejected with a 400, and requests using` app_id` fan out to that app's endpoints as expected
- **Agent ingest rate limit** :` CONVOY_INSTANCE_INGEST_RATE` now applies to agent ingest too, so one setting covers every ingest route
- **Portal links with no endpoints** return an empty` endpoints_metadata` array instead of one containing` null` , which broke strictly typed clients
- **Team invites** default to Organisation Admin. The old default wasn't a valid role, so those invites failed on accept


**Security hardening**


Three security reviews landed this month. Webhook delivery now blocks requests to cloud metadata and link-local addresses, checked after the hostname resolves so it can't be pointed at one indirectly. We also closed a cross-tenant read that exposed another project's source credentials, stopped portal links from retargeting subscriptions onto endpoints they don't own or editing event types, removed signing secrets from portal link responses, and made revoked portal tokens and logged-out sessions stop working immediately.


Install an SDK for your language, then turn on verification if you want dynamic events matched to a destination before Convoy acknowledges them.
