---
schema_version: "1.0.0"
document_id: "ebd39487c085235aac5bfab210e83ec8d097e52d827aede6ec01d38546cac452"
company_key: "yc-convoy-2"
company: "Convoy"
source_id: "yc-convoy-2-news-import-6ac8b1bcd379"
canonical_url: "https://www.getconvoy.io/changelog/org-feature-flags-oauth2-billing"
published_at: null
first_seen_at: "2026-07-25T00:37:26.853065+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:1e6504ff6eb387a590648e6746768624231f6b86faca11531aabc5a8c94d1eb2"
---

# Organization Feature Flags, OAuth2 Endpoint Auth & Billing

We shipped four features this week: organization-level feature flags, OAuth2 authentication for endpoints, a billing module, and better test infrastructure.


**Organization Feature Flags**


Feature flags used to be all-or-nothing across your entire Convoy instance. You can now manage feature flags at the organization level. Go to **Settings → Early Adopter Features** to enable features like OAuth2 token exchange and mTLS for your organization. Instance admins can also override feature flags for specific organizations from the admin panel.


**OAuth2 Authentication for Endpoints**


Some endpoints require OAuth2 authentication. Endpoints now support OAuth2 client credentials authentication with shared secret or JWT client assertion. Configure your OAuth2 settings in the endpoint form, and Convoy handles token exchange automatically. This feature requires an enterprise license and the OAuth Token Exchange early adopter feature flag enabled.


**Billing Module**


We added a billing module that calculates usage from your Convoy data. View monthly usage, manage subscriptions, handle invoices, and set up payment methods, all from the Convoy dashboard. Go to **Settings → Usage and Billing** to get started. This feature requires a billing module license and billing admin or organization admin role.


**E2E Test Infrastructure**


We rebuilt our end-to-end test infrastructure to make it more reliable and easier to maintain. Tests now use Docker containers for Redis and PostgreSQL, making them faster and more consistent across environments.


**Other Improvements**


- **UI refresh** : We refreshed the dashboard UI with a cleaner design and better navigation
- **Batch tracking** : Track progress when retrying multiple event deliveries at once
- **Admin UI** : Instance admins can manage feature flags and circuit breaker configurations from a dedicated admin panel
- **Job IDs** : All queue jobs now use ULID-based job IDs for better tracking


All features are available now. See the[endpoints documentation](https://www.getconvoy.io/docs/product-manual/endpoints) for OAuth2 configuration details.
