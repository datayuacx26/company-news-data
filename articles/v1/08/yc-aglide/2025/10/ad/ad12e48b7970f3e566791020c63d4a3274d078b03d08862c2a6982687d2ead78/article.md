---
schema_version: "1.0.0"
document_id: "ad12e48b7970f3e566791020c63d4a3274d078b03d08862c2a6982687d2ead78"
company_key: "yc-aglide"
company: "Aglide"
source_id: "yc-aglide-rss-a73e409223ac"
canonical_url: "https://blog.aglide.com/guides/enable-sso-saml-scim-for-cubix-with-aglide/"
published_at: "2025-10-13T18:18:23+00:00"
first_seen_at: "2026-07-24T14:45:00.121943+00:00"
fetched_at: "2026-07-28T21:59:43.481629+00:00"
content_hash: "sha256:4c2f79531eb0828550baebd9eb21c01445a0b582915ec2c45bfdd05e14cacfb6"
---

# Enable SSO (SAML, SCIM) for Customers Bank

Most teams using CUBIX or Customers Bank discover that it lacks built-in Single Sign-On (SSO) support, whether through SAML or OIDC.


That’s where Aglide comes in. Aglide lets customers enable SAML authentication and SCIM lifecycle management for apps that don't natively support it, like CUBIX. This guide will outline how to set it up.


---


### **How Aglide connects CUBIX to your IdP**


Aglide makes CUBIX act like a native SAML/OIDC app, allowing direct connection to their IdP (Okta, Microsoft Entra ID/Azure AD, Ping Identity, etc.).


It works by giving it its own SAML and SCIM configuration, letting users log in securely while admins control access centrally.


---


### **Benefits of connecting CUBIX via Aglide**


Aglide **** is the only product of its type that create complete native IdP integrations - no bookmarks or half solutions - giving:


- **Phishing-resistant SSO** - full access policy enforcement at the IdP level like any other SAML app.
- **SCIM lifecycle automation** - auto-create, update, and disable user access instantly, easily meeting compliance requirements.
- **Central visibility** - audit all CUBIX access and sign-ins from your IdP alongside every other banking portal.


Aglide also enables:


- **SSO for shared logins** - manage shared accounts safely... not over Slack. Admin service accounts, or developer access to banks is covered by Aglide.
- **More secure than SWA** - passwords never enter the browser, so can never be miss-handled.
- **Automated credential rotation** - good password hygiene can be enforced, but minimum requirements, and regular rotation.
- **Flag unexpected user access** - Aglide detected irregularities at the bank level, notifying you of orphaned or unauthorised access.


There's even more to[Aglide](https://aglide.com/?ref=blog.aglide.com) .


---


### **Setup overview**


1. Create a new CUBIX connector in Aglide.
2. Follow the quick Show & Tell setup flow.
3. Connect your IdP (Okta, Entra, Ping).
4. Test sign-ins and SCIM provisioning.
5. Assign CUBIX to the right users, individually or by group.


Setup takes just a few minutes, and can be completed for any application without relying on a catalogue of integrations **.** See related guides for[Citibank](https://blog.aglide.com/add-sso-saml-scim-to-citibank-with-aglide/) and[J.P. Morgan](https://blog.aglide.com/enable-enterprise-sso-for-jp-morgan-with-aglide/) .


---


### **Secure teams use Aglide**


Aglide aligns CUBIX with your IGA and full security stack (SailPoint, Omada, Opal, ConductorOne) for unified governance and access reviews.


Automations run on device and credentials are locally zero-trust encrypted. This means your accounts are never accessible to Aglide servers.[Aglide's security whitepaper](https://www.aglide.com/security?ref=blog.aglide.com) provides complete details.


---


### **Get started**


Connect Cubix to your Okta, Entra, or Ping environment today.


Book a demo to see how Aglide turns disconnected apps into secure, managed SSO integrations.


[Book Demo](https://aglide.com/book-demo?ref=blog.aglide.com)
