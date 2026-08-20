---
schema_version: "1.0.0"
document_id: "37c5b8e14d63f6ce358a4902b6c3e41527f9692548bffa73736cb315f61fc655"
company_key: "yc-convoy-2"
company: "Convoy"
source_id: "yc-convoy-2-news-import-6ac8b1bcd379"
canonical_url: "https://www.getconvoy.io/changelog/bulk-onboarding-basic-auth-endpoints-workspace-slugs"
published_at: null
first_seen_at: "2026-07-25T00:37:26.853065+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:4de00dd82194a5ba285b7a5698f2243d392930fc10bd85d916681836e258ac63"
---

# Bulk Onboarding, Basic Auth Endpoints & Workspace Slugs

Here's what shipped in February and March across the gateway and dashboard.


**Bulk onboarding**


Stand up many endpoints at once. Send a list of destinations (name, URL, event type, and optional basic auth) to the bulk onboard API and Convoy creates the endpoints and their subscriptions in a single call. Run it as a dry run first to catch bad rows before anything is created. Basic auth on the items follows the same gate as below; without the license, those endpoints are created without auth.


**Basic auth for endpoints**


Endpoints now support basic auth. Set a username and password in the endpoint form and Convoy sends the` Authorization` header on every delivery, so you can reach endpoints that sit behind basic auth.


Basic auth is a Premium feature. Turn on the **Basic Auth** early adopter flag for your organization under **Settings → Early Adopter Features** .


**Workspace slugs**


Organizations can set a workspace slug on cloud, and it takes the organization admin role to edit. The slug gives your team a named SSO sign-in instead of a generic screen, which uses Enterprise SSO.


**Under the hood**


- **Faster, safer queries** : we moved the events, event deliveries, endpoints, and event types data layer onto` sqlc` , so queries are type-checked at build time
- **Fanout fix** : events with no matching subscriptions no longer get stuck
- **Billing** : usage now falls back to the current month when no period is set


More cleanup landed across SSO redirects and login UX. Add basic auth to an endpoint, or stand up a batch of them with one API call.
