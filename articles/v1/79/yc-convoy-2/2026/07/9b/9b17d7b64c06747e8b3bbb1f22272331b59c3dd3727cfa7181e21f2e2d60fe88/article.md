---
schema_version: "1.0.0"
document_id: "9b17d7b64c06747e8b3bbb1f22272331b59c3dd3727cfa7181e21f2e2d60fe88"
company_key: "yc-convoy-2"
company: "Convoy"
source_id: "yc-convoy-2-news-import-6ac8b1bcd379"
canonical_url: "https://www.getconvoy.io/changelog/queue-monitoring-object-storage-and-security-hardening"
published_at: null
first_seen_at: "2026-07-25T00:37:26.853065+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:1ecfcdf9dc2b33d42368249d2f58d50d0c893cdcd7aef707e61525783c3c73d5"
---

# Queue Monitoring, Object Storage & Security Hardening

Here's what shipped in April. This one is mostly operational: better visibility into the queue, a sturdier storage layer, and a round of security fixes.


**Queue monitoring**


Instance admins can watch the queue from the admin dashboard. See depth and health at a glance, so you catch a backlog before it turns into late deliveries.


**Object storage refactor**


We reworked the object storage layer. Exports and large payloads now go through a cleaner path that's easier to reason about and less likely to break under load.


**API versioning**


We upgraded to request-migrations v2. Older API clients keep working as we evolve the API, so an upgrade on our side doesn't break your integration.


**Security and reliability**


- **No token replay** : password reset tokens are now single-use
- **AMQP credentials** : special characters in RabbitMQ connection credentials are escaped correctly
- **SSO** : hardened the login and retry-deactivation paths
- **Split proxies** : the webhook dispatcher honors` NO_PROXY`
- **CI** : nightly CVE scans flag vulnerable dependencies early


Update when you get a chance, then keep an eye on your queue from the admin dashboard.
