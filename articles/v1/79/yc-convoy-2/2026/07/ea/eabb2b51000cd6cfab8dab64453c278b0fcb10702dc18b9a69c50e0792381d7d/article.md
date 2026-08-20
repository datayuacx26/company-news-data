---
schema_version: "1.0.0"
document_id: "eabb2b51000cd6cfab8dab64453c278b0fcb10702dc18b9a69c50e0792381d7d"
company_key: "yc-convoy-2"
company: "Convoy"
source_id: "yc-convoy-2-news-import-6ac8b1bcd379"
canonical_url: "https://www.getconvoy.io/changelog/endpoint-url-templates-billing-and-delivery-insights"
published_at: null
first_seen_at: "2026-07-25T00:37:26.853065+00:00"
fetched_at: "2026-07-28T21:38:29.998390+00:00"
content_hash: "sha256:6ef77a05c63607e9f6f9cd6564fa79e9af9ef76f423aea7e7c35c972f853a0e0"
---

# Endpoint URL Templates, Billing & Delivery Insights

Here's what shipped in June across the gateway and dashboard.


**Endpoint URL templates**


Endpoints support URL templates. Register one endpoint with placeholder tokens in its path or query, and Convoy matches incoming dynamic events against it and delivers to each event's concrete URL, so one endpoint can route to many destinations without you creating each one by hand.


URL templates are a Premium feature. Turn on the **Endpoint URL Templates** early adopter flag for your organization under **Settings → Early Adopter Features** .


**Sign up with Google**


Google sign-in already lived on the login page; now the signup page has it too. Click **Sign up with Google** , complete the OAuth flow, and your account is created. One less password for new users. The button shows up when Google OAuth is enabled for your instance.


**Delivery insights**


We added more detail to every delivery attempt so debugging is easier:


- **Request and response timestamps** on each attempt, so you can see exactly when Convoy sent and when your endpoint replied
- **Attempt source IP** , so you know which Convoy instance made the call
- **Delivery descriptions** that explain why an attempt landed where it did
- **Retry budget sync** so the configured retry limit and the queue stay in agreement


**Delivery modes**


Subscriptions now expose a **Delivery Mode** when you create or edit them. **At least once** (the default) keeps Convoy's retry behavior: failed deliveries retry until your endpoint responds successfully or retries are exhausted. **At most once** treats any HTTP response from your endpoint as final, so there is a single attempt; Convoy only retries when there is no HTTP response at all, such as a network error.


**Dashboard improvements**


- **Mobile responsiveness** : the dashboard now works across phone and tablet sizes
- **Org switcher** : search, counts, and pagination make it quick to jump between organizations


**Billing**


Self-hosted instances can now buy a license with guest checkout, right from the dashboard. We also fixed billing cycles to track your real subscription period (start, end, and next invoice date) instead of a fixed calendar month, so usage lines up with what you're actually billed. Go to **Settings → Usage and Billing** . Billing is available to billing admins and organization admins.


**Security hardening**


We tightened auth and outbound request handling, scoped portal-link access to owned endpoints, and required org membership on dashboard org routes.


Try a URL template on your next endpoint, then check **Settings → Usage and Billing** to see where you stand.
