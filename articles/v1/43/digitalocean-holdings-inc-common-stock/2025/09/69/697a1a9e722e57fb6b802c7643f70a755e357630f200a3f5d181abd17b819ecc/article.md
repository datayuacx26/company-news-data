---
schema_version: "1.0.0"
document_id: "697a1a9e722e57fb6b802c7643f70a755e357630f200a3f5d181abd17b819ecc"
company_key: "digitalocean-holdings-inc-common-stock"
company: "DigitalOcean Holdings Inc."
source_id: "digitalocean-holdings-inc-common-stock-atom-50ed4adbc240"
canonical_url: "https://www.digitalocean.com/blog/support-for-single-sign-on"
published_at: "2025-09-02T15:43:19.934+00:00"
first_seen_at: "2026-07-20T03:30:06.260557+00:00"
fetched_at: "2026-07-28T20:56:25.546971+00:00"
content_hash: "sha256:d73dfad745665456191e4579a58b97fc074b1fd6af782a0764d0c54473824854"
---

# Single Sign-On is Now Available, Strengthening Security and Simplifying Authentication

# Single Sign-On is Now Available, Strengthening Security and Simplifying Authentication


Good news: Single Sign-On (SSO) is now available for all DigitalOcean customers via Okta OpenID Connect (OIDC). With this launch, organizations can streamline user access, reduce IT overhead, and give teams a faster, more secure way to log in and start building. Single Sign-On represents a major step forward in strengthening security, simplifying user management, and empowering teams to be more productive on DigitalOcean.


## Why Single Sign-On matters


For growing teams, managing user accounts across multiple platforms can quickly become a challenge. Password resets, manual onboarding, and offboarding all add up, not just for the IT team–but developers feel it too in terms of lost productivity.


Single Sign-On solves this by letting users sign in with their existing corporate credentials through trusted identity providers like Okta. That means:


-


No more juggling multiple usernames and passwords.


-


Centralized access management that scales with your team.


-


No more wasting time being locked out of critical apps.


-


Stronger, consistent security across your entire toolchain.


## How Single Sign-On can benefit your business


As organizations grow, managing user access across multiple tools and platforms can become complex, time-consuming, and costly. Single Sign-On solves this problem by allowing employees to securely access all their applications with a single set of credentials.


By centralizing authentication, SSO not only strengthens security but also streamlines onboarding, reduces IT workload, and gives teams a seamless login experience that helps them stay focused on building and innovating. Here’s what Single Sign-On can do for your company:


-


**Streamlined user access:** Centralize identity management and give seamless access with corporate credentials—no separate passwords required.


-


**Increased simplicity:** Reduce friction for users and make access straightforward and intuitive.


-


**Scalability of teams:** Easily manage access as your team grows, ensuring consistent policies across all users.


-


**Reduced IT burden:** Automate onboarding and offboarding, giving IT greater control with less manual work.


-


**Enhanced security:** Enforce strong authentication (MFA, adaptive access) with Okta OIDC to protect sensitive data.


-


**Faster, more secure login:** Provide a consistent login experience so teams can quickly access projects and start building.


## How SSO on DigitalOcean works


For administrators, setting up Single Sign-On (SSO) with DigitalOcean through Okta is quick and uses standard OIDC protocols.


1.


**Configure in Okta** : In the Okta Admin Console, create DigitalOcean as an OIDC web application. Use the[SSO setup guide](https://docs.digitalocean.com/platform/teams/how-to/configure-sso/) to map user groups and roles.


2.


**Enable in DigitalOcean** : In the DigitalOcean Control Panel, add your Identity Provider (IdP) credentials (client ID, client secret, and domain). Once enabled, you’ll receive a login URL unique to your organization.


3.


**Connect back in Okta** : Paste the generated URL into your DigitalOcean application settings in Okta to finalize the connection.


After setup, administrators can choose to enforce SSO-only login. New team members will be automatically provisioned in DigitalOcean at first login, eliminating manual user management.


For users, the experience is seamless and secure. They simply log in through their Okta dashboard and click the DigitalOcean tile or use the direct login URL. Their access always respects your organization’s security policies, delivering a safe and frictionless login every time.


## Get started with SSO on DigitalOcean


Explore any of the resources below to get started:


-


[Product documentation](https://docs.digitalocean.com/platform/teams/settings/single-sign-on/)


-


[Identity and Access Management website](https://www.digitalocean.com/products/identity-access-management)
